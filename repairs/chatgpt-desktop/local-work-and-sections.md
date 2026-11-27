# Restore Local Work routing and Section actions

**App:** ChatGPT desktop for macOS. **Patched:** `26.1120.50957` (30866). **Compared with:** signed `26.1111.71025` (28661).

## Reproduce

In a disposable native Local Work project, choose Local and send the first message. Check whether the chat stays local and the project stays unpaired with the cloud. Check the sidebar's right-click and ellipsis menus for Section on a local and a ChatGPT-backed chat.

## Repair

1. Keep the signed app available. Use a separate app copy and an isolated test profile. Copies of these builds can share local data because they have the same bundle identifier.
2. Trace the standard and merged Work composers from project selection through submission. Route a native, unpaired project to the existing Local submission path unless the user explicitly chooses Cloud. Preserve routing for projects already paired with the cloud.
3. Find both bundled copies of the Section action builder. Restore the existing Section action on the eligible local and ChatGPT-backed rows, then check both menu entry points. If the app or its tools reject the modified copy's signing identity, record the failure and use the signed app for those tools.

## Verify and undo

Check first send in two chats in the same fresh Local Work project, an explicit Cloud submission, and a Section move for each chat type. Restart and check the assignments again.

The patched copy completed four turns in one fresh, unpaired local project. Local Section placement survived restart. A clean first-send test and a controlled cloud-backed Section move followed by restart remain unverified. Native App Tools failed its signing-identity check on the patched copy.

To undo, quit the patched copy and launch the original signed app. Any test data changes belong in the isolated profile.

Read the [original Note](https://kogen.dev/notes/2026-11-26/fine-i-ll-fix-chatgpt-myself/).
