# venus-multirs-mppsolar-emulator
Venus OS service emulating Victron MultiRS for MPP Solar. Features DVCC control and SQLite history. Includes a smart watchdog: if the process stops or system shuts down, it forces a failsafe transition from USER mode back to the user's preferred mode (e.g., LIB), ensuring safe battery management whenever the emulator is inactive.
