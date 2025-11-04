````markdown
# 🧭 Complete GitHub & Jenkins Deployment Guide

## 🧩 Part 1 — Create and Configure Your GitHub Repository

### **1. Login to GitHub**
- Go to [GitHub.com](https://github.com)  
- Sign in with your credentials.

---

### **2. Create a New Repository**
1. Navigate to your **profile or organization**.  
2. Click **Repositories → New** (or use the **“+” → New repository** option).  
3. Fill out:  
   - **Repository Name**  
   - **Description** *(optional)*  
   - **Visibility:** select **Private** ✅  
4. Click **Create Repository**.

> 📝 **Note:** Keeping the repository private ensures only authorized collaborators can access the code.

---

### **3. Invite Collaborators**
1. Go to your repository → **Settings**.  
2. Choose **Collaborators and teams** or **Manage access**.  
3. Click **Add people**.  
4. Enter GitHub usernames or email addresses of the members you want to invite.  
5. Assign roles (Admin / Write / Read).  
6. Click **Add Collaborator**.

> 💡 **Tip:** Team members from other repositories or organizations can also be invited.

---

### **4. Get the SSH Repository URL**
1. In your repository, click the **Code** button (green).  
2. Select the **SSH** tab.  
3. Copy the SSH URL (e.g. `git@github.com:username/repo-name.git`).  

> Save this URL — you’ll use it for Jenkins pipelines and manual cloning.

---

### ✅ **Summary — GitHub Setup**
| Task | Path | Notes |
|------|------|-------|
| Create Repo | GitHub → Repositories → New | Set as **Private** |
| Add Collaborators | Repo → Settings → Manage Access | Invite users & assign roles |
| Copy SSH URL | Repo → Code → SSH | Use for pipeline & cloning |

---

## 🚀 Part 2 — Clone the Git Repository on the Server

> Only perform this step for a new site or clean setup.

### **1. Access the Server**
- Log in to the server as **root** or an appropriate user.

### **2. Prepare the Web Root**
1. Navigate to your document root:  
   ```bash
   cd /home/username/
````

2. Backup the existing folder (if any):

   ```bash
   mv public_html public_html_bk
   ```
3. Clone the repository into the new folder:

   ```bash
   git clone git@github.com:username/repo-name.git public_html
   ```
4. Correct ownership and permissions:

   ```bash
   chown -R username:group public_html
   ```

---

## ⚙️ Part 3 — Configure Jenkins for Automatic Deployment

### **1. Access Jenkins**

Open Jenkins in your browser:

```
http://your-server-ip:8080
```

---

### **2. Create a New Jenkins Job**

1. Click **New Item**.
2. Enter an item name (e.g., your domain or project name).
3. Choose **Freestyle Project**.
4. Click **OK**.

---

### **3. Configure Jenkins**

#### **General**

* Leave default options.

#### **Source Code Management**

1. Select **Git**.
2. In **Repository URL**, paste:

   ```
   git@github.com:username/repo-name.git
   ```
3. Under **Credentials**, choose or add your SSH credentials.
4. Specify the branch:

   ```
   main
   ```

---

#### **Build Triggers**

Enable **Poll SCM** and use:

```
H/5 * * * *
```

> Jenkins will check for changes every 5 minutes.

---

#### **Build Steps**

1. Click **Add build step → Execute shell script on remote host using SSH**.
2. Select or add your target server (`root@server-ip`).

##### **Command Script**

> 📝 **Note:** The following script is intended for **Laravel-based websites**.
> It performs Git updates, dependency installation via Composer, and resets file permissions.

```bash
cd /home/username/public_html/

git stash
git pull origin main
composer install

chown -R username:group *
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;
```

---

### **4. Run the Build**

* Click **Save**.
* On the project dashboard, click **Build Now** to trigger deployment.

---

## ✅ **Overall Summary**

| Step | Section       | Description                                  |
| ---- | ------------- | -------------------------------------------- |
| 1    | GitHub Repo   | Create a private repository                  |
| 2    | Collaborators | Invite team members                          |
| 3    | Clone Repo    | Backup and clone project files               |
| 4    | Jenkins Setup | Create and configure Jenkins job             |
| 5    | SCM Config    | Connect GitHub repo via SSH                  |
| 6    | Poll SCM      | Automate updates every 5 minutes             |
| 7    | Deploy        | Pull latest Laravel code and set permissions |

```
```