# Fork Notice

This is a fork of [`jaywcjlove/PermissionFlow`](https://github.com/jaywcjlove/PermissionFlow) maintained by [@lifedever](https://github.com/lifedever) for use with [PasteMemo](https://github.com/lifedever/PasteMemo-app).

The upstream package retains its original MIT license and copyright (see `LICENSE`). All credit for the original design and the floating drag-authorization concept goes to the upstream author.

## Changes in this fork

The `pastememo-enhancements` branch adds three optional improvements on top of upstream:

1. **Auto-hide on focus loss** — the floating panel hides when System Settings is no longer the frontmost application and reappears when the user comes back to it. Avoids the panel hovering on top of unrelated apps.

2. **Auto-close on accessibility grant** — when `pane == .accessibility`, the controller polls `AXIsProcessTrusted()` and closes the panel automatically once the grant succeeds. Other panes are unchanged.

3. **Optional `panelHint`** — a new `panelHint:` parameter on `authorize(...)`. The supplied string is rendered as a small warning banner above the drag card. Useful for re-authorization flows that need to remind the user to remove a stale entry before dragging.

```swift
controller.authorize(
    pane: .accessibility,
    suggestedAppURLs: [Bundle.main.bundleURL],
    sourceFrameInScreen: clickFrame,
    panelHint: "Click − to remove the existing entry first, then drag the new version below."
)
```

All changes are additive and backward-compatible with upstream call sites.

## Syncing with upstream

```bash
git fetch upstream
git rebase upstream/main
```
