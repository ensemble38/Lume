# Install Lume 3.2.0 from AltServer on Windows

The prepared IPA is on the Windows desktop. Prefer the versioned name so an
older build cannot be selected accidentally:

`Lume-v3.2.0-build38-20261114.ipa`

1. Connect the unlocked iPhone to this PC by USB and accept **Trust** if asked.
2. Leave AltServer running in the Windows notification area.
3. Hold **Shift** and left-click the AltServer tray icon.
4. Choose **Sideload .ipa**, then choose the iPhone.
5. Select the desktop file `Lume-iPhone-unsigned.ipa`.
6. Enter the Apple ID in AltServer when prompted. Do not save the password in scripts, GitHub, or this repository.
7. Wait for the AltServer success notification, then open Lume on the iPhone.

The IPA contains `LumeWidgets.appex`. A free Apple ID has a limited App ID allowance. If AltServer offers to remove app extensions, removing the widget does not remove Lume's IPTV, M3U, EPG, or playback features.

Direct **Sideload .ipa** installation by AltServer may not add Lume to AltStore Classic's **My Apps** list. That list tracks apps installed through AltStore's `+` button. The installed Lume app itself is still valid, but automatic **Refresh All** tracking requires installing the same IPA through AltStore Classic instead.
