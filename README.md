# StoryKernel StoryBlueprint (Dev)

This profile connects to Pickford dev. Sign in with any registered dev account with verified email and accepted terms; a creator role is not required. Your production account and saved stories are separate.

Develop a story idea in a short conversation, then build a StoryBundle and watch it play. Install the plugin, sign in with Pickford in your browser, and run the **create-a-story** prompt (or say “Help me turn my idea into an interactive StoryKernel show”). The assistant guides the whole path, from the interview to your show playing. Your saved drafts follow your Pickford account across connected tools.

This is a release candidate. The package is prepared for publication; a public marketplace address and a deployed service must be verified before customer installation is enabled. No public directory listing is implied by this package.

## Install the plugin

Use the marketplace address supplied by Pickford in place of `https://github.com/PickfordAI/storykernel-plugins.git` below. The client downloads this small plugin automatically. You do not need a Pickford source checkout, Python, or a manually copied credential.

For Codex, register the marketplace from its terminal:

```text
codex plugin marketplace add https://github.com/PickfordAI/storykernel-plugins.git --ref dev
```

Then open the app's Plugins directory, select **Pickford Dev**, and install **StoryKernel StoryBlueprint (Dev)**. Complete the Pickford sign-in prompt and start a new task. On clients that provide the installation command, `codex plugin add storykernel-story-blueprint@pickford-dev` also installs it.

For Claude Code, run these commands in a Claude Code session:

```text
/plugin marketplace add https://github.com/PickfordAI/storykernel-plugins.git#dev
/plugin install storykernel-story-blueprint@pickford-dev
/reload-plugins
/mcp
```

In `/mcp`, select StoryKernel's authentication action and complete the browser sign-in. Start with `/storykernel-story-blueprint:storykernel-story-blueprint`, or ask to create an interactive StoryKernel show. There is no separate `claude mcp login` command.

Your client may ask you to approve connecting to the service or using a tool. Pickford's consent screen identifies the client and requested access. To disconnect, revoke the connection in Pickford's account integrations page; you can also remove the plugin from your client.

## Resume and create

A sentence is enough for each opening answer. The plugin saves progress as you develop the story and can resume your recent drafts. If a draft was edited in another conversation, it retrieves that version before applying your changes. If creation is interrupted, resume the same draft to recover its result.

A successful build returns your real StoryBundle identifiers. Draft creation and rendered playback are separate stages; the result explains any remaining setup.

## Play your StoryBundle

After the build, the assistant checks your StoryBundle's state for you. `ready` means publication and any required preparation are ready. `preparing` means Pickford is preparing images and compositions for every playable variant; the assistant reports progress and waits. `blocked` includes the owner's failure and inspection link. The assistant distinguishes fixing invalid inputs from retrying failed preparation and keeps your existing saved session. Preparation time varies.

Once the StoryBundle is ready, Pickford plays it for you. Open [your stories](https://dev.pickford.ai/my-stories), pick the show, and press **Play as video**. Pickford runs the video renderer on its own machines: there is nothing to install and no fal.ai account to open.

Pickford serves a small number of shows at the same time right now. If every slot is busy, the page says so and asks you to try again in a few minutes; the assistant relays that and waits with you rather than treating it as a failure.

For a publication failure, the assistant preserves successful work when retrying. Moderation failures require your approval of revised image descriptions before a new build.

## Run the renderer yourself (optional)

If you would rather render on your own machine, the Pickford video renderer is open source and that path still works. The assistant walks you through it and can run the commands: you need Node.js 22 or newer, FFmpeg, and Docker, then it clones [video-renderer](https://github.com/PickfordAI/video-renderer), builds it, starts the media relay, and runs it. You then open `http://localhost:4174`, sign in with Pickford on that page, enter your own fal.ai API key there, pick a StoryBundle, and press Play. If your H3 viewer is already running and signed in, reuse it.

Your fal.ai API key is yours: you enter it only on the renderer's own local page, where it stays. It never reaches Pickford and never reaches the assistant, which will not ask you for it.

## Connect the tools without the skill

The remote service uses Streamable HTTP at `https://api.dev.pickford.ai/storykernel/mcp`. OAuth sign-in is discovered by the client. These advanced alternatives add the tools alone; install the plugin above for the guided creative workflow.

Codex:

```text
codex mcp add storykernel --url https://api.dev.pickford.ai/storykernel/mcp
codex mcp login storykernel
```

Claude Code:

```text
claude mcp add --transport http --scope user storykernel https://api.dev.pickford.ai/storykernel/mcp
```

Then run `/mcp` inside Claude Code to sign in. Use either the plugin connection or the direct connection to avoid duplicate tool entries.

## Package contents

Both hosts use the same creative instructions and remote service. This package contains host manifests, their marketplace catalogs, the StoryBlueprint skill, and `release-manifest.json` with SHA-256 content hashes. It contains no executable server, installation hooks, backend source, or credentials.

Installation follows the [OpenAI plugin packaging documentation](https://developers.openai.com/plugins/build/plugins), [Codex MCP documentation](https://developers.openai.com/codex/mcp), [Claude Code marketplace guide](https://code.claude.com/docs/en/plugin-marketplaces), and [Claude Code MCP guide](https://code.claude.com/docs/en/mcp).

## Replace the retired onboarding plugin

If you installed `storykernel-onboarding`, remove that old plugin before installing
`storykernel-story-blueprint@pickford-dev`. Existing installs keep their cached
name; refreshing a marketplace alone does not rename an installed plugin.

Codex:

```text
codex plugin remove storykernel-onboarding@pickford-dev
codex plugin marketplace add https://github.com/PickfordAI/storykernel-plugins.git --ref dev
codex plugin add storykernel-story-blueprint@pickford-dev
```

Claude Code:

```text
/plugin uninstall storykernel-onboarding@pickford-dev
/plugin marketplace add https://github.com/PickfordAI/storykernel-plugins.git#dev
/plugin install storykernel-story-blueprint@pickford-dev
/reload-plugins
/mcp
```

If the old install belongs to another marketplace, use its actual marketplace
name when removing it. Reinstall the new plugin, reconnect through browser OAuth,
and start a new session. If the renamed plugin is already installed but stale,
remove `storykernel-story-blueprint@pickford-dev` and reinstall it the same way.
