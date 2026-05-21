# Go SDK for Workshop

A development environment for Go projects. It provides the official Go toolchain,
manages module caches via persistent mounts, and preserves Go environment
settings across workshop updates.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: go-app
base: ubuntu@24.04
sdks:
  - name: go
    channel: 1.24/stable

actions:
  build: |
    go build
  test: |
    go test ./...
```

This demonstrates a basic Go build workflow with persistent module caching.

---

## Using the SDK

### Prerequisites, project layout

1. No prerequisite SDKs are required.
2. Your Go project (with a `go.mod` file) should be in your project directory:

   ```bash
   git clone <YOUR_REPO_URL>
   ```

3. On launch, the SDK configures `PATH` and `GOMODCACHE`. No dependency
   download happens automatically; dependencies are fetched during the first
   `go build` or `go test`.

### Build the project

Once the workshop is ready:

```bash
workshop shell
go build
```

The first build downloads dependencies into `~/go/pkg/mod`, which is mapped to
your host via the `mod-cache` mount plug. Subsequent builds reuse cached
modules.

To see where the module cache is stored on the host:

```bash
workshop info
```

### Test and run

From within the workshop shell:

```bash
workshop shell
go test ./...
go run .
```

Use standard Go commands; the toolchain behaves exactly as it would in a native
Go installation.

### Environment configuration

Go environment variables set via `go env -w` persist across workshop updates:

```bash
workshop shell
go env -w GOPRIVATE=github.com/yourorg/*
```

These settings are automatically saved when the SDK is updated and restored
when the workshop is refreshed.

---

## Plugs (resources this SDK consumes)

### `mod-cache`

- Interface: `mount`
- Workshop target: `/home/workshop/go/pkg/mod`
- Purpose: Persists Go module downloads between workshop updates.

## Slots (resources this SDK provides)

This SDK doesn't define any slots.

---

## Documentation and guidance

- [Go official documentation](https://go.dev/doc/)
- [Go modules reference](https://go.dev/ref/mod)
- [Workshop documentation](https://ubuntu.com/workshop/docs/)

---

## Community and support

- Go community: [Go Forum](https://forum.golangbridge.org/)
- Workshop forum:
  [Discourse](https://discourse.ubuntu.com/)
- Please review our
  [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct) before
  participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- See `CONTRIBUTING.md` for guidelines.
- Open issues or pull requests on the official repository.

---

## License and copyright

Copyright 2025 Canonical Ltd.

This program is free software: you can redistribute it and/or modify it under
the terms of the
[GNU Lesser General Public License version 2.1 (LGPLv2.1)](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.html)
as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details.
