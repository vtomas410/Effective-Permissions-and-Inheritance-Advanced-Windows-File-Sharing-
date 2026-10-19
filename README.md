# **Effective Permissions and Inheritance (Advanced Windows File Sharing)**

What is effective permissions?

Effective  permissions to the actual set of permissions a user or group has on a file or folder. These permissions come from multiple sources such as: 

- Explicit permissions: assigned directly to a user or group
- Inherited Permissions: Passed down from parent folders
- Group Memberships: User is part of a group with permissions
- Deny Permissions: Overrides Allow Permissions

---

Understanding Inheritance in File Permissions.

In NTFS permissions you can allow or deny access to files and folders.

Explicit Deny is when you manually set a deny permission for a user or group and overrides any inherited or allowed permissions the user might have.

---

Prequisites:

- Windows Server 22 or 25: installed on a virtual machine (VM).
- Windows 11 client VM: to test access from a non-server system.
- Active Directory Domain Services installed.

---

**Before we begin Lets set this up.**

Step 1: Open Active Directory → Click Users → Right click then select “New Group” → Name Project →  Click Ok.

Step 2: Open files → Select This PC → Local Disk (C:) → Create a new folder called “Common”

Step 3: Create subfolders under Common → Name them “Project” and “Events”.

Step 4: Right click “Common” → Click properties → Go to Sharing Tab → Click Share and add Everyone. 

Step 5: Click Advance Sharing → Select permissions → Everyone should have permissions.

Step 6: Click on Security tab → Select everyone → Allow full control → click apply and okay.

Step 7: Verify these permissions passed on to their subfolders. You can create a subfolder under projects as a test named “Contracts” and it should inherit permissions.

---

First Scenario: 

There is a common folder that has full control to everyone in the network. The common subfolders has inherited full control permissions. The Project group Needs access to only one subfolder called “Project”. What you can do is apply NTFS on the subfolder to restrict access to the project group.

First you have to disable inheritance from the project subfolder. Then remove the everyone permissions. So the project folder won’t have full control permissions and give a modify or full control permissions to project group for project subfolder.

Step 1: Disable inheritance. Right click on project → Select properties → Go to security tab → Click on Advance → select Disable inheritance → Convert inherited permissions into explicit permissions on this object. 

This takes all the currently inherited permissions ands makes them explicit permissions on the object. For Example, if you want to keep the current permissions like the everyone full control but stop feature inheritance from the parent folder or object, this is what you will choose and then  for remove all inherited permissions from this object.

This completely removes all inherited permissions from this object. you will use this if you want full control and don’t want the object to have any permissions from the parent and if you want to start with a clean slate. 

if we select remove all inherited permissions from this object, it will lose all of the inherited access from our common folder earlier but if we select the convert inherited permissions, it will still keep the everyone full control permissions. It will not get the future permissions that will be set for common if there’s more permissions that will be added from the parent folder.

Step 2: Now if you check the “Everyone” Group should be removed.

Step 3: Click on Advance → Select Add → CLick select principle → Type in Project name “Project” - Click OK

Step 4: Select full control → click on OK → Verify group project is added and select click ok and apply.

---

Scenario Two: 

**Implementing Explicit Deny in NTFS Permissions.**

You are an IT administrator at a company where all employees have access to a shared project folder. However the confidential folder contains sensitive financial data that should not be accessed by John, a member of the employees group. The project folder has read and write permissions for all the employees. You need to block John from opening or reading confidential folder without affecting other employees access. 

Step 1: Create a new folder → name it “Project” 

Step 2: Create subfolders: Confidential and Materials.

Step 3: In active directory create the user “John Doe” under User

Step 4: Right click → Sharing Tab→ Share the folder → select Everyone → Select Read/Write

Step 5: Verify under advance sharing → select permissions → then verify

Step 6: Verify the permissions are inherited to the subfolders.

Step 7: Right click → Sharing Tab → Share the folder → select Everyone → Verify its selected as Read/Write

Step 8: Next we want to explicitly Deny John on this confidential folder. Right Click → select Sharing Tab → select Edit → Click on Add → Type in John Doe → Select Deny Read

Step 9: Select yes on the Warning Window. This means if a user is a member of two groups, one that is allowed a permission and another that is denied the same permission, the user is denied that permission.

Step 10: Verify the settings are the same for Everyone under materials while under confidential it has an explicit deny permission for John Doe.

**This is how you deny a person or a Specific group.**

---
