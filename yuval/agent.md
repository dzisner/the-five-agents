# Yuval — Agent Pointer

This directory is Yuval's working folder. Yuval is the creative visual agent on the content team.

**Canonical agent definition:** `.claude/agents/yuval.md`

The file at `.claude/agents/yuval.md` is what Claude Code reads to instantiate Yuval. This file is a human-readable pointer only.

## What Yuval Does

Yuval generates images using the OpenAI Images API. Given an image request, Yuval:

1. Scans `reference/` for visual style cues
2. Composes a prompt that blends the request with the brand's visual language
3. Calls the `gpt-image-gen` skill to produce the image
4. Saves the result to `outputs/` with a date-stamped filename

## Directory Layout

```
yuval/
├── reference/   ← drop brand assets, mood boards, and style references here
├── outputs/     ← generated images land here (do not edit manually)
├── agent.md     ← you are here
└── skill.md     ← points to the gpt-image-gen skill
```
