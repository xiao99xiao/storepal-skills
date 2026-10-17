---
name: storepal
description: Prepare and publish an app's privacy policy or terms of service with hosted URLs, add a feedback entry so users can send messages, and show what changed after an app upgrade using StorePal. Also handles FAQ and policy updates after data-use changes. Use for these outcomes even when the user does not mention StorePal or a Skill.
---

# StorePal

Work in the user's existing app project. Finish the requested outcome: a published page, a support entry, or an in-app What's New prompt. Installing this Skill is only setup. Use the CLI (0.1.3 or newer) with the user's StorePal account. Free accounts can publish pages for up to five apps, including their app icons. The user's existing AI prepares the content; StorePal hosts it. Every feature, including runtime API access, attachments, webhooks and in-app update prompts, is included on Free. Pro only adds unlimited apps.

## Recognize the user's goal

Users describe what they want their app to do. Do not require the StorePal name, an explicit Skill invocation, command names, or a fixed phrase. These are examples, not keywords to match:

| A request might sound like | Complete this outcome |
| --- | --- |
| “这个 App 要上架了，帮我准备隐私政策和网址。” / “I need a privacy policy URL for the store.” | Prepare the policy from project facts, publish when authorized, and return the usable URL. A local draft alone does not supply a hosted URL. |
| “帮我准备这个 App 的服务条款和网址。” / “Prepare this app's terms of service and a URL.” | Prepare and publish a separate terms page, check it, and return its URL. If only terms were requested, do not add an unrelated privacy policy. |
| “在设置里加个意见反馈，让用户能给我发消息。” / “Let users contact me from Settings.” | Reuse or create the support form and add its entry to the app. Check the app entry and the form separately. |
| “用户升级后，第一次打开时展示这次更新的内容。” / “Show what's new after an update.” | Prepare content for the correct version and integrate the in-app prompt. Publishing a release-history page alone is incomplete. |
| “加了登录，隐私政策也更新一下。” / “Update the policy for the new account feature.” | Inspect the actual data-use change, read the existing policy, and update that same app and URL. |

Infer the relevant workflow from the request and project context. Reuse the project's known StorePal app; do not ask the user to choose between CLI, API, and SDK. Respect an explicitly chosen alternative service or a request for a draft only. Installing the Skill does not authorize unrelated publication: an ordinary feature change does not by itself request a policy update or a release announcement.

## First privacy policy

For other requests, reuse the session and app selection steps below, then follow the matching workflow. Do not create a privacy policy when the user only asked for an update prompt or another task.

1. Inspect the project to identify the app and the facts needed for the requested page. Ask only for facts you cannot establish, using specific questions. An unknown answer is not evidence that the app collects no data.
2. Check the session with `npx --yes storepal@latest auth status --json`. If not authenticated, run `npx --yes storepal@latest auth login`, give the user the printed sign-in link, and wait for the result. The user can create a free account on that page. Explain that they should return to this AI conversation afterward. Login uses a browser on the same machine as the CLI; do not describe it as working on an unverified remote environment.
3. Run `npx --yes storepal@latest apps --json`. Reuse the project's known StorePal slug. If several apps could match, confirm the target before writing. If there is no matching app, choose a URL name from the project and create it:
   ```sh
   npx --yes storepal@latest apps create --name "App name" --slug app-name --json
   ```
   When creating an app, first look for its existing icon in the current project (for example, an AppIcon asset catalog, manifest, or app branding assets). If the intended icon is unambiguous, append `--icon ./path/to/AppIcon.png`. Use PNG, JPG or WebP up to 2 MB; export SVG, ICNS or ICO to PNG with an available project tool without modifying the source asset. Ask for the intended icon if none is identifiable or several variants could apply; the user may continue without one. Do not invent a logo or choose an unrelated image.

   Store the selected slug in appropriate project context so later tasks can find the same app. Creating the same owned slug again reuses it without overwriting page content. Omitting `--icon` preserves the existing icon; supplying it explicitly applies that icon to the reused app. For an existing app, preserve its current icon unless the user requested a change or is completing missing app branding. A taken slug belonging to another account needs a different name. Do not tell the user to create an app in the dashboard.
4. For an existing policy, read it before editing. Prepare the requested content from confirmed facts. When the user asks to review before publishing, show the complete text and obtain their confirmation. Keep an already authorized continuous task moving; do not add unrelated pages or store submissions.
5. Save the reviewed policy as a local Markdown file and publish it:
   ```sh
   npx --yes storepal@latest privacy set --slug app-name --file privacy.md --json
   ```
   The command reads the saved policy back and returns `url`, `updated_at`, and `content_verified`. If the write response is lost, read the current policy before repeating the write.
6. Open the returned URL with a browser when available and check the rendered text. An HTTP 200 or `content_verified` alone does not prove the public page rendered. Report exactly what was checked. Return the page link, its purpose, and one useful follow-up request.

Example completion, only after the corresponding operations succeed:

- Privacy policy: the actual URL. This goes in your store listing's privacy policy URL field.
- Check: saved content matches the reviewed file; report the browser check separately.
- Next time: “We added accounts. Check and update this app's privacy policy too.”

If the Skill was just installed, check whether the client has loaded it. If a client reload is required, tell the user how to return and continue the same task. Do not reinstall or restart creation blindly after an interruption: check session, target app, and saved content first. Treat CLI errors as incomplete work, never as success. Tokens remain local; do not print them or ask the user to paste them into chat.

## Terms of service

Reuse the session and app selection steps above. Read existing terms with `npx --yes storepal@latest terms get --slug app-name` before editing. Inspect the app's actual features and business model; ask only for missing details such as the service provider and usage conditions. Do not invent subscription, refund, or other business rules, or claim the terms guarantee store approval or legal compliance.

Prepare a separate Markdown file. Follow the user's review and publishing instructions, then publish with `npx --yes storepal@latest terms set --slug app-name --file terms.md`. This command returns a timestamp, not a verified page URL: read the saved text back with `terms get`, get the actual terms URL from `urls`, and check its rendered page in a browser when available. If a write response is lost, read the current terms before retrying. Report the checks that actually ran.

Terms publishing works on Free. When both privacy and terms are requested, keep their files and URLs separate and publish only the authorized content. When an in-app terms link is requested, add the actual URL through the project's existing navigation and check that entry too. Do not insert a terms URL into a privacy-policy field.

## Other pages

Do only the requested task. Support pages with a contact form exist when an app is created; messages arrive in the StorePal dashboard inbox. `urls` returns addresses, not evidence that a policy or release exists.

| Task | Command |
| --- | --- |
| Read the policy | `npx --yes storepal@latest privacy get --slug app-name --json` |
| Add/replace an app icon | `npx --yes storepal@latest apps icon set --slug app-name --file ./AppIcon.png --json` |
| Get hosted URLs | `npx --yes storepal@latest urls --slug app-name` |
| Read/update terms | `npx --yes storepal@latest terms get --slug app-name` / `terms set --slug app-name --file terms.md` |
| List/add FAQs | `npx --yes storepal@latest faq list --slug app-name` / `faq add --slug app-name --question "..." --answer "..."` |
| List/add release notes | `npx --yes storepal@latest releases list --slug app-name` / `releases add --slug app-name --version 1.2 --file release.md` |

Icon uploads work on Free and Pro. The CLI checks the local file before creating an app, compares its hash with the upload response, and reads back the saved icon URL (`icon_verified`); check the image on the hosted page separately. If icon setup fails after app creation, retry the same owned slug and file or run `apps icon set`; do not create another app. Repeating the same file keeps the current icon URL. An icon upload does not modify page content.

For an in-app feedback or contact request, the default simple path is an app entry that opens the hosted support form; this works on Free and does not require a native runtime API key. Reuse the project's existing navigation or browser-opening conventions, add the actual support URL, then identify and check the modified entry and the form. If the user explicitly wants a native form or an existing integration requires one, inspect the matching runtime documentation and app-scoped credentials. Creating a support page does not prove the app was changed. The current CLI has no developer inbox-reading or developer-reply command. Do not promise autonomous feedback processing or background monitoring.

## Show what's new after an app update

The main outcome is that users see the changes inside the app after upgrading. A public release-history URL is a secondary result.

1. Establish the target version and change range from the project; ask if ambiguous. Summarize user-visible changes from confirmed work. Check existing releases before publishing so a retry does not add a duplicate. If the user only requested content, finish that request without adding an app integration.
2. When asked for the in-app prompt, inspect the platform, existing startup flow, and any installed StorePal integration. Use the matching SDK or API documentation; do not assume a macOS SDK works on every platform. For the API, request the installed app version with `GET /api/v1/releases?version=<version>` and the app's selected language, rather than displaying an unrelated latest release.
3. In-app runtime access and publishing release content are included on every plan. Verify the app integration before reporting that it works. Use an app-scoped `sp_live_` credential via the project's existing configuration workflow. Never embed the account-wide `sp_user_` CLI credential in an app, print tokens, or purchase a plan without authorization.
4. Make the project change so the prompt appears once after upgrading, skips the first install, and does not block startup if content is unavailable. Follow existing app conventions for presenting and dismissing it. Fetching or dismissing the same version must not cause repeated prompts on every launch.
5. Check the published content and the app behavior separately. Verify an upgrade, another launch on the same version, a first install, and missing or unreachable content when the environment allows. A successful content publish or API read is not proof of an in-app prompt. Report what was implemented and checked, and any remaining integration step. Lead the result with the user-visible behavior, not the public URL.

## Access and external stores

The local `sp_user_` credential manages all apps in the StorePal account and works on Free and Pro. It is distinct from an app-scoped `sp_live_` key for native runtime integration. Install and login do not buy a subscription or authorize App Store Connect.

Only update App Store Connect when the user requests it and an authorized store tool is available. Use that tool's current documentation for field mappings. Returning a StorePal URL does not mean store metadata was updated.
