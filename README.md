# Extend.ai Java Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2Fextend-hq%2Fextend-java-sdk)
[![Maven Central](https://img.shields.io/maven-central/v/ai.extend/extend-java-sdk)](https://central.sonatype.com/artifact/ai.extend/extend-java-sdk)

The Extend Java library provides convenient access to the Extend APIs from Java.

## Documentation

API reference documentation is available [here](https://docs.extend.ai/2026-01-01/developers).

## Installation

### Gradle

Add the dependency in your `build.gradle` file:

```groovy
dependencies {
  implementation 'ai.extend:extend-java-sdk'
}
```

### Maven

Add the dependency in your `pom.xml` file:

```xml
<dependency>
  <groupId>ai.extend</groupId>
  <artifactId>extend-java-sdk</artifactId>
  <version>0.0.3-beta</version>
</dependency>
```

## Reference

A full reference for this library is available [here](https://github.com/extend-hq/extend-java-sdk/blob/HEAD/./reference.md).

## Usage

Instantiate and use the client with the following:

```java
package com.example.usage;

import ai.extend.ExtendClient;
import ai.extend.resources.workflowrun.requests.WorkflowRunCreateRequest;

public class Example {
    public static void main(String[] args) {
        ExtendClient client = ExtendClient
            .builder()
            .token("<token>")
            .build();

        client.workflowRun().create(
            WorkflowRunCreateRequest
                .builder()
                .workflowId("workflow_id_here")
                .build()
        );
    }
}
```

## Environments

This SDK allows you to configure different environments for API requests.

```java
import ai.extend.ExtendClient;
import ai.extend.core.Environment;

ExtendClient client = ExtendClient
    .builder()
    .environment(Environment.production)
    .build();
```

## Base Url

You can set a custom base URL when constructing the client.

```java
import ai.extend.ExtendClient;

ExtendClient client = ExtendClient
    .builder()
    .url("https://example.com")
    .build();
```

## Wrapper Client with Polling and Webhooks

The SDK includes a wrapper client (`ExtendClientWrapper`) that provides convenient methods for common patterns like polling and webhook verification.

### Using the Wrapper Client

```java
import ai.extend.wrapper.ExtendClientWrapper;
import ai.extend.resources.extractruns.requests.ExtractRunsCreateRequest;

ExtendClientWrapper client = ExtendClientWrapper.builder()
    .apiKey("your-api-key")
    .build();
```

### Create and Poll

The wrapper adds `createAndPoll()` methods to all run resources. This creates a run and polls until it reaches a terminal state:

```java
import ai.extend.resources.extractruns.types.ExtractRunsRetrieveResponse;
import ai.extend.types.ProcessorRunStatus;

// Create an extract run and wait for completion
ExtractRunsRetrieveResponse response = client.extractRuns().createAndPoll(
    ExtractRunsCreateRequest.builder()
        .file(...)
        .extractor(...)
        .build()
);

if (response.getExtractRun().getStatus() == ProcessorRunStatus.PROCESSED) {
    // Handle successful extraction
    var output = response.getExtractRun().getOutput();
}
```

#### Custom Polling Options

```java
import ai.extend.wrapper.utilities.polling.PollingOptions;

// With custom timeout
ExtractRunsRetrieveResponse response = client.extractRuns().createAndPoll(
    request,
    PollingOptions.builder()
        .maxWaitMs(300000)      // 5 minute timeout (default: no timeout)
        .initialDelayMs(1000)   // Start with 1 second delay (default)
        .maxDelayMs(60000)      // Cap at 60 seconds (default)
        .jitterFraction(0.25)   // ±25% randomization (default)
        .build()
);
```

#### Workflow Runs

```java
var response = client.workflowRuns().createAndPoll(
    WorkflowRunsCreateRequest.builder()
        .file(...)
        .workflow(...)
        .build()
);
```

### Webhook Verification

The wrapper includes utilities for verifying webhook signatures:

```java
import ai.extend.wrapper.webhooks.Webhooks;
import java.util.Map;

// In your webhook handler
String body = request.getBody();
Map<String, String> headers = request.getHeaders();
String signingSecret = "wss_your_signing_secret";

Webhooks webhooks = client.webhooks();

// Verify and parse the webhook
Map<String, Object> event = webhooks.verifyAndParse(body, headers, signingSecret);

String eventType = (String) event.get("eventType");
Map<String, Object> payload = (Map<String, Object>) event.get("payload");
```

#### Handling Signed URL Payloads

For large payloads, Extend may send a signed URL instead of the full data:

```java
import ai.extend.wrapper.webhooks.VerifyAndParseOptions;
import ai.extend.wrapper.webhooks.VerifyAndParseResult;
import ai.extend.wrapper.webhooks.WebhookEventWithSignedUrl;

// Allow signed URL payloads
VerifyAndParseResult result = webhooks.verifyAndParseWithOptions(body, headers, signingSecret,
    VerifyAndParseOptions.builder().allowSignedUrl(true).build());

if (result.isSignedUrlEvent()) {
    WebhookEventWithSignedUrl signedEvent = result.getSignedUrlEvent();
    
    // Fetch the full payload
    Map<String, Object> fullEvent = webhooks.fetchSignedPayload(signedEvent);
} else {
    Map<String, Object> event = result.getEvent();
}
```

#### Verify Without Parsing

```java
// Just verify signature (returns true/false)
boolean isValid = webhooks.verify(body, headers, signingSecret);
```

### Error Handling

The wrapper defines custom exceptions for common scenarios:

```java
import ai.extend.wrapper.errors.PollingTimeoutError;
import ai.extend.wrapper.errors.WebhookSignatureVerificationError;
import ai.extend.wrapper.errors.SignedUrlNotAllowedError;

try {
    client.extractRuns().createAndPoll(request);
} catch (PollingTimeoutError e) {
    System.out.println("Polling timed out after " + e.getElapsedMs() + "ms");
}

try {
    webhooks.verifyAndParse(body, headers, secret);
} catch (WebhookSignatureVerificationError e) {
    System.out.println("Invalid webhook signature: " + e.getMessage());
} catch (SignedUrlNotAllowedError e) {
    System.out.println("Received signed URL but not enabled");
}
```

## Exception Handling

When the API returns a non-success status code (4xx or 5xx response), an API exception will be thrown.

```java
import ai.extend.core.ExtendClientApiException;

try {
    client.workflowRun().create(...);
} catch (ExtendClientApiException e) {
    // Do something with the API exception...
}
```

## Advanced

### Custom Client

This SDK is built to work with any instance of `OkHttpClient`. By default, if no client is provided, the SDK will construct one.
However, you can pass your own client like so:

```java
import ai.extend.ExtendClient;
import okhttp3.OkHttpClient;

OkHttpClient customClient = ...;

ExtendClient client = ExtendClient
    .builder()
    .httpClient(customClient)
    .build();
```

### Retries

The SDK is instrumented with automatic retries with exponential backoff. A request will be retried as long
as the request is deemed retryable and the number of retry attempts has not grown larger than the configured
retry limit (default: 2).

A request is deemed retryable when any of the following HTTP status codes is returned:

- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [5XX](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) (Internal Server Errors)

Use the `maxRetries` client option to configure this behavior.

```java
import ai.extend.ExtendClient;

ExtendClient client = ExtendClient
    .builder()
    .maxRetries(1)
    .build();
```

### Timeouts

The SDK defaults to a 60 second timeout. You can configure this with a timeout option at the client or request level.

```java
import ai.extend.ExtendClient;
import ai.extend.core.RequestOptions;

// Client level
ExtendClient client = ExtendClient
    .builder()
    .timeout(10)
    .build();

// Request level
client.workflowRun().create(
    ...,
    RequestOptions
        .builder()
        .timeout(10)
        .build()
);
```

### Custom Headers

The SDK allows you to add custom headers to requests. You can configure headers at the client level or at the request level.

```java
import ai.extend.ExtendClient;
import ai.extend.core.RequestOptions;

// Client level
ExtendClient client = ExtendClient
    .builder()
    .addHeader("X-Custom-Header", "custom-value")
    .addHeader("X-Request-Id", "abc-123")
    .build();
;

// Request level
client.workflowRun().create(
    ...,
    RequestOptions
        .builder()
        .addHeader("X-Request-Header", "request-value")
        .build()
);
```

## Contributing

While we value open-source contributions to this SDK, this library is generated programmatically.
Additions made directly to this library would have to be moved over to our generation code,
otherwise they would be overwritten upon the next generated release. Feel free to open a PR as
a proof of concept, but know that we will not be able to merge it as-is. We suggest opening
an issue first to discuss with us!

On the other hand, contributions to the README are always very welcome!
