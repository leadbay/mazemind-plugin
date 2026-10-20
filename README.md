# Mazemind

Mazemind is a shared memory of public facts that AI agents read and write. It holds companies, the people who
run them and their roles, places and products. Each fact comes with the page it was read on and its date. Today it
holds mainly French companies: their people and job titles, offerings, certifications such as Qualiopi or RGE, and
the tools they use.

## Install

In the Claude app or Cowork: Customize, Plugins, search for Mazemind. In Claude Code:

```
/plugin install mazemind --marketplace leadbay/mazemind-plugin
```

## What the plugin adds

| Part | What it does |
|---|---|
| The Mazemind connector (`.mcp.json`) | connects your assistant to `https://mcp.mazemind.ai/claude`. You sign in once with Google or GitHub. |
| The `mazemind` skill | tells your assistant what Mazemind holds and which tool answers which task: `lookup` for one named company or person, `search` for a list that fits conditions, `catalog` for the facts and links it holds, `contribute` to save what a web search found, `tour` for a first walk-through. |
| The `daily-prospecting` skill | sets up a morning list of new companies that fit who you sell to, each with the person who decides there. |

The plugin runs no code on your machine and has no hooks.

## How to use it

Ask what you would ask anyway. For example:

- "Who runs Freya Games, and who heads sales there?"
- "Find RGE-certified electricians in Vaulx-en-Velin, near Lyon, with a contact at each."
- "I just connected Mazemind. What can it do for me?"

## What it sends, and where

The plugin itself sends nothing: your assistant calls Mazemind's server, `mcp.mazemind.ai`, through the connector.
Mazemind keeps its usage records with its analytics provider, PostHog, in the EU. The `daily-prospecting` skill also
has your assistant read the French company registry, `recherche-entreprises.api.gouv.fr`, with its own web tool.

- **Your requests.** Each tool call carries what the assistant is looking for. With each call, Mazemind records the
  request, the call's arguments and the start of its answer, with keys, card numbers, email
  addresses and phone numbers removed, to learn which requests its answers serve. To stop it, ask your assistant to "stop sharing usage with Mazemind": the `telemetry`
  tool turns it off for every connection of your account. Mazemind never shows another user what you asked.
- **The tour.** Mazemind records on your account that you saw or hid the tour, so it is not offered again.
- **What your assistant saves.** `contribute` sends public facts read on public pages, each with its page and date.
  They become visible to every Mazemind user. Nothing you did or plan, and nothing private, is accepted.

Details: [privacy policy](https://mazemind.ai/privacy) and [terms](https://mazemind.ai/terms).

## Support

- Questions and problems: open an issue on [this repository](https://github.com/leadbay/mazemind-plugin/issues).
- Personal data, correction or removal: privacy@mazemind.ai
- Security: security@mazemind.ai
- Abuse: safety@mazemind.ai
