# 🚀 Setting Up an SVN & Mercurial Server on Linux

**By: Saubhik Mallick**

## 📌 Introduction

Subversion (SVN) and Mercurial (hg) are popular version control systems that help manage changes to files and directories over time. This guide walks you through setting up both an SVN and a Mercurial server on Linux, configuring users, creating branches, merging changes, and resolving common errors in an easy-to-follow manner.

[🎥 Watch the SVN Demo](https://github.com/Saubhik2003/Devops/raw/main/Svndevops.mp4)
[🎥 Watch the MERCURIAL Demo](https://github.com/Saubhik2003/Devops/raw/main/mercury.mp4)



---

# 🛠 Setting Up an SVN Server

## 🔧 1. Install Subversion

On a Linux system, install Subversion using your package manager:

```bash
sudo apt update
sudo apt install subversion
```

## 📂 2. Create a Repository

Create a directory for your repositories and initialize a new repository:

```bash
sudo mkdir -p /var/svn/repos
sudo svnadmin create /var/svn/repos/myrepo
```

## ⚙️ 3. Configure svnserve

Edit the `svnserve.conf` file to configure access:

```bash
sudo nano /var/svn/repos/myrepo/conf/svnserve.conf
```

Modify the file as follows:

```
[general]
anon-access = none      # ❌ Disable anonymous access
auth-access = write     # ✅ Allow authenticated users to write
password-db = passwd    # 🔑 Use the passwd file for authentication
```

## 👤 4. Set Up Users

Edit the `passwd` file to add users:

```bash
sudo nano /var/svn/repos/myrepo/conf/passwd
```

Add user credentials:

```
[users]
alice = alicepassword
bob = bobpassword
```

## 🚀 5. Start svnserve

Start the svnserve daemon:

```bash
sudo svnserve -d -r /var/svn/repos
```

The `-d` flag runs it in daemon mode, and `-r` specifies the root directory for repositories.

## 🌿 6. Create a Branch

Before creating a branch, set up the standard SVN structure:

```bash
svn mkdir svn://localhost/myrepo/trunk -m "Creating trunk"
svn mkdir svn://localhost/myrepo/branches -m "Creating branches directory"
svn mkdir svn://localhost/myrepo/tags -m "Creating tags directory"
```

Then create a branch for development:

```bash
svn copy svn://localhost/myrepo/trunk svn://localhost/myrepo/branches/feature-branch -m "Creating feature branch"
```

## 🔀 7. Switch to a Branch

Switch your working copy to the new branch:

```bash
svn switch svn://localhost/myrepo/branches/feature-branch
```

## 🔄 8. Merge Changes

Merge changes from the branch back to the trunk:

```bash
svn merge svn://localhost/myrepo/branches/feature-branch
svn commit -m "Merged feature-branch into trunk"
```

## ⚔️ 9. Resolve Conflicts

If there are conflicts during an update or merge, resolve them:

```bash
svn resolve --accept working file.txt
```

## 🗑️ 10. Delete a File

Delete a file and commit the change:

```bash
svn delete file.txt
svn commit -m "Deleted file.txt"
```

---

# 🚀 Setting Up Mercurial (hg)

## 📌 Introduction to Mercurial

Mercurial is a distributed version control system that allows developers to track changes in their code and collaborate with others. The following steps guide you through setting up Mercurial, cloning a repository, making changes, and pushing those changes back to the remote repository.

## 🔧 1. Install Mercurial

Install Mercurial on your Linux system using the package manager:

```bash
sudo apt update
sudo apt install mercurial
```

Verify the installation:

```bash
hg --version
```

## 📂 2. Clone a Repository

To clone a repository, use the following command:

```bash
hg clone <repository_url>
```

🔍 **Explanation:** This command creates a local copy of the repository located at the specified URL. The local copy will be created in a directory named after the repository.

## ✍️ 3. Edit Files

Make the necessary changes to the files in the repository. You can use any text editor or Integrated Development Environment (IDE) of your choice to edit the files.

## ➕ 4. Add New Files

If you have created new files or modified existing files, you need to add them to the staging area using the following command:

```bash
hg add
```

## ✅ 5. Commit Changes

Once you have added the files, commit your changes with a descriptive message:

```bash
hg commit -m "My changes"
```

🔍 **Explanation:** The `-m` flag allows you to include a commit message that describes the changes you made.

## 📤 6. Push Changes

Finally, push your committed changes back to the remote repository:

```bash
hg push
```

🔍 **Explanation:** This command uploads your local changes to the remote repository, making them available to others.

---

# ❗ Common Errors and Fixes

### ❌ **Error: Authorization Failed in Mercurial**

✅ **Cause:** Mercurial is unable to authenticate your credentials while pushing changes. 🔧 **Fix:**

1️⃣ **Check Access Permissions** 🔏
Ensure you have the necessary permissions to push to the repository.

✔️ **Solution:** If you are collaborating on a project, confirm with the repository owner that you have been granted the appropriate access rights.

### ❌ **Error: Checked out revision 0 in SVN**

✅ **Cause:** The repository is empty. 🔧 **Fix:** Add and commit files:

```bash
echo "Hello, SVN!" > file.txt
svn add file.txt
svn commit -m "Added file.txt"
```

### ❌ **Error: File Not Found in SVN**

✅ **Cause:** The repository does not have a `trunk/branches/tags` structure. 🔧 **Fix:** Create the structure:

```bash
svn mkdir svn://localhost/myrepo/trunk -m "Creating trunk"
svn mkdir svn://localhost/myrepo/branches -m "Creating branches directory"
svn mkdir svn://localhost/myrepo/tags -m "Creating tags directory"
```

This guide should help you set up and manage an SVN and Mercurial server with ease. 🚀🎉
