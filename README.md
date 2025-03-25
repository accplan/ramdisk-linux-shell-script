# 📁 RAM Disk Creator & Backup Tool
*A Bash script to create a RAM disk with automatic backup/restore functionality.*

## 🔥 Features
✅ **RAM Disk in `/dev/shm`** - Fast, volatile memory storage  
✅ **Automatic Backups** - Safely persists data to disk (with `.bak` rollback protection)  
✅ **User Ownership** - Mount point always belongs to your user (no `sudo` needed for files)  
✅ **Periodic Flushing** - Auto-backup every 30 minutes  
✅ **Resilient Recovery** - Restores from backup if available  

## 🛠️ Installation
Run these commands:
```sh
git clone https://github.com/accplan/ramdisk-linux-shell-script.git
cd ramdisk-linux-shell-script
chmod +x ramdisk
```

## 🚀 Usage
### Basic Syntax
```sh
./ramdisk --mount=/path/to/mount --size=1G --backup=/path/to/backup.img
```

### Options
--mount=   Mount point directory (required)   Example: --mount=/mnt/ramdisk  
--size=    RAM disk size (e.g., 1G, 512M)    Example: --size=2G  
--backup=  Backup file path (required)       Example: --backup=~/ramdisk.img  

## ⚙️ How It Works
1. Creates a loop device in `/dev/shm` (RAM)  
2. Formats as `ext4` (or restores from backup)  
3. Mounts with user ownership (`chown` applied)  
4. Periodic backups:  
   - Temporary → `.1` file (atomic write)  
   - On success: rotates `.bak` → main backup  
   - On failure: cleans up `.1` without touching backups  

## 🧹 Cleanup
The script handles SIGINT/SIGTERM to:
1. Flush changes to backup  
2. Unmount the RAM disk  
3. Delete temporary files  
*(Just Ctrl+C to exit safely)*

## 📜 License
GLWTS © Deepseek R1

## 💡 Pro Tip
Add this to your bashrc/zshrc for quick access:
```sh
alias ramdisk='/path/to/ramdisk'
```
