# Windows x64 build (experimental)

This branch contains a Windows build configuration for the LegionAura GUI and CLI.
It does not claim that the Windows USB driver path has been validated on hardware.

## Build using GitHub Actions

1. Push this source tree to a GitHub repository (or a `windows-port` branch).
2. Open **Actions → Windows build → Run workflow**.
3. Download the `LegionAura-Windows-x64` artifact when the workflow finishes.
4. Extract it and run `legionaura-gui.exe`.

The workflow uses MSVC, vcpkg, Qt 6 and libusb, then stages the Qt runtime DLLs
and `libusb-1.0.dll` beside the executables.

## Important USB-driver warning

The source communicates with the RGB controller through libusb interface 0.
On Windows, libusb normally needs a compatible driver such as WinUSB or libusbK
bound to that interface. The repository does not establish which Windows driver
is currently bound to the user's controller interface. Do not replace drivers
with Zadig until the exact VID/PID/interface has been identified and it is clear
that doing so will not detach the normal keyboard HID interface. The Windows
binary can therefore launch correctly while hardware access still fails.

## Known validation gap

This environment does not contain the Windows MSVC toolchain, Qt 6 Windows
packages, or a Windows USB device. The workflow is provided to perform the
actual Windows compilation; a successful compile and physical keyboard test
must be confirmed by its Actions result and on-device testing.
