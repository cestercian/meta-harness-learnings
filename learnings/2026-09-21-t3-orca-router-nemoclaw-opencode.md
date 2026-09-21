# 2026-09-21 — T3 merge wave, OrcaRouter-Lite gateway seams, NemoClaw 5-PR cap, OpenCode boundary honesty

Sep 19–20 contribution wave after the 2026-09-18 NAT/OpenCode/Orca-CR note. Patterns worth carrying; not a PR laundry list.

## T3 Code — control-plane honesty at CSP, a11y, ACP, and boot env (five merges + one redesign)

Five PRs merged 2026-09-19 on `pingdotgg/t3code`. Shared theme: keep the surface the user (or next session) can still correct, and prefer documented files over fragile unit/plist round-trips.

| PR | Seam | Pattern |
| --- | --- | --- |
| `#12636` merged | Desktop annotation + CSP | `fetch(dataUrl)` is a `connect-src` CSP violation on Windows wraps. Decode PNG with the existing `dataUrlToFile` path; leave CSP unchanged. Malformed screenshots still send text only. |
| `#12635` merged | Linux SnapShot a11y | Flatpak: PID lookup against the compositor fails through `xdg-dbus-proxy` and was swallowed. GTK4: unnamed `group` + title match rejected the window. Fall back to `App.list()` title+size; accept a unique PID-scoped bounds match when the window has no name. |
| `#12625` merged | ACP process exit | When `cursor-agent acp` rejects `.cursor/cli.json` and exits 1, do not collapse to `ProviderAdapterSessionClosedError`. Keep a redacted stderr tail on the process-exit error so the schema keys are visible. |
| `#12624` merged | Claude homePath | Empty `CLAUDE_CONFIG_DIR` / `homePath` must resolve like the CLI (`homePath` → inherited env → `~/.claude`), not bare `$HOME`, or empty and explicit `~/.claude` get different continuation group keys. |
| `#12577` merged | Question option click | Selecting an option must park non-empty typed custom answer onto the thread draft; do not wipe composer text. Option still submits; typed text returns as the next draft. |

### Prefer a documented env file over preserving unit keys (`#12633` open)

`t3 update` / `t3 service install` re-render the boot unit and drop user environment (e.g. Bitbucket credentials). Preserving unknown `Environment=` / plist keys is fragile (systemd C-escapes corrupt on re-quote). Triage steered to an optional `$T3CODE_HOME/service.env` the launcher merges at start (`util.parseEnv`); unit-owned keys stay skipped. Macroscope initially rejected the unit-key round-trip approach — redesign to the env-file path before fighting review on the fragile option.

Issues: #12265, #12597, #12451, #12616, #12569, #12626.

## OrcaRouter-Lite — forward what clients send; fail loud on auth/cache seams

Ten open fixes on `Continuum-AI-Corp/OrcaRouter-Lite` cluster into three seams:

### Request fidelity (do not drop OpenAI-compatible fields)

`parallel_tool_calls` (#147), `max_completion_tokens` for openai-python ≥1.51 (#148), `logit_bias` (#149), LangChain `response_format` / `json_schema` shape normalization before LiteLLM (#156). Routers that silently strip fields look healthy while clients misbehave.

### Auth and key governance

Prefer the **most specific** `AuthError` across credential candidates (#150). After encryption-key rotation, surface undecryptable provider keys instead of opaque failure (#152) — and never re-seal real credentials with a publicly known dev key when `CREDENTIAL_ENCRYPTION_KEY` is unset (Orca-CR P1). `model_allowlist` write path (#151): restricted keys may clear/update their own allowlist with clear rules; block self-escalation to unrestricted (`null`) when that would widen scope; validate catalog membership. Startup credential audits need compare-and-swap / lock so concurrent PUTs are not clobbered.

### Streaming, fallback, cache honesty

When a local BYOK key 429s and hosted fallback serves the request, set `x-orca-fallback: true` (#153). Emit a dedicated usage SSE chunk **before** `[DONE]` (#154) so clients that stop at `finish_reason` still see tokens. Prompt-cache key bump to `v: 2` (#155): `top_p < 1` is not deterministic without a `seed`; omitted temperature does not imply 0.

## NemoClaw — contrib posture and OpenShell policy seams

### Hard contrib gotcha: 5 open PRs max

`NVIDIA/NemoClaw` auto-closes surplus PRs (`github-actions`: “limits you to 5 open pull requests”). `#12112`/`#12113`/`#12114` were closed on open for that reason while five others stayed. Claim/queue carefully; close or land before opening the next.

### Runtime / policy patterns (still open)

| PR | Seam | Pattern |
| --- | --- | --- |
| `#12104` | MCP add policy | Generated OpenShell policies must allow the credential-resolution curl probe (`CONNECT` attributed to `/proc/<pid>/exe`); otherwise `mcp add` denies its own health check. |
| `#12108` | Backup | Do not `2>/dev/null` away permission-denied dirs then treat a parent archive as complete — emit audit failures and exit nonzero. |
| `#12109` | OpenClaw devices approve | Local-fallback `Approved` without `exit(0)` leaves Node handles open; NemoClaw `connect` hangs with it. |
| `#12110` | Tool disclosure | Progressive tool disclosure breaks local Ollama/vLLM (malformed nested tool_call). Default those routes to **direct**; keep progressive for cloud. |
| `#12111` | Policy remove verify | OpenShell `--base` readback re-includes `_provider_*` keys — compare after stripping provider-composed keys so a successful remove exits 0. |

## OpenCode — second boundary-honesty wave

Eight open fixes after the 2026-09-18 batch. Same theme: fail where the user or next session can still correct.

| PR | Seam | Pattern |
| --- | --- | --- |
| `#49805` | Client ↔ bind address | Service registered as `0.0.0.0` / `::` must be rewritten to loopback for CLI probes. |
| `#49807` | Exit splash | Resume hint must use the **invoked** bin name, not a hard-coded `opencode`. |
| `#49937` | Theme tokens | Alias pre-2.0.9 text token names after renames (`text.subdued` → `text.muted`, …). |
| `#49940` | Custom providers | V1 migrations often omit `capabilities.tools` — default omitted capability tools rather than reject. |
| `#50046` | MCP OAuth issuer | Treat trailing-slash issuer identifiers as equivalent (RFC 8414 well-known strip). |
| `#50047` | VS Code serve | Foreground `serve` must print the `opencode server listening` marker the extension waits for. |
| `#50048` | Plugin Slot | ErrorBoundary around plugin sidebar slots so a broken plugin cannot crash the TUI. |
| `#50050` | Windows npm/npx | Prefer `.cmd` over nvm `.ps1` shims (Bun can ShellExecute `.ps1` as a document / Notepad). |

Issues: #49796, #49764, #49922, #49912, #50036, #50043, #50027, #50040.

## Contrib process update (2026-09-20)

Cursor cloud agents may open the **upstream** PR again (fork head → upstream base). Still: author as `cestercian` only; never AI `Co-authored-by`; strip Cursor footers; fork-only PR descriptions must not mention the issue (`Fixes`/`Closes` only on the upstream PR). Babysits escalate **all** comments/reviews (human or bot), not only @mentions; quiet only for duplicate green CI with no new comments; delete babysit on upstream merge/close.

Prioritize faster-reply targets (T3, OpenClaw when forkable, NemoClaw within the 5-PR cap) when choosing new work; keep the full harness sweep sequential.

## Carry-forward

- Desktop wraps: decode `data:` locally under CSP; never `fetch(dataUrl)` as connect.
- Sandboxed a11y (Flatpak/GTK4): PID compositor lookup fails — title+size / bounds fallbacks.
- Provider adapters: surface ACP/process stderr; align homePath resolution with the real CLI.
- Boot/service config: documented merge-in env file beats preserving unknown unit/plist keys.
- LLM routers: forward OpenAI-compatible fields; normalize LangChain shapes; advertise fallback and usage before `[DONE]`; never re-encrypt with a known-dev key.
- NemoClaw: respect the 5-open-PR cap; local models → direct tool disclosure; fail loud on incomplete backups; strip provider keys when verifying policy remove.
- OpenCode: rewrite wildcard binds for clients; ErrorBoundary plugin slots; Windows shim preference; issuer slash equivalence; default omitted capability tools.
- Process: Cursor may open upstream PRs; babysit every comment; no AI co-author trailers.

## Links

- https://github.com/pingdotgg/t3code/pull/12636
- https://github.com/pingdotgg/t3code/pull/12635
- https://github.com/pingdotgg/t3code/pull/12625
- https://github.com/pingdotgg/t3code/pull/12624
- https://github.com/pingdotgg/t3code/pull/12577
- https://github.com/pingdotgg/t3code/pull/12633
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/147
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/151
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/152
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/154
- https://github.com/NVIDIA/NemoClaw/pull/12104
- https://github.com/NVIDIA/NemoClaw/pull/12110
- https://github.com/anomalyco/opencode/pull/49805
- https://github.com/anomalyco/opencode/pull/50048
- https://github.com/anomalyco/opencode/pull/50050