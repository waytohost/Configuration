````md
# 📘 Website → GitHub Upload Workflow (Server Side)

### 🔹 1. Take Full Backup (MANDATORY)

### Website files:
```bash
tar -czvf site-backup.tar.gz public_html/
````

### Database:

```bash
mysqldump -u USER -p DB_NAME > db-backup.sql
```

---

## 🔹 2. Clean Old Git (if exists)

```bash
rm -rf .git
```

---

## 🔹 3. Generate SSH Key

```bash
ssh-keygen -t ed25519 -C git
```

---

## 🔹 4. Add SSH Key to GitHub Repo (Deploy Key)

### Get key:

```bash
cat ~/.ssh/id_ed25519.pub
```

### Add in GitHub:

```
Repo → Settings → Deploy Keys → Add Deploy Key
```

* Title: Server Key
* Key: (paste key)
* ☑ Allow write access

---

## 🔹 5. Verify GitHub Connection

```bash
ssh -T git@github.com
```

---

## 🔹 6. Create `.gitignore` (IMPORTANT)

```bash
nano .gitignore
```

### Add:

```
# Logs
*.log
writable/logs/
error_log

# Backups
*.gz
*.zip
backup/

# Uploads
uploads/
public/global_files/uploads/
s3/

# Cache
cache/

# System
node_modules/
vendor/

# Env
.env
wp-config.php
```

---

## 🔹 7. Initialize Git

```bash
git init
git add .
```

---

## 🔹 8. Set Git Identity

```bash
git config --global user.name "Vikas Gupta"
git config --global user.email "vikas@ephpsolutions.com"
```

---

## 🔹 9. Commit Code

```bash
git commit -m "Initial clean upload"
```

---

## 🔹 10. Setup Branch

```bash
git branch -M main
```

---

## 🔹 11. Add Remote Repo

```bash
git remote add origin git@github.com:USERNAME/REPO.git
```

---

## 🔹 12. Push to GitHub

```bash
git push -u origin main
```

---

# ⚠️ Large File Handling

## Check large files:

```bash
du -sh * | sort -h
```

## Remove from Git (keep on server):

```bash
git rm -r --cached uploads/
git rm -r --cached writable/logs/
```

## Commit cleanup:

```bash
git add .gitignore
git commit -m "Exclude large files"
git push
```

---

# 🔁 Daily Usage

```bash
git add .
git commit -m "update"
git push
```

---

# 🎯 Notes

* Always take backup before starting
* Always create `.gitignore` before `git add .`
* Do NOT upload logs, backups, uploads, cache
* GitHub limits: max file 100MB, recommended <50MB

```
```
