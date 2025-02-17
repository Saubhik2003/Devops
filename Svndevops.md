🚀 Setting Up an SVN Server on Linux
By: Saubhik Mallick

📌 Introduction
Apache Subversion (SVN) is a powerful version control system that helps track changes in files and directories over time. This guide will walk you through setting up an SVN server on a Linux machine, managing users, creating branches, merging changes, and troubleshooting common errors.

🎥 Watch the SVN Setup Demo

🔧 1. Installing Subversion
To install SVN on Linux, run:

bash
sudo apt update  
sudo apt install subversion  

📂 2. Setting Up a Repository
Create and initialize an SVN repository:

bash
sudo mkdir -p /var/svn/repos  
sudo svnadmin create /var/svn/repos/myrepo  

⚙️ 3. Configuring svnserve
Modify svnserve.conf to manage access permissions:

bash
sudo nano /var/svn/repos/myrepo/conf/svnserve.conf  
Make these changes:

ini
[general]  
anon-access = none      # ❌ Disable anonymous access  
auth-access = write     # ✅ Allow authenticated users to write  
password-db = passwd    # 🔑 Use the password file for authentication  

👤 4. Adding Users
Edit the passwd file to define user credentials:

bash
sudo nano /var/svn/repos/myrepo/conf/passwd  
Add users in this format:

ini
[users]  
alice = alicepassword  
bob = bobpassword  

🚀 5. Running the SVN Server
Start the SVN service:

bash
Copy
Edit
sudo svnserve -d -r /var/svn/repos  
-d runs it in daemon mode
-r sets the repository root

🌿 6. Setting Up Branches
Before branching, create the recommended directory structure:

bash
svn mkdir svn://localhost/myrepo/trunk -m "Creating trunk"  
svn mkdir svn://localhost/myrepo/branches -m "Creating branches directory"  
svn mkdir svn://localhost/myrepo/tags -m "Creating tags directory"  
To create a new branch:

bash
svn copy svn://localhost/myrepo/trunk svn://localhost/myrepo/branches/feature-branch -m "Creating feature branch"  

🔀 7. Switching to a Branch
bash
svn switch svn://localhost/myrepo/branches/feature-branch  

🔄 8. Merging Changes
To merge branch changes back into trunk:

bash
svn merge svn://localhost/myrepo/branches/feature-branch  
svn commit -m "Merged feature-branch into trunk"  

⚔️ 9. Handling Conflicts
If there’s a conflict, resolve it with:

bash
svn resolve --accept working file.txt  
🗑️ 10. Deleting Files
To remove a file from SVN:

bash
svn delete file.txt  
svn commit -m "Deleted file.txt"  
❗ Troubleshooting Common SVN Errors
❌ Error: Checked out revision 0
✅ Cause: The repository is empty.
🔧 Fix: Add and commit a file:

bash
echo "Hello, SVN!" > file.txt  
svn add file.txt  
svn commit -m "Initial file added"  
❌ Error: File not found: revision 1, path '/trunk'
✅ Cause: Missing trunk/branches/tags structure.
🔧 Fix: Create the structure:

bash
svn mkdir svn://localhost/myrepo/trunk -m "Creating trunk"  
svn mkdir svn://localhost/myrepo/branches -m "Creating branches directory"  
svn mkdir svn://localhost/myrepo/tags -m "Creating tags directory"  
Move existing files into trunk:

bash
svn move svn://localhost/myrepo/file.txt svn://localhost/myrepo/trunk/file.txt -m "Moving file.txt to trunk"  
❌ Error: Path '.' does not share common version control ancestry
✅ Cause: The working copy was checked out incorrectly.
🔧 Fix:

bash
rm -rf myrepo  # ⚠️ WARNING: This deletes local changes!  
svn checkout svn://localhost/myrepo/trunk myrepo  
cd myrepo  
svn switch svn://localhost/myrepo/branches/feature-branch  
Or force the switch:

bash
svn switch --ignore-ancestry svn://localhost/myrepo/branches/feature-branch  
This guide will help you successfully set up and manage your SVN server on Linux. 🚀 Happy coding! 🎉
