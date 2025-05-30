<!--- Hugo front matter used to generate the website version of this page:
linkTitle: Spans
--->

# Semantic conventions for CICD spans

**Status**: [Development][DocumentStatus]

<!-- toc -->

- [CICD Spans](#cicd-spans)
  - [Pipeline run](#pipeline-run)
  - [Pipeline task run](#pipeline-task-run)

<!-- tocstop -->

## CICD Spans

The conventions described in this section are specific to Continuous Integration / Continuous Deployment (CICD) systems.

Any resources of the [CICD and VCS resource conventions][cicdres] that apply SHOULD be used.

[cicdres]: /docs/resource/cicd.md (CICD and VCS resource conventions)

### Pipeline run

<!-- semconv span.cicd.pipeline.run.server -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

**Status:** ![Development](https://img.shields.io/badge/-development-blue)

This span describes a CICD pipeline run.

For all pipeline runs, a span with kind `SERVER` SHOULD be created corresponding to the execution of the pipeline run.

**Span name** MUST follow the overall [guidelines for span names](https://github.com/open-telemetry/opentelemetry-specification/tree/v1.43.0/specification/trace/api.md#span).

The span name SHOULD be `{action} {pipeline}` if there is a (low-cardinality) pipeline name available.
If the pipeline name is not available or is likely to have high cardinality, then the span name SHOULD be `{action}`.

The `{action}` SHOULD be the [`cicd.pipeline.action.name`](/docs/registry/attributes/cicd.md#cicd-pipeline-action-name).

The `{pipeline}` SHOULD be the [`cicd.pipeline.name`](/docs/registry/attributes/cicd.md#cicd-pipeline-name).

**Span kind** SHOULD be `SERVER`.

**Span status** SHOULD follow the [Recording Errors](/docs/general/recording-errors.md) document.

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`cicd.pipeline.result`](/docs/registry/attributes/cicd.md) | string | The result of a pipeline run. | `success`; `failure`; `timeout`; `skipped` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`error.type`](/docs/registry/attributes/error.md) | string | Describes a class of error the operation ended with. [1] | `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500` | `Conditionally Required` if the pipeline result is `failure` or `error` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`cicd.pipeline.action.name`](/docs/registry/attributes/cicd.md) | string | The kind of action a pipeline run is performing. | `BUILD`; `RUN`; `SYNC` | `Opt-In` | ![Development](https://img.shields.io/badge/-development-blue) |

**[1] `error.type`:** The `error.type` SHOULD be predictable, and SHOULD have low cardinality.

When `error.type` is set to a type (e.g., an exception type), its
canonical class name identifying the type within the artifact SHOULD be used.

Instrumentations SHOULD document the list of errors they report.

The cardinality of `error.type` within one instrumentation library SHOULD be low.
Telemetry consumers that aggregate data from multiple instrumentation libraries and applications
should be prepared for `error.type` to have high cardinality at query time when no
additional filters are applied.

If the operation has completed successfully, instrumentations SHOULD NOT set `error.type`.

If a specific domain defines its own set of error identifiers (such as HTTP or gRPC status codes),
it's RECOMMENDED to:

- Use a domain-specific attribute
- Set `error.type` to capture all errors, regardless of whether they are defined within the domain-specific set or not.

---

`cicd.pipeline.action.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `BUILD` | The pipeline run is executing a build. | ![Development](https://img.shields.io/badge/-development-blue) |
| `RUN` | The pipeline run is executing. | ![Development](https://img.shields.io/badge/-development-blue) |
| `SYNC` | The pipeline run is executing a sync. | ![Development](https://img.shields.io/badge/-development-blue) |

---

`cicd.pipeline.result` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `cancellation` | The pipeline run was cancelled, eg. by a user manually cancelling the pipeline run. | ![Development](https://img.shields.io/badge/-development-blue) |
| `error` | The pipeline run failed due to an error in the CICD system, eg. due to the worker being killed. | ![Development](https://img.shields.io/badge/-development-blue) |
| `failure` | The pipeline run did not finish successfully, eg. due to a compile error or a failing test. Such failures are usually detected by non-zero exit codes of the tools executed in the pipeline run. | ![Development](https://img.shields.io/badge/-development-blue) |
| `skip` | The pipeline run was skipped, eg. due to a precondition not being met. | ![Development](https://img.shields.io/badge/-development-blue) |
| `success` | The pipeline run finished successfully. | ![Development](https://img.shields.io/badge/-development-blue) |
| `timeout` | A timeout caused the pipeline run to be interrupted. | ![Development](https://img.shields.io/badge/-development-blue) |

---

`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `_OTHER` | A fallback error value to be used when the instrumentation doesn't define a custom value. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

### Pipeline task run

<!-- semconv span.cicd.pipeline.task.internal -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

**Status:** ![Development](https://img.shields.io/badge/-development-blue)

This span describes task execution in a pipeline run.

**Span kind** SHOULD be `INTERNAL`.

**Span status** SHOULD follow the [Recording Errors](/docs/general/recording-errors.md) document.

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`cicd.pipeline.task.name`](/docs/registry/attributes/cicd.md) | string | The human readable name of a task within a pipeline. Task here most closely aligns with a [computing process](https://wikipedia.org/wiki/Pipeline_(computing)) in a pipeline. Other terms for tasks include commands, steps, and procedures. | `Run GoLang Linter`; `Go Build`; `go-test`; `deploy_binary` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`cicd.pipeline.task.run.id`](/docs/registry/attributes/cicd.md) | string | The unique identifier of a task run within a pipeline. | `12097` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`cicd.pipeline.task.run.result`](/docs/registry/attributes/cicd.md) | string | The result of a task run. | `success`; `failure`; `timeout`; `skipped` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`cicd.pipeline.task.run.url.full`](/docs/registry/attributes/cicd.md) | string | The [URL](https://wikipedia.org/wiki/URL) of the pipeline task run, providing the complete address in order to locate and identify the pipeline task run. | `https://github.com/open-telemetry/semantic-conventions/actions/runs/9753949763/job/26920038674?pr=1075` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`error.type`](/docs/registry/attributes/error.md) | string | Describes a class of error the operation ended with. [1] | `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500` | `Conditionally Required` if the task result is `failure` or `error` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

**[1] `error.type`:** The `error.type` SHOULD be predictable, and SHOULD have low cardinality.

When `error.type` is set to a type (e.g., an exception type), its
canonical class name identifying the type within the artifact SHOULD be used.

Instrumentations SHOULD document the list of errors they report.

The cardinality of `error.type` within one instrumentation library SHOULD be low.
Telemetry consumers that aggregate data from multiple instrumentation libraries and applications
should be prepared for `error.type` to have high cardinality at query time when no
additional filters are applied.

If the operation has completed successfully, instrumentations SHOULD NOT set `error.type`.

If a specific domain defines its own set of error identifiers (such as HTTP or gRPC status codes),
it's RECOMMENDED to:

- Use a domain-specific attribute
- Set `error.type` to capture all errors, regardless of whether they are defined within the domain-specific set or not.

The following attributes can be important for making sampling decisions
and SHOULD be provided **at span creation time** (if provided at all):

* [`cicd.pipeline.task.name`](/docs/registry/attributes/cicd.md)
* [`cicd.pipeline.task.run.id`](/docs/registry/attributes/cicd.md)
* [`cicd.pipeline.task.run.url.full`](/docs/registry/attributes/cicd.md)

---

`cicd.pipeline.task.run.result` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `cancellation` | The task run was cancelled, eg. by a user manually cancelling the task run. | ![Development](https://img.shields.io/badge/-development-blue) |
| `error` | The task run failed due to an error in the CICD system, eg. due to the worker being killed. | ![Development](https://img.shields.io/badge/-development-blue) |
| `failure` | The task run did not finish successfully, eg. due to a compile error or a failing test. Such failures are usually detected by non-zero exit codes of the tools executed in the task run. | ![Development](https://img.shields.io/badge/-development-blue) |
| `skip` | The task run was skipped, eg. due to a precondition not being met. | ![Development](https://img.shields.io/badge/-development-blue) |
| `success` | The task run finished successfully. | ![Development](https://img.shields.io/badge/-development-blue) |
| `timeout` | A timeout caused the task run to be interrupted. | ![Development](https://img.shields.io/badge/-development-blue) |

---

`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `_OTHER` | A fallback error value to be used when the instrumentation doesn't define a custom value. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

[DocumentStatus]: https://opentelemetry.io/docs/specs/otel/document-status
