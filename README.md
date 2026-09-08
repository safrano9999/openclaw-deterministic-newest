# OpenClaw Deterministic Newest

[![Pinned version](https://img.shields.io/badge/OpenClaw-2026.9.2-111827)](#version-pin)
[![Patch](https://img.shields.io/badge/patch-dummy%2Fdummy%20%7C%20dummy%2Fnote-2563eb)](patches/openclaw-2026.9.2-deterministic.patch)
[![Image chain](https://img.shields.io/badge/images-Fedora%2044%20latest-0ea5e9)](https://github.com/safrano9999/fedora-ai-safrano9999/blob/main/README.md)

The independently maintained, exact deterministic gateway patch used by the
Safrano OpenClaw newest image line. This repository is separate from the original
version-pinned distribution.

This is a standalone public repository owned by `safrano9999`. It is not a
GitHub fork and has no pull-request relationship to another repository.

## Patch

The canonical patch is:

```text
patches/openclaw-2026.9.2-deterministic.patch
```

SHA-256:

```text
1a1ce6ba2c68100baa82dded78c296f71a333ebde9eeb80be760fd8807275b69
```

It contains the 37-file deterministic, per-server MCP private-network and provider
activation performance change set without unrelated repository history or automation.

## Migration evidence

The [previous 2026.7.1 patch](patches/openclaw-2026.7.1-deterministic.patch)
is retained for comparison. The current port follows upstream's new owners:

| Contract | 2026.7.1 patch | 2026.9.2 port / upstream equivalent |
|---|---|---|
| Fixed reply and NOTE precedence | Early fallback in `get-reply.ts` | Admitted-run `before-agent-reply.ts` owner; handled replies retain durable recovery bookkeeping. Deterministic turns skip model-backed maintenance. |
| Hook model identity | Added provider/model context fields | Upstream already supplies `modelProviderId` and `modelId`; no duplicate patch. |
| Hook user/media context | Legacy `MediaPaths` and related fields | Canonical `media[]`, location, and structured context project into the existing hook payload fields. |
| Catalog, picker, and auth | Added both dummy models and bypassed model auth | Both models remain available through manifest/prepared catalogs and picker paths, without changing native route variants or account-entitlement filtering. |
| Editable reply asset | `runtime-postbuild.mjs` copied the text file | The `.mts` postbuild owner and compiled test workers copy the same text asset. |
| Model-list activation cost | Per-row plugin activation | Reuse the existing operation-owned activation and same-turn auto-enable cache, including persisted manifest metadata; preserve aliases, provider normalization, freshness and lifecycle invalidation. |
| MCP private networking | Per-server explicit opt-in | Current transport and `zod-schema.root-support.ts` retain the opt-in; default SSRF protection and upstream OAuth/timeout handling remain intact. |

Source validation passed exact-base patch checks, core TypeScript checking, focused
regression suites, and the embedded-runner integration suite. Existing route,
auth, and capability assertions remain in place; exact inventory expectations add
the two deterministic choices. A fresh independent P0 review was scoped-clean.
These source checks do not substitute for a successful GitHub artifact build or
live LiteLLM/native ChatGPT model-discovery and request verification.

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
not byte-exact release wording or `2026.9.2` build/runtime verification; the
canonical behavior is the version-pinned patch in this repository.

### NOTE full mode

`dummy/note` lets the separate NOTE plugin claim ordinary non-command messages,
store them without a model request, acknowledge the save, and return them
through `/note show`.

![NOTE full mode in Telegram](https://raw.githubusercontent.com/safrano9999/NOTE/2026.7.36/docs/full-mode.jpg)

The NOTE screenshot is a workflow illustration captured on OpenClaw `2026.6.11`,
not a `2026.9.2` build-verification artifact.

## Version pin

The patch applies only to OpenClaw `2026.9.2`, upstream commit
`3928bad9badfcb6c7d140530435e806fb8092190`.
The complete machine-readable build input is recorded in [build.conf](build.conf).
Its corresponding upstream runtime image is `ghcr.io/openclaw/openclaw:2026.9.2`.
The artifact build uses Node `24.18.0` and pnpm `12.1.0`.

```bash
git apply patches/openclaw-2026.9.2-deterministic.patch
```

There is no automatic forward-port or compatibility layer. A newer OpenClaw
version requires an explicit new patch and release.

The pinned release target is
[`2026.9.2-deterministic.3`](https://github.com/safrano9999/openclaw-deterministic-newest/releases/tag/2026.9.2-deterministic.3),
with asset `openclaw-2026.9.2-deterministic.tar.gz` and its SHA-256 sidecar.
The workflow uploads verification artifacts before optional release publication;
manual dispatch defaults to `publish=false`.

## Public package

The distribution is intentionally split into three public repositories:

| Repository | Responsibility |
|---|---|
| [openclaw-deterministic-newest](https://github.com/safrano9999/openclaw-deterministic-newest) | This exact version-pinned patch |
| [NOTE](https://github.com/safrano9999/NOTE) | Independent storage plugin for `dummy/note` |
| [openclaw-ephemeral-newest](https://github.com/safrano9999/openclaw-ephemeral-newest) | Python startup configuration |

The current image integration starts at
[fedora-ai-safrano9999](https://github.com/safrano9999/fedora-ai-safrano9999)'s Core layer:

```text
ghcr.io/safrano9999/fedora44-ai-core-latest:latest
```

A standalone newest ephemeral image has not been published.

## License

See [LICENSE](LICENSE).
