---
name: storepal
description: Manage App Store pages (privacy policy, terms, support, FAQ, release notes) and feedback with StorePal CLI. Use when asked to set up App Store required pages, get page URLs for App Store Connect metadata, update privacy policies or terms, or integrate StorePal with asc CLI.
---

# StorePal CLI

Use this skill when you need to create or manage the pages the App Store requires — privacy policy, terms of service, support page, FAQ, and release notes — or when you need their URLs for App Store Connect metadata.

## Setup

### First-time login
```bash
npx storepal auth login
```
Opens a browser for authentication. Token is saved to `~/.storepal/credentials.json`.

### Verify session
```bash
npx storepal auth status
```

## Common Workflows

### Get all page URLs for an app
```bash
npx storepal urls --slug my-app
```
Returns JSON:
```json
{
  "support": "https://storepal.app/my-app/support",
  "privacy": "https://storepal.app/my-app/privacy",
  "terms": "https://storepal.app/my-app/terms",
  "faq": "https://storepal.app/my-app/faq",
  "releases": "https://storepal.app/my-app/releases"
}
```
If the user has exactly one app, `--slug` can be omitted.

### Update privacy policy from a file
```bash
npx storepal privacy set --slug my-app --file privacy.md
```

### Update terms of service from a file
```bash
npx storepal terms set --slug my-app --file terms.md
```

### Read current privacy policy
```bash
npx storepal privacy get --slug my-app
```

### List all apps
```bash
npx storepal apps
```

## Integration with ASC CLI

StorePal provides the URLs that `asc` needs for App Store Connect metadata fields. Here is how to use them together.

### Set privacy policy URL in App Store Connect

The privacy policy URL is an **app info localization** field (not version localization).

```bash
# 1. Get the URL from StorePal
PRIVACY_URL=$(npx storepal urls --slug my-app | node -e "process.stdout.write(JSON.parse(require('fs').readFileSync('/dev/stdin','utf8')).privacy)")

# 2. Find the app info ID
asc apps info list --app "$ASC_APP_ID"

# 3. Set the privacy policy URL in App Store Connect
asc localizations upload --app "$ASC_APP_ID" --type app-info --app-info "$APP_INFO_ID" --path "./app-info-localizations"
```

Where the `.strings` file contains:
```
"privacyPolicyUrl" = "https://storepal.app/my-app/privacy";
```

Or use the quick edit command:
```bash
asc apps info edit --app "$ASC_APP_ID" --locale "en-US" --privacy-policy-url "https://storepal.app/my-app/privacy"
```

### Set support URL in App Store Connect

The support URL is a **version localization** field.

```bash
asc apps info edit --app "$ASC_APP_ID" --locale "en-US" --support-url "https://storepal.app/my-app/support"
```

### Full setup: create pages and push URLs to App Store Connect

When setting up a new app for the App Store:

1. **Log in to StorePal** (if not already):
   ```bash
   npx storepal auth login
   ```

2. **Write and upload privacy policy**:
   ```bash
   # Write privacy.md for the app, then:
   npx storepal privacy set --slug my-app --file privacy.md
   ```

3. **Write and upload terms of service**:
   ```bash
   npx storepal terms set --slug my-app --file terms.md
   ```

4. **Get all URLs**:
   ```bash
   npx storepal urls --slug my-app
   ```

5. **Push URLs to App Store Connect**:
   ```bash
   # Privacy policy URL (app-level)
   asc apps info edit --app "$ASC_APP_ID" --locale "en-US" \
     --privacy-policy-url "https://storepal.app/my-app/privacy"

   # Support URL (version-level)
   asc apps info edit --app "$ASC_APP_ID" --locale "en-US" \
     --support-url "https://storepal.app/my-app/support"
   ```

## CLI Reference

| Command | Description |
|---------|-------------|
| `storepal auth login` | Sign in via browser |
| `storepal auth logout` | Clear saved credentials |
| `storepal auth status` | Show current session and apps |
| `storepal apps` | List all apps |
| `storepal urls [--slug x]` | Get all public URLs as JSON |
| `storepal privacy get [--slug x]` | Print privacy policy markdown |
| `storepal privacy set [--slug x] --file path` | Update privacy policy from file |
| `storepal terms get [--slug x]` | Print terms of service markdown |
| `storepal terms set [--slug x] --file path` | Update terms from file |
| `storepal faq list [--slug x]` | List FAQ items |
| `storepal faq add [--slug x] --question "..." --answer "..."` | Add FAQ item |
| `storepal faq remove [--slug x] --id uuid` | Remove FAQ item |
| `storepal releases list [--slug x]` | List release notes |
| `storepal releases add [--slug x] --version x --file path` | Add release note from file |
| `storepal releases remove [--slug x] --id uuid` | Remove release note |

## REST API

All endpoints accept `Authorization: Bearer sp_user_xxx` (CLI token) or `Bearer sp_live_xxx` (app-scoped API key, Pro plan).

For `sp_user_` tokens, single-app endpoints require `?slug=my-app`.

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/apps` | List all apps with URLs |
| POST | `/api/v1/apps` | Create a new app |
| GET | `/api/v1/app?slug=x` | Get app info and all URLs |
| GET | `/api/v1/privacy?slug=x` | Get privacy policy markdown |
| PUT | `/api/v1/privacy?slug=x` | Update privacy policy |
| GET | `/api/v1/terms?slug=x` | Get terms markdown |
| PUT | `/api/v1/terms?slug=x` | Update terms |
| GET | `/api/v1/faq?slug=x` | List FAQ items |
| POST | `/api/v1/faq?slug=x` | Create FAQ item |
| GET | `/api/v1/releases?slug=x` | List release notes |
| POST | `/api/v1/releases?slug=x` | Create release note |
| POST | `/api/v1/feedback?slug=x` | Submit feedback |

Base URL: `https://storepal.app`

## Notes
- StorePal pages are live instantly. The support page with feedback form is active as soon as an app is created.
- Privacy policy and terms support full Markdown.
- The support page URL doubles as a feedback inbox — submissions appear in the StorePal dashboard.
- Free plan: up to 3 apps, 30 feedback per month. Pro plan: unlimited.
- CLI tokens (`sp_user_`) work on all plans. App-scoped API keys (`sp_live_`) require Pro.
