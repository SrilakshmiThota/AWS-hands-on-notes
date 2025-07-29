
# Linux: Files, Directories, and Permissions

## 📄 1. Working with Files

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

## 📁 2. Working with Directories

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

## 🔐 3. File Permissions

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
