# create a mount location
create a directory as mount location:
```bash
sudo mkdir /mnt/new_drive
```
# mount `local storage` / `drive`
```bash
sudo mount -t drvfs d: /mnt/new_drive
```

# mount network storage
```base
mount -t drvfs '\\server\share' /mnt/new_drive
```
