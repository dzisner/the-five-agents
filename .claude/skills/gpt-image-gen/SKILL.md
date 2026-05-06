# Skill: gpt-image-gen

## When to Use

Use this skill whenever you need to generate an image via the OpenAI Images API. Any agent that needs to produce a visual asset calls this skill — do not call the API directly outside this skill.

## Prerequisites

- `OPENAI_API_KEY` set in `.env` at the project root
- `curl` available in the shell
- `jq` **or** `python3` available for base64 decoding

## Parameters (caller must supply)

| Parameter | Description |
|---|---|
| `prompt` | The full image generation prompt (string) |
| `output_path` | Destination file path ending in `.png` (e.g. `yuval/outputs/2026-05-06-my-image.png`) |

---

## Step 1 — Load API Key

```bash
# Load from .env if not already in environment
if [ -z "$OPENAI_API_KEY" ]; then
  export $(grep -v '^#' .env | grep OPENAI_API_KEY | xargs)
fi
```

---

## Step 2a — Generate + Decode (primary: jq)

```bash
curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"model\": \"gpt-image-2\",
    \"prompt\": \"<the prompt>\",
    \"size\": \"1024x1024\",
    \"quality\": \"medium\",
    \"output_format\": \"png\"
  }" | jq -r '.data[0].b64_json' | base64 --decode > "<output-path>.png"
```

Replace `<the prompt>` and `<output-path>` with actual values before running.

---

## Step 2b — Generate + Decode (fallback: python3, use when jq is unavailable)

```bash
curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"model\": \"gpt-image-2\",
    \"prompt\": \"<the prompt>\",
    \"size\": \"1024x1024\",
    \"quality\": \"medium\",
    \"output_format\": \"png\"
  }" | python3 -c "
import json, base64, sys
data = json.load(sys.stdin)
b64 = data['data'][0]['b64_json']
with open('$OUTPUT_PATH', 'wb') as f:
    f.write(base64.b64decode(b64))
"
```

Where `$OUTPUT_PATH` is the desired output file path set before running.

---

## Step 3 — Verify Output

```bash
[ -s "<output-path>.png" ] && echo "OK" || echo "FAILED: file missing or empty"
```

A non-empty `.png` file at the output path = success.

---

## Error Handling

| Situation | Action |
|---|---|
| `jq` not found | Switch to python3 fallback (Step 2b) |
| File missing or size 0 after decode | Retry once with a simplified version of the prompt |
| API returns non-200 or error JSON | Log the error body; do not retry auth errors (401, 403) |
| `OPENAI_API_KEY` is empty | Stop immediately; report that the key is missing from `.env` |
