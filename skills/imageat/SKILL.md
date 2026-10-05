---
name: imageat
description: Generate AI images and videos with the imageat API. Use when the user asks to create, edit or animate images, remove a background, generate a video from text or a photo, or check their imageat balance.
---

# imageat

imageat generates images and videos from prompts and reference images. Every call is
billed to the user's imageat account, so confirm before generating many outputs or
long videos.

## Setup

- The user creates an API key at https://imageat.com/projects/keys (keys start with
  `iat_live_`). Read it from the `IMAGEAT_API_KEY` environment variable; never print it.
  If it is not set, ask the user to set it (or paste a key) before calling the API.
- Base URL: `https://imageat.com/api/v1`
- Send `Authorization: Bearer $IMAGEAT_API_KEY` and `Content-Type: application/json`.
- Send an `Idempotency-Key` header (any unique string) on POSTs you might retry.
- MCP clients can connect to https://mcp.imageat.com/mcp instead (OAuth or the same key).

## Generate an image (synchronous)

```bash
curl -X POST "https://imageat.com/api/v1/images" \
  -H "Authorization: Bearer $IMAGEAT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"nano-banana-2","prompt":"A cinematic product photo on sculpted stone","aspectRatio":"1:1","resolution":"1K","numImages":1}'
```

The response contains `url` (one image) or `urls` (several), hosted on cdn.imageat.com.

Models (live list: `GET https://imageat.com/api/v1/models/images`):

- `nano-banana-pro` (Nano Banana Pro): High-fidelity generation and editing with up to 4K output. From $0.40 / image.
- `nano-banana-2` (Nano Banana 2): Fast, cost-efficient generation for iterative creative work. From $0.16 / image.
- `gpt-image-2.5` (GPT Image 2.5): OpenAI's newest image model: five quality tiers, scoped edits, and transparent cut-outs. Add gpt-image-2.5-sunburst for the slower, finer-detail variant. From $0.02 / image.
- `gpt-image-2` (GPT Image 2): Precise instruction following with selectable quality tiers. From $0.02 / image.
- `seedream-5.0-pro` (Seedream 5 Pro): High-fidelity ByteDance generation and reference-guided editing. From $0.14 / image.

- GPT Image models accept `quality`; GPT Image 2.5 also takes `xhigh`/`max` and
  `transparentBackground` (needs png or webp output).
- Editing / reference-guided generation: pass public URLs or base64 data URLs in `images`.
- Other fields: `aspectRatio`, `resolution` (1K, 2K, 4K), `outputFormat` (png, jpeg, webp), `numImages`.

## Edit an image

`GET https://imageat.com/api/v1/edit/features` lists the edit features (id, name, credit cost): background
removal and replacement, object eraser, inpainting, relight, virtual try-on and more.

```bash
curl -X POST "https://imageat.com/api/v1/edit" \
  -H "Authorization: Bearer $IMAGEAT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"feature":"ideogram-remove-background","imageUrl":"https://example.com/photo.jpg"}'
```

Body: `feature`, `imageUrl` (public URL or base64 data URL), plus `prompt`, `maskUrl`,
`backgroundPrompt` or `backgroundImageUrl` when the feature needs them.

## Generate a video (asynchronous)

```bash
curl -X POST "https://imageat.com/api/v1/videos" \
  -H "Authorization: Bearer $IMAGEAT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"seedance25","providerVariant":"more","prompt":"A paper airplane glides through a softly lit studio","duration":"5s","resolution":"720p","aspectRatio":"adaptive"}'
```

The response has `status: pending`, `taskId` and `generationId`. Poll every 5-10 seconds:

```bash
curl "https://imageat.com/api/v1/videos/status?taskId=TASK_ID&generationId=GENERATION_ID" \
  -H "Authorization: Bearer $IMAGEAT_API_KEY"
```

Stop when `status` is `completed` (read `url`) or `failed`. Cancel with `POST https://imageat.com/api/v1/videos/cancel` and body
`{"taskId":"TASK_ID","generationId":"GENERATION_ID"}`.

Models (live list: `GET https://imageat.com/api/v1/models/videos`):

- `minimax-h3-max` (MiniMax H3 Max): Text, image, or first/last-frame video; 5–15s · 480p/768p; From $0.25.
- `seedance25` (Seedance 2.5): Text, image, reference, or frame-controlled video; 4–30s · 480p/720p; From $0.83.
- `pixverse` (Pixverse V6): Text/image video with optional audio; 1–15s · up to 1080p; From $0.09/sec.

- Image-to-video: pass `imageUrl` (first frame), or `firstFrameUrl` + `lastFrameUrl`.

## Check balance

`GET https://imageat.com/api/v1/credits` returns the account's API balance in USD (and spendable web
credits for subscribers).

## Errors

- `401`: missing or invalid key. Ask the user for a valid key.
- `402`: not enough balance (`balance` and `required` in the body). Send the user to
  https://imageat.com/api-pricing; do not retry.
- `429`: rate limited. Wait for `Retry-After` seconds, then retry.
- `500`: retry once with the same `Idempotency-Key`.

## Reference

- Full API docs (markdown): https://imageat.com/docs/api/md
- OpenAPI: https://imageat.com/openapi.yaml
- Pricing: https://imageat.com/api-pricing
