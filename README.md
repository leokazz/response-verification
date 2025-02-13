# Response Verification

## Overview

Response verification on the [Internet Computer](https://dfinity.org) is the process of verifying that a canister response from a replica has gone through consensus with other replicas hosting the same canister.

This package encapsulates the protocol for such verification. It is used by [the Service Worker](https://github.com/dfinity/ic/tree/master/typescript/service-worker) and [ICX Proxy](https://github.com/dfinity/ic/tree/master/rs/boundary_node/icx_proxy) and may be used by other implementations of the [HTTP Gateway Protocol](https://internetcomputer.org/docs/current/references/ic-interface-spec/#http-gateway) in the future.

## Projects

| Command       | Description             |
| ------------- | ----------------------- |
| `cargo build` | Build all Cargo crates  |
| `cargo test`  | Test all Cargo crates   |
| `cargo fmt`   | Format all Cargo crates |
| `pnpm build`  | Build all NPM packages  |
| `pnpm test`   | Test all NPM packages   |
| `pnpm format` | Format all NPM packages |

### HTTP Certification

- [Cargo crate](./packages/ic-http-certification/README.md)

| Command                                               | Description            |
| ----------------------------------------------------- | ---------------------- |
| `cargo build -p ic-http-certification`                | Build Cargo crate      |
| `cargo test -p ic-http-certification`                 | Test Cargo crate       |
| `cargo doc -p ic-http-certification --no-deps --open` | Build Cargo crate docs |

### Response Verification

- [Cargo crate](./packages/ic-response-verification/README.md)
- [NPM package](./packages/ic-response-verification-wasm/README.md)
- [e2e tests](./packages/ic-response-verification-tests/README.md)
- [NodeJS example](./examples/nodejs/README.md)
- [Rust example](./examples/rust/README.md)
- [Web example](./examples/web/README.md)
- [Service Worker example](./examples/service-worker/README.md)

| Command                                                                 | Description            |
| ----------------------------------------------------------------------- | ---------------------- |
| `cargo build -p ic-response-verification`                               | Build Cargo crate      |
| `cargo test -p ic-response-verification`                                | Test Cargo crate       |
| `wasm-pack test --node packages/ic-response-verification --features=js` | Test Cargo crate WASM  |
| `cargo doc -p ic-response-verification --no-deps --open`                | Build Cargo crate docs |
| `pnpm run -F @dfinity/response-verification build`                      | Build NPM package      |
| `pnpm run -F @dfinity/response-verification test`                       | Test NPM package       |
| `./scripts/e2e.sh`                                                      | Run e2e tests          |

### Certificate Verification

- [NPM package](./packages/certificate-verification-js/README.md)
- [Cargo crate](./packages/ic-certificate-verification/README.md)
- [Certified Counter example](./examples/certified-counter/README.md)

| Command                                               | Description       |
| ----------------------------------------------------- | ----------------- |
| `pnpm run -F @dfinity/certificate-verification build` | Build NPM package |
| `pnpm run -F @dfinity/certificate-verification test`  | Test NPM package  |

### Certification

- [Cargo crate](./packages/ic-certification/README.md)

| Command                                          | Description            |
| ------------------------------------------------ | ---------------------- |
| `cargo build -p ic-certification`                | Build Cargo crate      |
| `cargo test -p ic-certification`                 | Test Cargo crate       |
| `cargo doc -p ic-certification --no-deps --open` | Build Cargo crate docs |

### Certification Testing

- [NPM package](./packages/ic-certification-testing-wasm/README.md)
- [Cargo crate](./packages/ic-certification-testing/README.md)

| Command                                                  | Description            |
| -------------------------------------------------------- | ---------------------- |
| `cargo build -p ic-certification-testing`                | Build Cargo crate      |
| `cargo doc -p ic-certification-testing --no-deps --open` | Build Cargo crate docs |
| `pnpm run -F @dfinity/certification-testing build`       | Build NPM package      |

### Representation Independent Hash

- [Cargo crate](./packages/ic-representation-independent-hash/README.md)

| Command                                                            | Description            |
| ------------------------------------------------------------------ | ---------------------- |
| `cargo build -p ic-representation-independent-hash`                | Build Cargo crate      |
| `cargo test -p ic-representation-independent-hash`                 | Test Cargo crate       |
| `cargo doc -p ic-representation-independent-hash --no-deps --open` | Build Cargo crate docs |

### CBOR

- [Cargo crate](./packages/ic-cbor/README.md)

| Command                                 | Description            |
| --------------------------------------- | ---------------------- |
| `cargo build -p ic-cbor`                | Build Cargo crate      |
| `cargo test -p ic-cbor`                 | Test Cargo crate       |
| `cargo doc -p ic-cbor --no-deps --open` | Build Cargo crate docs |

## Contributing

Check out our [contribution guidelines](./.github/CONTRIBUTING.md).

### System Setup

- [Install pre-commit](https://pre-commit.com/#installation)
- [Install commitizen](https://commitizen-tools.github.io/commitizen/#installation)
- [Install Rust](https://www.rust-lang.org/learn/get-started)
- [Install wasm-pack](https://rustwasm.github.io/wasm-pack/installer)
- [Install NVM](https://github.com/nvm-sh/nvm)
- [Install DFX](https://internetcomputer.org/docs/current/developer-docs/setup/install)

Install the correct version of NodeJS:

```shell
nvm install
```

Activate the correct version of NodeJS:

```shell
nvm use
```

Install and activate the correct version of PNPM:

```shell
corepack enable
```

### Working on WASM crates

Until Cargo supports [per package targets](https://github.com/rust-lang/cargo/issues/9406), the WASM crates are excluded from the `default_members` array of the Cargo workspace.
Commands such as `cargo build` and `cargo check` will not include these crates, so they must be built separately with the corresponding `pnpm` command listed under [projects](#projects).

Since `rust-analyzer` will also apply the same target to all crates, these crates will show errors in the IDE. To workaround this, create a `.cargo/config.toml` file:

```toml
[build]
target = "wasm32-unknown-unknown"
```

While this file exists, some of the non-WASM crates will show errors instead. Delete the file to work on the non-WASM crates.

### Making a Commit

```shell
cz commit
```

See [Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) for more information on the commit message formats.

### Adding a new package

- Follow the [Package naming conventions](#package-naming-conventions) when naming the package.
- Add the package's package manager file to the `version_files` field in `.cz.yaml`.
  - `package.json` for NPM packages
  - Nothing for for Cargo crates, it is already covered by the root `Cargo.toml`
- Set the initial version of the package in its package manager file to match the current version in the `version` field in `.cz.yaml`
  - For `package.json`, set the version manually
  - For `Cargo.toml`, use `version.workspace = true`
- Add the package's package manager file(s) to the `add-paths` property in the `Create Pull Request` job of the `Create Release PR` workflow in `.github/workflows/create-release-pr.yml`
  - `package.json` for NPM packages
  - No files need to be added for Cargo crates
- If the package is a Rust crate:
  - Add the package to the `members` section in `Cargo.toml` and the `default-members` section
    - If the package must be compiled to WASM then do not add it to the `default-members` section
  - Add a `Release ic-<package-name> Cargo crate` job to the `Release` workflow in `.github/workflows/release.yml`
  - Add `target/package/ic-<package-name>-${{ github.ref_name }}.crate` to the `artifacts` property in the `Create Github release` job of the `Release` workflow in `.github/workflows/release.yml`
    - Make sure every entry except the last is comma delimited
  - If the crate has dependenencies in this repository, make sure it is published _after_ the dependencies
  - If the crate has a dependent in this repository, make sure it is published _before_ the dependents
- If the package is an NPM package:
  - Add the package to `pnpm-workspace.yaml`
  - Add a `Pack @dfinity/<package-name> NPM package` job to the `Release` workflow in `.github/workflows/release.yml`
  - Add a `Release @dfinity/<package-name> NPM package` job to the `Release` workflow in `.github/workflows/release.yml`
  - Add `dfinity-<package-name>-${{ github.ref_name }}.tgz` to the `artifacts` property in the `Create Github release` job of the `Create Release PR` workflow in `.github/workflows/create-release-pr.yml`
    - Make sure every entry except the last is comma delimited

### Package naming conventions

Cargo crates are named `ic-<package-name>`, likewise for the folder name.
If the Cargo crate will be compiled to WASM then the folder name is `ic-<package-name>-wasm`.

NPM packages are named `@dfinity/<package-name>`.
If the NPM package is a pure JS package then the folder name is `<package-name>-js`.
If the NPM package is built from a Rust crate then the folder name is `ic-<package-name>-wasm`.

### Referencing a Cargo crate

A Cargo crate can be referenced using a relative path in `Cargo.toml`:

```toml
[dependencies]
ic-response-verification-test-utils = { path = "../ic-response-verification-test-utils" }
```

If the _referencing_ Cargo crate is published to crates.io then the current version must be included and the _referenced_ crate must also be published:

```toml
[dependencies]
ic-response-verification-test-utils = { path = "../ic-response-verification-test-utils", version = "1.0.0" }
```

If a version is included in a dev dependency then the referenced dev dependency must also be published, but the version can be omitted for dev dependencies to avoid this.

### Referencing an NPM package

An NPM package can be referenced using the package name and [PNPM workspace protocol](https://pnpm.io/workspaces#workspace-protocol-workspace) in `package.json`:

```json
{
  "dependencies": {
    "@dfinity/certificate-verification": "workspace:*"
  }
}
```
