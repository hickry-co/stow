# Stow

> Everything you see on your iPhone, saved in one place.

Stow is an iOS app that lives in the system **Share** sheet. See something in any app, tap **Share → Stow**, and it lands in your personal portal — automatically categorized, grouped by the app it came from, and synced through iCloud.

Repo: <https://github.com/hickry-co/stow> · Made by [hickry.co](https://hickry.co), Istanbul.

## Features

- **Share Extension + Action Extension** — save links, text and images from any app. Stow shows up in the share sheet's app row *and* in the **Actions** list ("Save to Stow"), and asks **where to save** (a category, or *Auto* to let Stow sort it) plus an optional note.
- **Home** — searchable (your items and shared collections); browse by **Categories** (sorted on-device by Apple's language model, with an EN/TR keyword fallback) or by **Apps** (Instagram, YouTube, …).
- **Seen** — an archive (with a *Personal / Shared* switch: items of shared collections can be marked as seen too — the mark is stored on the shared item, so everyone in the collection sees it; people with read-only access keep a local mark) for things you've read, watched or listened to. Long-press (or swipe) any item for *Favorite*, *Mark as read/watched/listened/seen* and *Delete*; Home only shows what's still unseen. Seen items move to **Old Records** (Profile → Data; a sidebar item on the Mac) after a time you choose in Profile → Retention (default 30 days); old records are then moved to Deleted Records after a second period (default 6 months). Both can be set to *Never*.
- **Onboarding** — after the first sign-in the iPhone app walks through six pages (saving, adding Stow to the share sheet, Home views, Seen, shared lists, Profile). Replay it from Profile → Data → *Show the intro*. The Mac app has its own six-page intro (`MacOnboardingView`: saving, enabling Stow in the Share menu via *Edit Extensions…*, sidebar, Seen, shared lists, Settings); replay it from Settings → *Show the intro*.
- **Shared-content notifications** — when someone adds something to a collection shared with you, Stow tells you ("Onur, Family listesine yeni bir içerik ekledi."). Delivered as a real push: the adder's device leaves a small `StowNotice` record in the container's *public* CloudKit database for each other participant, and every participant has a query subscription that matches only their own notices (`NoticeCenter`). The text includes the kind of content ("…yeni bir video ekledi"). If the subscription can't be created, Stow falls back to a local notification built from CloudKit's silent change notification. Can be turned off in Profile / Settings → Notifications. Needs the `StowNotice` record type deployed to Production (see docs). **Reminders** — content added by somebody else that is still not marked as seen gets a reminder at 15 and 30 days (10:00 local time, merged per collection; scheduled locally, so they arrive even when Stow is closed; also switchable in Notifications).
- **Your own categories** — create new categories (and new shared collections) right in the *Save to Stow* screen. **Bulk actions** — *Select* in any list to share several items to a collection, mark them seen or delete them at once (also inside shared collections).
- **Profile**
  - Appearance: System / Light / Dark
  - Language: English (default) / Türkçe — your own titles and notes can be translated on-device (Translation framework), or you can type the other-language version per item
  - Sync with iCloud on/off
  - Export / Import (JSON)
  - Deleted Records, protected by Face ID / Touch ID / passcode (auto-purged after 30 days)
  - Delete all data, delete account
- **Save to an app folder**: in the "Where to save?" screen pick a category, an app (Instagram, YouTube… or one you create), and optionally a shared collection.
- **Shared collections**: invite people with Apple's sharing sheet (CloudKit `CKShare`); the Home screen has a *Shared* view, and Profile → Sharing manages people, links and access.
- **Reads more than text**: summaries for links, text, images (OCR + Vision), audio (SpeechAnalyzer) and video (speech + on-screen text). All on-device. The iOS 27 multimodal Foundation Models input plugs in at `Summarizer.multimodalSummary`.
- **Native macOS app** (`StowMac/`): sidebar (All / Favorites / Seen / categories / apps / shared collections), item list with search, detail view with editable title + note, category and source pickers, media player, *New Item* (⌘N, paste ⌘V, or drag & drop links, text, images, audio and video), shared collections (entries, notes, invite via Apple's sharing UI, rename, leave), Settings (appearance, language, iCloud, export/import, Deleted Records behind Touch ID / password). Same bundle ID as the iPhone app (universal purchase) and the same iCloud library.
- **Sign in with Apple**, so the iPhone and Mac apps share the same identity and iCloud library.
- **Liquid Glass** UI (iOS 26) on a custom palette with light and dark variants, plus light/dark app icons.

## Palette

| Use | Light | Dark |
|---|---|---|
| Background | `#FAF7F2` | `#0D0B1E` |
| Cards | `#FFFFFF` | `#19162E` |
| Text | `#211C3B` | `#F1EEFF` |
| Secondary text | `#716B82` | `#ADA6C3` |
| Accent | `#5140D9` | `#A79BFF` |
| Selection | `#EDE9FF` | `#302750` |
| Border | `#E4DFEB` | `#353047` |

Defined in `Stow/Assets.xcassets` (named colors) and exposed in `Stow/Theme.swift`.

## Getting started

Requires Xcode 26+ and [XcodeGen](https://github.com/yonaskolb/XcodeGen).

```bash
brew install xcodegen
git clone https://github.com/hickry-co/stow.git && cd stow
# set DEVELOPMENT_TEAM in project.yml, then:
xcodegen generate
open Stow.xcodeproj
```

Apple Developer portal setup (one time):

| Item | Identifier |
|---|---|
| App ID | `co.hickry.stow` — Sign in with Apple, iCloud (CloudKit), Push Notifications, App Groups |
| Mac App ID | Same `co.hickry.stow`, add the **macOS** platform — enable Sign in with Apple, iCloud (CloudKit), Push Notifications, App Groups for it too |
| Extension App ID | `co.hickry.stow.share` — App Groups |
| App Group | `group.co.hickry.stow` |
| iCloud container | `iCloud.co.hickry.stow` |

Change the identifiers in `project.yml` and `Shared/AppGroup.swift` if you use different ones. Shared collections create the CloudKit record types `StowCollection` and `StowSharedItem` on first use. After the first CloudKit run, deploy the schema to Production in the CloudKit Console before shipping.

## Architecture

```
Shared/            AppGroup + Inbox (used by app and extension)
StowShare/         Share Extension (app row)
StowMac/           Native macOS app (SwiftUI). Reuses Shared/ (minus SaveFlow), Stow/Models, Stow/Services, Stow/Theme.swift
StowAction/        Action Extension (Actions list) — both reuse Shared/SaveFlow.swift and only write to the App Group inbox
Stow/
  Models/          StowItem (SwiftData, CloudKit-safe), categories, source apps, settings
  Services/        Persistence, Auth, IngestService, Classifier, Backup
  Views/           Welcome, Home, Seen, Archive (Old Records), ItemDetail, Profile, DeletedRecords
  Assets.xcassets  Palette + AppIcon (light/dark)
  Localizable.xcstrings   EN + TR
docs/index.html    Landing page (GitHub Pages)
```

Flow: **Share → extension drops a JSON (+ image) into the App Group inbox → app ingests on next activation → fetches link metadata → classifier picks a category → SwiftData saves → CloudKit syncs.** The extension stays tiny on purpose (extensions have strict memory limits).

## Notes & known limits

- New actions may need enabling once: in the share sheet scroll to the bottom → **Edit Actions** → add *Save to Stow* (favorite it to pin it to the top).
- iOS doesn't tell a Share Extension which app it was invoked from, so the "app" folder is inferred from the link's host (Instagram, YouTube, …); plain text → *Text*, images → *Photos*.
- Changing the iCloud sync toggle applies on the next launch.
- **Account deletion & App Store:** deleting the account removes all local + iCloud data and signs out, but revoking the Sign in with Apple token requires a small server call (`/auth/revoke`). Add one before App Store submission (guideline 5.1.1(v)).
- **Mac** has a Share menu extension (Safari → Share → Stow; enable it under *Edit Extensions…*), drag & drop, ⌘V and ⌘N. It has no batch translation of old notes; everything else matches iPhone. Audio / video files are kept per device, as on iPhone.
- The in-app language is independent of the system language (English by default).

## Support

Stow saves what you come across on your iPhone and Mac in one place. It needs iOS 26 or macOS 26 or later.

- **Add Stow to the share sheet (iPhone):** tap Share in any app, swipe to the end of the app row, tap **More** and turn **Stow** on. For the *Save to Stow* action, scroll to the bottom of the share sheet, tap **Edit Actions** and add it.
- **Add Stow to the Share menu (Mac):** click Share in Safari, choose **Edit Extensions…** and tick **Stow**.
- **Sync:** sign in to iCloud on every device and keep **iCloud sync** on in Profile (iPhone) or Settings (Mac).
- **Notifications about shared collections:** allow notifications for Stow in your device settings and turn on **Notify me about new shared content**.
- **Export or delete your data:** Profile (iPhone) or Settings (Mac), Data and Danger Zone.

Questions or problems: [open an issue](https://github.com/hickry-co/stow/issues).

## Privacy Policy

*Effective November 17, 2026.*

Stow has no servers of its own, collects no data, does not track you and shows no ads.

- Your library (links, text, photos, video, audio, notes and summaries) stays on your device and, if iCloud sync is on, in your own private iCloud storage. The developer cannot see it.
- Sign in with Apple gives Stow your name and email. They are stored on your device, and your name also in iCloud, so people you share collections with can see who added what.
- Shared collections are stored in your iCloud and shared with the people you invite through Apple's CloudKit. They can see the items, who added them and whether they were seen. Stopping the share ends their access.
- To notify someone about a new shared item, your device leaves a small notice (your name, the collection name, the kind of content, and a hash addressing the recipient) in the app's public CloudKit database. It is deleted after about two days. It does not contain the item itself.
- To title and summarize a link, your device contacts the website you saved, like a browser. Summaries, categories, translation and speech-to-text run on your device with Apple's on-device technology.
- Face ID or Touch ID, when used for Deleted Records, is checked by your device. Stow never receives biometric data.
- A short diagnostics log stays on your device and is never sent. It may contain item names.
- You can export your library, turn off iCloud sync or notifications, and use **Delete All Data** or **Delete Account** in the app to remove your data, including from iCloud.
- Stow is not directed at children under 13.

Apple handles data for iCloud, Sign in with Apple and push notifications under [Apple's Privacy Policy](https://www.apple.com/legal/privacy/). If this policy changes, the date above is updated.

Contact: [open an issue](https://github.com/hickry-co/stow/issues) or visit [hickry.co](https://hickry.co).

## License

TBD.
