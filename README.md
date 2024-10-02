# SecurePatchedEmulator
SecurePatchedEmulator provides a prebuilt Android emulator image with the latest security patches, enabling security researchers to easily access a secure, reliable, and up-to-date Android environment for testing and development purposes. Before using SecurePatchedEmulator, ensure that you have the latest version of the Android SDK Platform-Tools and Android Emulator installed.



### Downloads

|  Version  | Security Patch |                        Download Link                         |
| :-------: | :------------: | :----------------------------------------------------------: |
| 14.0.0_r1 |    2024-06     | [android-14.0.0_r1.zip](https://github.com/cxxsheng/SecurePatchedEmulator/releases/download/2024-06/android-14.0.0_r1.zip) |
| 13.0.0_r1 |    2024-06     | [android-13.0.0_r1.zip](https://github.com/cxxsheng/SecurePatchedEmulator/releases/download/2024-06/android-13.0.0_r1.zip) |
| 12.0.0_r1 |    2024-06     | [android-12.0.0_r1.zip](https://github.com/cxxsheng/SecurePatchedEmulator/releases/download/2024-06/android-12.0.0_r1.zip) |

### Usage
To set up and launch the SecurePatchedEmulator, follow these steps:
1. Download the desired Android emulator image. For example, to launch `android-14.0.0_r1`:

   ```shell
   wget https://github.com/cxxsheng/SecurePatchedEmulator/releases/download/2024-06/android-14.0.0_r1.zip
   ```

2. Extract the downloaded zip file to the desired output directory:

   ```shell
   unzip android-14.0.0_r1.zip -d /path/to/emulator/
   ```

   Replace `/path/to/emulator` with the actual path where you want to extract the emulator files.

3. Launch the emulator, specifying the path to the extracted emulator directory:

   ```shell
   cd /path/to/sdk/tools
   ./emulator @YourAVDName -sysdir /path/to/emulator/x86_64
   ```
   
   Replace `YourAVDName` with the name of your Android Virtual Device (AVD) and `/path/to/emulator` with the actual path where you extracted the emulator files. Ensure that the Android version used to create `YourAVDName` matches the Android version of the emulator image.

### Building
To create a secure Android emulator image with the latest security patches, follow these steps:

0. Initialize and sync the AOSP repository:
   ```shell
   repo init -u https://android.googlesource.com/platform/manifest -b android-14.0.0_r1
   repo sync
   ```
   Replace `android-14.0.0_r1` with the desired Android version.

1. Identify the latest security patch tag from the[ `platform_build`](https://github.com/aosp-mirror/platform_build/tags ) repository on GitHub. For example, [`android-security-14.0.0_r9`](https://github.com/aosp-mirror/platform_build/releases/tag/android-security-14.0.0_r9):

2. Fetch the security patch tag across all AOSP repositories:

   ```shell
   2. repo forall -p -c 'git fetch aosp android-security-14.0.0_r9 --depth 1'
   ```

   Replace `android-security-14.0.0_r9` with the actual security patch tag.

3. Generate a diff file to review the changes between the current HEAD and the security patch:

   ```shell
   repo forall -p -c 'git log --oneline HEAD..FETCH_HEAD' > 14.0.0_r1-to-14.0.0_r9.diff.txt
   ```

4. Merge the security patch into the AOSP repositories:

   ```shell
   repo forall -p -c 'git merge FETCH_HEAD'
   ```

5. Modify the security patch date in the emulator image:

   In the `build/make/core/version_defaults.mk` file, locate the `ro.build.version.security_patch` variable and change its value to the desired security patch date, e.g., `2024-06-05`.

6. Build the Android emulator image: 

   For Android 13 and above:

   ```shell
   source ./build/envsetup.sh
   lunch sdk_phone_x86_64-userdebug
   make emu_img_zip -j$(nproc)
   ```

   This generates an `sdk-repo-linux-system-images-eng.[username]].zip` file.

   For Android 12 and below:

   ```shell
   source build/envsetup.sh
   lunch sdk_phone_x86_64-userdebug
   make -j$(nproc) sdk sdk_repo
   ```

   The `make sdk sdk_repo` command creates two files under `aosp-main/out/host/linux-x86/sdk/sdk_phone_x86_64`:

   - `sdk-repo-linux-system-images-eng.[username].zip`
   - `repo-sys-img.xml`

### Postscript

After `android-14.0.0_r29`, using `lunch sdk_phone_x86_64-userdebug` will result in the following error:

```shell
Valid combos must be of the form <product>-<release>-<variant>
```

After consulting Google's documentation, it was found that the `trunk_staging` option needs to be added, but using `lunch sdk_phone_x86_64-trunk_staging-userdebug` will result in the following error:

```shell
In file included from build/make/core/config.mk:380:
In file included from build/make/core/envsetup.mk:369:
build/make/core/product_config.mk:226: error: Cannot locate config makefile for product "sdk_phone_x86_64"
```

I found that the `target/product/sdk_phone_x86_64.mk` file no longer exists, and I looked at the [commit](https://cs.android.com/android/_/android/platform/build/+/4bf479f6057ad532c792e26d3d958a8b50fc1f02) that describes the changes:

```diff
Retire obsolete emulator targets and boards

the sdk_phone* targets are replaced with sdk_phone64*,
the emulator* boards are replaced with emu64* in
the device/generic/goldfish folder.

Bug: 295259752
Test: presubmit
Change-Id: I069a06baf02aea30db617f183abbfeaa6f968f29
Signed-off-by: Roman Kiryanov <rkir@google.com>
```

Therefore, the correct command to use should be:

```shell
lunch sdk_phone64_x86_64-trunk_staging-userdebug
```


###  Reference：

1. [Merging AOSP Security Patches into Custom ROMs | Aditya Telange.](https://adityatelange.in/blog/aosp/merge-security-patches-aosp/)
2. [Use Android Emulator virtual devices | Android Open Source Project.](https://source.android.com/docs/setup/create/avd#build_avd_images)
