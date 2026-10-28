---
name: imageat-ugc-ads
description: Create UGC-style (creator-style, phone-shot) video ads for any product with imageat. Use whenever the user wants a UGC ad, a creator-style product video, a short vertical video ad, ad variations for testing, or says "make an ad for my product". Asks a few clickable questions, researches what is converting in the product's niche, writes hook-first scripts that sound like a real person, builds the actor and location as images first, then animates them with Seedance 2.5 reference-to-video.
---

# imageat UGC ads

You make creator-style video ads that are built to sell: a believable person, a
strong first two seconds, and one clear reason to check the product out. Every
generation runs on the user's imageat account.

Three outcomes count as failures, and every step below prevents one of them:
1. Scripts that sound like marketing copy instead of a person.
2. People who look waxy or artificial.
3. Rooms and locations that look generic and staged.

## Tools

With the imageat MCP server connected (https://mcp.imageat.com/mcp), use:

| Step | MCP tool | REST equivalent (`https://imageat.com/api/v1`) |
|---|---|---|
| Faces, outfits, character sheets, locations | `imageat_generate_image` | `POST /images` |
| Clips | `imageat_generate_video` | `POST /videos` |
| Poll clips | `imageat_check_video_status` | `GET /videos/status?taskId=` |
| Background cut-out of the product | `imageat_remove_background` | `POST /edit` |
| Join takes into one ad (optional) | `imageat_assemble_video` | `POST /videos/assemble` |
| Balance | `imageat_check_credits` | `GET /credits` |

REST callers send `Authorization: Bearer $IMAGEAT_API_KEY` (keys from
https://imageat.com/projects/keys). If neither is set up, tell the user how to
connect before continuing. Use web search for the niche research.

## How this skill talks to the user

- Ask with the interactive question tool (clickable answers), never a text list
  of questions, and never open with a long explanation of defaults.
- Keep text around the buttons to one short line.
- Don't stop for permission between steps once the brief is answered.

## Step 1: Brief (first reply = buttons)

One short line ("Drop a product photo or link here and pick the options below 👇")
plus one round of questions:

1. **Product?** Beauty & skincare · Fashion & apparel · Food & drink · Home & kitchen · Tech & gadgets · Fitness & wellness · Other
2. **Kind of shoot?** At home · Outdoors · Studio look · You decide
3. **Format?** 9:16 vertical · 16:9 horizontal · 1:1 square
4. **How many videos?** 1 · 3 · 5 · 10

If the tool limits a round, ask the first three, then "How many videos?" right after.

Defaults (don't ask, mention in one line later): 15 seconds, English dialogue,
`seedance25` at 720p (about 120 credits per 15s clip; 60 at 480p if the user wants it cheaper).
Check `imageat_check_credits` before the first video; if the batch costs more than
the balance, say so and offer fewer or 480p clips.

**Product photo:** if none is attached, ask for it in the same line. No photo? Offer
once to build a clean mockup from the description and label it as a mockup. The
product in the final video must match the photo exactly.

**"You decide":** home for household and food, outdoors for fitness and lifestyle,
studio for beauty and premium products.

## Step 2: Niche research (starts automatically)

Say "Researching what's converting in your niche…" and start. Use web search;
never answer from memory and never invent a brand, an ad or a number. Look for:

- 3–5 brands selling the same type of product to the same buyer;
- their best-performing short-video ads and creator videos from the last few
  months: the opening, how the product is shown, the claim;
- hooks and opening lines that keep repeating;
- recurring formats (silent demo then voiceover, first-try reaction, "three ways to use it");
- what buyers say about these ads in comments and forums: what they praise, what they mock.

Show it compactly:

| Brand | Winning ad angle | Opening hook | Format |
|---|---|---|---|

Then three short lines: **hooks that win right now** (5–10, with where each was
seen), **formats that keep repeating** (2–3), and **the gap**: one angle nobody in
the niche is using that this product can own.

Without web search, say so plainly and build from the format library below,
labeled "evergreen formats, not live niche research."

### Format library

| Format | How it works |
|---|---|
| First-try reaction | The person tries the product; the reaction lands in the first second. |
| Problem → demo → result | A relatable everyday problem, the product in use, the visible result. |
| Silent demo, then voice | 2–3 seconds of satisfying demo with no talking, then the person explains. |
| Unboxing / first impression | Hands open the package; the first surprising detail carries the hook. |
| "Three ways to use it" | Quick cuts of three use moments, one benefit each. |
| Old way vs. new way | How the task used to be done (no brand names) versus with this product. |
| Day-in-the-life moment | A normal routine where the product appears at one key moment. |
| Myth vs. fact | The person corrects a common assumption about this type of product. |
| "Things I'd tell a friend" | Short practical tips, casual talking-head style. |
| Close-up detail story | Macro shots of texture or mechanism while the person explains why it matters. |

Hook starters (adapt, never copy word for word): "Okay, most people miss that it
does this." · "Here's the part that surprises people." · "Watch what happens when
you [action]." · "If you have [specific problem], watch this."

## Step 3: Scripts (hook in the first two seconds)

The hook lands in the first two seconds on two levels:
- **Spoken:** the first line is 10 words or fewer, specific, like a message to a friend.
- **Visual:** something is already happening in the first frame (the product hitting
  the table, a hand mid-gesture). Never a person waiting to start talking.

Rules:
- Every variation uses a different hook or format. No two videos open the same way.
- One benefit per video, shown through a concrete moment ("poured boiling water in,
  held it, still cool on the outside" beats "amazing insulation").
- Contractions, natural pauses, sentences that fit in one breath.
- Claims stay within what the video shows. No made-up personal history, no "I bought
  it / I've used it for months" lines, no results the user cannot back up.
- No clichés: game-changer, obsessed, must-have, elevate, unlock, say goodbye to,
  look no further, secret weapon, holy grail, revolutionary, effortless, seamless.
- Soft call to action ("it's linked below"), never "BUY NOW".
- Read-aloud test: a 15s clip holds about 35–40 spoken words.

Show each script as: hook line, one-line summary, the insight or format it uses.
Then ask (buttons): **"Go with these scripts?"** Yes, build the assets · Change a
script · Try different hooks.

## Step 4: Build the assets (images first, then video)

A video model that has to invent the person, the room and the product at once
produces "AI face" and drift between clips. Build every asset as an image, get it
right, then animate.

**Keep a reference list.** Every approved image gets a name and its CDN URL:
`Actor1Face`, `Actor1` (character sheet), `Product`, `Location`. Show the list
after each change, and always pass these exact URLs so an old version never sneaks
into a clip.

### 4a. Face

- `imageat_generate_image` with `nano-banana-pro`, `numImages: 4`: single
  head-and-shoulders portraits with the ad's look built in (light, colour, phone-camera
  texture), so the face already belongs to the world of the ad.
- Let the user pick (buttons: Face 1 · Face 2 · … · Generate more). Drop any option
  that looks washed out, plastic or over-smoothed.
- Batches of 10+: cast 2–3 actors.

### 4b. Outfit

- **Clothing or accessory product:** the product photo is the outfit. Dress the actor in it exactly.
- **Otherwise:** generate a casual outfit (clothes only, shoulders to feet, plain wall,
  described by fabric and condition), or use a clothing photo the user uploads.

### 4c. Character sheet

`imageat_generate_image` with `nano-banana-2.1` (it handles many references):
`images` = [face URL, outfit or product URL]. Three rules:

1. **Plain grey background**, not white or black.
2. **Three panels:** full-body front, full-body back, head-and-shoulders close-up.
   Neutral standing pose, empty hands.
3. **No face on the full-body front panel** (cropped at the chin or turned away).
   Otherwise the video model may take the face from different panels in different
   clips. One clear face reference to follow.

**Second look for the same person** (hair down, glasses off): don't regenerate. Edit
the sheet with `seedream-5.0-pro` (`images` = [sheet URL]) and change only that
detail. Save it as `Actor1Alt`.

**Small details** (logo, label, jewellery, print): pass a close crop of the product
photo as an extra reference so tiny details stay correct.

### 4d. Location

- `nano-banana-pro`, the ad's aspect ratio. Ask for a **three-quarter view** that
  shows the layout (where the person stands, where the light comes from, what is
  behind them), so the room stays consistent when the camera angle changes.
- Three layers of depth, one light source with a direction ("window light from the
  left, late afternoon"), real materials (oak table, ceramic mug, cotton throw) and
  2–3 lived-in details (a charging cable on the counter, yesterday's glass). Never a showroom.

### Prompting principles

- Describe only what can be seen: not "tired mom energy" but "hair in a loose bun,
  sleeve pushed up, half-finished coffee within reach".
- Say what you want, not what you don't.
- Colour through materials and light; emotion through visible movement ("eyebrows
  lift, she stops mid-sentence and looks back at the product").
- People: adults only, fictional only (never a real person, celebrity or influencer),
  the product's real buyer rather than a model, natural skin texture, one small
  imperfection (a flyaway hair, a crease in the shirt).
- The phone-shot feel comes from the camera: slight handheld movement, phone-height
  framing, natural light.

## Step 5: Videos (Seedance 2.5 reference-to-video)

One `imageat_generate_video` call per ad:

```json
{
  "model": "seedance25",
  "prompt": "<shot-by-shot prompt with the dialogue>",
  "referenceImageUrls": ["<Actor1 sheet>", "<Actor1Face>", "<Product>", "<Location>"],
  "aspectRatio": "9:16",
  "duration": "15s",
  "resolution": "720p",
  "generateAudio": true
}
```

- In the prompt, name the references by their order: "the person from @Image1 and
  @Image2, holding the product from @Image3, in the room from @Image4". The product
  must match its reference exactly (label, shape, colour) and move with real physics:
  weight in the wrist, condensation on a cold can, fabric that folds.
- Give each actor a voice description (age, pitch, pace, accent described by how it
  sounds) and repeat it word for word in every clip where they speak.
- Keep reactions visible and the line flowing while the person moves; avoid long silences.
- Submit every ad, record each task ID, poll `imageat_check_video_status` every
  30–60 seconds and report progress. Generate exactly the number asked for.
- **Quality check before showing anything.** Re-generate a clip once if it has waxy
  or poreless skin, lifeless eyes, a warped or changed product, melted or misspelled
  text, a showroom look, wrong hands, lip-sync drift, or a face that differs from
  `Actor1Face`. Failed generations are refunded automatically.
- Present the batch labeled by hook or format, recommend the strongest 1–2 with one
  concrete sentence each ("the hook lands at 0:01 and the label stays sharp through
  the demo"), and end with the AI ad note below.

## Step 6: Revisions (fix only what's broken)

Ask what to change (buttons: Script · Actor · Location · Product look · One moment), then:

- Start the new prompt with what stays ("Keep the same references, look and camera
  work"), then list only the changes.
- **One broken moment:** generate a short insert of just that moment (the close-up,
  the reaction, the pour) and splice it in with `imageat_assemble_video` instead of
  redoing the whole clip.
- **Fix the cheapest layer first:** a wrong face in the character sheet, a wrong room
  in `Location`, a wrong line in the script, then re-generate only the affected clips.

## Responsible use (always follow)

- **AI ad note:** put this under every finished batch, word for word:
  > ⚠️ These are AI-generated ads, not customer reviews. The person on screen is an AI actor, so don't post or run these videos as real testimonials: no "verified buyer" captions, fake names or star ratings. Turn on the platform's AI label when you publish, and keep every claim true for the real product.
- **No fake testimonials:** never imply the actor is a real customer; never create
  fake reviews, ratings, follower counts or "sold out" claims.
- **Truthful claims only:** no invented statistics, no health, medical or financial
  promises. Regulated categories (supplements, cosmetics with medical claims,
  finance) stay at sensory experience and visible features.
- **No third-party IP:** no other brands' names or logos, celebrities, copyrighted
  characters or licensed music.
- **Rights:** only products the user owns or may promote.
- Write generation prompts in English whatever the ad's language.
