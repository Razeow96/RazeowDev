# Dev team — Frontend reference

REFERENCE class (structure-law): read when choosing a data-freshness strategy or a motion format; the law lives in `frontend.md`.

## Freshness strategy by data class (FE-19)

| Data class | Strategy |
|---|---|
| **static / reference** — catalogs, config, labels | fetched once, cached with a version or TTL |
| **user-session state** | the platform's session mechanism, server-controlled |
| **live / critical** — status, logs, prices, anything acted upon | server-push (WebSocket/SSE/subscription) or an event, never a tight poll |
| **slow-changing lists** | fetch-on-view + refresh on visibility return (FE-17), optionally a long poll |

## Motion format by what it is (FE-16)

| What it is | Format |
|---|---|
| UI micro-interaction | CSS/transform-and-opacity animation (GPU-friendly, no layout thrash) |
| icon and vector motion | SVG, or Lottie/JSON |
| short looping video-like motion | a video element (MP4/WebM) — **never an animated GIF** (an order of magnitude larger for worse quality) |
| a still frame | a still image |

`FE REFERENCE LOADED`
