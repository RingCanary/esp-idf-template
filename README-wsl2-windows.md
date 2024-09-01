Using WSL2 does not exhibit [path length issues]([Error: Too long output directory · Issue #252 · esp-rs/esp-idf-sys (github.com)](https://github.com/esp-rs/esp-idf-sys/issues/252)); furthermore, using WSL2 reduces the waiting time between command line cargo invocations and Rust Analyzer operating on the same projects

## Setup
1. Follow [WSL2 setup guide](https://learn.microsoft.com/en-us/windows/wsl/install) and [wsl2 development enviroment setup guide](https://learn.microsoft.com/en-gb/windows/wsl/setup/environment#file-storage)
2. Install a linux distro as per the guide or Ubuntu App; - setup and update the packages
   - Follow [Rust on ESP](https://docs.esp-rs.org/book/) & [ESP-IDF Toolchain setup guide](https://docs.esp-rs.org/book/) for toolchain setup
3. Configure USB access in WSL2
   - Install [usbipd-win](https://github.com/dorssel/usbipd-win) on Windows
   - (optional) In your WSL2 terminal, run:
     ```bash
     sudo apt install linux-tools-generic hwdata
     sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*-generic/usbip 20
     ```

## Flashing an ESP32 target from WSL2

1. Launch WSL2 from powershell and run your linux distro or launch the Ubuntu app as installed from Microsft store
2. Create a new project folder in your $HOME dir.
3. [Cargo generate](https://docs.esp-rs.org/book/writing-your-own-application/generate-project/index.html) new project
4. Build the project; run **cargo build** or **cargo b**
5. Open a Powershell terminal, from start, as admin; run the [usbipd commands](https://learn.microsoft.com/en-us/windows/wsl/connect-usb) to share the usb device with the linux distro.
6. Open linux term and check attached usb devices using **lsusb**
7. Check attached target with **espflash board-info**
8. Flash the device:
   ```bash
   lsusb (check if device has attached)
   espflash flash target/xtensa-esp32-espidf/debug/<your-project-name>
   ```
   OR
   ```bash
   espflash board-info
   cargo run (from project dir)
   ```

7. Monitor the device:
   ```bash
   espflash monitor
   ```

## Notes
- Once a usb device is bound; it won't loose status till a disconnect command > usbipd detach --busid <busid> is run from windowns powershell; even after system restart,
- Although the device will get detached from wsl2 after a few minutes of staying idle, when system resets or WSL2 is exited, thus you will have to run **> usbipd attach --wsl --busid <busid>** in every such case.
- It is recommended to create the project in the WSL2 hosted environment only as calling project form C:\Users\<UserName>\Project or /mnt/c/Users/<UserName>/Project$ will lead to performance issue
- Calling project form C:\Users\<UserName>\Project or /mnt/c/Users/<UserName>/Project$ also casues esp-idf build to fail with OS Error 2
