# strfry-build

Prebuilt Linux binaries of [strfry](https://github.com/hoytech/strfry), a relay for the [nostr protocol](https://github.com/nostr-protocol/nostr) — so you don't have to compile it yourself.

Binaries are built from unmodified upstream sources by the [Build and Release workflow](.github/workflows/build-and-release.yml) and published on the [releases page](../../releases).

## Releases

Each release corresponds to an upstream strfry tag: release `1.1.0` here is built from [`hoytech/strfry@1.1.0`](https://github.com/hoytech/strfry/tags). The exact upstream commit is recorded in the release notes.

Every release ships four archives, each containing a single `strfry` binary:

| Asset | Architecture | Linkage |
| --- | --- | --- |
| `strfry-linux-amd64-static-<version>.tar.gz` | x86_64 | static — runs on any Linux distro |
| `strfry-linux-arm64-static-<version>.tar.gz` | aarch64 | static — runs on any Linux distro |
| `strfry-linux-amd64-<version>.tar.gz` | x86_64 | dynamic — needs Ubuntu 24.04-compatible libs |
| `strfry-linux-arm64-<version>.tar.gz` | aarch64 | dynamic — needs Ubuntu 24.04-compatible libs |

**When in doubt, use the static variant.** The dynamic builds link against Ubuntu 24.04 (glibc 2.39) system libraries and will not run on older distros such as Debian 12.

## Usage

```sh
VERSION=1.1.0
ARCH=amd64   # or arm64

curl -fsSLO "https://github.com/mkettn/strfry-build/releases/download/${VERSION}/strfry-linux-${ARCH}-static-${VERSION}.tar.gz"
curl -fsSLO "https://github.com/mkettn/strfry-build/releases/download/${VERSION}/strfry-linux-${ARCH}-static-${VERSION}.tar.gz.sha256"

sha256sum -c "strfry-linux-${ARCH}-static-${VERSION}.tar.gz.sha256"
tar -xzf "strfry-linux-${ARCH}-static-${VERSION}.tar.gz"

./strfry --help
```

See the [strfry documentation](https://github.com/hoytech/strfry#setup) for configuration and deployment.

## Building a release

Releases are triggered manually. In the [Actions tab](../../actions), run the **Build and Release strfry** workflow:

- Leave the `tag` input empty to build the latest stable upstream tag (`x.y.z` — betas are ignored).
- Or enter a specific upstream tag (e.g. `1.0.4`) to build that version.

The workflow builds natively on `ubuntu-24.04` (amd64) and `ubuntu-24.04-arm` (arm64) runners, smoke-tests the binaries, and publishes a release tagged with the upstream version. If a release for that version already exists, the run exits without doing anything.

## License

The workflow and scripts in this repository are provided as-is. strfry itself is licensed under the [GPL-3.0 license](https://github.com/hoytech/strfry/blob/master/LICENSE) — the released binaries are subject to its terms.
