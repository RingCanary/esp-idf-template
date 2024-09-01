Using WSL2 does not exhibit [path length issues]([Error: Too long output directory · Issue #252 · esp-rs/esp-idf-sys (github.com)](https://github.com/esp-rs/esp-idf-sys/issues/252)); furthermore, using WSL2 reduces the waiting time between command line cargo invocations and Rust Analyzer operating on the same projects

### Setup
1. [WSL2 setup guide](https://learn.microsoft.com/en-us/windows/wsl/install)
2. Install Ubuntu App - setup and update the packages
   - Follow [Rust on ESP](https://docs.esp-rs.org/book/) & [ESP-IDF Toolchain setup guide](https://docs.esp-rs.org/book/) for toolchain setup
3. Configure USB access in WSL2
   - Install [usbipd-win](https://github.com/dorssel/usbipd-win/releases) on Windows
   - In your WSL2 terminal, run:
     ```bash
     sudo apt install linux-tools-generic hwdata
     sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*-generic/usbip 20
     ```

## Using esp-idf-template with WSL2

1. Setup new project 

4. Connect ESP32 Device
   - [Link to windows doc](https://learn.microsoft.com/en-us/windows/wsl/connect-usb)
   - Plug in your ESP32 device
   - In PowerShell (as Administrator), run:
     ```powershell
     usbipd list
     usbipd bind --busid <busid>
     ```
   - Note the bus ID of your ESP32 device, then attach it to WSL2:
     ```powershell
     usbipd attach --wsl --busid <busid>
     ```

(cargo run OR > )
6. Flash the device:
   ```bash
   lsusb (check if device has attached)
   espflash flash target/xtensa-esp32-espidf/debug/<your-project-name>
   ```

7. Monitor the device:
   ```bash
   espflash monitor
   ```

## Troubleshooting

- If you encounter permission issues with the USB device, try running the flash and monitor commands with `sudo`.
- Ensure you've sourced the ESP-IDF environment in each new terminal session:
  ```bash
  source ~/export-esp.sh
  ```
- If you face any issues with USB device recognition, try restarting the WSL2 instance or your computer.
