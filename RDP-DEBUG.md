# RDP reconnect diagnostics (AltSnap 1.68)

Build: GitHub Actions → AltSnap x64 RDP debug → artifact AltSnap-x64-RDP-debug.

Exit the installed AltSnap using its tray menu. Extract the artifact into a writable folder, copy your existing AltSnap.ini there if desired, and run AltSnap.exe from that folder (PowerShell: `Set-Location <folder>; .\AltSnap.exe`). Keep the EXE and DLL together. Run with the same elevation as the original installation.

1. Verify Alt+drag works. Move the mouse and tap/release Alt.
2. Reproduce Smart Reconnect; note the time and test Alt+drag again.
3. Before restarting, wait at least 5 seconds, tap Alt and move the mouse again.
4. Toggle AltSnap off/on and repeat the test. Collect ad.log from the working directory.

RDP lines contain uptime in milliseconds, WTS event codes (3 remote connect, 4 remote disconnect, 7 lock, 8 unlock), hook installation/removal status, cumulative callback counts, interval maximum callback duration (GetTickCount resolution), Alt state/transition counts, and mouse watchdog attempts. Mouse callbacks may be absent when the mouse hook is intentionally not installed; synthetic/manual callback invocations can also contribute. A non-null hook handle is not proof the hook remains installed.

New callback instrumentation records counters only, not keystrokes or mouse coordinates. Existing LOG messages reached on the hook thread inside callbacks are suppressed; other upstream debug messages still use synchronous file I/O and may include window or configuration information. Review/redact ad.log before sharing. No recovery behavior is intentionally changed. This diagnostic build is not production-ready.
