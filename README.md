<img src="assets/icon.png" alt="Spacery" width="72" height="72">

# Spacery MCP server and agent skill

[Spacery](https://spacery.dev) hosts 3D captures as virtual spaces with guided tours, shared with a link.
This repository holds the Spacery connector (an MCP server) and agent skill, so your agent can publish
captures for you and build tours and scavenger hunts in them.

[![Spacery MCP server on Glama](https://glama.ai/mcp/servers/lukehollis/spacery/badges/score.svg)](https://glama.ai/mcp/servers/lukehollis/spacery)

- Publish E57 and other laser scans, Matterport exports, 3D Gaussian splats, 360 photos and video, meshes
  and photo sets as spaces, straight from your computer.
- Build guided tours and scavenger hunts in your own spaces or Spacery's public ones (museums, temples,
  tombs, gardens). Spacery's tour agent writes the stops and places 3D models, effects, sound, looks
  (line drawing, blueprint, film noir, night vision) and skies (a sunset, a storm, the Milky Way).
- Your account links through a code you approve in your browser. Payment goes through Stripe Checkout
  in your browser. The connector never sees a password or a card.

Live demos: [an observatory from a Matterport E57](https://app.spacery.dev/s/bc8d61ff42cf/loomis-michael-telescope-observatory)
and [a garden as 3D Gaussian splats](https://app.spacery.dev/?demo=garden).
Free to try. Hosting from $5 a month ([pricing](https://app.spacery.dev/pricing)).

## Two connectors

| | Local connector | Hosted connector |
|---|---|---|
| Runs | on your computer, over stdio | on Spacery, over Streamable HTTP |
| Address | `npx -y https://spacery.dev/agents/spacery-0.2.3.tgz` | `https://app.spacery.dev/mcp` |
| Reads files on your computer | yes, and uploads them in the background with resume | no, it takes files the agent holds and gives you a page to drop the rest on |
| Use it in | Claude Code, Claude Desktop, Codex, Grok Build, Antigravity, Gemini CLI, Cursor | claude.ai, ChatGPT, grok.com, Meta Muse and other apps that take a connector URL |

The local connector is one readable JavaScript file with no dependencies. It needs Node.js 18 or later.

## Add it

Claude Code, as a plugin with the connector and the skill.

```bash
claude plugin marketplace add lukehollis/spacery
claude plugin install spacery@spacery
```

Or the connector alone.

```bash
claude mcp add --scope user spacery -- npx -y https://spacery.dev/agents/spacery-0.2.3.tgz
```

Claude Desktop. Download [spacery.mcpb](https://spacery.dev/agents/spacery.mcpb) and open it.

Codex.

```bash
codex mcp add spacery -- npx -y https://spacery.dev/agents/spacery-0.2.3.tgz
```

Gemini CLI.

```bash
gemini extensions install https://github.com/lukehollis/spacery
```

Grok Build.

```bash
grok mcp add spacery -- npx -y https://spacery.dev/agents/spacery-0.2.3.tgz
```

Antigravity.

```bash
agy mcp add spacery -- npx -y https://spacery.dev/agents/spacery-0.2.3.tgz
```

Cursor and any other agent that reads an `mcpServers` config.

```json
{
  "mcpServers": {
    "spacery": { "command": "npx", "args": ["-y", "https://spacery.dev/agents/spacery-0.2.3.tgz"] }
  }
}
```

claude.ai, ChatGPT, grok.com, Meta Muse and other apps in the cloud. Add a custom connector with the URL
`https://app.spacery.dev/mcp`.

## The skill

[`skills/spacery/SKILL.md`](skills/spacery/SKILL.md) teaches an agent the publishing and tour steps. Add it with

```bash
npx skills add lukehollis/spacery
```

or save it as `spacery/SKILL.md` in your agent's skills folder (`~/.claude/skills`, `~/.agents/skills`
for Codex, `~/.grok/skills`, `~/.gemini/config/skills` for Antigravity).

## Tools

| Tool | What it does |
|---|---|
| `check_files` | Looks at capture files and folders on this computer (local connector) |
| `link_account` | Links the agent to your Spacery account through a code you approve in the browser |
| `list_plans` | Shows the ways to pay for hosting |
| `create_space` | Creates a space and opens Stripe Checkout when hosting needs payment |
| `wait_for_payment` | Waits for Checkout to finish |
| `upload_files` | Uploads a capture in the background and submits it for processing |
| `finish_upload` | Submits files the agent uploaded itself (hosted connector) |
| `space_status` | Shows upload and processing progress |
| `set_visibility` | Makes a ready space public or private |
| `find_tour_spaces` | Finds your spaces and public spaces to build a tour or scavenger hunt on |
| `create_tour` | Starts a guided tour or scavenger hunt on a space |
| `draft_tour` | Asks Spacery's tour agent to write the tour, with models, effects, sound, looks and skies |
| `wait_for_tour` | Waits for a draft and summarizes the tour |
| `get_tour`, `save_tour` | Shows a tour with everything it can use, and saves one edited by hand |
| `search_models` | Searches the library of ready-made 3D models |
| `upload_model`, `upload_sky` | Adds your own `.glb` model or 360 sky image to a tour (local connector) |
| `list_tours`, `share_tour` | Lists your tours, and shares one by link or makes it private again |
| `unlink_account` | Unlinks the agent |

## Try it

Ask your agent

- "Publish the scan in ~/Captures/studio as a Spacery space."
- "Make a scavenger hunt for five artifacts in a Spacery museum, ending in a line drawing."
- "Model a bronze lantern in Blender and put it by the altar in my temple tour."

## More

- Setup for agents: [spacery.dev/agents.md](https://spacery.dev/agents.md)
- The open source viewer behind Spacery, which you can host yourself: [lukehollis/sphr](https://github.com/lukehollis/sphr)
- [Privacy](https://app.spacery.dev/privacy) and [terms](https://app.spacery.dev/terms)

MIT licensed.
