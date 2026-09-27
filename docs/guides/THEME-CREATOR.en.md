# Create lock-screen passcode themes

[Back to English guide](README.en.md) · [简体中文](THEME-CREATOR.zh-CN.md) · [Compatibility records](COMPATIBILITY.en.md)

This guide follows the Theme Creator implementation in this repository. Creating, exporting, and inspecting a `.passthm` file can be done on the Mac without an iPhone connected; flashing requires a connected, trusted device. No new device testing is claimed here. A successful preview or export does not verify how the theme will appear on an iPhone.

## Choose a workflow

Open **Passcode (.passthm) → Theme Creator**.

| Mode | Use it for | Output behavior |
| --- | --- | --- |
| **Poster Slice (Puzzle)** | One photo or illustration across the keypad | Slices artwork for digits 0–9 from one image, with seamless or circular styling |
| **Individual Keys** | A different image for each digit | Crops each configured digit into a circle, with its own pan and zoom |

The editor uses a fixed keypad layout with a **915 × 1148** poster-slicing canvas. That serves a different purpose from the **1536 × 969** Wallet artwork size. Export a theme through the app; renaming a PNG to `.passthm` does not create a theme package.

## Workflow 1: one picture across the keypad

1. Select **Poster Slice (Puzzle)** and click **Choose Poster...**, or drop an image in **Drop poster or wallpaper here**. The picker accepts PNG, JPEG, HEIC, WebP, and other images macOS can decode.
2. Under **Slicing Style**, choose **Seamless Poster** for rectangular keypad tiles, with digit 0 using the entire bottom row, or **Circle Buttons** to keep only circular button regions and make the rest transparent. Both operate on keypad artwork; neither sets the full lock-screen wallpaper.
3. Drag on the keypad preview to reposition the image and use the **Zoom & Framing** slider (0.5–3.0×). Zooming out too far or moving the image too far can expose transparent areas. Check the edges and the bottom row containing 0.
4. **Reset Position** restores the default zoom and centered position. **Change...** also resets framing when loading another image. **Remove** clears the whole creator, including any individual-key work.
5. Check that digits 0–9 have artwork, click **Export .passthm...**, and perform the local export check below.

To customize individual digits after slicing a poster, switch to **Individual Keys**, click **Fill from Poster**, and edit the desired digit. This copies every poster slice and overwrites existing individual artwork for those digits; it does not only fill empty keys. The copies are independent images. Reframing them applies the individual-key circular crop, so they no longer retain their original seamless tile shape.

## Workflow 2: a separate image for each digit

1. Select **Individual Keys**. Click an empty digit on the preview to choose an image, or drop an image onto that digit.
2. Select a configured key, use its **Key … Framing** slider to zoom, or drag directly on that key to pan. Only the selected digit changes.
3. **Change Image...** replaces that key's image, **Reset** resets its framing, and **Remove** removes its pending artwork. The context menu also provides change, reset, and clear actions.
4. Repeat for the other digits. Check for **10 of 10 keys configured** when creating a complete theme. The app allows export with just one configured key, so export availability does not mean all ten are present.
5. Export and re-import to inspect every digit. Missing digits are not filled automatically. Clearing artwork in the Mac editor does not restore an earlier theme on the phone.

**The white digits, letters, and outlines in the creator preview are UI overlays.** Image processing and export save the artwork slices without drawing those preview labels into the PNG files. If the design needs digits embedded in the images, add them to the source artwork and inspect the actual exported images.

## Edit an existing `.passthm`

1. Keep the original theme file. If the creator contains unsaved work, export it first, then use **Clear All** in **Theme Creator**. This avoids retaining old artwork when the next theme is missing some digits.
2. Switch to **Apply .passthm** and click **Choose .passthm File...** or drop in the file. The picker accepts `.passthm`, `.passtheme`, and `.zip`, but the archive must contain recognizable image assets; changing its extension cannot repair a broken archive.
3. After the name, asset count, and keypad preview load, click **Edit in Creator**. The app switches to **Individual Keys**.
4. Change the required digits, export with a new name such as `MyTheme-v2.passthm`, then re-import it to check.

The creator uses one preview image for each imported digit. It is **not a lossless archive editor**: separate language or font-weight variants for a digit are not retained as independent layers. Export rebuilds the package and its `_big` markers; custom package structure or an original `_small` marker is not guaranteed to survive. Reframing an imported rectangular poster tile also recrops it into a circle. Keep the original and compare the new package if those distinctions matter.

## Check the exported file locally

These steps check the file on the Mac without flashing a device:

1. Click **Export .passthm...**, save under a new name, and confirm the export success message. Export uses the active mode: poster slices in poster mode, individual artwork in individual-key mode.
2. Open **Apply .passthm → Choose .passthm File...** and choose the new file. Confirm its name, a nonzero asset count, and the expected artwork on each digit from 0 to 9.
3. To inspect the PNG files, duplicate the package in Finder, rename the **copy** to `.zip`, and extract it. Keep the original `.passthm` for importing. Check positioning, transparent edges, digit 0, and whether any expected text is actually in the image.
4. For **Failed to inspect .passthm file**, first check that the archive is complete and contains images; try a fresh export and import. The displayed asset count includes parsed/expanded filename variants. A large asset count does not establish that all ten digits are present.

The current **Export .passthm...** action uses the exporter's defaults: all supported languages, regular and bold filename variants, and both `TelephonyUI-10` and `TelephonyUI-9` directories. The language, font, and **Target** selectors do not narrow this manually saved package. Flashing directly from the creator instead stages a package using the selected language/font settings, then writes to the selected target directory or directories.

## Before flashing: language, bold text, and Target

Check the settings under **Flash & Language Target** against your iPhone. With a device connected, **Auto-detect** reapplies the detected settings:

| Setting | What it actually controls |
| --- | --- |
| **System Language** | Asset filenames to generate/write. A specific language also includes `other` fallback names. It does not change the iPhone's language. |
| **Font Weight / Style** | Regular, bold, or both filename variants, corresponding to iPhone **Bold Text**. It does not change the weight of text already in your image. |
| **Target** | The cache directory. Automatic mapping is iOS 18+ → `TelephonyUI-10`, iOS 16–17 → `TelephonyUI-9`, older → `TelephonyUI-8`. This mapping is not evidence of compatibility with those OS versions. |

Detection uses the first language-code segment (for example, `zh-Hans` → `zh`). If no menu entry matches, the existing language selection remains. If the bold-text value is unavailable, its previous selection remains too. The device helper may also fall back to `en` when no language value is available, so compare the result with the actual phone settings.

Three separate controls use the word **Universal**:

- **All Languages (Universal)** expands the language filenames supported by the code's locale list.
- **Universal (Regular + Bold)** produces both font-weight filenames using the same image data.
- **Target → Universal (All 8, 9, 10)** writes to all three cache directories. It means more files, not verified support for every iPhone or iOS version.

Choosing the correct specific language and font weight generally reduces the number of files written. The UI's “~600 files” text is an estimate; the actual count depends on the theme and targets. Universal settings do not solve every loading or compatibility problem.

**Importing a theme changes Target to the directory detected in that archive.** An archive containing both `TelephonyUI-9` and `-10` can be detected as `-9`. Import first, then use **Auto-detect** on the connected device and check the target, language, and bold-text settings. An archive directory name is not a detection result for the phone.

## Apply and record the result

Connect, unlock, and trust the iPhone. Check the preview and targets, then click **Flash Passcode Theme**. An app success message means the flashing workflow reported completion. Following the upstream instructions, restart the iPhone and inspect the lock-screen keypad. Record “flashing reported success” separately from “correct artwork displayed on the phone.” If you have not observed the latter, mark it **not tested**.

Use the [feedback template](compatibility-report-template.md) to report results. Wallet scanning does not establish passcode-theme compatibility, or vice versa; see the [compatibility records](COMPATIBILITY.en.md).

## Source references

- [AirCardApp.swift](../../AirCardApp.swift): `KeypadSlicer`, `PasscodeThemeExporter`, `applyDevicePreferences`, `inspectPasscodeTheme`, `editLoadedThemeInCreator`, `openSavePasscodeThemePanel`, and the creator UI.
- [aircard_backend.py](../../aircard_backend.py): archive parsing, previews, and target handling in `parse_passthm_archive`, `cmd_inspect_passthm`, and `cmd_flash_passthm`.
- [Sources/device_helper.m](../../Sources/device_helper.m): reads `Language` and `EnhancedTextLegibility`.
- [Upstream README](https://github.com/Mak5er/AirCard/blob/main/README.md#how-to-apply-lockscreen-passcode-themes-passthm): device refresh steps.
