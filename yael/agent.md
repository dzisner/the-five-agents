# Yael — Content Writer Agent

This is the human-readable pointer doc for Yael.

## Where to Find Things

| File/Directory | Purpose |
|---|---|
| `.claude/agents/yael.md` | **Canonical agent definition** — Yael's instructions, tools, and workflow |
| `yael/style-guide.md` | **Style guide** — fill this in with your team's writing voice |
| `yael/reference/` | **Reference examples** — drop `.md` files here that represent the writing style Yael should emulate |

## How to Use Yael

1. Drop a raw article as a `.md` file into `Content/`
2. Ask the CEO (Reuven): *"Yael, please rewrite the article in Content/filename.md"*
3. Yael will:
   - Read the style guide and reference examples
   - Rewrite the article in the team's voice
   - Save the result to `Output/filename.md`
   - Archive the original to `Content/Ready/filename.md`
   - Report back with a summary and any `{{IMAGE_NEEDED}}` placeholders
4. The CEO will then dispatch Yuval for any image placeholders and assemble the final output

## How to Add Reference Examples

Drop any `.md` file into `yael/reference/`. Name it descriptively (e.g., `linkedin-post-example.md`, `article-about-automation.md`). Yael reads all `.md` files in this directory at the start of each session.

## How to Update the Style Guide

Edit `yael/style-guide.md` directly. It is a structured template — fill in each section with your actual guidelines. The more specific, the better Yael's output will be.

## Architecture Note

Yael cannot call other agents. When she identifies a place that needs an image, she inserts:

```
{{IMAGE_NEEDED: "<detailed description>"}}
```

The CEO (Reuven) is responsible for extracting these placeholders, dispatching Yuval for each one, and replacing them with the generated image paths in the final output.
