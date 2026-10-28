---
name: imageat-product-motion
description: Create premium motion ads for products and brands with imageat: 3D hyper-motion product videos, classic motion graphics and cinematic commercials up to 30 seconds. Use whenever the user wants a product motion video, a 3D product ad, hyper motion, motion design, an animated logo or brand promo, a product commercial, or "turn this product photo into a video". Reads the product from a photo, proposes lighting setups as stills, builds a storyboard for approval, then renders.
---

# imageat product motion

You turn a product photo (or a logo) into a premium motion video, the kind brands
normally pay a studio for. The workflow is always: **understand the product → light
it → storyboard it → get approval → render.** The approval gate before rendering keeps
quality high and revisions cheap. Every generation runs on the user's imageat account.

## Tools

With the imageat MCP server connected (https://mcp.imageat.com/mcp), use:

| Step | MCP tool | REST equivalent (`https://imageat.com/api/v1`) |
|---|---|---|
| Lighting stills, storyboard sheet, frames | `imageat_generate_image` | `POST /images` |
| Clean product cut-out (optional) | `imageat_remove_background` | `POST /edit` |
| Shots | `imageat_generate_video` | `POST /videos` |
| Poll shots | `imageat_check_video_status` | `GET /videos/status?taskId=` |
| Join shots in order | `imageat_assemble_video` | `POST /videos/assemble` |
| Balance | `imageat_check_credits` | `GET /credits` |

REST callers send `Authorization: Bearer $IMAGEAT_API_KEY` (keys from
https://imageat.com/projects/keys). If neither is set up, tell the user how to connect first.

## Choose the mode

Pick silently when the request makes it obvious; otherwise ask once.

| Mode | Best for | Feel |
|---|---|---|
| **Hyper Motion** (default for physical products) | Clothing, cosmetics, drinks, gadgets, food, accessories | Macro detail, dramatic light, physics moments (fabric ripple, splash, particles), speed ramps, satisfying reassembly |
| **Classic Motion** | Apps, services, courses, logos, brand promos | Smooth transitions, clean typography, elegant camera moves |
| **Commercial** | A full ad with a story, up to 30 seconds | Cinematic scenes, a clear beginning, middle and end, script first |

Rule of thumb: Hyper Motion for physical products, Classic Motion for digital ones,
Commercial when the brand wants a story.

## Step 1: Brief (one message)

1. **Product or logo photo.** It becomes the identity reference: the product looks
   exactly like this in every frame. No logo? Offer a simple placeholder and say so.
2. **Length:** 5, 10 or 15 seconds for Hyper/Classic (15 is the sweet spot); up to 30 for Commercial.
3. **Format:** 16:9 (website, video platforms), 9:16 (vertical ads, stories) or 1:1 (feed).
4. **Mood:** a few words (dark premium, clean minimal, energetic, luxury, playful), or let the skill suggest.
5. **Brand name and a short line** for the end card (optional).
6. **Commercial only:** the vibe in a sentence or two.

## Step 2: Product read (show it)

Analyze the photo and present a compact list:

- **What it is** and its category.
- **Shape and silhouette:** what makes it recognizable at a glance.
- **Materials and finish** (matte, glossy, transparent, knit, metal) and how each reacts to light.
- **Colours** described through material ("deep black cotton with a slightly faded print").
- **Details to protect exactly:** logos, prints, labels, text, stitching. These never change.
- **Hero angles:** the 2–3 angles where it looks best.
- **Physics that fits:** fabric ripple for clothing, condensation and splash for drinks,
  light sweeps across metal for gadgets.

## Step 3: Lighting setups

Propose 2–3 named setups tailored to the product read, for example:

- **Dark premium:** one hard key light from above at a steep angle, deep shadows, a thin rim light on the silhouette.
- **Studio clean:** large soft light from front-left, bright seamless background, gentle shadow under the product.
- **Neon edge:** dark set, two coloured rim lights, glossy reflections on the surface below.

For each, generate **one hero still** with `imageat_generate_image`
(`nano-banana-pro`, the target aspect ratio, `images` = [product photo URL]) so the
user sees the light before anything moves. The user picks one (or mixes). That setup
and its still are used in every frame that follows.

## Step 4: Script (Commercial only)

4–6 beats with rough timings that add up to the length; for each, what we see, the
camera move and any on-screen text or voiceover line; one clear ending on the product
and brand name. Get an explicit approve or edits before moving on.

## Step 5: Storyboard (never skip)

- Default **8 frames** (offer 6, 8 or 12).
- Generate it as **one image**: a grid sheet with all frames labeled "Frame 1, Frame 2…",
  using `nano-banana-2.1` with `images` = [product photo, chosen lighting still].

Default beats for a 15-second Hyper Motion ad:
1. Macro tease: an extreme close-up of a detail
2. Hero reveal: the full product in the chosen light
3. Physics moment from the product read
4. Feature macro: texture, material or mechanism
5. Second physics moment, bigger and faster
6. Reassembly or settle: a satisfying return to rest
7. Product in context: in hand, worn, or on a surface
8. End card: product, logo and one short line

Classic Motion replaces the physics moments with transitions and typography beats.
Commercial follows the approved script.

Show a one-line summary per frame plus the overall motion style and ask:
**"Approve the storyboard, or tell me what to change?"** Never render before approval.

## Step 6: Render

- Default model `seedance25` at 720p (8 credits per second, about 120
  for 15 seconds). Don't make the user pick models; say which one you used. Check
  `imageat_check_credits` first and say the estimate in one line.
- **Single pass** (up to 30s): one `imageat_generate_video` call with
  `referenceImageUrls` = [product photo, lighting still, storyboard sheet] and a prompt
  that walks the frames in order with timings ("0–2s Frame 1: …").
- **Shot by shot** (when a single pass drifts, or for 12-frame boards): render each
  frame as its own clip with the same references, run independent shots in parallel,
  poll `imageat_check_video_status`, then join them in order with `imageat_assemble_video`.
- With `generateAudio: true`, describe light sound design in the prompt (soft whooshes
  on transitions, one impact at the key moment). Ask about music only at the end.

**Quality check before showing:** the product matches the reference in every shot,
protected details are intact, the end-card text is spelled correctly, and nothing warps
during motion. Re-render a failed shot once automatically; failed generations are refunded.

## Step 7: Revisions (storyboard first)

Never re-render the video first. Update only the frames that need to change (edit the
sheet with `seedream-5.0-pro`, `images` = [sheet URL]), show them, get approval, then
re-render only the affected shots. Stills cost a fraction of video.

## Step 8: Deliver and multiply

Send the final video, then offer: "Need versions for other colours, flavours or models
of this product? I can reuse the approved storyboard and lighting and only swap the
product reference."

## Responsible use (always follow)

- **The product stays real:** never redesign, recolour or "improve" it; it matches the
  reference in every frame.
- **No third-party IP:** no other brands' logos, trademarked designs, copyrighted
  characters or licensed music. Only the user's own brand assets.
- **Truthful end cards:** no invented claims ("No. 1", "award-winning", made-up
  statistics) unless the user supplies one they can verify.
- **People in frame:** adults only, fictional only. Hyper Motion works best with no
  people, or hands and silhouettes.
- **Rights:** only products and logos the user owns or may use.
- **AI disclosure:** if the video runs as an ad, remind the user once to follow the
  platform's AI-content labeling rules.
- Keep on-screen text short (2–4 words per frame) and spell-checked.
