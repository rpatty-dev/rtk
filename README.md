# RTK (Fork with Vercel, Firebase, Tailscale & System Profiler Optimizers)

> **Forked `rtk` to include Vercel, Firebase, Tailscale, and macOS system diagnostics optimizers, reducing command output token consumption by up to 61.3%.**

This fork extends upstream [`rtk-ai/rtk`](https://github.com/rtk-ai/rtk) with native token compression and auto-rewrite filters for:
- **Vercel CLI** (`vercel`, `npx vercel`) &mdash; **61.3% verified token reduction**
- **macOS System Diagnostics** (`system_profiler`) &mdash; **57.3% verified token reduction**
- **Tailscale CLI** (`tailscale status`, `ping`) &mdash; **40.1% verified token reduction**
- **Firebase CLI** (`firebase`, `npx firebase`) &mdash; **32.1% verified token reduction**

---

> [!IMPORTANT]
> ### ⚡ Agent One-Shot Prompts
>
> Paste either prompt below into your AI coding assistant (Claude Code, Gemini CLI, Cursor, Antigravity, OpenCode). The agent will detect your operating system and configure the token optimizers automatically:
>
> #### Prompt A: If you already have `rtk` installed
> ```text
> Please install the custom RTK optimizers from rpatty-dev's fork (covering Vercel, Firebase, Tailscale, and system diagnostics) to lower our context token usage.
>
> Check my OS and run the appropriate command:
> - On macOS/Linux/WSL, run:
>   python3 -c 'import urllib.request, os, pathlib; base="https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/"; d = pathlib.Path(os.path.expanduser("~/Library/Application Support/rtk" if os.uname().sysname=="Darwin" else "~/.config/rtk")); d.mkdir(parents=True, exist_ok=True); fpath = d / "filters.toml"; content = "schema_version = 1\n\n"; [content := content + urllib.request.urlopen(base + name).read().decode() + "\n\n" for name in ["vercel.toml","firebase.toml","tailscale.toml","system-profiler.toml"]]; fpath.write_text(content); os.system("rtk trust --yes")'
> - On Windows PowerShell, run:
>   $d = "$env:APPDATA\rtk"; if (-not (Test-Path $d)) { New-Item -ItemType Directory -Path $d -Force | Out-Null }; $c = "schema_version = 1`n`n"; @("vercel.toml","firebase.toml","tailscale.toml","system-profiler.toml") | ForEach-Object { $c += (Invoke-RestMethod "https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/$_") + "`n`n" }; Set-Content -Path "$d\filters.toml" -Value $c; rtk trust --yes
>
> Verify the filters are trusted with `rtk trust` and confirm when finished.
> ```
>
> ---
>
> #### Prompt B: If you do NOT have `rtk` installed yet
> ```text
> Please install RTK (Rust Token Killer) along with rpatty-dev's custom cloud & CLI optimizers (Vercel, Firebase, Tailscale, system diagnostics) so we can cut our command output token usage by up to 61%.
>
> Check my OS and execute:
> - On macOS/Linux/WSL, run:
>   curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh && python3 -c 'import urllib.request, os, pathlib; base="https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/"; d = pathlib.Path(os.path.expanduser("~/Library/Application Support/rtk" if os.uname().sysname=="Darwin" else "~/.config/rtk")); d.mkdir(parents=True, exist_ok=True); fpath = d / "filters.toml"; content = "schema_version = 1\n\n"; [content := content + urllib.request.urlopen(base + name).read().decode() + "\n\n" for name in ["vercel.toml","firebase.toml","tailscale.toml","system-profiler.toml"]]; fpath.write_text(content); os.system("rtk trust --yes")'
> - On Windows PowerShell, run:
>   winget install rtk-ai.rtk; $d = "$env:APPDATA\rtk"; if (-not (Test-Path $d)) { New-Item -ItemType Directory -Path $d -Force | Out-Null }; $c = "schema_version = 1`n`n"; @("vercel.toml","firebase.toml","tailscale.toml","system-profiler.toml") | ForEach-Object { $c += (Invoke-RestMethod "https://raw.githubusercontent.com/rpatty-dev/rtk/develop/src/filters/$_") + "`n`n" }; Set-Content -Path "$d\filters.toml" -Value $c; rtk trust --yes
>
> Verify `rtk --version` works and confirm when all optimizers are active.
> ```

---

## What is Different in This Fork?

1. **Vercel CLI Optimizer (`src/filters/vercel.toml`)**:
   - Strips boilerplate disclaimers, next-step tutorials, and repetitive upload lines.
   - Preserves critical deployment URLs, live alias targets, and exit statuses.
2. **macOS System Diagnostics (`src/filters/system-profiler.toml`)**:
   - Strips vendor strings, driver buses, and internal connection lines to present display resolution and GPU core counts cleanly.
3. **Tailscale CLI Optimizer (`src/filters/tailscale.toml`)**:
   - Strips offline/idle node spam to deliver instant connectivity status for active machines.
4. **Firebase CLI Optimizer (`src/filters/firebase.toml`)**:
   - Eliminates ASCII table borders, static asset upload spam, and console link noise while keeping hosting URLs and version finalization status.
5. **Auto-Discovery Rewrites (`src/discover/rules.rs`)**:
   - Seamlessly rewrites all target commands directly through agent hooks without manual syntax changes.

---

## Verified Benchmarks & Proof

> All metrics below are calculated by UTF-8 byte count comparison (`bytes / 4 = est. tokens`) against real CLI executions.

| Command | Raw Output Size | `rtk` Filtered Size | Real Token Reduction |
| :--- | :--- | :--- | :--- |
| `vercel --prod` | 860 bytes (~215 tokens) | 333 bytes (~83 tokens) | **-61.3%** |
| `system_profiler SPDisplaysDataType` | 464 bytes (~116 tokens) | 198 bytes (~50 tokens) | **-57.3%** |
| `tailscale status` | 404 bytes (~101 tokens) | 242 bytes (~60 tokens) | **-40.1%** |
| `firebase deploy` | 427 bytes (~107 tokens) | 290 bytes (~72 tokens) | **-32.1%** |

### Verified Vercel Deployment Proof (`nitech.dev`):
```text
Vercel CLI 65.0.0
  Inspect         https://vercel.com/rpattys-projects/nitech/6Nfz5mUzyHBRTET4MP6XaPTa6x1X
  Production      https://nitech-1izpygdbd-rpattys-projects.vercel.app
▲ Aliased         https://nitech.dev
✓ Ready in 12s
https://nitech-1izpygdbd-rpattys-projects.vercel.app
```

### Verified Tailscale Output:
```text
100.112.33.47  ryans-macbook-pro  rpatty-dev@  macOS    -                       
100.71.60.14   ryan-work-pc       rpatty-dev@  windows  idle; offers exit node  
```

---

Built with help from [Nitech](https://nitech.dev).
