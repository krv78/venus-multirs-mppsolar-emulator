# venus-multirs-mppsolar-emulator

Venus OS service for MPP Solar / Voltronic inverters. This project emulates a **Victron MultiRS** device, allowing seamless integration of non-Victron inverters into the Victron Energy ecosystem (Cerbo GX, Venus GX, etc.).

## 🚀 Key Features

*   **MultiRS Emulation**: Full integration into the Venus OS DBus, appearing as a native MultiRS inverter.
*   **DVCC Control**: Dynamic management of charge currents and voltages based on Victron's DVCC logic.
*   **Safety Watchdog**: A smart background process that monitors the emulator and system state.
*   **Failsafe Logic**: Automatically forces a transition from **USER mode** (PBT02) back to the **user's preferred mode** (e.g., LIB) if the process stops or the system shuts down.
*   **SQLite History**: Built-in logging and settings management using an SQLite database.
*   **Easy Config**: Management via `config.ini` for port settings and safe-mode definitions.

## 🛡 Safety & Watchdog Logic

The system is designed with a "Safety First" approach using a dedicated Watchdog script:

1.  **Event 00/A (Module Failure)**: If `dbus-multirs-emulator.py` is not running, the Watchdog checks the inverter state. If it's not in the **user's preferred mode**, it forces the switch (e.g., to LIB) to ensure the battery BMS or internal logic takes back control.
2.  **Event B (System Shutdown)**: On Venus OS shutdown, the Watchdog performs an aggressive port intercept (using `fuser -k`) and sends the command to restore the user's defined safe mode before the USB power is cut.
3.  **Automatic Recovery**: When the emulator starts, it restores the **USER mode** (PBT02) only if DVCC is active and the system is ready for external control.

## 📂 Project Structure

*   `dbus-multirs-emulator.py`: Main service script.
*   `watchdog.py`: Background monitor and failsafe executor.
*   `config.ini`: User configuration (ports, safe modes, intervals).
*   `create_demo_db.py`: Database initialization tool for history logging.
*   `db_clean.py`: Database maintenance and reset utility.
*   `install.sh` / `uninstall.sh`: Scripts for easy installation and removal.
*   `restart.sh`: Quick service restart utility.
*   `run-emulator` / `run-watchdog`: Service execution scripts for daemontools.

## 🛠 Installation

1.  Access your Venus OS device via SSH.
2.  Navigate to the data directory:
    ```bash
    cd /data/etc
    ```
3.  Clone this repository:
    ```bash
    git clone https://github.com
    cd venus-multirs-mppsolar-emulator
    ```
4.  Run the installer:
    ```bash
    bash install.sh
    ```

## 🤝 Credits & Inspiration

This project is standing on the shoulders of giants. Special thanks to:

*   [mr-manuel](https://github.com) — for the excellent architectural foundation of the dbus emulator.
*   [DarkZeros](https://github.com) — for the comprehensive implementation of the MPP Solar protocol.
*   The **Victron Energy** community for their open-source approach and detailed DBus documentation.

## 📝 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.  
Copyright (c) 2024 krv78
