# Extendconfig Java Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2Fextend-hq%2Fextend-java-sdk)

The Extendconfig Java library provides convenient access to the Extendconfig APIs from Java.

## Documentation

API reference documentation is available [here](https://docs.extend.ai/2025-04-21/developers).

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

## Exception Handling

When the API returns a non-success status code (4xx or 5xx response), an API exception will be thrown.

```java
import ai.extend.core.ExtendconfigApiApiException;

try {
    client.workflowRun().create(...);
} catch (ExtendconfigApiApiException e) {
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