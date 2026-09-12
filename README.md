# OpenClaw Deterministic Latest

[![Pinned version](https://img.shields.io/badge/OpenClaw-2026.9.4-111827)](#version-pin)
[![Patch](https://img.shields.io/badge/patch-dummy%2Fdummy%20%7C%20dummy%2Fnote-2563eb)](patches/openclaw-2026.9.4-deterministic.patch)
[![Image chain](https://img.shields.io/badge/images-Fedora%2045%20latest-0ea5e9)](https://github.com/safrano9999/fedora45-ai-safrano9999/blob/main/README.md)

The independently maintained, exact deterministic gateway patch used by the
Safrano OpenClaw latest image line. This repository is separate from the original
version-pinned distribution.

This is a standalone public repository owned by `safrano9999`. It is not a
GitHub fork and has no pull-request relationship to another repository.

## Patch

The canonical patch is:

```text
patches/openclaw-2026.9.4-deterministic.patch
```

SHA-256:

```text
0e02f43f7be7ce0aeed8143fd0952da94935cc71a45ff6c248d44cfd6365582a
```

It contains the 37-file deterministic, per-server MCP private-network and provider
activation performance change set without unrelated repository history or automation.

## Migration evidence

The [previous 2026.7.1 patch](patches/openclaw-2026.7.1-deterministic.patch),
[2026.9.2 patch](patches/openclaw-2026.9.2-deterministic.patch), and
[2026.9.3 patch](patches/openclaw-2026.9.3-deterministic.patch) are retained for comparison. The current port follows upstream's new owners:

| Contract | 2026.7.1 patch | 2026.9.4 port / upstream equivalent |
|---|---|---|
| Fixed reply and NOTE precedence | Early fallback in `get-reply.ts` | Admitted-run `before-agent-reply.ts` owner; handled replies retain durable recovery bookkeeping. Deterministic turns skip model-backed maintenance. |
| Hook model identity | Added provider/model context fields | Upstream already supplies `modelProviderId` and `modelId`; no duplicate patch. |
| Hook user/media context | Legacy `MediaPaths` and related fields | Canonical `media[]`, location, and structured context project into the existing hook payload fields. |
| Catalog, picker, and auth | Added both dummy models and bypassed model auth | Both models remain available through manifest/prepared catalogs and picker paths, without changing native route variants or account-entitlement filtering. |
| Editable reply asset | `runtime-postbuild.mjs` copied the text file | The `.mts` postbuild owner and compiled test workers copy the same text asset. |
| Model-list activation cost | Per-row plugin activation | Reuse the existing operation-owned activation and same-turn auto-enable cache, including persisted manifest metadata; preserve aliases, provider normalization, freshness and lifecycle invalidation. |
| MCP private networking | Per-server explicit opt-in | Current transport and `zod-schema.root-support.ts` retain the opt-in; default SSRF protection and upstream OAuth/timeout handling remain intact. |

The 2026.9.4 port reuses the operation-scoped activation projection in upstream's
`resolveProviderLoadOptions`, preserving its separate setup/runtime eligibility,
source-config validation, and retained registry checks. Catalog imports and
postbuild fixtures follow their current owners; route tests still verify the
upstream identities and entitlements alongside both deterministic models.
Deterministic turns continue to skip model-backed compaction and memory flush.
The release workflow verifies the exact upstream base, runs the existing focused
contract tests, builds the runtime, and checks packaged imports before publishing.
These checks do not substitute for deployment and live service verification.

## Deterministic routes

| Model | Behavior |
|---|---|
| `dummy/dummy` | Commands and plugins run first. An unclaimed message receives a fixed reply without a normal model turn. |
| `dummy/note` | Commands and plugins run first. NOTE can claim and persist the message without an LLM call. |

Normal providers, tools, plugins, and LLM-backed models remain available when
another model is selected.

## Historical visible proof

The preserved native Telegram Desktop recording shows the route concept before
and after enabling `dummy/dummy`: an unavailable normal model produces a
provider error, while the deterministic route handles the ordinary message
immediately. Commands and plugin hooks remain ahead of the fallback.

<table>
  <thead>
    <tr>
      <th width="50%">Before</th>
      <th width="50%">Deterministic route</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <a href="docs/demo/deterministic-before.mp4">
          <img src="docs/demo/deterministic-before.png" alt="Telegram before the deterministic patch">
        </a>
      </td>
      <td>
        <a href="docs/demo/deterministic-after.mp4">
          <img src="docs/demo/deterministic-after.png" alt="Telegram using the deterministic route">
        </a>
      </td>
    </tr>
  </tbody>
</table>

Click either screenshot to open its MP4 recording.

These recordings predate the `2026.7.1` port. They demonstrate routing behavior,
not byte-exact release wording or `2026.9.4` build/runtime verification; the
canonical behavior is the version-pinned patch in this repository.

### NOTE full mode

`dummy/note` lets the separate NOTE plugin claim ordinary non-command messages,
store them without a model request, acknowledge the save, and return them
through `/note show`.

![NOTE full mode in Telegram](https://raw.githubusercontent.com/safrano9999/NOTE/2026.7.36/docs/full-mode.jpg)

The NOTE screenshot is a workflow illustration captured on OpenClaw `2026.6.11`,
not a `2026.9.4` build-verification artifact.

## Version pin

The patch applies only to OpenClaw `2026.9.4`, upstream commit
`3a9d69db306cd7f081e06254cb89c4bcc14a7107`.
The complete machine-readable build input is recorded in [build.conf](build.conf).
Its corresponding upstream runtime image is `ghcr.io/openclaw/openclaw:2026.9.4`.
The artifact build uses Node `26.8.2` and pnpm `12.3.4`; the Fedora image uses its
own DNF-provided Node runtime.

```bash
git apply patches/openclaw-2026.9.4-deterministic.patch
```

There is no automatic forward-port or compatibility layer. A newer OpenClaw
version requires an explicit new patch and release.

The pinned release target is
[`2026.9.4-deterministic.1`](https://github.com/safrano9999/openclaw-deterministic-latest/releases/tag/2026.9.4-deterministic.1),
with asset `openclaw-2026.9.4-deterministic.tar.gz` and its SHA-256 sidecar.
The workflow uploads verification artifacts before optional release publication;
manual dispatch defaults to `publish=false`.

## Public package

The distribution is intentionally split into three public repositories:

| Repository | Responsibility |
|---|---|
| [openclaw-deterministic-latest](https://github.com/safrano9999/openclaw-deterministic-latest) | This exact version-pinned patch |
| [NOTE](https://github.com/safrano9999/NOTE) | Independent storage plugin for `dummy/note` |
| [openclaw-ephemeral](https://github.com/safrano9999/openclaw-ephemeral) | Python startup configuration |

The current image integration starts at
[fedora45-ai-safrano9999](https://github.com/safrano9999/fedora45-ai-safrano9999)'s Core layer:

```text
ghcr.io/safrano9999/fedora45-ai-core:latest
```

A standalone latest ephemeral image has not been published.

## License

See [LICENSE](LICENSE).
