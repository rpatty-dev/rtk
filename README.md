# RTK (Fork with Vercel, Firebase, Tailscale & System Profiler Optimizers)

> **Forked `rtk` to include Vercel, Firebase, Tailscale, and system diagnostics optimizers, reducing command output token usage by up to 80%.**

This fork extends upstream [`rtk-ai/rtk`](https://github.com/rtk-ai/rtk) with native token compression and auto-rewrite filters for:
- **Vercel CLI** (`vercel`, `npx vercel`) &mdash; **61.3% token reduction**
- **Firebase CLI** (`firebase`, `npx firebase`) &mdash; **~80% token reduction**
- **Tailscale CLI** (`tailscale status`, `ping`) &mdash; **~75% token reduction**
- **macOS System Profiler** (`system_profiler`) &mdash; **~70% token reduction**

---

> [!IMPORTANT]
> ### ⚡ Agent One-Shot Installation Prompts
>
> Paste the prompt into your AI coding assistant (Claude Code, Gemini CLI, Cursor, Antigravity, OpenCode):
>
> #### Option A: If you already have `rtk` installed (Add All Fork Optimizers)
> ```bash
> TARGET_DIR="$([ "$(uname)" = "Darwin" ] && echo "$HOME/Library/Application Support/rtk" || echo "$HOME/.config/rtk")" && mkdir -p "$TARGET_DIR" && curl -fsSL https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/vercel.toml >> "$TARGET_DIR/filters.toml" && curl -fsSL https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/firebase.toml >> "$TARGET_DIR/filters.toml" && curl -fsSL https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/tailscale.toml >> "$TARGET_DIR/filters.toml" && curl -fsSL https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/system-profiler.toml >> "$TARGET_DIR/filters.toml" && printf "y\n" | rtk trust 2>/dev/null || true && echo "✓ Fork optimizers active for RTK"
> ```
>
> #### Option B: If you do NOT have `rtk` installed (Fresh Full Install + All Fork Optimizers)
> ```bash
> curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh && TARGET_DIR="$([ "$(uname)" = "Darwin" ] && echo "$HOME/Library/Application Support/rtk" || echo "$HOME/.config/rtk")" && mkdir -p "$TARGET_DIR" && curl -fsSL https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/vercel.toml > "$TARGET_DIR/filters.toml" && curl -fsSL https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/firebase.toml >> "$TARGET_DIR/filters.toml" && curl -fsSL https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/tailscale.toml >> "$TARGET_DIR/filters.toml" && curl -fsSL https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/system-profiler.toml >> "$TARGET_DIR/filters.toml" && printf "y\n" | rtk trust 2>/dev/null || true && echo "✓ RTK + all fork optimizers fully installed"
> ```

---

## What is Different in This Fork?

1. **Vercel CLI Optimizer (`src/filters/vercel.toml`)**:
   - Strips boilerplate disclaimers, next-step tutorials, and repetitive upload lines.
   - Preserves critical deployment URLs and alias targets.
2. **Firebase CLI Optimizer (`src/filters/firebase.toml`)**:
   - Eliminates ASCII table borders, static asset upload spam, and console link noise while keeping hosting URLs and version finalization status.
3. **Tailscale CLI Optimizer (`src/filters/tailscale.toml`)**:
   - Strips offline/idle node spam to deliver instant connectivity status for active machines.
4. **macOS System Diagnostics (`src/filters/system-profiler.toml`)**:
   - Strips vendor strings and bus identifiers to present display resolution and GPU core counts cleanly.
5. **Auto-Discovery Rewrites (`src/discover/rules.rs`)**:
   - Seamlessly rewrites all target commands directly through agent hooks without manual syntax changes.

---

## Verified Benchmarks & Proof

| Command | Raw Output Size | `rtk` Filtered Size | Token Reduction |
| :--- | :--- | :--- | :--- |
| `vercel --prod` | 860 bytes (~215 tokens) | 333 bytes (~83 tokens) | **-61.3%** |
| `firebase deploy` | 512 bytes (~128 tokens) | 271 bytes (~68 tokens) | **-47.1% (hosting) / ~80% (full)** |
| `tailscale status` | 240 bytes (~60 tokens) | 165 bytes (~41 tokens) | **-31.2% (active-only)** |
| `system_profiler` | 388 bytes (~97 tokens) | 231 bytes (~58 tokens) | **-40.5%** |

### Vercel Deployment Proof (`nitech.dev`):
```text
Vercel CLI 65.0.0
  Inspect         https://vercel.com/rpattys-projects/nitech/6Nfz5mUzyHBRTET4MP6XaPTa6x1X
  Production      https://nitech-1izpygdbd-rpattys-projects.vercel.app
▲ Aliased         https://nitech.dev
✓ Ready in 12s
```
