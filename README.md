# imageat skills

Official [agent skills](https://imageat.com/skills) for [imageat](https://imageat.com):

- **imageat**: teaches Claude Code, Codex, Cursor and other agents that read `SKILL.md` to
  generate images, edit photos, create videos and check your balance through the imageat API.
- **imageat-explainer-video**: turns a topic into a finished narrated explainer video
  (researched script, style-locked clips, voiceover, subtitles, one mp4). Works best with the
  [imageat MCP server](https://imageat.com/mcp). See the
  [AI explainer video generator](https://imageat.com/ai-explainer-video-generator).

## Install

**Claude Code**

```
/plugin marketplace add incendies/imageat-skills
/plugin install imageat@imageat
```

The plugin adds both skills and the hosted [imageat MCP server](https://imageat.com/mcp)
(`https://mcp.imageat.com/mcp`). Run `/mcp`, pick **imageat** and sign in with your imageat
account; the MCP tools need no API key.

**Cursor**

Install **imageat** from the Cursor marketplace. The plugin adds both skills and the hosted
[imageat MCP server](https://imageat.com/mcp) (`https://mcp.imageat.com/mcp`); Cursor opens an
imageat sign-in the first time a tool runs, so the MCP tools need no API key.

**Codex, Gemini CLI, OpenCode and other agents**

```
npx skills add incendies/imageat-skills
```

**claude.ai and Claude Desktop**

Download [imageat.zip](https://imageat.com/skills/imageat.zip) and upload it under
Settings → Capabilities → Skills. Code execution needs network access to `imageat.com`.

**Manual**

Copy [`skills/imageat/SKILL.md`](skills/imageat/SKILL.md) into your agent's skills folder,
for example `~/.claude/skills/imageat/SKILL.md`.

## API key

Create a key at <https://imageat.com/projects/keys> and set it before you start your agent:

```
export IMAGEAT_API_KEY="iat_live_xxxxxxxxxxxx"
```

Every call is billed to your imageat account. See [API pricing](https://imageat.com/api-pricing).

## Updates

`SKILL.md` is generated from the live imageat API catalog and published at
<https://imageat.com/.well-known/agent-skills/imageat/SKILL.md>. A daily GitHub Action copies
it into this repo. In Claude Code, run `/plugin marketplace update imageat` to pull changes.

Prefer tools over a skill? Use the [imageat MCP server](https://imageat.com/mcp).

## What this plugin runs and sends

- **No local code.** The plugin ships two `SKILL.md` files and one remote MCP server entry
  (`.mcp.json`). It has no hooks, scripts, commands or bundled binaries.
- **MCP server:** Claude Code connects to `https://mcp.imageat.com/mcp` over HTTPS and signs you
  in with OAuth. Tool calls send your prompts, settings and any image or video URLs you pass to
  imageat to run the generation or edit.
- **Skills:** the skills tell Claude to call the imageat REST API at `https://imageat.com/api/v1`
  with the API key you provide in `IMAGEAT_API_KEY`. Requests contain your prompt, chosen model and
  settings, and reference image URLs.
- **Billing:** generations, edits and voiceovers use credits from your imageat account. Balance
  checks, model lists and status polls are free.
- Results come back as URLs on imageat's CDN (`cdn.imageat.com`).

## Privacy

imageat stores your prompts, generated results and media URLs in your account, and sends uploaded
media to third-party AI and storage providers to process each request. See the
[privacy policy](https://imageat.com/privacy-policy) for what is collected, how long it is kept and
who it is shared with. Questions: info@imageat.com.
