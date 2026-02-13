# Reference
<details><summary><code>client.parse(request) -> ParserRun</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Parse files to get cleaned, chunked target content (e.g. markdown).

The Parse endpoint allows you to convert documents into structured, machine-readable formats with fine-grained control over the parsing process. This endpoint is ideal for extracting cleaned document content to be used as context for downstream processing, e.g. RAG pipelines, custom ingestion pipelines, embeddings classification, etc.

For more details, see the [Parse File guide](/product/parsing/parse).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.parse(
    ParseRequest
        .builder()
        .file(
            ParseRequestFile
                .builder()
                .build()
        )
        .responseType(ParseRequestResponseType.JSON)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**responseType:** `Optional<ParseRequestResponseType>` 

Controls the format of the response chunks. Defaults to `json` if not specified.
* `json` - Returns parsed outputs in the response body
* `url` - Return a presigned URL to the parsed content in the response body
    
</dd>
</dl>

<dl>
<dd>

**file:** `ParseRequestFile` — A file object containing either a URL or a fileId.
    
</dd>
</dl>

<dl>
<dd>

**config:** `Optional<ParseConfig>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.parseAsync(request) -> ParserRunStatus</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Parse files **asynchronously** to get cleaned, chunked target content (e.g. markdown).

The Parse Async endpoint allows you to convert documents into structured, machine-readable formats with fine-grained control over the parsing process. This endpoint is ideal for extracting cleaned document content to be used as context for downstream processing, e.g. RAG pipelines, custom ingestion pipelines, embeddings classification, etc.

Parse files asynchronously and get a parser run ID that can be used to check status and retrieve results with the [Get Parser Run](https://docs.extend.ai/2025-04-21/developers/api-reference/parse-endpoints/get-parser-run) endpoint.

This is useful for:
* Large files that may take longer to process
* Avoiding timeout issues with synchronous parsing.

For more details, see the [Parse File guide](/product/parsing/parse).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.parseAsync(
    ParseAsyncRequest
        .builder()
        .file(
            ParseAsyncRequestFile
                .builder()
                .build()
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**file:** `ParseAsyncRequestFile` — A file object containing either a URL or a fileId.
    
</dd>
</dl>

<dl>
<dd>

**config:** `Optional<ParseConfig>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## File
<details><summary><code>client.file.list() -> FileListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

List files in your account. Files represent documents that have been uploaded to Extend. This endpoint returns a paginated response. You can use the `nextPageToken` to fetch subsequent results.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.file().list(
    FileListRequest
        .builder()
        .nameContains("nameContains")
        .sortDir(SortDirEnum.ASC)
        .nextPageToken("xK9mLPqRtN3vS8wF5hB2cQ==:zWvUxYjM4nKpL7aDgE9HbTcR2mAyX3/Q+CNkfBSw1dZ=")
        .maxPageSize(1)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**nameContains:** `Optional<String>` 

Filters files to only include those that contain the given string in the name.

Example: `"invoice"`
    
</dd>
</dl>

<dl>
<dd>

**sortDir:** `Optional<SortDirEnum>` — Sorts the files in ascending or descending order. Ascending order means the earliest file is returned first.
    
</dd>
</dl>

<dl>
<dd>

**nextPageToken:** `Optional<String>` 
    
</dd>
</dl>

<dl>
<dd>

**maxPageSize:** `Optional<Integer>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.file.get(id) -> FileGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Fetch a file by its ID to obtain additional details and the raw file content.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.file().get(
    "file_id_here",
    FileGetRequest
        .builder()
        .rawText(true)
        .markdown(true)
        .html(true)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

Extend's ID for the file. It will always start with `"file_"`. This ID is returned when creating a new File, or the value on the `fileId` field in a WorkflowRun.

Example: `"file_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>

<dl>
<dd>

**rawText:** `Optional<Boolean>` — If set to true, the raw text content of the file will be included in the response. This is useful for indexing text-based files like PDFs, Word Documents, etc.
    
</dd>
</dl>

<dl>
<dd>

**markdown:** `Optional<Boolean>` 

If set to true, the markdown content of the file will be included in the response. This is useful for indexing very clean content into RAG pipelines for files like PDFs, Word Documents, etc.

Only available for files with a type of PDF, IMG, or .doc/.docx files that were auto-converted to PDFs.
    
</dd>
</dl>

<dl>
<dd>

**html:** `Optional<Boolean>` 

If set to true, the html content of the file will be included in the response. This is useful for indexing html content into RAG pipelines.

Only available for files with a type of DOCX.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.file.delete(id) -> FileDeleteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Delete a file and all associated data from Extend. This operation is permanent and cannot be undone.

This endpoint can be used if you'd like to manage data retention on your own rather than automated data retention policies. Or make one-off deletions for your downstream customers.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.file().delete("file_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the file to delete.

Example: `"file_xK9mLPqRtN3vS8wF5hB2cQ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.file.upload(request) -> FileUploadResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Upload and create a new file in Extend.

This endpoint accepts file contents and registers them as a File in Extend, which can be used for [running workflows](https://docs.extend.ai/2025-04-21/developers/api-reference/workflow-endpoints/run-workflow), [creating evaluation set items](https://docs.extend.ai/2025-04-21/developers/api-reference/evaluation-set-endpoints/bulk-create-evaluation-set-items), [parsing](https://docs.extend.ai/2025-04-21/developers/api-reference/parse-endpoints/parse-file), etc.

If an uploaded file is detected as a Word or PowerPoint document, it will be automatically converted to a PDF.

Supported file types can be found [here](/product/general/supported-file-types).

This endpoint requires multipart form encoding. Most HTTP clients will handle this encoding automatically (see the examples).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.file().upload(
    FileUploadRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## ParserRun
<details><summary><code>client.parserRun.get(id) -> ParserRunGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve the status and results of a parser run.

Use this endpoint to get results for a parser run that has already completed, or to check on the status of an asynchronous parser run initiated via the [Parse File Asynchronously](https://docs.extend.ai/2025-04-21/developers/api-reference/parse-endpoints/parse-file-async) endpoint.

If parsing is still in progress, you'll receive a response with just the status. Once complete, you'll receive the full parsed content in the response.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.parserRun().get(
    "parser_run_id_here",
    ParserRunGetRequest
        .builder()
        .responseType(ParserRunGetRequestResponseType.JSON)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The unique identifier for the parser run.

Example: `"parser_run_xK9mLPqRtN3vS8wF5hB2cQ"`
    
</dd>
</dl>

<dl>
<dd>

**responseType:** `Optional<ParserRunGetRequestResponseType>` 

Controls the format of the response chunks. Defaults to `json` if not specified.
* `json` - Returns chunks with inline content
* `url` - Returns chunks with presigned URLs to content instead of inline data
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.parserRun.delete(id) -> ParserRunDeleteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Delete a parser run and all associated data from Extend. This operation is permanent and cannot be undone.

This endpoint can be used if you'd like to manage data retention on your own rather than automated data retention policies. Or make one-off deletions for your downstream customers.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.parserRun().delete("parser_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the parser run to delete.

Example: `"parser_run_xK9mLPqRtN3vS8wF5hB2cQ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Edit
<details><summary><code>client.edit.create(request) -> EditRun</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Edit and manipulate PDF documents by detecting and filling form fields.
This is a synchronous endpoint that will wait for the edit operation to complete (up to 5 minutes) before returning results. For longer operations, use the [Edit File Async](/developers/api-reference/edit-endpoints/edit-file-async) endpoint.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.edit().create(
    EditCreateRequest
        .builder()
        .file(
            EditCreateRequestFile
                .builder()
                .build()
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**file:** `EditCreateRequestFile` — A file object containing either a URL or a fileId.
    
</dd>
</dl>

<dl>
<dd>

**config:** `Optional<EditCreateRequestConfig>` — Configuration for the edit operation. Field values should be specified using `extend_edit:value` on each field in the schema.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.edit.createAsync(request) -> EditRunStatus</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Edit and manipulate PDF documents **asynchronously** by filling forms, adding/modifying text fields, and applying structured changes.

The Edit Async endpoint allows you to convert and edit documents asynchronously and get an edit run ID that can be used to check status and retrieve results with the [Get Edit Run](/developers/api-reference/edit-endpoints/get-edit-run) endpoint.

This is useful for:
* Large files that may take longer to process
* Avoiding timeout issues with synchronous editing
* Processing multiple files in parallel
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.edit().createAsync(
    EditCreateAsyncRequest
        .builder()
        .file(
            EditCreateAsyncRequestFile
                .builder()
                .build()
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**file:** `EditCreateAsyncRequestFile` — A file object containing either a URL or a fileId.
    
</dd>
</dl>

<dl>
<dd>

**config:** `Optional<EditCreateAsyncRequestConfig>` — Configuration for the edit operation. Field values should be specified using `extend_edit:value` on each field in the schema.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.edit.get(id) -> EditGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve the status and results of an edit run.

Use this endpoint to get results for an edit run that has already completed, or to check on the status of an asynchronous edit run initiated via the [Edit File Asynchronously](/developers/api-reference/edit-endpoints/edit-file-async) endpoint.

If editing is still in progress, you'll receive a response with just the status. Once complete, you'll receive the full edited file information in the response.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.edit().get("edit_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The unique identifier for the edit run.

Example: `"edit_run_xK9mLPqRtN3vS8wF5hB2cQ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.edit.delete(id) -> EditDeleteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Delete an edit run and all associated data from Extend. This operation is permanent and cannot be undone.

This endpoint can be used if you'd like to manage data retention on your own rather than relying on automated data retention policies, or to make one-off deletions for your downstream customers.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.edit().delete("edit_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the edit run to delete.

Example: `"edit_run_xK9mLPqRtN3vS8wF5hB2cQ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Workflow
<details><summary><code>client.workflow.create(request) -> WorkflowCreateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Create a new workflow in Extend. Workflows are sequences of steps that process files and data in a specific order to achieve a desired outcome.

This endpoint will create a new workflow in Extend, which can then be configured and deployed. Typically, workflows are created from our UI, however this endpoint can be used to create workflows programmatically. Configuration of the flow still needs to be done in the dashboard.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.workflow().create(
    WorkflowCreateRequest
        .builder()
        .name("Invoice Processing")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**name:** `String` — The name of the workflow
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## WorkflowRun
<details><summary><code>client.workflowRun.list() -> WorkflowRunListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

List runs of a Workflow. Workflows are sequences of steps that process files and data in a specific order to achieve a desired outcome. A WorkflowRun represents a single execution of a workflow against a file.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.workflowRun().list(
    WorkflowRunListRequest
        .builder()
        .status(WorkflowStatus.PENDING)
        .workflowId("workflowId")
        .batchId("batchId")
        .fileNameContains("fileNameContains")
        .sortBy(SortByEnum.UPDATED_AT)
        .sortDir(SortDirEnum.ASC)
        .nextPageToken("xK9mLPqRtN3vS8wF5hB2cQ==:zWvUxYjM4nKpL7aDgE9HbTcR2mAyX3/Q+CNkfBSw1dZ=")
        .maxPageSize(1)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**status:** `Optional<WorkflowStatus>` 

Filters workflow runs by their status. If not provided, no filter is applied.

 The status of a workflow run:
 * `"PENDING"` - The workflow run has not started yet
 * `"PROCESSING"` - The workflow run is in progress
 * `"NEEDS_REVIEW"` - The workflow run requires manual review
 * `"REJECTED"` - The workflow run was rejected during manual review
 * `"PROCESSED"` - The workflow run completed successfully
 * `"FAILED"` - The workflow run encountered an error
 * `"CANCELLED"` - The workflow run was cancelled
 * `"CANCELLING"` - The workflow run is being cancelled
    
</dd>
</dl>

<dl>
<dd>

**workflowId:** `Optional<String>` 

Filters workflow runs by the workflow ID. If not provided, runs for all workflows are returned.

Example: `"workflow_BMdfq_yWM3sT-ZzvCnA3f"`
    
</dd>
</dl>

<dl>
<dd>

**batchId:** `Optional<String>` 

Filters workflow runs by the batch ID. This is useful for fetching all runs for a given batch created via the [Batch Run Workflow](/developers/api-reference/workflow-endpoints/batch-run-workflow) endpoint.

Example: `"batch_7Ws31-F5"`
    
</dd>
</dl>

<dl>
<dd>

**fileNameContains:** `Optional<String>` 

Filters workflow runs by the name of the file. Only returns workflow runs where the file name contains this string.

Example: `"invoice"`
    
</dd>
</dl>

<dl>
<dd>

**sortBy:** `Optional<SortByEnum>` — Sorts the workflow runs by the given field.
    
</dd>
</dl>

<dl>
<dd>

**sortDir:** `Optional<SortDirEnum>` — Sorts the workflow runs in ascending or descending order. Ascending order means the earliest workflow run is returned first.
    
</dd>
</dl>

<dl>
<dd>

**nextPageToken:** `Optional<String>` 
    
</dd>
</dl>

<dl>
<dd>

**maxPageSize:** `Optional<Integer>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.workflowRun.create(request) -> WorkflowRunCreateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Run a Workflow with files. A Workflow is a sequence of steps that process files and data in a specific order to achieve a desired outcome. A WorkflowRun will be created for each file processed. A WorkflowRun represents a single execution of a workflow against a file.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.workflowRun().create(
    WorkflowRunCreateRequest
        .builder()
        .workflowId("workflow_id_here")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**workflowId:** `String` 

The ID of the workflow to run.

Example: `"workflow_BMdfq_yWM3sT-ZzvCnA3f"`
    
</dd>
</dl>

<dl>
<dd>

**files:** `Optional<List<WorkflowRunFileInput>>` — An array of files to process through the workflow. Either the `files` array or `rawTexts` array must be provided. Supported file types can be found [here](/product/general/supported-file-types). There is a limit if 50 files that can be processed at once using this endpoint. If you wish to process more at a time, consider using the [Batch Run Workflow](/developers/api-reference/workflow-endpoints/batch-run-workflow) endpoint.
    
</dd>
</dl>

<dl>
<dd>

**rawTexts:** `Optional<List<String>>` — An array of raw strings. Can be used in place of files when passing raw data. The raw data will be converted to `.txt` files and run through the workflow. If the data follows a specific format, it is recommended to use the files parameter instead. Either `files` or `rawTexts` must be provided.
    
</dd>
</dl>

<dl>
<dd>

**version:** `Optional<String>` 

An optional version of the workflow that files will be run through. This number can be found when viewing the workflow on the Extend platform. When a version number is not supplied, the most recent published version of the workflow will be used. If no published versions exist, the draft version will be used. To run the `"draft"` version of a workflow, use `"draft"` as the version.

Examples:
- `"3"` - Run version 3 of the workflow
- `"draft"` - Run the draft version of the workflow
    
</dd>
</dl>

<dl>
<dd>

**priority:** `Optional<Integer>` — An optional value used to determine the relative order of WorkflowRuns when rate limiting is in effect. Lower values will be prioritized before higher values.
    
</dd>
</dl>

<dl>
<dd>

**metadata:** `Optional<Map<String, Object>>` 

An optional metadata object that can be assigned to a specific WorkflowRun to help identify it. It will be returned in the response and webhooks. You can place any arbitrary `key : value` pairs in this object.

To categorize workflow runs for billing and usage tracking, include `extend:usage_tags` with an array of string values (e.g., `{"extend:usage_tags": ["production", "team-eng", "customer-123"]}`). Tags must contain only alphanumeric characters, hyphens, and underscores; any special characters will be automatically removed.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.workflowRun.get(workflowRunId) -> WorkflowRunGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Once a workflow has been run, you can check the status and output of a specific WorkflowRun.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.workflowRun().get("workflow_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**workflowRunId:** `String` 

The ID of the WorkflowRun that was outputted after a Workflow was run through the API.

Example: `"workflow_run_8k9m-xyzAB_Pqrst-Nvw4"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.workflowRun.update(workflowRunId, request) -> WorkflowRunUpdateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

You can update the name and metadata of an in progress WorkflowRun at any time using this endpoint.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.workflowRun().update(
    "workflow_run_id_here",
    WorkflowRunUpdateRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**workflowRunId:** `String` 

The ID of the WorkflowRun. This ID will start with "workflow_run". This ID can be found in the API response when creating a Workflow Run, or in the "history" tab of a workflow on the Extend platform.

Example: `"workflow_run_8k9m-xyzAB_Pqrst-Nvw4"`
    
</dd>
</dl>

<dl>
<dd>

**name:** `Optional<String>` — An optional name that can be assigned to a specific WorkflowRun
    
</dd>
</dl>

<dl>
<dd>

**metadata:** `Optional<Map<String, Object>>` 

A metadata object that can be assigned to a specific WorkflowRun. If metadata already exists on this WorkflowRun, the newly incoming metadata will be merged with the existing metadata, with the incoming metadata taking field precedence.

You can include any arbitrary `key : value` pairs in this object.

To categorize workflow runs for billing and usage tracking, include `extend:usage_tags` with an array of string values (e.g., `{"extend:usage_tags": ["production", "team-eng", "customer-123"]}`). Tags must contain only alphanumeric characters, hyphens, and underscores; any special characters will be automatically removed.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.workflowRun.delete(workflowRunId) -> WorkflowRunDeleteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Delete a workflow run and all associated data from Extend. This operation is permanent and cannot be undone.

This endpoint can be used if you'd like to manage data retention on your own rather than automated data retention policies. Or make one-off deletions for your downstream customers.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.workflowRun().delete("workflow_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**workflowRunId:** `String` 

The ID of the workflow run to delete.

Example: `"workflow_run_xKm9pNv3qWsY_jL2tR5Dh"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.workflowRun.cancel(workflowRunId) -> WorkflowRunCancelResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Cancel a running workflow run by its ID. This endpoint allows you to stop a workflow run that is currently in progress.

Note: Only workflow runs with a status of `PROCESSING` or `PENDING` can be cancelled. Workflow runs that are completed, failed, in review, rejected, or already cancelled cannot be cancelled.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.workflowRun().cancel("workflow_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**workflowRunId:** `String` 

The ID of the workflow run to cancel.

Example: `"workflow_run_xKm9pNv3qWsY_jL2tR5Dh"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## WorkflowRunOutput
<details><summary><code>client.workflowRunOutput.update(workflowRunId, outputId, request) -> WorkflowRunOutputUpdateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Use this endpoint to submit corrected outputs for a WorkflowRun for future processor evaluation and tuning in Extend.

If you are using our Human-in-the-loop workflow review, then we already will be collecting your operator submitted corrections. However, if you are receiving data via the API without human review, there could be incorrect outputs that you would like to correct for future usage in evaluation and tuning within the Extend platform. This endpoint allows you to submit corrected outputs for a WorkflowRun, by providing the correct output for a given output ID.

The output ID, would be found in a given entry within the outputs arrays of a Workflow Run payload. The ID would look something like `dpr_gwkZZNRrPgkjcq0y-***`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.workflowRunOutput().update(
    "workflow_run_id_here",
    "output_id_here",
    WorkflowRunOutputUpdateRequest
        .builder()
        .reviewedOutput(
            ProvidedProcessorOutput.ofProvidedExtractionOutput(
                ProvidedExtractionOutput.ofProvidedJsonOutput(
                    ProvidedJsonOutput
                        .builder()
                        .value(
                            new HashMap<String, Object>() {{
                                put("key", "value");
                            }}
                        )
                        .build()
                )
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**workflowRunId:** `String` 
    
</dd>
</dl>

<dl>
<dd>

**outputId:** `String` 
    
</dd>
</dl>

<dl>
<dd>

**reviewedOutput:** `ProvidedProcessorOutput` 

The corrected output of the processor when run against the file.

This should conform to the output type schema of the given processor.

If this is an extraction result, you can include all fields, or just the ones that were corrected, our system will handle merges/dedupes. However, if you do include a field, we assume the value included in the final reviewed value.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## BatchWorkflowRun
<details><summary><code>client.batchWorkflowRun.create(request) -> BatchWorkflowRunCreateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

This endpoint allows you to efficiently initiate large batches of workflow runs in a single request (up to 1,000 in a single request, but you can queue up multiple batches in rapid succession). It accepts an array of inputs, each containing a file and metadata pair. The primary use case for this endpoint is for doing large bulk runs of >1000 files at a time that can process over the course of a few hours without needing to manage rate limits that would likely occur using the primary run endpoint.

Unlike the single [Run Workflow](/developers/api-reference/workflow-endpoints/run-workflow) endpoint which returns the details of the created workflow runs immediately, this batch endpoint returns a `batchId`.

Our recommended usage pattern is to integrate with [Webhooks](/product/webhooks/configuration) for consuming results, using the `metadata` and `batchId` to match up results to the original inputs in your downstream systems. However, you can integrate in a polling mechanism by using a combination of the [List Workflow Runs](https://docs.extend.ai/2025-04-21/developers/api-reference/workflow-endpoints/list-workflow-runs) endpoint to fetch all runs via a batch, and then [Get Workflow Run](https://docs.extend.ai/2025-04-21/developers/api-reference/workflow-endpoints/get-workflow-run) to fetch the full outputs each run.

**Priority:** All workflow runs created through this batch endpoint are automatically assigned a priority of 90.

**Processing and Monitoring:**
Upon successful submission, the endpoint returns a `batchId`. The individual workflow runs are then queued for processing.

- **Monitoring:** Track the progress and consume results of individual runs using [Webhooks](/product/webhooks/configuration). Subscribe to events like `workflow_run.completed`, `workflow_run.failed`, etc. The webhook payload for these events will include the corresponding `batchId` and the `metadata` you provided for each input.
- **Fetching Results:** You can also use the [List Workflow Runs](https://docs.extend.ai/2025-04-21/developers/api-reference/workflow-endpoints/list-workflow-runs) endpoint and filter using the `batchId` query param.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.batchWorkflowRun().create(
    BatchWorkflowRunCreateRequest
        .builder()
        .workflowId("workflow_id_here")
        .inputs(
            new ArrayList<BatchWorkflowRunCreateRequestInputsItem>(
                Arrays.asList(
                    BatchWorkflowRunCreateRequestInputsItem
                        .builder()
                        .build()
                )
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**workflowId:** `String` 

The ID of the workflow to run. This ID will start with "workflow_". This ID can be found viewing the workflow on the Extend platform.

Example: `"workflow_BMdfq_yWM3sT-ZzvCnA3f"`
    
</dd>
</dl>

<dl>
<dd>

**version:** `Optional<String>` — An optional version of the workflow to use. This can be a specific version number (e.g., `"1"`, `"2"`) found on the Extend platform, or `"draft"` to use the current unpublished draft version. When a version is not supplied, the latest deployed version of the workflow will be used. If no deployed version exists, the draft version will be used.
    
</dd>
</dl>

<dl>
<dd>

**inputs:** `List<BatchWorkflowRunCreateRequestInputsItem>` — An array of input objects to be processed by the workflow. Each object represents a single workflow run to be created. The array must contain at least 1 input and at most 1000 inputs.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## BatchProcessorRun
<details><summary><code>client.batchProcessorRun.get(id) -> BatchProcessorRunGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve details about a batch processor run, including evaluation runs
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.batchProcessorRun().get("batch_processor_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The unique identifier of the batch processor run to retrieve. The ID will always start with "bpr_".

Example: `"bpr_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## EvaluationSet
<details><summary><code>client.evaluationSet.list() -> EvaluationSetListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

List evaluation sets in your account. You can use the `processorId` parameter to filter evaluation sets by processor. 

This endpoint returns a paginated response. You can use the `nextPageToken` to fetch subsequent results.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.evaluationSet().list(
    EvaluationSetListRequest
        .builder()
        .processorId("processor_id_here")
        .sortBy(SortByEnum.UPDATED_AT)
        .sortDir(SortDirEnum.ASC)
        .nextPageToken("xK9mLPqRtN3vS8wF5hB2cQ==:zWvUxYjM4nKpL7aDgE9HbTcR2mAyX3/Q+CNkfBSw1dZ=")
        .maxPageSize(1)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**processorId:** `Optional<String>` 

The ID of the processor to filter evaluation sets by.

Example: `"dp_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>

<dl>
<dd>

**sortBy:** `Optional<SortByEnum>` — Sorts the evaluation sets by the given field.
    
</dd>
</dl>

<dl>
<dd>

**sortDir:** `Optional<SortDirEnum>` — Sorts the evaluation sets in ascending or descending order. Ascending order means the earliest evaluation set is returned first.
    
</dd>
</dl>

<dl>
<dd>

**nextPageToken:** `Optional<String>` 
    
</dd>
</dl>

<dl>
<dd>

**maxPageSize:** `Optional<Integer>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.evaluationSet.create(request) -> EvaluationSetCreateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Evaluation sets are collections of files and expected outputs that are used to evaluate the performance of a given processor in Extend. This endpoint will create a new evaluation set in Extend, which items can be added to using the [Create Evaluation Set Item](https://docs.extend.ai/2025-04-21/developers/api-reference/evaluation-set-endpoints/create-evaluation-set-item) endpoint.

Note: it is not necessary to create an evaluation set via API. You can also create an evaluation set via the Extend dashboard and take the ID from there.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.evaluationSet().create(
    EvaluationSetCreateRequest
        .builder()
        .name("My Evaluation Set")
        .description("My Evaluation Set Description")
        .processorId("processor_id_here")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**name:** `String` 

The name of the evaluation set.

Example: `"Invoice Processing Test Set"`
    
</dd>
</dl>

<dl>
<dd>

**description:** `String` 

A description of what this evaluation set is used for.

Example: `"Q4 2023 vendor invoices"`
    
</dd>
</dl>

<dl>
<dd>

**processorId:** `String` 

The ID of the processor to create an evaluation set for. Evaluation sets can in theory be run against any processor, but it is required to associate the evaluation set with a primary processor.

Example: `"dp_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.evaluationSet.get(id) -> EvaluationSetGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve a specific evaluation set by ID. This returns an evaluation set object, but does not include the items in the evaluation set. You can use the [List Evaluation Set Items](https://docs.extend.ai/2025-04-21/developers/api-reference/evaluation-set-endpoints/list-evaluation-set-items) endpoint to get the items in an evaluation set.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.evaluationSet().get("evaluation_set_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the evaluation set to retrieve.

Example: `"ev_2LcgeY_mp2T5yPaEuq5Lw"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## EvaluationSetItem
<details><summary><code>client.evaluationSetItem.list(id) -> EvaluationSetItemListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

List all items in a specific evaluation set. Evaluation set items are the individual files and expected outputs that are used to evaluate the performance of a given processor in Extend. 

This endpoint returns a paginated response. You can use the `nextPageToken` to fetch subsequent results.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.evaluationSetItem().list(
    "evaluation_set_id_here",
    EvaluationSetItemListRequest
        .builder()
        .sortBy(SortByEnum.UPDATED_AT)
        .sortDir(SortDirEnum.ASC)
        .nextPageToken("xK9mLPqRtN3vS8wF5hB2cQ==:zWvUxYjM4nKpL7aDgE9HbTcR2mAyX3/Q+CNkfBSw1dZ=")
        .maxPageSize(1)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the evaluation set to retrieve items for.

Example: `"ev_2LcgeY_mp2T5yPaEuq5Lw"`
    
</dd>
</dl>

<dl>
<dd>

**sortBy:** `Optional<SortByEnum>` — Sorts the evaluation set items by the given field.
    
</dd>
</dl>

<dl>
<dd>

**sortDir:** `Optional<SortDirEnum>` — Sorts the evaluation set items in ascending or descending order. Ascending order means the earliest evaluation set is returned first.
    
</dd>
</dl>

<dl>
<dd>

**nextPageToken:** `Optional<String>` 
    
</dd>
</dl>

<dl>
<dd>

**maxPageSize:** `Optional<Integer>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.evaluationSetItem.create(request) -> EvaluationSetItemCreateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Evaluation set items are the individual files and expected outputs that are used to evaluate the performance of a given processor in Extend. This endpoint will create a new evaluation set item in Extend, which will be used during an evaluation run.

Best Practices for Outputs in Evaluation Sets:
- **Configure First, Output Later**
  - Always create and finalize your processor configuration before creating evaluation sets
  - Field IDs in outputs must match those defined in your processor configuration
- **Type Consistency**
  - Ensure output types exactly match your processor configuration
  - For example, if a field is configured as "currency", don't submit a simple number value
- **Field IDs**
  - Use the exact field IDs from your processor configuration
  - Create your own semantic IDs instead in the configs for each field/type instead of using the generated ones
- **Value**
  - Remember that all results are inside the value key of a result object, except the values within nested structures.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.evaluationSetItem().create(
    EvaluationSetItemCreateRequest
        .builder()
        .evaluationSetId("evaluation_set_id_here")
        .fileId("file_id_here")
        .expectedOutput(
            ProvidedProcessorOutput.ofProvidedExtractionOutput(
                ProvidedExtractionOutput.ofProvidedJsonOutput(
                    ProvidedJsonOutput
                        .builder()
                        .value(
                            new HashMap<String, Object>() {{
                                put("key", "value");
                            }}
                        )
                        .build()
                )
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**evaluationSetId:** `String` 

The ID of the evaluation set to add the item to.

Example: `"ev_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>

<dl>
<dd>

**fileId:** `String` 

Extend's internal ID for the file. It will always start with "file_".

Example: `"file_xK9mLPqRtN3vS8wF5hB2cQ"`
    
</dd>
</dl>

<dl>
<dd>

**expectedOutput:** `ProvidedProcessorOutput` — The expected output that will be used to evaluate the processor's performance.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.evaluationSetItem.update(id, request) -> EvaluationSetItemUpdateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

If you need to change the expected output for a given evaluation set item, you can use this endpoint to update the item. This can be useful if you need to correct an error in the expected output or if the output of the processor has changed.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.evaluationSetItem().update(
    "evaluation_set_item_id_here",
    EvaluationSetItemUpdateRequest
        .builder()
        .expectedOutput(
            ProvidedProcessorOutput.ofProvidedExtractionOutput(
                ProvidedExtractionOutput.ofProvidedJsonOutput(
                    ProvidedJsonOutput
                        .builder()
                        .value(
                            new HashMap<String, Object>() {{
                                put("key", "value");
                            }}
                        )
                        .build()
                )
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the evaluation set item to update.

Example: `"evi_kR9mNP12Qw4yTv8BdR3H"`
    
</dd>
</dl>

<dl>
<dd>

**expectedOutput:** `ProvidedProcessorOutput` — The expected output of the processor when run against the file
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.evaluationSetItem.delete(id) -> EvaluationSetItemDeleteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Delete an evaluation set item from an evaluation set. This operation is permanent and cannot be undone.

This endpoint can be used to remove individual items from an evaluation set when they are no longer needed or if they were added in error.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.evaluationSetItem().delete("evaluation_set_item_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the evaluation set item to delete.

Example: `"evi_kR9mNP12Qw4yTv8BdR3H"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.evaluationSetItem.createBatch(request) -> EvaluationSetItemCreateBatchResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

If you have a large number of files that you need to add to an evaluation set, you can use this endpoint to create multiple evaluation set items at once. This can be useful if you have a large dataset that you need to evaluate the performance of a processor against.

Note: you still need to create each File first using the file API.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.evaluationSetItem().createBatch(
    EvaluationSetItemCreateBatchRequest
        .builder()
        .evaluationSetId("evaluation_set_id_here")
        .items(
            new ArrayList<EvaluationSetItemCreateBatchRequestItemsItem>(
                Arrays.asList(
                    EvaluationSetItemCreateBatchRequestItemsItem
                        .builder()
                        .fileId("file_id_here")
                        .expectedOutput(
                            ProvidedProcessorOutput.ofProvidedExtractionOutput(
                                ProvidedExtractionOutput.ofProvidedJsonOutput(
                                    ProvidedJsonOutput
                                        .builder()
                                        .value(
                                            new HashMap<String, Object>() {{
                                                put("key", "value");
                                            }}
                                        )
                                        .build()
                                )
                            )
                        )
                        .build()
                )
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**evaluationSetId:** `String` 

The ID of the evaluation set to add the items to.

Example: `"ev_2LcgeY_mp2T5yPaEuq5Lw"`
    
</dd>
</dl>

<dl>
<dd>

**items:** `List<EvaluationSetItemCreateBatchRequestItemsItem>` — An array of objects representing the evaluation set items to create
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## ProcessorRun
<details><summary><code>client.processorRun.list() -> ProcessorRunListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

List runs of a Processor. A ProcessorRun represents a single execution of a processor against a file.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processorRun().list(
    ProcessorRunListRequest
        .builder()
        .status(ProcessorStatus.PENDING)
        .processorId("processorId")
        .processorType(ProcessorType.EXTRACT)
        .sourceId("sourceId")
        .source(ProcessorRunListRequestSource.ADMIN)
        .fileNameContains("fileNameContains")
        .sortBy(SortByEnum.UPDATED_AT)
        .sortDir(SortDirEnum.ASC)
        .nextPageToken("xK9mLPqRtN3vS8wF5hB2cQ==:zWvUxYjM4nKpL7aDgE9HbTcR2mAyX3/Q+CNkfBSw1dZ=")
        .maxPageSize(1)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**status:** `Optional<ProcessorStatus>` 

Filters processor runs by their status. If not provided, no filter is applied.

 The status of a processor run:
 * `"PENDING"` - The processor run has not started yet
 * `"PROCESSING"` - The processor run is in progress
 * `"PROCESSED"` - The processor run completed successfully
 * `"FAILED"` - The processor run encountered an error
 * `"CANCELLED"` - The processor run was cancelled
    
</dd>
</dl>

<dl>
<dd>

**processorId:** `Optional<String>` 

Filters processor runs by the processor ID. If not provided, runs for all processors are returned.

Example: `"dp_BMdfq_yWM3sT-ZzvCnA3f"`
    
</dd>
</dl>

<dl>
<dd>

**processorType:** `Optional<ProcessorType>` 

Filters processor runs by the processor type. If not provided, runs for all processor types are returned.

Example: `"EXTRACT"`
    
</dd>
</dl>

<dl>
<dd>

**sourceId:** `Optional<String>` 

Filters processor runs by the source ID. The source ID corresponds to the entity that created the processor run.

Example: `"workflow_run_123"`
    
</dd>
</dl>

<dl>
<dd>

**source:** `Optional<ProcessorRunListRequestSource>` 

Filters processor runs by the source that created them. If not provided, runs from all sources are returned.

The source of the processor run:
* `"ADMIN"` - Created by admin
* `"BATCH_PROCESSOR_RUN"` - Created from a batch processor run
* `"PLAYGROUND"` - Created from playground
* `"WORKFLOW_CONFIGURATION"` - Created from workflow configuration
* `"WORKFLOW_RUN"` - Created from a workflow run
* `"STUDIO"` - Created from Studio
* `"API"` - Created via API
    
</dd>
</dl>

<dl>
<dd>

**fileNameContains:** `Optional<String>` 

Filters processor runs by the name of the file. Only returns processor runs where the file name contains this string.

Example: `"invoice"`
    
</dd>
</dl>

<dl>
<dd>

**sortBy:** `Optional<SortByEnum>` — Sorts the processor runs by the given field.
    
</dd>
</dl>

<dl>
<dd>

**sortDir:** `Optional<SortDirEnum>` — Sorts the processor runs in ascending or descending order. Ascending order means the earliest processor run is returned first.
    
</dd>
</dl>

<dl>
<dd>

**nextPageToken:** `Optional<String>` 
    
</dd>
</dl>

<dl>
<dd>

**maxPageSize:** `Optional<Integer>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.processorRun.create(request) -> ProcessorRunCreateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Run processors (extraction, classification, splitting, etc.) on a given document.

**Synchronous vs Asynchronous Processing:**
- **Asynchronous (default)**: Returns immediately with `PROCESSING` status. Use webhooks or polling to get results.
- **Synchronous**: Set `sync: true` to wait for completion and get final results in the response (5-minute timeout).

**For asynchronous processing:**
- You can [configure webhooks](https://docs.extend.ai/product/webhooks/configuration) to receive notifications when a processor run is complete or failed.
- Or you can [poll the get endpoint](https://docs.extend.ai/2025-04-21/developers/api-reference/processor-endpoints/get-processor-run) for updates on the status of the processor run.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processorRun().create(
    ProcessorRunCreateRequest
        .builder()
        .processorId("processor_id_here")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**processorId:** `String` 
    
</dd>
</dl>

<dl>
<dd>

**version:** `Optional<String>` 

An optional version of the processor to use. When not supplied, the most recent published version of the processor will be used. Special values include:
- `"latest"` for the most recent published version. If there are no published versions, the draft version will be used.
- `"draft"` for the draft version.
- Specific version numbers corresponding to versions your team has published, e.g. `"1.0"`, `"2.2"`, etc.
    
</dd>
</dl>

<dl>
<dd>

**file:** `Optional<ProcessorRunFileInput>` — The file to be processed. One of `file` or `rawText` must be provided. Supported file types can be found [here](/product/general/supported-file-types).
    
</dd>
</dl>

<dl>
<dd>

**rawText:** `Optional<String>` — A raw string to be processed. Can be used in place of file when passing raw text data streams. One of `file` or `rawText` must be provided.
    
</dd>
</dl>

<dl>
<dd>

**sync:** `Optional<Boolean>` 

Whether to run the processor synchronously. When `true`, the request will wait for the processor run to complete and return the final results. When `false` (default), the request returns immediately with a `PROCESSING` status, and you can poll for completion or use webhooks. For production use cases, we recommending leaving sync off and building around an async integration for more resiliency, unless your use case is predictably fast (e.g. sub < 30 seconds) run time or otherwise have integration constraints that require a synchronous API.

**Timeout**: Synchronous requests have a 5-minute timeout. If the processor run takes longer, it will continue processing asynchronously and you can retrieve the results via the GET endpoint.
    
</dd>
</dl>

<dl>
<dd>

**priority:** `Optional<Integer>` — An optional value used to determine the relative order of ProcessorRuns when rate limiting is in effect. Lower values will be prioritized before higher values.
    
</dd>
</dl>

<dl>
<dd>

**metadata:** `Optional<Map<String, Object>>` 

An optional object that can be passed in to identify the run of the document processor. It will be returned back to you in the response and webhooks.

To categorize processor runs for billing and usage tracking, include `extend:usage_tags` with an array of string values (e.g., `{"extend:usage_tags": ["production", "team-eng", "customer-123"]}`). Tags must contain only alphanumeric characters, hyphens, and underscores; any special characters will be automatically removed.
    
</dd>
</dl>

<dl>
<dd>

**config:** `Optional<ProcessorRunCreateRequestConfig>` — The configuration for the processor run. If this is provided, this config will be used. If not provided, the config for the specific version you provide will be used. The type of configuration must match the processor type.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.processorRun.get(id) -> ProcessorRunGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve details about a specific processor run, including its status, outputs, and any edits made during review.

A common use case for this endpoint is to poll for the status and final output of an async processor run when using the [Run Processor](https://docs.extend.ai/2025-04-21/developers/api-reference/processor-endpoints/run-processor) endpoint. For instance, if you do not want to not configure webhooks to receive the output via completion/failure events.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processorRun().get("processor_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The unique identifier for this processor run.

Example: `"dpr_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.processorRun.delete(id) -> ProcessorRunDeleteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Delete a processor run and all associated data from Extend. This operation is permanent and cannot be undone.

This endpoint can be used if you'd like to manage data retention on your own rather than automated data retention policies. Or make one-off deletions for your downstream customers.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processorRun().delete("processor_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the processor run to delete.

Example: `"dpr_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.processorRun.cancel(id) -> ProcessorRunCancelResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Cancel a running processor run by its ID. This endpoint allows you to stop a processor run that is currently in progress.

Note: Only processor runs with a status of `"PROCESSING"` can be cancelled. Processor runs that have already completed, failed, or been cancelled cannot be cancelled again.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processorRun().cancel("processor_run_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The unique identifier for the processor run to cancel.

Example: `"dpr_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Processor
<details><summary><code>client.processor.list() -> ListProcessorsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

List all processors in your organization
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processor().list(
    ProcessorListRequest
        .builder()
        .type(ProcessorType.EXTRACT)
        .nextPageToken("nextPageToken")
        .maxPageSize(1)
        .sortBy(ProcessorListRequestSortBy.CREATED_AT)
        .sortDir(ProcessorListRequestSortDir.ASC)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**type:** `Optional<ProcessorType>` — Filter processors by type
    
</dd>
</dl>

<dl>
<dd>

**nextPageToken:** `Optional<String>` — Token for retrieving the next page of results
    
</dd>
</dl>

<dl>
<dd>

**maxPageSize:** `Optional<Integer>` — Maximum number of processors to return per page
    
</dd>
</dl>

<dl>
<dd>

**sortBy:** `Optional<ProcessorListRequestSortBy>` — Field to sort by
    
</dd>
</dl>

<dl>
<dd>

**sortDir:** `Optional<ProcessorListRequestSortDir>` — Sort direction
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.processor.create(request) -> ProcessorCreateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Create a new processor in Extend, optionally cloning from an existing processor
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processor().create(
    ProcessorCreateRequest
        .builder()
        .name("My Processor Name")
        .type(ProcessorType.EXTRACT)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**name:** `String` — The name of the new processor
    
</dd>
</dl>

<dl>
<dd>

**type:** `ProcessorType` 
    
</dd>
</dl>

<dl>
<dd>

**cloneProcessorId:** `Optional<String>` 

The ID of an existing processor to clone. One of `cloneProcessorId` or `config` must be provided.

Example: `"dp_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>

<dl>
<dd>

**config:** `Optional<ProcessorCreateRequestConfig>` — The configuration for the processor. The type of configuration must match the processor type. One of `cloneProcessorId` or `config` must be provided.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.processor.update(id, request) -> ProcessorUpdateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Update an existing processor in Extend
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processor().update(
    "processor_id_here",
    ProcessorUpdateRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the processor to update.

Example: `"dp_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>

<dl>
<dd>

**name:** `Optional<String>` — The new name for the processor
    
</dd>
</dl>

<dl>
<dd>

**config:** `Optional<ProcessorUpdateRequestConfig>` 

The new configuration for the processor. The type of configuration must match the processor type:
* For classification processors, use `ClassificationConfig`
* For extraction processors, use `ExtractionConfig`
* For splitter processors, use `SplitterConfig`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## ProcessorVersion
<details><summary><code>client.processorVersion.list(id) -> ProcessorVersionListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

This endpoint allows you to fetch all versions of a given processor, including the current `draft` version.

Versions are typically returned in descending order of creation (newest first), but this should be confirmed in the actual implementation.
The `draft` version is the latest unpublished version of the processor, which can be published to create a new version. It might not have any changes from the last published version.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processorVersion().list("processor_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the processor to retrieve versions for.

Example: `"dp_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.processorVersion.create(id, request) -> ProcessorVersionCreateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

This endpoint allows you to publish a new version of an existing processor. Publishing a new version creates a snapshot of the processor's current configuration and makes it available for use in workflows.

Publishing a new version does not automatically update existing workflows using this processor. You may need to manually update workflows to use the new version if desired.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processorVersion().create(
    "processor_id_here",
    ProcessorVersionCreateRequest
        .builder()
        .releaseType(ProcessorVersionCreateRequestReleaseType.MAJOR)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**id:** `String` 

The ID of the processor to publish a new version for.

Example: `"dp_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>

<dl>
<dd>

**releaseType:** `ProcessorVersionCreateRequestReleaseType` — The type of release for this version. The two options are "major" and "minor", which will increment the version number accordingly.
    
</dd>
</dl>

<dl>
<dd>

**description:** `Optional<String>` — A description of the changes in this version. This helps track the evolution of the processor over time.
    
</dd>
</dl>

<dl>
<dd>

**config:** `Optional<ProcessorVersionCreateRequestConfig>` — The configuration for this version of the processor. The type of configuration must match the processor type.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.processorVersion.get(processorId, processorVersionId) -> ProcessorVersionGetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve a specific version of a processor in Extend
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.processorVersion().get("processor_id_here", "processor_version_id_here");
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**processorId:** `String` 

The ID of the processor.

Example: `"dp_Xj8mK2pL9nR4vT7qY5wZ"`
    
</dd>
</dl>

<dl>
<dd>

**processorVersionId:** `String` 

The ID of the specific processor version to retrieve.

Example: `"dpv_QYk6jgHA_8CsO8rVWhyNC"`
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>
