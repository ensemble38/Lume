# Seven-day refresh

Free Apple ID signatures normally expire after seven days.

## If Lume appears in AltStore Classic > My Apps

1. Run AltServer on the Windows PC.
2. Put the PC and iPhone on the same private Wi-Fi, or connect the iPhone by USB.
3. Open AltStore Classic and use **Refresh All** before expiry.

## If Lume was installed directly with AltServer > Sideload .ipa

AltStore Classic may not list or refresh it. Before expiry, repeat **Sideload .ipa** with the same desktop IPA and the same Apple ID. Rebuilding is not required unless the Lume source changes.

To make AltStore track Lume, install the IPA once through AltStore Classic's `+` button while AltServer is available. Afterwards it should appear in **My Apps** and can be refreshed there.

If a refresh fails, first confirm that AltServer is running, Apple Mobile Device Service and Bonjour are running, the iPhone is unlocked/trusted, and both devices are on the same private network. USB is the recovery path when Wi-Fi discovery fails.
