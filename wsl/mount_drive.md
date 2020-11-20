# mount drive

## create a mount location
create a directory as mount location:
```bash
sudo mkdir /mnt/new_drive
```
## mount `local storage` / `drive`
```bash
sudo mount -t drvfs d: /mnt/new_drive
```

## mount network storage
```bash
mount -t drvfs '\\server\share' /mnt/new_drive
```

## reference
- [File System Improvements to the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/archive/blogs/wsl/file-system-improvements-to-the-windows-subsystem-for-linux)
- [Get started mounting a Linux disk in WSL 2 (preview)](https://docs.microsoft.com/en-us/windows/wsl/wsl2-mount-disk)
