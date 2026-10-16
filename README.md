# StorePal Skill

Let the AI building your app prepare and publish its privacy policy and terms of service. StorePal hosts each page and gives you a stable URL.

## Install it in your project’s AI

```sh
npx skills add xiao99xiao/storepal-skills
```

Use an AI that can run project commands and load Agent Skills. You can ask that AI to run the install command for you.

## Then tell your AI

> This app needs a privacy policy and terms of service. Show me the full text first. After I confirm, publish them and give me each page’s URL.

Your AI checks the project, helps you sign in or create a free StorePal account, asks for missing facts, and prepares the requested pages. After your confirmation it publishes each page and returns its URL. You do not need to create an app in the dashboard first. These requests are examples: once the Skill is loaded, describe the result you want in your own words. You do not need to name StorePal each time.

The Skill uses StorePal CLI 0.1.2 or newer. Skill installation is free, you use your existing AI, and Free includes up to three apps. No manual API key setup is needed.

## Keep using the same AI

- “We added accounts. Check and update this app’s privacy policy too.”
- “Prepare this app’s terms of service and a URL. Show me the text first.”
- “Add a feedback option in the app’s settings so users can send me a message.”
- “Show users what changed the first time they open the app after an update.”

For the feedback request, your AI adds an app entry that opens the hosted contact form. This works on Free. Messages arrive in your StorePal inbox. The AI checks the app entry and the form separately. StorePal does not run a background AI or automatically submit anything to an app store.

Terms are a separate page, available on Free. Your AI checks existing content and the app's actual usage conditions, prepares the text, follows your publishing instructions, and returns the verified terms URL.

For updates, your AI prepares the version's content and adds the in-app prompt to your project. Users see what changed when they open the app after upgrading. Publishing release content is available on Free; reading it from a running app requires Pro. Your AI checks publishing and app behavior separately; a public release-history URL alone does not complete the in-app prompt.

[Privacy-policy example](https://storepal.app/examples/privacy) · [Terms example](https://storepal.app/examples/terms) · [Installation help](https://storepal.app/docs/cli) · [StorePal](https://storepal.app)

For a command-level reference, see the [Skill](skills/storepal/SKILL.md). The [sample Markdown files](examples/launch-kit) are for disposable demo apps.
