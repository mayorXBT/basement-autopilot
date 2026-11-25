# Basement Autopilot

Basement Autopilot is a supervised GitLab CI/CD lifecycle, with bounded agent help, built around [Basement](https://basement.cefo.dev) — an anonymous impromptu speaking practice app. Spin a topic, speak for a minute, and get delivery-focused coaching without creating an account.

This repository is a public GitHub showcase of the GitLab AI Hackathon **"Life After Code"** (Transcend, October 2026) entry, so hiring managers can inspect the code, the pipelines, and the evidence. Continuous integration runs on GitLab, not GitHub. The live app stays on Vercel; GitLab checks that deployment and does not deploy it.

- **Devpost:** [devpost.com/software/basement-autopilot](https://devpost.com/software/basement-autopilot)
- **Demo video:** [youtu.be/Pzs5F1gSvks](https://youtu.be/Pzs5F1gSvks)
- **Live app:** [basement.cefo.dev](https://basement.cefo.dev)
- **Original GitLab repo:** [gitlab-ai-hackathon/.../showcase](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase)

See [PROGRESS.md](PROGRESS.md) and [docs/STAGES.md](docs/STAGES.md) before repeating any autonomy claim.

## Why it's useful

**The problem.** Teams shipping software still have to stitch planning, review, tests, security, packaging, release evidence, production checks, monitoring, and governance by hand. Privacy promises often live in a policy doc with no machine check. AI coding agents can open merge requests, but they are easy to trust with shipping gates they should not own.

**Who it's for.** Platform, DevOps, and application teams who want AI help inside a real delivery pipeline — especially on privacy-sensitive products — without giving the model the final say.

**What you get in practice.**

- One GitLab pipeline that covers all nine software lifecycle stages, with opened run evidence for **9/9**. Plan and Create are proven as GitLab work artifacts (an issue and a merged MR), not as Duo execution.
- Deterministic jobs, not the model, own the shipping gates: tests, privacy canaries, packaging, release receipts, production health checks, and governance comments.
- Agents may plan, propose bounded fixes, and explain results under supervision. This submission is Path B, Supervised; hands-off delivery is not claimed.
- A speaking app whose privacy promises (no stored audio, no speech logs, no accounts or tracking) are checked in CI and against the live site.
- **Duo/AI Catalog execution is pending.** The flow source exists; no Duo session or Duo-authored Builder MR was captured, and that run is not one of the nine stages.

## Tech stack

GitLab CI/CD, GitLab Duo Agent Platform (flow source only; execution pending), Node.js, Express, Docker, Vercel, Gemini, Deepgram, and Groq.

## How it works

```mermaid
flowchart LR
    I[Issue] --> T[Plan: Duo Triage]
    T --> B[Create: Duo Builder]
    B --> MR[Merge request]
    MR --> V[Verify: tests and speech eval]
    MR --> S[Secure: scans and privacy canary]
    V --> R[Release: merge, receipt, SBOM]
    S --> R
    R --> C[Configure: Vercel production target]
    C --> G[Govern: deterministic policy]
    G --> P[Vercel production check]
    P --> E[Evidence comment and issue close]
    P --> M[Monitor: live probes]
```

Agents plan, change code, fix bounded failures, and explain results. Ordinary deterministic jobs own every shipping gate. The default is supervised, and GitLab records the existing Vercel production URL without provisioning another hosting platform.

## The nine stages

Evidence status: **9/9 stages proven** for a supervised issue-to-production evidence run plus a schedule-sourced Monitor run. Plan and Create are proven as GitLab work artifacts, not as Duo execution. The Duo/AI Catalog run is still pending and is not one of the nine stages. Hands-off delivery is not claimed.

| Stage | Main implementation | Runtime proof |
|---|---|---|
| Plan | [Conductor flow](.gitlab/duo/flows/conductor.yaml) | [Issue #4](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/issues/4) records the failure and expected outcome. This does not prove a Duo Triage run. |
| Create | [Conductor flow](.gitlab/duo/flows/conductor.yaml) | [MR !11](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/merge_requests/11) and its [green MR pipeline](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/pipelines/2984569889). The MR came from the GitHub mirror, not Duo. |
| Verify | [`verify.yml`](.gitlab/ci/verify.yml) | [Main pipeline](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/pipelines/2984574204), [unit job](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123003), and [six synthetic evals](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123007) |
| Secure | [`secure.yml`](.gitlab/ci/secure.yml) | [SAST](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123009), [static privacy](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123012), and [runtime canary](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123013) |
| Package | [`package.yml`](.gitlab/ci/package.yml) | [Package job](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123014) built and pushed the commit-addressed image |
| Release | [`release.yml`](.gitlab/ci/release.yml) | [v1.0.11 release](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/releases/v1.0.11), [privacy receipt](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123017/artifacts/file/release-assets/privacy-receipt.md), and [passing tag pipeline](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/pipelines/2984578289) |
| Configure | [`deploy.yml`](.gitlab/ci/deploy.yml) | [`deploy:vercel`](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123018) checked the existing [live Vercel app](https://basement.cefo.dev). GitLab did not deploy Vercel. |
| Monitor | [`monitor.yml`](.gitlab/ci/monitor.yml) | [Schedule pipeline 2985152954](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/pipelines/2985152954) and [`monitor:synthetic`](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17441379147) |
| Govern | [`govern.yml`](.gitlab/ci/govern.yml) | [`govern:evidence`](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17438123020) and the [issue #4 evidence comment](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/issues/4#note_4082415574) |

The detailed map, including disclosed unproven boundaries, is in [docs/STAGES.md](docs/STAGES.md).

## Privacy promises

Basement treats privacy copy as a release contract. The full wording and enforcement rules are in [PRIVACY_PROMISES.md](PRIVACY_PROMISES.md).

| Promise | How it is checked |
|---|---|
| P1 audio is never stored | Static filesystem rule and isolated container diff |
| P2 speech data is never logged | Static logging rule and unique runtime canary |
| P3 no accounts or tracking | Source scan plus cookie checks |
| P4 keys stay server-side | Header-only provider tests, exact health schema, and image scan |
| P5 only declared processors receive speech | Egress allowlist and fake-provider routing recorder |
| P6 security headers stay enabled | Exact static, container, and live header checks |
| P7 payload and abuse limits remain | Source rule and eleventh-request rejection probe |
| P8 spoken words are data, not instructions | Prompt rule and injection eval fixture |
| P9 browser storage remains bounded | Static browser-storage rule |
| P10 microphone tracks stop with the session | Privacy Guard and targeted browser review; not machine-passed |

P1 through P9 have deterministic checks that pass both locally and in the showcase pipeline. P10 is deliberately shown as review-required unless a real Guard artifact proves it. Release receipts fail closed instead of turning missing evidence into a pass.

## Coaching evaluation

The evaluation suite has six synthetic, labelled fixtures covering clean speech, filler precision, pace, pauses, prompt injection, and garbled input. Layer A verifies deterministic metrics and prompt invariants. Layer B runs three samples per fixture, requires a two-of-three majority, compares against the protected baseline, and can reuse redacted cached scores without storing transcripts or provider responses.

The current local baseline passes 6/6 fixtures with `gemini-3.1-flash-lite`, and [Layer A passes on GitLab](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/jobs/17397617385). These are synthetic fixtures, not the owner's recorded speech; the MR-only Layer B report remains pending. Run `node scripts/eval/run-eval.mjs a` for Layer A and `node scripts/eval/run-eval.mjs b` for Layer B.

## Try the Autopilot

In an enabled GitLab project, create an issue from the [`autopilot` template](.gitlab/issue_templates/autopilot.md), describe one small outcome, and keep the `autopilot` label. Triage should post bounded acceptance criteria and Builder should open the linked MR. The demo-only “Add a Job interview niche” feature is intentionally absent from this branch so the flow, not a person, can create it.

CI for this project runs on the [original GitLab repo](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase); the GitLab CI configuration in this repository is kept because it is the core of the hackathon entry. For judges: check `PROGRESS.md` first. `AUTOPILOT_PAUSED=1` may intentionally disable agent repair and production changes during judging. The [showcase project](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase) receives this branch through a non-force GitHub Actions mirror; its Duo configuration becomes active only after the branch lands on GitLab's default branch through an MR.

## Before and after

| Before at [`91810f0`](https://github.com/mayorXBT/Basement/commit/91810f090ebea09ff67a724e3bb6358bc68a2fb9) | Autopilot branch today |
|---|---|
| Vercel deploy from GitHub, with no CI configuration | GitLab CI source for all nine lifecycle stages |
| Tests ran only when invoked locally | 57 tests plus syntax, dependency, privacy, eval, and container gates in a [green GitLab pipeline](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/pipelines/2978492394) |
| Privacy promises were prose | P1-P9 deterministic checks; P10 review-required |
| No coaching regression suite | Six labelled synthetic fixtures with majority scoring and a protected baseline |
| No release evidence bundle | Receipt, tamper-evident ledger, SBOM, and eval assets published with [GitLab release v1.0.11](https://gitlab.com/gitlab-ai-hackathon/transcend-october-2026/43556545/showcase/-/releases/v1.0.11) |

## What It Does

- Generates speaking prompts with Gemini, with local topic fallbacks when the provider is unavailable.
- Supports off-the-cuff and deep-research practice modes.
- Runs a speech timer with pause and reset controls.
- Requests microphone access only while a speech session is active.
- Shows a live microphone waveform and pitch readout in the timer.
- Detects audible speaking coverage and long pauses in the browser.
- Records audio as a temporary browser blob for transcription.
- Sends completed audio to Deepgram first, with Groq Whisper as a fallback.
- Sends the transcript and measured delivery signals to Gemini for coaching.
- Shows clarity, confidence, filler control, speaking pace, pitch span, transcript, strengths, and next moves.
- Plays a completion sound and confetti effect when a mission ends.

## Application architecture

```text
Browser
  ├─ topic reel and timer
  ├─ Web Audio analyser: speaking ratio, pauses, pitch span
  ├─ MediaRecorder: temporary audio blob
  └─ sessionStorage: current analysis only

Vercel Node function / Express server
  ├─ POST /api/spin        -> Gemini topic generation
  ├─ POST /api/transcribe -> Deepgram -> Groq fallback
  ├─ POST /api/analyze    -> Gemini speech coaching
  └─ GET  /api/health     -> provider configuration status
```

The app does not use Supabase because it currently has no accounts, saved history, relational data, or persistence requirement. Vercel is the appropriate backend host for the existing stateless API.

## Requirements

- Node.js 18 or newer
- A Gemini API key for topic generation and analysis
- A Deepgram API key for primary transcription
- A Groq API key for transcription fallback
- A browser that supports `MediaRecorder` and microphone access for full sessions

## Local Setup

```powershell
npm ci
Copy-Item .env.example .env.local
```

Fill in `.env.local`:

```env
GEMINI_API_KEY=your-gemini-key
GROQ_API_KEY=your-groq-key
DEEPGRAM_API_KEY=your-deepgram-key
GEMINI_MODEL=gemini-3.6-flash
GEMINI_ANALYSIS_MODEL=gemini-3.6-flash
DEEPGRAM_MODEL=nova-3
GROQ_TRANSCRIPTION_MODEL=whisper-large-v3-turbo
PORT=3000
```

Start the local server:

```powershell
npm start
```

Open [http://localhost:3000/i](http://localhost:3000/i).

For development with automatic server restarts:

```powershell
npm run dev
```

## API

### `GET /api/health`

Returns provider configuration booleans. It never returns secret values.

### `POST /api/spin`

Request:

```json
{
  "mode": "off-the-cuff",
  "niche": "general"
}
```

`mode` is `off-the-cuff` or `deep-research`. Niche values are allowlisted by the server.

### `POST /api/transcribe`

Accepts an audio request body. The server accepts supported audio content types and limits uploads to 25 MB. Deepgram is tried first, then Groq Whisper.

### `POST /api/analyze`

Request:

```json
{
  "topic": "A difficult decision",
  "transcript": "The spoken transcript goes here.",
  "metrics": {
    "words": 31,
    "fillers": 1,
    "pauses": 2,
    "wpm": 124,
    "speakingRatio": 68,
    "pitchAverage": 148,
    "pitchRange": 42,
    "transcriptionProvider": "deepgram"
  }
}
```

Gemini returns structured delivery feedback for clarity, confidence, filler control, pace, strengths, and improvements. Transcript and topic fields are bounded and treated as untrusted quoted data in the analysis prompt.

## Security

- API keys are read only on the server through environment variables.
- `.env`, `.env.*`, and `.env.local` are ignored by Git; `.env.example` is safe to commit.
- API routes have IP-based rate limits with stricter limits on provider calls.
- Future `/api/auth` routes are reserved for a five-attempt-per-15-minute limiter.
- JSON, transcript, topic, metrics, and audio payloads are validated and bounded.
- Security headers include CSP, frame protection, MIME sniffing protection, referrer policy, and microphone permissions policy.
- Gemini receives transcript data as untrusted content and is instructed not to follow embedded commands.
- The app does not log API key values or audio contents.

Before a public launch, rotate any API keys that have been used during development and configure HTTPS through the deployment platform.

## Deployment

Production remains on Vercel through the existing `api/index.js` and `vercel.json`. GitLab's `deploy:vercel` job points the production environment at [basement.cefo.dev](https://basement.cefo.dev) and runs a read-only health, headers, and cookie check. It does not provision or deploy infrastructure and requires no Google Cloud variables.

Configure these Vercel environment variables for Preview and Production:

- `GEMINI_API_KEY`
- `GROQ_API_KEY`
- `DEEPGRAM_API_KEY`
- `GEMINI_MODEL`
- `GEMINI_ANALYSIS_MODEL`
- `DEEPGRAM_MODEL`
- `GROQ_TRANSCRIPTION_MODEL`

The production custom domain is `basement.cefo.dev`.

## Credits and licence

Basement is available under the [MIT License](LICENSE). The engineering approach was inspired by the [Pragmatic Programmer skill](https://github.com/iamarunr/pragmatic-programmer-skill); that skill is credited but not included because its upstream repository has no licence file.

## Verification

```powershell
node --check server.js
node --check public\app.js
node --check public\analysis.js
npm audit --omit=dev
```

The app should return `200` from `/api/health`, `400` for malformed API payloads, `415` for unsupported audio types, and `429` after the configured rate limit is exceeded.

## Project Files

- `server.js`: Express API, provider integrations, validation, rate limiting, and security headers.
- `api/index.js`: Vercel serverless entry point.
- `public/index.html`: practice interface and timer modal.
- `public/app.js`: timer, recording, waveform, local metrics, and analysis handoff.
- `public/analysis.html`: dedicated analysis route.
- `public/analysis.js`: analysis page rendering.
- `public/styles.css`: Basement visual system and responsive layout.
- `.env.example`: safe environment variable template.
