# Top.gg Ad Skill

We want to make it easier for people to get an ad running on Top.gg. You should be able to explain what you're building, who it's for, and what you're comfortable spending, then get some help putting it together.

That's what this skill does. It asks a few questions, helps with the copy and image, and puts together a campaign you can review before it goes live. Once it's running, you can come back and ask what's working and what to try next.

It's an initial version. We're trying to make the whole thing feel like a useful conversation, even if you've never run an ad before.

## Get set up

You'll need Codex, Claude Code or Cursor, and your own Top.gg account. Each connects to our [MCP server](https://mcp.top.gg) to work with your ads.

Download the [ZIP file](https://github.com/top-gg/ad-skill/archive/refs/heads/main.zip) and extract it. If you prefer Git, you can clone the repository:

```sh
git clone https://github.com/top-gg/ad-skill.git
```

Copy the entire `skills/topgg-ads-advisor` folder into the skills directory for your app:

| App | Personal skills directory |
|---|---|
| Codex | `$CODEX_HOME/skills`, or `~/.codex/skills` by default |
| Claude Code | `~/.claude/skills` |
| Cursor | `~/.cursor/skills` |

Here, `~` means your home folder. On Windows, that's usually `%USERPROFILE%`. Keep `SKILL.md` and the `references` folder together. The included `agents/openai.yaml` is Codex metadata; the MCP connection is configured separately in each app.

### Codex

Connect your own Top.gg account:

```sh
codex mcp add topgg --url https://mcp.top.gg
codex mcp login topgg
```

Already connected to this endpoint as `topgg`? Skip the add command. Finish signing in through your browser. If the campaign tools still aren't available, restart Codex. There's more detail in the [MCP setup docs](https://developers.openai.com/codex/mcp).

Your account connection stays with you. There are no credentials in this repo.

### Claude Code

Add the server for your user account:

```sh
claude mcp add --transport http --scope user topgg https://mcp.top.gg
```

Open Claude Code, run `/mcp`, select Top.gg and finish authentication. Start the interview with:

```text
/topgg-ads-advisor Help me get an ad running. Ask me one question at a time.
```

[Claude Code MCP setup](https://code.claude.com/docs/en/mcp) and [skill installation](https://code.claude.com/docs/en/skills).

### Cursor

Add this entry to `~/.cursor/mcp.json`. If you already have servers configured, merge `topgg` into the existing `mcpServers` object rather than replacing the file.

```json
{
  "mcpServers": {
    "topgg": {
      "url": "https://mcp.top.gg"
    }
  }
}
```

Open Cursor's MCP settings and finish the sign-in for Top.gg when prompted. You can also put the configuration in `.cursor/mcp.json` inside a project if you only want it there. Ask Agent to use the `topgg-ads-advisor` skill once it's available.

[Cursor MCP setup](https://prod.cursor.com/help/customization/mcp) and [skill installation](https://cursor.com/docs/skills).

Claude Code and Cursor setup follows their current documentation; we haven't completed an end-to-end campaign test in those clients yet. Each client handles its own sign-in. If the skill or tools don't appear after setup, restart the client and check its MCP connection status.

## Try it

Start with:

> Use $topgg-ads-advisor to help me advertise my Discord bot. Ask me one question at a time.

Or:

- “Review my campaigns and recommend improvements.”
- “Help me make a new ad for my server, but create it paused.”
- “My campaign isn't getting clicks. What should I check?”
- “Update the headline on my campaign.”

It'll ask what you're promoting, who would enjoy it, what you want to get out of the ad, and how much you'd like to spend. If you've already answered something, it should move on. You don't need to know about bids or placements: by default, Top.gg handles those.

You can use your bot's existing icon or bring a new image. If your assistant supports image generation, you can work on an illustration together too. A custom ad needs a supported image file before it can be created.

For a practice run, say **“Walk me through it without creating anything.”** For a real campaign that should not deliver yet, say **“Create it paused.”**

## Once you've got an ad

- Change the copy or image, or switch to another ad you've made.
- Pause a campaign and resume it when you're ready.
- Adjust a campaign's daily budget or dates, and see its full settings.
- See what you've spent, how many people clicked, and how results are changing.
- Work through why an ad might not be getting much attention.
- Choose your own placements and bid if you want manual control.
- Add funds through a checkout link that you complete yourself.

It should show you what it's proposing and get your approval before making changes. Asking how your campaigns are doing doesn't mean you're asking it to change them. If several campaigns use the same ad, it should point that out before editing it.

## A few things we're still missing

- The current tools can show an existing lifetime budget, but don't offer a way to set one. A plan for five euros over a week isn't the same as an enforced five-euro limit.
- A reported conversion doesn't tell us whether someone kept using your bot. We don't have retention or revenue data in these tools yet.
- Changing an ad and comparing the results isn't a randomized A/B test. There's no built-in split-test tool here.
- If you create something paused and come back later, check its dates before switching it on.

Extending the end date of an active campaign that had ended can make it serve again immediately. A paused campaign stays paused when its dates change.

The skill checks what the server actually supports. If something isn't possible, it should explain the gap instead of quietly creating a replacement campaign.

## What's in here

- [Skill entry point](skills/topgg-ads-advisor/SKILL.md)
- [Guided advertiser interview](skills/topgg-ads-advisor/references/advertiser-interview.md)
- [Integration and maintenance notes](skills/topgg-ads-advisor/references/mcp-contract.md)

The interview file also has some practice scenarios for checking how the flow feels without touching real campaigns. These are walkthroughs, not automated tests.

## Feedback

Try it and tell us where it gets awkward. Did it ask too many questions? Miss something obvious? Suggest an ad that didn't sound like you?

Open an issue with what happened and what you expected. A short example helps; just leave out tokens, payment details, signed upload links, and private account information.

## License

[MIT](LICENSE).
