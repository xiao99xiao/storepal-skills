# Markdown to App Store page URLs

A 90-second walkthrough after account setup. This example uses your own disposable demo app, never customer apps. Commands below publish sample content immediately. Samples are deliberately labeled as demos; replace them with reviewed app-specific content before using the links in App Store Connect.

## See the result

[StorePal-owned demo privacy page](https://storepal.app/storepal-cli-walkthrough/privacy) · [Demo FAQ](https://storepal.app/storepal-cli-walkthrough/faq) · [Demo update](https://storepal.app/storepal-cli-walkthrough/releases)

## Setup

1. Create a free StorePal account and a disposable app at https://storepal.app/dashboard/apps/new. Choose a unique URL name. The current CLI does not create apps.
2. Download privacy.md, terms.md and release.md from this directory into your working folder.
3. Run `npx storepal@0.1.0 auth login`, approve the browser authorization, and return to your terminal. This uses a user-scoped CLI token and works on the free plan.

## Walkthrough

Replace `your-demo-slug` with the demo app's URL name. Keep the quotes around the shell variable below.

```sh
DEMO_SLUG='your-demo-slug'
npx storepal@0.1.0 apps
npx storepal@0.1.0 privacy set --slug "$DEMO_SLUG" --file privacy.md
npx storepal@0.1.0 terms set --slug "$DEMO_SLUG" --file terms.md
npx storepal@0.1.0 faq add --slug "$DEMO_SLUG" --question "Is this a demo?" --answer "Yes. This is the StorePal CLI walkthrough."
npx storepal@0.1.0 releases add --slug "$DEMO_SLUG" --version demo-1 --file release.md
npx storepal@0.1.0 urls --slug "$DEMO_SLUG"
```

The final command prints JSON containing support, privacy, terms, faq and releases URLs under `https://storepal.app/your-demo-slug/`. Open privacy and releases to inspect the result. Copy the support/privacy URLs to App Store Connect only after replacing the sample content with your actual content.

Privacy and terms commands replace their page content. FAQ and release commands append entries: do not repeat those two commands unless you want another entry. For a second maintenance pass, edit privacy.md and repeat only `privacy set`.

## Use an AI coding assistant

Install the skill with `npx skills add xiao99xiao/storepal-skills`, then ask:

> Use my disposable StorePal demo app with slug YOUR-DEMO-SLUG. Review the local sample files with me, publish privacy.md and terms.md with the StorePal CLI, then return the hosted URLs. Do not touch other apps or submit anything to App Store Connect.

For a real app, supply the app's actual data practices and reviewed text. The agent must not invent them.

## Cleanup

Delete only the disposable demo app from its StorePal dashboard Settings. This also removes its demo pages and entries. `npx storepal@0.1.0 auth logout` removes the CLI's local saved credentials; revoke the token in the dashboard if necessary. Never commit credentials or CLI token files.

Prefer the browser? Try https://storepal.app/launch-kit?utm_source=github&utm_medium=example&utm_campaign=launch-kit
