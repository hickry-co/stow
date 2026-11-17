# Stow Privacy Policy

**Effective date: November 17, 2026**

Stow ("the app") is made by hickry.co, Istanbul. This policy explains what the app does with your information. In short: **Stow has no servers of its own, does not collect your data, does not track you and shows no ads.** Your library stays on your devices and in your own iCloud account.

## What Stow handles

**Content you save.** Links, text, notes, photos, video and audio that you share to Stow, with the titles, notes, categories, summaries and thumbnails Stow creates for them. This is stored on your device. If iCloud sync is on, it is also stored in your private iCloud (CloudKit) storage, which is how it reaches your other Apple devices. We cannot see it.

**Sign in with Apple.** When you sign in, Apple gives Stow your name and, if you choose to share it, your email address or Apple's private relay address. Stow stores your name and email on your device, and your name in iCloud key-value storage so your iPhone and Mac use the same name. The name is shown to people you share collections with, next to the items you add.

**Shared collections.** When you share a collection, its items are stored in your iCloud and shared with the people you invite through Apple's CloudKit sharing. Those people can see the items, the name of whoever added them, and whether they were marked as seen, and can add and change items according to the permission you give them. Removing someone, or stopping the share, ends their access.

**Notifications about shared collections.** To tell someone that you added something, Stow on your device writes a small notice record to the app's public CloudKit database. It contains your name, the name of the collection, the kind of content (for example video or article), and a one-way hash that addresses it to the recipient. It does not contain the item itself, its link or its text. The recipient's device receives it as a push notification through Apple's push service. Your device deletes the notices it created after about two days. During that time, the collection name and your name are visible in the notice record to anyone who can read the app's public database and finds that record. Notifications can be turned off in Profile (iPhone) or Settings (Mac).

**Links you save.** To title, classify and summarize a link, Stow on your device contacts the website you saved, in the same way a browser would, and reads the page. That request goes directly from your device to that website, not through us, and the website can see it as a normal visit.

**Audio and video.** If you save audio or video, Stow can turn the speech in it into text on your device, using Apple's speech recognition, in order to write a summary. Saved media files stay on the device that saved them.

**Summaries, categories and translation.** Stow uses Apple's on-device language technology (such as Apple Foundation Models, Natural Language and Translation, when your device supports them) to sort items, write short summaries and translate your own notes. This happens on your device.

**Face ID and Touch ID.** Stow can ask for Face ID, Touch ID or your device passcode before showing Deleted Records. The check is done by your device. Stow never receives your biometric data.

**Diagnostics log.** Stow keeps a short technical log on your device to help find problems. It is not sent anywhere. It may contain item or collection names. You can read it in the Diagnostics screen and clear it, and you decide whether to share any part of it.

## What Stow does not do

- It has no analytics, advertising or tracking software of any kind.
- It does not sell, rent or share your information.
- It does not send your content to any server operated by us.
- It does not create profiles about you.

Apple processes data when you use its services, such as iCloud, Sign in with Apple and push notifications, under [Apple's Privacy Policy](https://www.apple.com/legal/privacy/).

## Where your data is kept and for how long

Your library stays on your device and in your iCloud until you delete it. Stow can move items from Seen to Old Records, and delete old records, automatically after the time you choose in Profile (iPhone) or Settings (Mac). Items you delete are kept in Deleted Records for a limited time so you can restore them, then removed.

## Your choices and control

- **Export** your library from Profile (iPhone) or Settings (Mac).
- **Delete All Data** removes your library from the device and from iCloud.
- **Delete Account** removes your account data and your library from iCloud.
- **Turn off iCloud sync** to keep your library on one device only.
- **Turn off notifications** for shared collections in the app, or in your device settings.
- Manage who can access a shared collection from Profile, Shared Collections (iPhone) or the sidebar (Mac), or stop the share.

## Children

Stow is not directed at children under 13, and we do not knowingly collect information from children.

## Changes to this policy

If we change this policy, we will update the date above and publish the new version at this address. If a change is significant, we will also tell you in the app.

## Contact

Questions about this policy: [open an issue](https://github.com/hickry-co/stow/issues) or visit [hickry.co](https://hickry.co).

Made with love, by hickry.co. Istanbul, 2026.
