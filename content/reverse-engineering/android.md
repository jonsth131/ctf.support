---
Title: Android
---

## APK 
APK (Android Package) files are the standard format for applications used on the Android platform.

[JADX](https://github.com/skylot/jadx) is a command line and GUI decompiler for Android Dex and Apk files.

[APKTool](https://ibotpeaches.github.io/Apktool/) is a tool for reverse engineering and modifying APK files.

## AAB
AAB (Android App Bundle) is a more recent format that supports multiple APKs for different platforms, locales and screen sizes.

To extract the APKs from an AAB file, [bundletool](https://developer.android.com/tools/bundletool) can be used.

```bash
bundletool build-apks --bundle=/path/to/app.aab --output=/path/to/output.apks
```

To extract the APKs from the APKS file, use a tool like 7zip.
