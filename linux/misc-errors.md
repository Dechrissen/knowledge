# Miscellaneous errors I've run into on Linux

1. System fails to boot / boots into emergency mode
    - `/etc/fstab` was using partition names for each partition (e.g. `/dev/nvme0n1p1`) rather than UUID
    - Make sure that UUID is used to identify each partition
    - the issue was that I had added a second drive for games, and the drive names got reassigned on boot, so `nvme0n1` became `nvme1n1`
    - when the system tried to boot, the new drive did not have a `/boot/efi` partition on it
    - needed to edit `/etc/fstab` in emergency mode, change to UUID for each partition, then `systemctl daemon-reload` and `mount -a` and `reboot`
