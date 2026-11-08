# Running on Android with Termux

AssetRipper does not have a native Android build. On ARM64 Android devices, you can run the Linux ARM64 release inside Ubuntu using [PRoot-Distro](https://github.com/termux/proot-distro). The Linux release needs a glibc-based environment and will not run directly in Termux.

## Setup

1. Install Termux from [F-Droid](https://f-droid.org/packages/com.termux/) or the [official GitHub releases](https://github.com/termux/termux-app/releases).
2. In Termux, install PRoot-Distro and grant access to shared storage:

   ```sh
   pkg update
   pkg install proot-distro
   termux-setup-storage
   proot-distro install ubuntu
   ```

3. Enter Ubuntu with Termux's home directory shared:

   ```sh
   proot-distro login --shared-home ubuntu
   ```

4. Download and extract the [Linux ARM64 release](https://github.com/AssetRipper/AssetRipper/releases/latest/download/AssetRipper_linux_arm64.zip):

   ```sh
   apt update
   apt install -y curl unzip
   cd /root
   curl -L -o AssetRipper_linux_arm64.zip https://github.com/AssetRipper/AssetRipper/releases/latest/download/AssetRipper_linux_arm64.zip
   mkdir AssetRipper
   unzip AssetRipper_linux_arm64.zip -d AssetRipper
   cd AssetRipper
   chmod +x AssetRipper.GUI.Free
   ```

5. Start AssetRipper:

   ```sh
   ./AssetRipper.GUI.Free --port 8080 --headless
   ```

   Open `http://127.0.0.1:8080` in the Android browser. Keep the Termux session running while using AssetRipper.

## Files and limitations

- After `termux-setup-storage`, shared Android files are available under `/root/storage/shared` inside Ubuntu. In the Commands page, enter the full path to the game files or export folder; native file picker dialogs are not available in this setup.
- This setup is intended for ARM64 devices. AssetRipper can use substantial memory and storage when processing or exporting large games; Android may stop the process if the device runs low on resources.
- The server listens on the device's loopback address, so it is available from the Android browser on that device and is not exposed to other devices on the network.
