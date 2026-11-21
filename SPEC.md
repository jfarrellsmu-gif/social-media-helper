# SPEC.md — Social Media Helper (from Instagram reel Ddw2K7_CtrK)

## Goal
Turn any Instagram reel / TikTok / YouTube link into a working Claude Code skill that does what the reel teaches. One command: give the URL, get the skill built and tested.

## Tools and services (CONFIRMED)
- **yt-dlp** (at `/opt/homebrew/bin/yt-dlp`) — downloads the video locally.
- **whisper-cli** (`/opt/homebrew/bin/whisper-cli`) + `ggml-small.bin` — on-Mac speech-to-text.
- **ffmpeg** — extracts audio to WAV.
- **Obscura** (https://github.com/h4ckf0r0day/obscura) — the tool the reel actually teaches. A Rust headless browser for AI agents; 36k stars; drop-in for headless Chrome with Puppeteer/Playwright; built-in anti-detect; 30 MB memory vs 200+ MB for Chrome; Cloudflare ported it to Workers (Kitesurf).
- **obscura.sh** — hosted CDP browser for agents.
- **FreeLLMAPI / free-reader** — summarizer for long READMEs and pages.

## Steps (CONFIRMED)
1. Download the reel: `yt-dlp -o <path> <URL>`.
2. Extract audio: `ffmpeg -i reel.mp4 -vn -ar 16000 -ac 1 -c:a pcm_s16le out.wav`.
3. Transcribe: `whisper-cli -m ggml-small.bin -f out.wav -l auto -np -oj -of out` → JSON with timestamps.
4. Read the transcript; identify the tool, repo, install command, and key usage.
5. Fetch the tool's README (via free-reader or Firecrawl) to get exact commands.
6. Build a Claude Code skill in `~/Claude/video-skills/<name>/skill/` with a SKILL.md that:
   - States what the skill does in one line.
   - Lists the exact commands to install and use the tool.
   - Includes a working example.
7. Test the skill once on a real input.

## Inputs and outputs
- Input: a reel/URL (Instagram, TikTok, YouTube) or a local video file path.
- Output: a working Claude Code skill folder with SKILL.md, plus a short plain-English explanation.

## Assumed prerequisites
- Homebrew installed: `brew install ffmpeg yt-dlp whisper-cpp`
- Whisper model downloaded: `ggml-small.bin` in `~/.local/share/whisper-models/`
- FreeLLMAPI app running (for free-reader)

## Gotchas
- macOS sandbox blocks `~/.config` reads; whisper-cli needs `--no-gpu` on this Mac (GGML_ASSERT otherwise).
- Instagram reels often have no captions → whisper is mandatory.
- If the reel only says "comment X for the link", search for the real tool (as done here: Obscura).

## OPEN QUESTIONS
- None. The reel was fully transcribed and the tool identified.