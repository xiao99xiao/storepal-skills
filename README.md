# StorePal Agent Skills

Agent Skills for [StorePal](https://storepal.app) — manage your App Store pages (privacy policy, terms, support, FAQ, release notes) from AI coding assistants.

## Install

```bash
npx skills add xiao99xiao/storepal-skills
```

Works with **Claude Code**, **Cursor**, **Windsurf**, and any tool that supports Agent Skills.

## What it does

Once installed, your AI assistant can:

- Create and update privacy policies, terms of service, FAQ, and release notes
- Get all your public page URLs as JSON
- Push URLs to App Store Connect using the [ASC CLI](https://asccli.sh)
- Set up all required App Store pages in one prompt

## Try a complete workflow

[Markdown to hosted URLs: sample files, commands, expected results and cleanup](examples/launch-kit/README.md). Use your own disposable demo app. The examples never require access to another developer’s app.

Only need a privacy policy URL? [Start the privacy editor](https://storepal.app/privacy-policy-generator?utm_source=github&utm_medium=readme&utm_campaign=privacy_oct26) with your app name and reviewed policy text. You can edit and preview before signing in; an account is required to publish.

For all five page types, [preview the browser launch kit](https://storepal.app/launch-kit?utm_source=github&utm_medium=readme&utm_campaign=cli_oct26).

Not sure which URL your store listing needs? Read [Privacy Policy URL requirements](https://storepal.app/guides/app-privacy-policy-url?utm_source=github&utm_medium=readme&utm_campaign=privacy_oct26) or [App Store Support URL setup](https://storepal.app/guides/app-store-support-url?utm_source=github&utm_medium=readme&utm_campaign=support_oct26).

## Example

```
"Set up privacy policy and terms for my app, then push the URLs to App Store Connect"
```

## Prerequisites

- A [StorePal](https://storepal.app) account (free)
- The StorePal CLI: `npx storepal auth login`
- Optional: [ASC CLI](https://asccli.sh) for App Store Connect integration

## Skills

| Skill | Description |
|-------|-------------|
| `storepal` | Manage App Store pages and feedback with the StorePal CLI. Integrates with ASC CLI for pushing URLs to App Store Connect. |

## Links

- [StorePal](https://storepal.app)
- [CLI & Skill Documentation](https://storepal.app/docs/cli?utm_source=github&utm_medium=readme&utm_campaign=cli_oct26)
- [ASC CLI](https://asccli.sh)
