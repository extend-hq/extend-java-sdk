# Extend Java Library

[![Maven Central](https://img.shields.io/maven-central/v/ai.extend/extend-java-sdk)](https://central.sonatype.com/artifact/ai.extend/extend-java-sdk)

The Extend Java library provides convenient, strongly typed access to the [Extend API](https://docs.extend.ai/2026-02-09/developers) — enabling you to parse, extract, classify, split, and edit documents with a few lines of code.

## Installation

### Gradle

```groovy
dependencies {
    implementation 'ai.extend:extend-java-sdk:0.x.x'
}
```

### Maven

```xml
<dependency>
    <groupId>ai.extend</groupId>
    <artifactId>extend-java-sdk</artifactId>
    <version>0.x.x</version>
</dependency>
```

> Requires Java 8+. Built on OkHttp and Jackson.

## Quick start

Parse any document in a few lines:

```java
import ai.extend.wrapper.ExtendClientWrapper;
import ai.extend.requests.ParseRequest;
import ai.extend.types.FileFromUrl;
import ai.extend.types.ParseRequestFile;
import ai.extend.types.ParseRun;

ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .build();

ParseRun result = client.parse(ParseRequest.builder()
    .file(ParseRequestFile.of(FileFromUrl.builder().url("https://example.com/invoice.pdf").build()))
    .build());

for (var chunk : result.getOutput().get().getChunks()) {
    System.out.println(chunk.getContent());
}
```

`client.parse` sends the file, waits for processing, and returns a fully populated `ParseRun` with parsed chunks ready to use. The same pattern works for every capability:

```java
import ai.extend.requests.*;
import ai.extend.types.*;

// Extract structured data
ExtractRun extractRun = client.extract(ExtractRequest.builder()
    .file(ExtractRequestFile.of(FileFromUrl.builder().url("https://example.com/invoice.pdf").build()))
    .extractor(ExtractRequestExtractor.of(
        ExtractorReference.builder().id("ext_abc123").build()))
    .build());

// Classify a document
ClassifyRun classifyRun = client.classify(ClassifyRequest.builder()
    .file(ClassifyRequestFile.of(FileFromUrl.builder().url("https://example.com/document.pdf").build()))
    .classifier(ClassifyRequestClassifier.of(
        ClassifierReference.builder().id("cls_abc123").build()))
    .build());

// Split a multi-document file
SplitRun splitRun = client.split(SplitRequest.builder()
    .file(SplitRequestFile.of(FileFromUrl.builder().url("https://example.com/packet.pdf").build()))
    .splitter(SplitRequestSplitter.of(
        SplitterReference.builder().id("spl_abc123").build()))
    .build());

// Fill form fields in a PDF
EditRun editRun = client.edit(EditRequest.builder()
    .file(EditRequestFile.of(FileFromUrl.builder().url("https://example.com/form.pdf").build()))
    .build());
```

> **Note:** The synchronous methods above have a 5-minute timeout and are best suited for onboarding and testing. For production workloads, use [polling helpers](#polling-helpers) or [webhooks](#webhook-verification) instead.

## Polling helpers

Every run resource exposes a `createAndPoll()` method that creates the run and automatically polls until it reaches a terminal state (`PROCESSED`, `FAILED`, or `CANCELLED`):

```java
import ai.extend.resources.extractruns.requests.ExtractRunsCreateRequest;

ExtractRun result = client.extractRuns().createAndPoll(
    ExtractRunsCreateRequest.builder()
        .file(ExtractRunsCreateRequestFile.of(
            FileFromUrl.builder().url("https://example.com/invoice.pdf").build()))
        .extractor(ExtractRunsCreateRequestExtractor.of(
            ExtractorReference.builder().id("ext_abc123").build()))
        .build()
);

if (result.getStatus() == ProcessorRunStatus.PROCESSED) {
    System.out.println(result.getOutput());
} else {
    System.out.println("Failed: " + result.getFailureMessage());
}
```

This works across all run types:

```java
ParseRun    parseRun    = client.parseRuns().createAndPoll(parseRequest);
ExtractRun  extractRun  = client.extractRuns().createAndPoll(extractRequest);
ClassifyRun classifyRun = client.classifyRuns().createAndPoll(classifyRequest);
SplitRun    splitRun    = client.splitRuns().createAndPoll(splitRequest);
WorkflowRun workflowRun = client.workflowRuns().createAndPoll(workflowRequest);
EditRun     editRun     = client.editRuns().createAndPoll(editRequest);
```

### Custom polling options

```java
import ai.extend.wrapper.utilities.polling.PollingOptions;

ExtractRun result = client.extractRuns().createAndPoll(
    request,
    PollingOptions.builder()
        .maxWaitMs(300_000)       // 5 minute timeout (default: no timeout)
        .initialDelayMs(1_000)    // start with 1s delay (default)
        .maxDelayMs(60_000)       // cap at 60s delay (default: 30s)
        .jitterFraction(0.25)     // +/-25% randomization (default)
        .build()
);
```

## Webhook verification

Verify and parse incoming webhook events using the built-in utilities:

```java
import ai.extend.wrapper.webhooks.Webhooks;
import java.util.Map;

Webhooks webhooks = client.webhooks();

// In your webhook handler
Map<String, Object> event = webhooks.verifyAndParse(
    requestBody,
    requestHeaders,
    "wss_your_signing_secret"
);

String eventType = (String) event.get("eventType");

switch (eventType) {
    case "extract_run.processed":
        System.out.println("Extraction complete");
        break;
    case "workflow_run.completed":
        System.out.println("Workflow complete");
        break;
    default:
        System.out.println("Received event: " + eventType);
}
```

### Manual verification

```java
// Verify signature without parsing
boolean isValid = webhooks.verify(body, headers, signingSecret);

// Parse without verification (not recommended for production)
RawWebhookEvent event = webhooks.parse(body);
```

### Signed URL payloads

For large payloads, Extend may send a signed URL instead of the full payload. The SDK handles this transparently:

```java
import ai.extend.wrapper.webhooks.VerifyAndParseOptions;
import ai.extend.wrapper.webhooks.VerifyAndParseResult;
import ai.extend.wrapper.webhooks.WebhookEventWithSignedUrl;

VerifyAndParseResult result = webhooks.verifyAndParseWithOptions(
    body, headers, signingSecret,
    VerifyAndParseOptions.builder().allowSignedUrl(true).build()
);

if (result.isSignedUrlEvent()) {
    WebhookEventWithSignedUrl signedEvent = result.getSignedUrlEvent();
    Map<String, Object> fullPayload = webhooks.fetchSignedPayload(signedEvent);
} else {
    Map<String, Object> event = result.getEvent();
}
```

## Exception handling

When the API returns a non-success status code (4xx or 5xx response), a typed exception is thrown:

```java
import ai.extend.core.ExtendClientApiException;

try {
    client.parse(request);
} catch (ExtendClientApiException e) {
    System.out.println(e.statusCode());  // 400, 401, 404, 429, etc.
    System.out.println(e.body());
}
```

Specific error classes are available for fine-grained handling:

```java
import ai.extend.errors.*;

try {
    client.extractRuns().create(request);
} catch (BadRequestError e) { /* 400 */ }
  catch (UnauthorizedError e) { /* 401 */ }
  catch (PaymentRequiredError e) { /* 402 */ }
  catch (ForbiddenError e) { /* 403 */ }
  catch (NotFoundError e) { /* 404 */ }
  catch (TooManyRequestsError e) { /* 429 */ }
  catch (InternalServerError e) { /* 500 */ }
```

### Polling timeout

When `createAndPoll()` exceeds its timeout, a `PollingTimeoutError` is thrown:

```java
import ai.extend.wrapper.errors.PollingTimeoutError;

try {
    client.extractRuns().createAndPoll(request,
        PollingOptions.builder().maxWaitMs(60_000).build());
} catch (PollingTimeoutError e) {
    System.out.println("Timed out after " + e.getElapsedMs() + "ms");
}
```

## Pagination

List endpoints return paginated results using `nextPageToken`:

```java
// First page
ExtractRunsListResponse response = client.extractRuns().list(
    ExtractRunsListRequest.builder().maxPageSize(10).build()
);

for (var run : response.getData()) {
    System.out.println(run.getId() + ": " + run.getStatus());
}

// Next page
if (response.getNextPageToken().isPresent()) {
    ExtractRunsListResponse nextPage = client.extractRuns().list(
        ExtractRunsListRequest.builder()
            .maxPageSize(10)
            .nextPageToken(response.getNextPageToken().get())
            .build()
    );
}
```

## Environments

The SDK defaults to the US production environment. Other regions are available:

```java
import ai.extend.core.Environment;

// US (default)
ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .build();

// US2 (HIPAA)
ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .environment(Environment.PRODUCTION_US_2)
    .build();

// EU
ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .environment(Environment.PRODUCTION_EU_1)
    .build();

// Custom base URL
ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .url("https://custom-api.example.com")
    .build();
```

## Advanced

### Retries

The SDK automatically retries failed requests with exponential backoff. Retries are triggered for:

- `408` Timeout
- `429` Too Many Requests
- `5xx` Server Errors

```java
// Configure max retries at client level
ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .maxRetries(1)
    .build();
```

### Timeouts

The default timeout is 60 seconds. Override globally or per-request:

```java
import ai.extend.core.RequestOptions;

// Global timeout
ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .timeout(30)
    .build();

// Per-request timeout
client.extractRuns().create(request,
    RequestOptions.builder().timeout(60).build());
```

### Custom headers

```java
// Client-level headers
ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .addHeader("X-Custom-Header", "value")
    .build();

// Per-request headers
client.extractRuns().create(request,
    RequestOptions.builder()
        .addHeader("X-Request-Id", "abc-123")
        .build());
```

### Custom HTTP client

The SDK is built on OkHttp. Pass your own client for full control:

```java
import okhttp3.OkHttpClient;

OkHttpClient customClient = new OkHttpClient.Builder()
    .connectTimeout(10, TimeUnit.SECONDS)
    .build();

ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("YOUR_API_KEY")
    .httpClient(customClient)
    .build();
```

### Raw response data

Access underlying HTTP response metadata through `withRawResponse()`:

```java
import ai.extend.core.ExtendClientHttpResponse;

ExtendClientHttpResponse<ParseRun> response = client.withRawResponse()
    .parse(request);

System.out.println(response.statusCode());
System.out.println(response.headers());
System.out.println(response.body());  // ParseRun
```

## Documentation

Full API reference documentation is available at [docs.extend.ai](https://docs.extend.ai/2026-02-09/developers).

A complete SDK reference is available in [reference.md](./reference.md).

## Contributing

While we value open-source contributions to this SDK, this library is generated programmatically. Additions made directly to this library would have to be moved over to our generation code, otherwise they would be overwritten upon the next generated release. Feel free to open a PR as a proof of concept, but know that we will not be able to merge it as-is. We suggest opening an issue first to discuss with us!

On the other hand, contributions to the README are always very welcome!
