# SecurePatchedEmulator
SecurePatchedEmulator provides a prebuilt Android emulator image with the latest security patches, enabling security researchers to easily access a secure, reliable, and up-to-date Android environment for testing and development purposes. Before using SecurePatchedEmulator, ensure that you have the latest version of the Android SDK Platform-Tools and Android Emulator installed.



### Downloads

|  Version  | Security Patch |                        Download Link                         |
| :-------: | :------------: | :----------------------------------------------------------: |
| 14.0.0_r1 |    2024-06     | [android-14.0.0_r1.zip](https://github.com/cxxsheng/SecurePatchedEmulator/releases/download/2024-06/android-14.0.0_r1.zip) |

### Usage
To set up and launch the SecurePatchedEmulator, follow these steps:
1. Download the desired Android emulator image. For example, to launch android-14.0.0_r1:

   ```shell
   wget https://github.com/cxxsheng/SecurePatchedEmulator/releases/download/2024-06/android-14.0.0_r1.zip
   ```

2. Extract the downloaded zip file to the desired output directory:

   ```shell
   unzip android-14.0.0_r1.zip -d /path/to/emulator/
   ```

   Replace `/path/to/emulator` with the actual path where you want to extract the emulator files.

3. Launch the emulator, specifying the path to the extracted emulator directory and the desired AVD name:

   ```shell
   emulator @YourAVDName -sysdir /path/to/emulator/x86_64
   ```

   Replace `YourAVDName` with the name of your Android Virtual Device (AVD) and `/path/to/emulator` with the actual path where you extracted the emulator files.
