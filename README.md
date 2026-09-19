KOBE OS 0.3
===========

A from-scratch 32-bit x86 operating system: its own two-stage bootloader,
kernel, desktop (windows, file manager, text editor, shell, calculator,
game of life), a real on-disk filesystem, a PE (.exe) loader that runs
small Windows console programs, and an updater that installs kernel
updates safely.

FILES
-----
  kobe-os-0.3.img   Raw disk image (6.1 MB). The real thing: persistent
                    (saved files and installed updates survive reboots).
  kobe-os-0.3.iso   Bootable ISO (20 MB) for CD/DVD, virtual machines, or
                    writing to a USB stick. Runs from RAM: nothing is ever
                    written to a real disk, and changes are lost at
                    power-off.
  SHA256SUMS.txt    Checksums for both files:  sha256sum -c SHA256SUMS.txt

REQUIREMENTS
------------
BIOS / legacy (CSM) boot on an x86 PC or VM, 32 MB+ RAM, VGA. There is no
UEFI boot. It has only been run in QEMU: NOT yet tested on real hardware.

TRY IT IN QEMU
--------------
  qemu-system-i386 -drive file=kobe-os-0.3.img,format=raw -m 32M
  qemu-system-i386 -cdrom kobe-os-0.3.iso -boot d -m 32M
(Startup takes several seconds while it loads its WiFi firmware and C#
runtime images into memory.)

PUT IT ON A USB STICK  (this ERASES the stick)
----------------------------------------------
  sudo dd if=kobe-os-0.3.img of=/dev/sdX bs=4M conv=fsync status=progress
Double-check /dev/sdX first (lsblk); dd will overwrite whatever it points
at. The .iso can be written the same way if you prefer the RAM-only mode.

WHAT'S IN IT
------------
- Desktop with draggable, resizable windows. Start menu: WELCOME, KOBE
  SHELL, UPDATE, SHUTDOWN, REBOOT. Desktop icons open the shell, the file
  manager, and the text editor.
- A real filesystem: the file manager lists actual files on disk; click a
  .TXT to edit (SAVE in the shell writes it), click a .EXE to run it.
  CRT.EXE and HELLO.EXE are included: ordinary compiler-built Win32
  console programs.
- The UPDATE window installs .KUP kernel packages into an inactive A/B
  slot; if a new kernel fails to start twice, the bootloader shows a
  repair screen and goes back to the old one. UIUPDATE.KUP (included in
  the filesystem) upgrades this v0.3 to a v0.4 with blue window title
  bars. "Check for updates" honestly reports that there is no network
  update client yet.
- Networking: DHCP/IP over Ethernet in QEMU. WiFi is experimental and
  does not work (see the project notes).

KNOWN LIMITS
------------
- In the .iso, SAVE and the updater work but only in RAM.
- .KUP updates replace the kernel only (not the bootloader or files).
- Windows programs: 32-bit console programs that use only the small set of
  kernel32/C-runtime functions implemented so far.
