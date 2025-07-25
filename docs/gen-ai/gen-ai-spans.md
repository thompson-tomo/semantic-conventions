<!--- Hugo front matter used to generate the website version of this page:
linkTitle: Spans
--->

# Semantic conventions for generative client AI spans

**Status**: [Development][DocumentStatus]

<!-- toc -->

- [Spans](#spans)
  - [Inference](#inference)
  - [Embeddings](#embeddings)
  - [Execute tool span](#execute-tool-span)
- [Capturing inputs and outputs](#capturing-inputs-and-outputs)

<!-- tocstop -->

> [!Warning]
>
> Existing GenAI instrumentations that are using
> [v1.36.0 of this document](https://github.com/open-telemetry/semantic-conventions/blob/v1.36.0/docs/gen-ai/README.md)
> (or prior):
>
> * SHOULD NOT change the version of the GenAI conventions that they emit by default.
>   Conventions include, but are not limited to, attributes, metric, span and event names,
>   span kind and unit of measure.
> * SHOULD introduce an environment variable `OTEL_SEMCONV_STABILITY_OPT_IN`
>   as a comma-separated list of category-specific values. The list of values
>   includes:
>   * `gen_ai_latest_experimental` - emit the latest experimental version of
>     GenAI conventions (supported by the instrumentation) and do not emit the
>     old one (v1.36.0 or prior).
>   * The default behavior is to continue emitting whatever version of the GenAI
>     conventions the instrumentation was emitting (1.36.0 or prior).
>
> This transition plan will be updated to include stable version before the
> GenAI conventions are marked as stable.

## Spans

### Inference

<!-- semconv span.gen_ai.inference.client -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

**Status:** ![Development](https://img.shields.io/badge/-development-blue)

This span represents a client call to Generative AI model or service that generates a response or requests a tool call based on the input prompt.

**Span name** SHOULD be `{gen_ai.operation.name} {gen_ai.request.model}`.
Semantic conventions for individual GenAI systems and frameworks MAY specify different span name format
and MUST follow the overall [guidelines for span names](https://github.com/open-telemetry/opentelemetry-specification/tree/v1.37.0/specification/trace/api.md#span).

**Span kind** SHOULD be `CLIENT`and MAY be set to `INTERNAL` on spans representing
call to models running in the same process. It's RECOMMENDED to use `CLIENT` kind
when the GenAI system being instrumented usually runs in a different process than its
client or when the GenAI call happens over instrumented protocol such as HTTP.

**Span status** SHOULD follow the [Recording Errors](/docs/general/recording-errors.md) document.

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`gen_ai.operation.name`](/docs/registry/attributes/gen-ai.md) | string | The name of the operation being performed. [1] | `chat`; `generate_content`; `text_completion` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.provider.name`](/docs/registry/attributes/gen-ai.md) | string | The Generative AI provider as identified by the client or server instrumentation. [2] | `openai`; `gcp.gen_ai`; `gcp.vertex_ai` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`error.type`](/docs/registry/attributes/error.md) | string | Describes a class of error the operation ended with. [3] | `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500` | `Conditionally Required` if the operation ended in an error | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`gen_ai.conversation.id`](/docs/registry/attributes/gen-ai.md) | string | The unique identifier for a conversation (session, thread), used to store and correlate messages within this conversation. [4] | `conv_5j66UpCpwteGg4YSxUnt7lPY` | `Conditionally Required` when available | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.output.type`](/docs/registry/attributes/gen-ai.md) | string | Represents the content type requested by the client. [5] | `text`; `json`; `image` | `Conditionally Required` [6] | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.choice.count`](/docs/registry/attributes/gen-ai.md) | int | The target number of candidate completions to return. | `3` | `Conditionally Required` if available, in the request, and !=1 | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.model`](/docs/registry/attributes/gen-ai.md) | string | The name of the GenAI model a request is being made to. [7] | `gpt-4` | `Conditionally Required` If available. | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.seed`](/docs/registry/attributes/gen-ai.md) | int | Requests with same seed value more likely to return same result. | `100` | `Conditionally Required` if applicable and if the request includes a seed | ![Development](https://img.shields.io/badge/-development-blue) |
| [`server.port`](/docs/registry/attributes/server.md) | int | GenAI server port. [8] | `80`; `8080`; `443` | `Conditionally Required` If `server.address` is set. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`gen_ai.request.frequency_penalty`](/docs/registry/attributes/gen-ai.md) | double | The frequency penalty setting for the GenAI request. | `0.1` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.max_tokens`](/docs/registry/attributes/gen-ai.md) | int | The maximum number of tokens the model generates for a request. | `100` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.presence_penalty`](/docs/registry/attributes/gen-ai.md) | double | The presence penalty setting for the GenAI request. | `0.1` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.stop_sequences`](/docs/registry/attributes/gen-ai.md) | string[] | List of sequences that the model will use to stop generating further tokens. | `["forest", "lived"]` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.temperature`](/docs/registry/attributes/gen-ai.md) | double | The temperature setting for the GenAI request. | `0.0` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.top_k`](/docs/registry/attributes/gen-ai.md) | double | The top_k sampling setting for the GenAI request. | `1.0` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.request.top_p`](/docs/registry/attributes/gen-ai.md) | double | The top_p sampling setting for the GenAI request. | `1.0` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.response.finish_reasons`](/docs/registry/attributes/gen-ai.md) | string[] | Array of reasons the model stopped generating tokens, corresponding to each generation received. | `["stop"]`; `["stop", "length"]` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.response.id`](/docs/registry/attributes/gen-ai.md) | string | The unique identifier for the completion. | `chatcmpl-123` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.response.model`](/docs/registry/attributes/gen-ai.md) | string | The name of the model that generated the response. [9] | `gpt-4-0613` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.usage.input_tokens`](/docs/registry/attributes/gen-ai.md) | int | The number of tokens used in the GenAI input (prompt). | `100` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.usage.output_tokens`](/docs/registry/attributes/gen-ai.md) | int | The number of tokens used in the GenAI response (completion). | `180` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`server.address`](/docs/registry/attributes/server.md) | string | GenAI server address. [10] | `example.com`; `10.1.2.80`; `/tmp/my.sock` | `Recommended` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

**[1] `gen_ai.operation.name`:** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.

**[2] `gen_ai.provider.name`:** The attribute SHOULD be set based on the instrumentation's best
knowledge and may differ from the actual model provider.

Multiple providers, including Azure OpenAI, Gemini, and AI hosting platforms
are accessible using the OpenAI REST API and corresponding client libraries,
but may proxy or host models from different providers.

The `gen_ai.request.model`, `gen_ai.response.model`, and `server.address`
attributes may help identify the actual system in use.

The `gen_ai.provider.name` attribute acts as a discriminator that
identifies the GenAI telemetry format flavor specific to that provider
within GenAI semantic conventions.
It SHOULD be set consistently with provider-specific attributes and signals.
For example, GenAI spans, metrics, and events related to AWS Bedrock
should have the `gen_ai.provider.name` set to `aws.bedrock` and include
applicable `aws.bedrock.*` attributes and are not expected to include
`openai.*` attributes.

**[3] `error.type`:** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library,
the canonical name of exception that occurred, or another low-cardinality error identifier.
Instrumentations SHOULD document the list of errors they report.

**[4] `gen_ai.conversation.id`:** Instrumentations SHOULD populate conversation id when they have it readily available
for a given operation, for example:

-  when client framework being instrumented manages conversation history
(see [LlamaIndex chat store](https://docs.llamaindex.ai/en/stable/module_guides/storing/chat_stores/))

- when instrumenting GenAI client libraries that maintain conversation on the backend side
(see [AWS Bedrock agent sessions](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-session-state.html),
[OpenAI Assistant threads](https://platform.openai.com/docs/api-reference/threads))

Application developers that manage conversation history MAY add conversation id to GenAI and other
spans or logs using custom span or log record processors or hooks provided by instrumentation
libraries.

**[5] `gen_ai.output.type`:** This attribute SHOULD be used when the client requests output of a specific type. The model may return zero or more outputs of this type.
This attribute specifies the output modality and not the actual output format. For example, if an image is requested, the actual output could be a URL pointing to an image file.
Additional output format details may be recorded in the future in the `gen_ai.output.{type}.*` attributes.

**[6] `gen_ai.output.type`:** when applicable and if the request includes an output format.

**[7] `gen_ai.request.model`:** The name of the GenAI model a request is being made to. If the model is supplied by a vendor, then the value must be the exact name of the model requested. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.

**[8] `server.port`:** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.

**[9] `gen_ai.response.model`:** If available. The name of the GenAI model that provided the response. If the model is supplied by a vendor, then the value must be the exact name of the model actually used. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.

**[10] `server.address`:** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.

---

`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `_OTHER` | A fallback error value to be used when the instrumentation doesn't define a custom value. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

---

`gen_ai.operation.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `chat` | Chat completion operation such as [OpenAI Chat API](https://platform.openai.com/docs/api-reference/chat) | ![Development](https://img.shields.io/badge/-development-blue) |
| `create_agent` | Create GenAI agent | ![Development](https://img.shields.io/badge/-development-blue) |
| `embeddings` | Embeddings operation such as [OpenAI Create embeddings API](https://platform.openai.com/docs/api-reference/embeddings/create) | ![Development](https://img.shields.io/badge/-development-blue) |
| `execute_tool` | Execute a tool | ![Development](https://img.shields.io/badge/-development-blue) |
| `generate_content` | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content) | ![Development](https://img.shields.io/badge/-development-blue) |
| `invoke_agent` | Invoke GenAI agent | ![Development](https://img.shields.io/badge/-development-blue) |
| `text_completion` | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions) | ![Development](https://img.shields.io/badge/-development-blue) |

---

`gen_ai.output.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `image` | Image | ![Development](https://img.shields.io/badge/-development-blue) |
| `json` | JSON object with known or unknown schema | ![Development](https://img.shields.io/badge/-development-blue) |
| `speech` | Speech | ![Development](https://img.shields.io/badge/-development-blue) |
| `text` | Plain text | ![Development](https://img.shields.io/badge/-development-blue) |

---

`gen_ai.provider.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `anthropic` | [Anthropic](https://www.anthropic.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `aws.bedrock` | [AWS Bedrock](https://aws.amazon.com/bedrock) | ![Development](https://img.shields.io/badge/-development-blue) |
| `azure.ai.inference` | Azure AI Inference | ![Development](https://img.shields.io/badge/-development-blue) |
| `azure.ai.openai` | [Azure OpenAI](https://azure.microsoft.com/products/ai-services/openai-service/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `cohere` | [Cohere](https://cohere.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `deepseek` | [DeepSeek](https://www.deepseek.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `gcp.gemini` | [Gemini](https://cloud.google.com/products/gemini) [11] | ![Development](https://img.shields.io/badge/-development-blue) |
| `gcp.gen_ai` | Any Google generative AI endpoint [12] | ![Development](https://img.shields.io/badge/-development-blue) |
| `gcp.vertex_ai` | [Vertex AI](https://cloud.google.com/vertex-ai) [13] | ![Development](https://img.shields.io/badge/-development-blue) |
| `groq` | [Groq](https://groq.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `ibm.watsonx.ai` | [IBM Watsonx AI](https://www.ibm.com/products/watsonx-ai) | ![Development](https://img.shields.io/badge/-development-blue) |
| `mistral_ai` | [Mistral AI](https://mistral.ai/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `openai` | [OpenAI](https://openai.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `perplexity` | [Perplexity](https://www.perplexity.ai/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `x_ai` | [xAI](https://x.ai/) | ![Development](https://img.shields.io/badge/-development-blue) |

**[11]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.

**[12]:** May be used when specific backend is unknown.

**[13]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

### Embeddings

<!-- semconv span.gen_ai.embeddings.client -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

**Status:** ![Development](https://img.shields.io/badge/-development-blue)

Describes GenAI embeddings span - a request to a Generative AI model or service that generates an embeddings based on the input.
The `gen_ai.operation.name` SHOULD be `embeddings`.
**Span name** SHOULD be `{gen_ai.operation.name} {gen_ai.request.model}`.

**Span kind** SHOULD be `CLIENT`.

**Span status** SHOULD follow the [Recording Errors](/docs/general/recording-errors.md) document.

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`gen_ai.operation.name`](/docs/registry/attributes/gen-ai.md) | string | The name of the operation being performed. [1] | `chat`; `generate_content`; `text_completion` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`error.type`](/docs/registry/attributes/error.md) | string | Describes a class of error the operation ended with. [2] | `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500` | `Conditionally Required` if the operation ended in an error | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`gen_ai.request.model`](/docs/registry/attributes/gen-ai.md) | string | The name of the GenAI model a request is being made to. [3] | `gpt-4` | `Conditionally Required` If available. | ![Development](https://img.shields.io/badge/-development-blue) |
| [`server.port`](/docs/registry/attributes/server.md) | int | GenAI server port. [4] | `80`; `8080`; `443` | `Conditionally Required` If `server.address` is set. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`gen_ai.request.encoding_formats`](/docs/registry/attributes/gen-ai.md) | string[] | The encoding formats requested in an embeddings operation, if specified. [5] | `["base64"]`; `["float", "binary"]` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.usage.input_tokens`](/docs/registry/attributes/gen-ai.md) | int | The number of tokens used in the GenAI input (prompt). | `100` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`server.address`](/docs/registry/attributes/server.md) | string | GenAI server address. [6] | `example.com`; `10.1.2.80`; `/tmp/my.sock` | `Recommended` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

**[1] `gen_ai.operation.name`:** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.

**[2] `error.type`:** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library,
the canonical name of exception that occurred, or another low-cardinality error identifier.
Instrumentations SHOULD document the list of errors they report.

**[3] `gen_ai.request.model`:** The name of the GenAI model a request is being made to. If the model is supplied by a vendor, then the value must be the exact name of the model requested. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.

**[4] `server.port`:** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.

**[5] `gen_ai.request.encoding_formats`:** In some GenAI systems the encoding formats are called embedding types. Also, some GenAI systems only accept a single format per request.

**[6] `server.address`:** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.

---

`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `_OTHER` | A fallback error value to be used when the instrumentation doesn't define a custom value. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

---

`gen_ai.operation.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `chat` | Chat completion operation such as [OpenAI Chat API](https://platform.openai.com/docs/api-reference/chat) | ![Development](https://img.shields.io/badge/-development-blue) |
| `create_agent` | Create GenAI agent | ![Development](https://img.shields.io/badge/-development-blue) |
| `embeddings` | Embeddings operation such as [OpenAI Create embeddings API](https://platform.openai.com/docs/api-reference/embeddings/create) | ![Development](https://img.shields.io/badge/-development-blue) |
| `execute_tool` | Execute a tool | ![Development](https://img.shields.io/badge/-development-blue) |
| `generate_content` | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content) | ![Development](https://img.shields.io/badge/-development-blue) |
| `invoke_agent` | Invoke GenAI agent | ![Development](https://img.shields.io/badge/-development-blue) |
| `text_completion` | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions) | ![Development](https://img.shields.io/badge/-development-blue) |

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

### Execute tool span

<!-- semconv span.gen_ai.execute_tool.internal -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

**Status:** ![Development](https://img.shields.io/badge/-development-blue)

Describes tool execution span.

`gen_ai.operation.name` SHOULD be `execute_tool`.

**Span name** SHOULD be `execute_tool {gen_ai.tool.name}`.

GenAI instrumentations that are able to instrument tool execution call SHOULD do so.
However, it's common for tools to be executed by the application code. It's recommended
for the application developers to follow this semantic convention for tools invoked
by the application code.

**Span kind** SHOULD be `INTERNAL`.

**Span status** SHOULD follow the [Recording Errors](/docs/general/recording-errors.md) document.

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`gen_ai.operation.name`](/docs/registry/attributes/gen-ai.md) | string | The name of the operation being performed. [1] | `chat`; `generate_content`; `text_completion` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`error.type`](/docs/registry/attributes/error.md) | string | Describes a class of error the operation ended with. [2] | `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500` | `Conditionally Required` if the operation ended in an error | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`gen_ai.tool.call.id`](/docs/registry/attributes/gen-ai.md) | string | The tool call identifier. | `call_mszuSIzqtI65i1wAUOE8w5H4` | `Recommended` if available | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.tool.description`](/docs/registry/attributes/gen-ai.md) | string | The tool description. | `Multiply two numbers` | `Recommended` if available | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.tool.name`](/docs/registry/attributes/gen-ai.md) | string | Name of the tool utilized by the agent. | `Flights` | `Recommended` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`gen_ai.tool.type`](/docs/registry/attributes/gen-ai.md) | string | Type of the tool utilized by the agent [3] | `function`; `extension`; `datastore` | `Recommended` if available | ![Development](https://img.shields.io/badge/-development-blue) |

**[1] `gen_ai.operation.name`:** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.

**[2] `error.type`:** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library,
the canonical name of exception that occurred, or another low-cardinality error identifier.
Instrumentations SHOULD document the list of errors they report.

**[3] `gen_ai.tool.type`:** Extension: A tool executed on the agent-side to directly call external APIs, bridging the gap between the agent and real-world systems.
  Agent-side operations involve actions that are performed by the agent on the server or within the agent's controlled environment.
Function: A tool executed on the client-side, where the agent generates parameters for a predefined function, and the client executes the logic.
  Client-side operations are actions taken on the user's end or within the client application.
Datastore: A tool used by the agent to access and query structured or unstructured external data for retrieval-augmented tasks or knowledge updates.

---

`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `_OTHER` | A fallback error value to be used when the instrumentation doesn't define a custom value. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

---

`gen_ai.operation.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `chat` | Chat completion operation such as [OpenAI Chat API](https://platform.openai.com/docs/api-reference/chat) | ![Development](https://img.shields.io/badge/-development-blue) |
| `create_agent` | Create GenAI agent | ![Development](https://img.shields.io/badge/-development-blue) |
| `embeddings` | Embeddings operation such as [OpenAI Create embeddings API](https://platform.openai.com/docs/api-reference/embeddings/create) | ![Development](https://img.shields.io/badge/-development-blue) |
| `execute_tool` | Execute a tool | ![Development](https://img.shields.io/badge/-development-blue) |
| `generate_content` | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content) | ![Development](https://img.shields.io/badge/-development-blue) |
| `invoke_agent` | Invoke GenAI agent | ![Development](https://img.shields.io/badge/-development-blue) |
| `text_completion` | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions) | ![Development](https://img.shields.io/badge/-development-blue) |

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

## Capturing inputs and outputs

User inputs and model responses may be recorded as events parented to GenAI operation span. See [Semantic Conventions for GenAI events](./gen-ai-events.md) for the details.

[DocumentStatus]: https://opentelemetry.io/docs/specs/otel/document-status
