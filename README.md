# StoryKernel for Pickford Dev

Create an interactive story through a short conversation in Codex or Claude Code. This public marketplace contains the StoryKernel onboarding skill and its hosted MCP connection for the **Pickford development environment**.

The marketplace is `pickford-dev`, the plugin is `storykernel-onboarding`, and this release lives on the `dev` branch. It uses `https://api.dev.pickford.ai/storykernel/mcp`. You need a Pickford Dev account with creator access, verified email, and accepted terms. Service availability depends on the current dev deployment.

## Codex

Run in your terminal:

```sh
codex plugin marketplace add https://github.com/PickfordAI/storykernel-plugins.git --ref dev
codex plugin add storykernel-onboarding@pickford-dev
```

You can also install through the app's Plugins directory: choose **Pickford Dev**, then **StoryKernel Onboarding (Dev)**. Complete the StoryKernel browser sign-in prompt and start a fresh task. Ask: “Help me turn my idea into an interactive StoryKernel show.”

## Claude Code

In a Claude Code session:

```text
/plugin marketplace add https://github.com/PickfordAI/storykernel-plugins.git
/plugin install storykernel-onboarding@pickford-dev
/reload-plugins
/mcp
```

This repository's default branch is `dev`. In `/mcp`, select StoryKernel to authenticate, then sign in to Pickford Dev in your browser and approve access. Start with `/storykernel-onboarding:storykernel-onboarding`, or ask to create an interactive StoryKernel show.

## Keep creating

The plugin saves your evolving brief to your signed-in account. Use the same Pickford Dev account to resume from another connected app. A successful build returns the actual project, episode, and Creator Tools inspection link. Rendered playback is a separate stage; the result explains any remaining setup.

You do not need a Pickford source checkout, Python, a local server, or a manually copied token. The host handles OAuth credentials. Manage or revoke a connection in Pickford Dev under **Account → Connected tools**.

## Advanced: tools only

Direct MCP setup does not install the guided creative skill. Use it instead of the plugin connection if you only need tools.

Codex:

```sh
codex mcp add storykernel-dev --url https://api.dev.pickford.ai/storykernel/mcp
codex mcp login storykernel-dev
```

Claude Code:

```sh
claude mcp add --transport http --scope user storykernel-dev https://api.dev.pickford.ai/storykernel/mcp
```

Then open `/mcp` in Claude Code to sign in. There is no separate `claude mcp login` command.

## Contents

The package contains two host manifests, two marketplace catalogs, one shared onboarding skill with its references, the hosted HTTP configuration, and `release-manifest.json` with SHA-256 hashes. It contains no executable server, installation hooks, backend source, or credentials. This is an independently distributed marketplace; it does not imply a listing in either host's official directory.

Installation follows the [OpenAI plugin packaging documentation](https://developers.openai.com/plugins/build/plugins), [Codex MCP documentation](https://developers.openai.com/codex/mcp), [Claude Code marketplace guide](https://code.claude.com/docs/en/plugin-marketplaces), and [Claude Code MCP guide](https://code.claude.com/docs/en/mcp).
