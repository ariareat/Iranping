[README.md](https://github.com/user-attachments/files/34425100/README.md)
# ایران پینگ (IranPing) - minimal auto-connect Xray client

1. Open this folder in Android Studio (Ladybug or newer, JDK 17). Let it sync (it downloads Gradle + AGP).
2. Build > Build Bundle(s)/APK(s) > Build APK(s), choose the *release* variant
   (or run `./gradlew assembleRelease` after generating the wrapper).
   APK: app/build/outputs/apk/release/app-release.apk
3. Install, paste your Marzban subscription link once, allow the VPN prompt. From then on it connects when you tap the button
  . Inside it picks the fastest live config automatically (Xray leastPing balancer).

arm64-v8a only (libhev-socks5-tunnel.so was provided for arm64).

## Build without Android Studio (GitHub Actions)
Upload this folder (including the hidden .github folder) to a private GitHub repo.
Open the Actions tab > "Build APK" > latest run > download the AutoVPN-apk artifact.

## Signing with your own key (recommended, fewer Play Protect warnings)
1. Create a keystore once and KEEP IT (updates must be signed with the same key):
   keytool -genkeypair -v -keystore iranping.jks -alias iranping -keyalg RSA -keysize 2048 -validity 10000
2. Create keystore.properties next to settings.gradle.kts:
   storeFile=iranping.jks
   storePassword=YOUR_PASSWORD
   keyAlias=iranping
   keyPassword=YOUR_PASSWORD
3. Build > Generate APKs (release). Do not commit keystore files.

## Self-update from GitHub (every 48 hours)
Repo: ariareat/Iranping. The app reads the newest release tag from github.com/ariareat/Iranping/releases/latest
and downloads https://github.com/ariareat/Iranping/releases/latest/download/IranPing.apk
(through the VPN when it is connected).
For every release:
1. Raise `versionCode` and `versionName` in app/build.gradle.kts (e.g. "1.1").
2. Build the signed APK with the SAME keystore and name the file exactly `IranPing.apk`.
3. Create a GitHub Release with tag `v1.1` (higher than the installed versionName) and attach IranPing.apk.
