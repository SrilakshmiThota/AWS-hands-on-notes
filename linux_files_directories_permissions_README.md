
# Linux: Files, Directories, and Permissions

This README provides a detailed understanding of Files, Directories, File System concepts, and Permissions in Linux. Useful for beginners, DevOps, and AWS learners.

---

## 📁 1. Linux File System Overview

- Linux uses a **hierarchical file system** starting from the root `/`.
- All files, directories, devices, and mounts are placed under `/`.

### 🔹 Common Directories

| Directory | Purpose |
|----------|---------|
| `/`      | Root directory |
| `/home`  | User home directories |
| `/etc`   | System configuration files |
| `/var`   | Log files and variable data |
| `/tmp`   | Temporary files |
| `/bin`   | Essential binaries |
| `/usr`   | User-installed software |
| `/mnt`   | Mount point for external devices |

---

## 📄 2. Working with Files

### Create a file
```bash
touch filename.txt
```

### View contents of a file
```bash
cat filename.txt
less filename.txt
head -n 10 filename.txt
tail -n 10 filename.txt
```

### Edit a file
```bash
nano filename.txt
vim filename.txt
```

### Delete a file
```bash
rm filename.txt
```

---

## 📁 3. Working with Directories

### Create a directory
```bash
mkdir myfolder
```

### Create nested directories
```bash
mkdir -p parent/child/subchild
```

### List contents
```bash
ls
ls -l
ls -a
```

### Change directory
```bash
cd /path/to/folder
cd ~       # Go to home directory
cd ..      # Go one level up
```

### Remove a directory
```bash
rmdir foldername        # Only if empty
rm -r foldername        # Recursive delete
```

---

## 🔐 4. File Permissions

### Syntax:
```bash
-rwxr-xr-- 1 user group size date time filename
```

| Symbol | Meaning        |
|--------|----------------|
| `-`    | File |
| `d`    | Directory |
| `r`    | Read |
| `w`    | Write |
| `x`    | Execute |

### Change Permissions

#### Using `chmod` (symbolic):
```bash
chmod u+x file.sh   # Add execute to user
chmod g-w file.txt  # Remove write from group
```

#### Using `chmod` (numeric):
```bash
chmod 755 file.sh   # rwxr-xr-x
chmod 644 file.txt  # rw-r--r--
```

### Change Owner/Group

```bash
chown user file.txt
chown user:group file.txt
```

---

## 💡 Bonus: File System Commands

```bash
df -h       # Disk usage
du -sh *    # Size of each item in current directory
mount       # Show mounted file systems
umount      # Unmount a file system
lsblk       # List block devices (disks)
```

---

## ✅ Summary

Understanding Linux file system structure, managing files/directories, and handling permissions is essential for:
- AWS EC2/Linux administration
- DevOps tasks (like Docker volumes, Kubernetes mounts)
- System troubleshooting and scripting

---

> Created for educational purpose. Suitable for GitHub README upload.
