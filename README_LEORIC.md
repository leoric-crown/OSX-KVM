# macOS VM Auto-Boot Configuration (OSX-KVM + OpenCore)

This VM is configured to **automatically boot into macOS** without requiring manual selection in the OpenCore boot picker.

## Configuration Summary

- **Base Repo**: https://github.com/kholia/OSX-KVM
- **Bootloader**: OpenCore
- **Host OS**: Linux (Pop!\_OS 24.04 with QEMU/KVM)

## Key Settings

### 1. Startup Disk

The default startup disk was set **from within macOS**:

**System Settings → General → Startup Disk → Macintosh HD → Restart**

This sets the NVRAM boot entry, allowing OpenCore to boot directly into macOS on every VM launch.

### 2. OpenCore `config.plist` Tweaks

To mount the OpenCore partition of a working VM install, you can use the following commands:

```bash
sudo guestmount -a ~/vms/OSX-KVM/OpenCore/OpenCore.qcow2 -m /dev/sda1 /mnt/OpenCore
sudo micro /mnt/OpenCore/EFI/OC/config.plist
sudo guestunmount /mnt/OpenCore
```

Located at: `EFI/OC/config.plist`

```xml
Misc:
  Boot:
    Timeout: 2             # 2-second timeout before auto-booting
    ShowPicker: true       # Set to false if you want to hide the picker entirely
  Security:
    AllowSetDefault: true  # Enables Ctrl+Enter to set default entry (for fallback)
```

### Notes

If boot picker reappears unexpectedly, verify that NVRAM is being saved properly.

You can re-select a new startup disk from macOS at any time.

Press Esc or Option at boot to interrupt auto-boot and show the OpenCore picker.

### Optional

Once stable, consider setting:

```xml
Misc → Boot → ShowPicker: false
```

To hide the picker completely for a clean Mac-like boot

### Last updated: 2025-07-08
