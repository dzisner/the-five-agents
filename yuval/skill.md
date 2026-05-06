# gpt-image-gen — Skill Pointer

**Canonical skill definition:** `.claude/skills/gpt-image-gen/SKILL.md`

The file at `.claude/skills/gpt-image-gen/SKILL.md` is what Claude Code reads to execute image generation. This file is a human-readable pointer only.

## What the Skill Does

`gpt-image-gen` wraps the OpenAI Images API (`POST /v1/images/generations`). It handles:

- Loading `OPENAI_API_KEY` from `.env`
- Sending the prompt to the `gpt-image-2` model
- Decoding the base64 PNG response (via `jq` or `python3` fallback)
- Writing the decoded image to a specified output path
- Verifying the output file is non-empty

## Required Environment Variable

```
OPENAI_API_KEY=<your key>
```

Set this in `.env` at the project root before running any image generation.
