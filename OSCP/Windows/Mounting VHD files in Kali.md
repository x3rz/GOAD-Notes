
```bash
sudo apt install libguestfs-tools

sudo apt install cifs-utils


mkdir /tmp/backup

mount -t cifs  //10.10.10.134/backups /tmp/backup -o rw

mkdir /tmp/vhd

guestmount --add /tmp/backups/path/to/vhdfile.vhd --inspector --ro /tmp/vhd -v
```