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
