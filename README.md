# Secure erase Mac SSD
1. Enter **Recovery Mode** of your Mac
1. Select **Disk Utility**
1. Select **Macintosh HD** → **Erase**
1. Select **APFS (Encrypted)** → set a password and erase disk.
1. (Code Disk Utility)
1. Select **Safari**
1. Open URL: https://github.com/tbsbdr/Secure-erase-Mac-SSD/
1. Copy this script:
````
#!/bin/bash

# Define the mount point for the target disk
MOUNT_POINT="/Volumes/Macintosh HD"

# Check if the mount point exists
if [ ! -d "$MOUNT_POINT" ]; then
    echo "Error: Mount point $MOUNT_POINT does not exist. Ensure /dev/disk3s1 is mounted."
    exit 1
fi

# Change directory to the mount point
cd "$MOUNT_POINT" || exit

# Set file index for naming
file_index=1

# Loop to write 1GB files until the storage is full
while :; do
    file_name="random_data_$file_index.bin"

    # Use 'dd' to generate random data and write it to a file
    echo "Creating $file_name..."
    dd if=/dev/urandom of="$file_name" bs=1M count=1024 status=progress

    # Check if the write was successful
    if [ $? -ne 0 ]; then
        echo "Error writing file. Likely the storage is full."
        break
    fi

    ((file_index++))
done

echo "Storage is full or an error occurred. Written $((file_index-1)) files."
exit 0
```
1. (Close Safari)
1. Select Utilities → Terminal
1. Type `vi erase.sh` (confirm the warning)
1. Paste the script ini the **vi** editor as follows:
1.1. Type `i` (insert mode)
1.1. Paste the script (`command` + `v`)
1.1. Save, press `ESC`
1.1. Enter `:wq` then press `enter`
1. Make the script file executable: `chmod +x ersae.sh`
1. Execute the script: `./erase`
1. (wait for ca. 5 min, depending on your ssd size)
1. (exit terminal)
1. Select **Disk Utility**
1. Select **Macintosh HD** → **Erase**
1. Select **APFS (Encrypted)** (or whatever you like) This throws away the encyption key we set at the beginning which again scambles the ssd.
1. Done ☑️ Your data traces are gone.