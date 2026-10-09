# log_box_dio_logger Context

## Purpose:
This repository contains a specialized plugin for the LogBox ecosystem that provides automated network logging for the Dio HTTP client. It intercepts requests, responses, and errors, converting them into structured `NetworkEntryModel` data for display and storage.

## Key Components:
- **lib/src/interceptor/log_box_network_interceptor.dart**: The core logic of the package. It implements a Dio `Interceptor` to capture network traffic and send it to the LogBox storage.
- **lib/src/model/network_entry_model.dart**: Defines the top-level structure for a network log, linking requests, responses, and potential errors.
- **lib/src/model/http_request_model.dart & http_response_model.dart**: Specialized data models that capture headers, query parameters, body content, and metadata for HTTP transactions.
- **lib/src/model/form_data_field_model.dart & form_data_file_model.dart**: Handle the complexity of multi-part form data requests, ensuring files and fields are correctly represented in logs.
- **lib/src/extension/**: Contains helper methods for internal data manipulation, such as formatting Dio headers or processing stream data.

## Dependencies:
- **dio**: The primary external dependency this package extends.
- **log_box**: The core internal module used for its `Storage` and base log abstractions.
- **rxdart**: Used for handling stream-based response bodies (`ResponseBody`) within the interceptor.
- **json_annotation**: Used for generating serialization logic for the network models.
- **equatable**: Used to simplify equality checks in the data models.

## Local Conventions:
- **Interceptor-Centric Design**: The primary way to use this package is by adding `LogBoxNetworkInterceptor` to a Dio instance's interceptor list.
- **Reactive Stream Handling**: When dealing with `ResponseBody` streams in Dio, the interceptor uses `ReplaySubject` from RxDart to capture data without consuming the stream for the original caller.
- **Model Partitioning**: Detailed HTTP data is sharded into `HttpRequestModel`, `HttpResponseModel`, and `HttpErrorModel` to maintain clean separation of concerns within a `NetworkEntryModel`.
- **Automated Serialization**: All models in `lib/src/model/` must use `json_serializable` and have corresponding `.g.dart` files generated.

## Development:
- **Commands**: Use the `Makefile` (`make` lists targets). `make analyze` and `make format-check` must pass — CI enforces both (infos are fatal).
- **Code Generation**: Run `make generate` after modifying `@JsonSerializable` models; commit the `.g.dart` files.
- **Releasing**: Bump `version:` in `pubspec.yaml` and add a matching `## <version>` section to `CHANGELOG.md` in the same PR; merging creates tag `v<version>` via `release.yaml`.

## Known Pitfalls:
- **Cross-Repo Dependency Bumps**: `log_box` is pinned by git tag (`ref: v<version>`), so CI never sees an unreleased core change. For a shared-constraint bump (e.g. rxdart), land and release it in [log_box](https://github.com/robzimpulse/log_box) first, then update `ref:` and the constraint here in its own PR. See core's `AGENTS.md` → "Cross-Repo Dependency Bumps".
- **Pushing Workflow Changes**: Pushing files under `.github/workflows/` requires a GitHub token with the `workflow` scope. If a push is rejected with "refusing to allow an OAuth App to create or update workflow", run `gh auth refresh -h github.com -s workflow` and make git use that token (`gh auth setup-git`) — a stale macOS Keychain token will otherwise keep failing.
