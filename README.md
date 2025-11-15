# blockinsight7000-proto

Protocol buffer definitions, generated Go stubs, and OpenAPI descriptions for the Blockinsight 7000 explorer API.

## Overview

The repository defines the public contract for querying blockchain networks exposed by the ExplorerService.  
It provides both gRPC and REST/JSON semantics through [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway), and publishes a Swagger document for downstream SDK generation or documentation portals.

### Core RPCs

- `ListChains` – discover supported coins and their deployable networks.
- `Health` – readiness signal for a particular coin/network pair.
- `ListBlocks`, `GetBlock` – paginated listings and single block lookups (hash or height).
- `GetTransaction` – detailed on-chain transaction view.
- `GetAddressBalance`, `ListAddressTransactions` – balance reporting and activity feed for on-chain addresses.

## Repository Layout

| Path | Description |
| --- | --- |
| `proto/` | Source `.proto` files grouped under `blockinsight7000/v1`. |
| `pkg/` | Generated Go code (both protobuf messages and gRPC/gateway bindings). |
| `docs/` | Generated OpenAPI v2 (`blockinsight7000.swagger.json`). |
| `buf.gen.yaml` / `buf.work.yaml` | Buf configuration used for generation. |

## Requirements

- [Buf CLI](https://buf.build/docs/installation) – orchestrates protoc plugins via `buf generate`.
- Go 1.21+ – for compiling the generated stubs and grpc-gateway code.

## Generating Code

All generated artifacts are committed, but you can regenerate them after changing any `.proto` file:

```bash
buf generate
```

This runs the plugin pipeline defined in `buf.gen.yaml`, producing:

- Go protobuf types (`pkg/blockinsight7000/v1/*.pb.go`)
- gRPC service stubs (`pkg/blockinsight7000/v1/*_grpc.pb.go`)
- grpc-gateway reverse proxy (`pkg/blockinsight7000/v1/*.pb.gw.go`)
- OpenAPI specification (`docs/blockinsight7000.swagger.json`)

If you introduce new service versions or files, remember to update the Buf workspace config as needed.

## Development Tips

1. **Edit protos under `proto/blockinsight7000/v1/` only.** Generated files will be overwritten.
2. **Document HTTP bindings.** Each RPC includes `google.api.http` options to keep the REST surface in sync.
3. **Regenerate and lint before submitting changes.**
   - `buf lint` to catch style or compatibility issues.
   - `buf generate` to refresh Go and Swagger outputs.
4. **Keep swagger consumers in mind.** Adding or renaming fields requires updating clients that rely on the schema in `docs/blockinsight7000.swagger.json`.

## Consuming the API

- **Go:** import the `blockinsight7000/v1` package from this module to leverage the generated stubs directly.
- **Other languages:** use the Swagger file to generate REST clients or translate the `.proto` files with your language-specific tooling.

## License

Distributed under the MIT License unless otherwise noted in individual files.
