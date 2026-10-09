# log_box_dio_logger

LogBox extension that captures Dio HTTP requests, responses and errors as structured network log entries.

Part of the [LogBox](https://github.com/robzimpulse/log_box) logging framework. See the LogBox README for setup and the [example app](https://github.com/robzimpulse/log_box/tree/master/example) for a full integration.

## Installation

Not published to pub.dev; depend on it via a git tag together with the core package:

```yaml
dependencies:
  log_box:
    git:
      url: https://github.com/robzimpulse/log_box.git
      ref: v0.1.0
  log_box_dio_logger:
    git:
      url: https://github.com/robzimpulse/log_box_dio_logger.git
      ref: v0.0.1
```

## Development

Requires Flutter 3.32.8 (pinned in `.fvmrc`, use [FVM](https://fvm.app)) and `make`. Run `make` to list targets.

```bash
make get                 # fetch dependencies
make test                # unit tests
make coverage            # unit tests with coverage/lcov.info
make analyze             # static analysis
make format-check        # formatting check (make format to fix)
make generate            # build_runner code generation
```

Pass `FLUTTER="fvm flutter"` / `DART="fvm dart"` to use the FVM-pinned SDK.

CI (`.github/workflows/unit-test.yaml`) runs tests, coverage diff and lint on every PR and push to `master`.

## Releasing

1. In a PR, bump `version:` in `pubspec.yaml` and add a matching `## <version>` section to `CHANGELOG.md`.
2. Merge it. `.github/workflows/release.yaml` runs the release gate (analyze, format check, tests) and creates the `v<version>` tag and GitHub Release.
