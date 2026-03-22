
To split (decompile) and rebuild an Android application (APK), the industry-standard tool is **Apktool**. It allows you to decode resources to nearly their original form and rebuild them after making modifications. 

Here is a straightforward guide on how to safely decompile, edit, and rebuild an APK for educational, debugging, or localization purposes. 



### Prerequisites
Before you start, you will need to have a few tools installed on your computer:
> $`sudo apt install apktool zipalign apksigner`

1.  **Java Development Kit (JDK):** `apktool` requires Java to run (Java 8 or higher).
2.  **Apktool:** The main program used for decompiling and rebuilding.
3.  **Android SDK Build-Tools:** You will need this for `zipalign` and `apksigner` to optimize and sign the rebuilt APK so it can actually be installed on a device.

---

### Step 1: Split (Decompile) the APK
Once you have Apktool installed and added to your system's PATH, open your terminal or command prompt.

To decode the APK, use the following command:
```bash
apktool d your_app.apk
```
* `d` stands for decode.
* This will create a new folder named `your_app` in your current directory containing the decompiled files. 

Inside this folder, you will find the `AndroidManifest.xml`, the `res` folder (containing layouts, strings, and images), and the `smali` folder (which contains the disassembled Java code).

### Step 2: Make Your Modifications
At this stage, you can modify the app. Common changes include:
* Editing XML files in the `res/values` folder to change text or translations.
* Swapping out images in the `res/drawable` folders.
* Modifying the `AndroidManifest.xml` to change permissions or app configurations.
* Editing the `.smali` files to change the app's logic (though this requires understanding Dalvik bytecode).

### Step 3: Rebuild the APK
Once you are done making your changes, you need to repackage the folder back into an APK. 

Run the following command:
```bash
apktool b your_app -o rebuilt_app.apk
```
* `b` stands for build.
* `your_app` is the directory created in Step 1.
* `-o rebuilt_app.apk` specifies the name of the new APK file you want to output.

### Step 4: Zipalign and Sign the APK
Android devices will refuse to install an APK that is not properly aligned and cryptographically signed.

**1. Zipalign:**
This optimizes the APK so that it interacts efficiently with the Android OS.
```bash
zipalign -p -f 4 rebuilt_app.apk aligned_app.apk
```

**2. Sign the APK:**
You need a keystore to sign the app. If you don't have one, you can generate a debug keystore using Java's `keytool`:
```bash
keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-alias
```
Then, sign the aligned APK using `apksigner` (found in your Android SDK build-tools folder):
```bash
apksigner sign --ks my-release-key.jks --out final_ready_to_install.apk aligned_app.apk
```

*Note: Always ensure you have the legal right to reverse-engineer or modify an application, as modifying third-party intellectual property without permission can violate Terms of Service or copyright laws.*

---
