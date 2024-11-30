# Secure Erase Mac SSD

Follow these steps to securely erase your Mac SSD:

_This procedure serves as a **calming measure** 💊 if you find it odd that your SSD can be securely erased in just seconds_ 😉


## TL;DR:

1. **Encrypt and Erase**: Use **Disk Utility** to erase the SSD with **APFS (Encrypted)** format.
2. **Overwrite with Random Data**: Run a script in **Terminal** to fill the SSD with random data until full.
3. **Erase Again**: Use **Disk Utility** to erase the SSD again, 🚮 **discarding the encryption key** ✅ .

Your SSD is now securely erased and all data traces are removed.

---
## Full Procedure
### 1. Boot into **Recovery Mode**
- Restart your Mac and hold down **Command (⌘) + R** until you see the Apple logo or spinning globe.

---

### 2. Use **Disk Utility** to Erase the Disk
1. Select **Disk Utility** from the macOS Utilities window.
2. Choose **Macintosh HD** → **Erase**.
3. Select **APFS (Encrypted)**, set a secure password, and confirm the erasure.

---

### 3. Prepare for Secure Overwriting
1. Close **Disk Utility**.
2. Open **Safari** from the macOS Utilities window.
3. Navigate to the URL: [https://github.com/tbsbdr/Secure-erase-Mac-SSD/](https://github.com/tbsbdr/Secure-erase-Mac-SSD/).
4. Copy the following script:

   ```bash
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
5. Close **Safari**.

---

### 4. Execute the Script
1. Open **Utilities** → **Terminal**.
2. Create the script file using `vi`:
   ```bash
   vi erase.sh
   ```
 3. Enter the script into **vi**:
   - Press `i` to enter **Insert Mode**.
   - Paste the copied script (`Command + V`).
   - Save the file:
     - Press `ESC`.
     - Type `:wq`, then press **Enter**.

4. Make the script executable:
   ```bash
   chmod +x erase.sh
   ```
5.	Run the script:
   ```bash
   ./erase.sh
   ```
6.	Wait for approximately 5 minutes (or longer, depending on SSD size) for the script to complete.

---

### 5. Finalize the Erasure
1.	Open **Disk Utility** again.
2.	Select **Macintosh HD** → **Erase**.
3.	Choose **APFS (Encrypted)** or any format of your choice.
	- This step discards the encryption key set earlier, effectively scrambling the SSD data.

### Done! ☑️
Your SSD is securely erased, and all data traces have been removed.
