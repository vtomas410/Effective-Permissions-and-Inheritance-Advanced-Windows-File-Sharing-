# **Effective Permissions and Inheritance (Advanced Windows File Sharing)**

# What is effective permissions?

Effective  permissions to the actual set of permissions a user or group has on a file or folder. These permissions come from multiple sources such as: 

- Explicit permissions: assigned directly to a user or group
- Inherited Permissions: Passed down from parent folders
- Group Memberships: User is part of a group with permissions
- Deny Permissions: Overrides Allow Permissions

---

# Understanding Inheritance in File Permissions.

In NTFS permissions you can allow or deny access to files and folders.

Explicit Deny is when you manually set a deny permission for a user or group and overrides any inherited or allowed permissions the user might have.

---

# Prequisites:

- Windows Server 22 or 25: installed on a virtual machine (VM).
- Windows 11 client VM: to test access from a non-server system.
- Active Directory Domain Services installed.

---

# Before we begin Lets set this up.

**Step 1:** Open Active Directory → Click Users → Right click then select “New Group” → Name Project →  Click Ok.
<img width="880" height="615" alt="1 New - Create Project Group" src="https://github.com/user-attachments/assets/22355ed2-9523-4da3-92d1-ccf1eea089c8" />



**Step 2:** Open files → Select This PC → Local Disk (C:) → Create a new folder called “Common”
<img width="791" height="632" alt="2 New Folder - common" src="https://github.com/user-attachments/assets/b32c31de-bed9-4b9f-8190-891e388ded16" />

**Step 3:** Create subfolders under Common → Name them “Project” and “Events”.
<img width="973" height="741" alt="3 Create Sub folders - Projects - Events" src="https://github.com/user-attachments/assets/594919a7-e48e-4b47-b556-d28206002b26" />


**Step 4:** Right click “Common” → Click properties → Go to Sharing Tab → Click Share and add Everyone. 
<img width="623" height="539" alt="4 Network access - Everyone - Add" src="https://github.com/user-attachments/assets/15cde312-4dfc-42b7-8fac-e0931342dd82" />


**Step 5:** Click Advance Sharing → Select permissions → Everyone should have permissions.
<img width="850" height="709" alt="6 Verifying the Inheritance woked " src="https://github.com/user-attachments/assets/6b87ffff-6273-4a93-8a97-b28f5ef0624f" />

**Step 6:** Click on Security tab → Select everyone → Allow full control → click apply and okay.
<img width="784" height="639" alt="5 Under security - Select everyone - Full control " src="https://github.com/user-attachments/assets/ca072a9e-1d46-4d9b-89df-b92fa75f41f0" />


**Step 7:** Verify these permissions passed on to their subfolders. You can create a subfolder under projects as a test named “Contracts” and it should inherit permissions.

<img width="795" height="679" alt="7 Successful test - projects - full control for everyone" src="https://github.com/user-attachments/assets/443f802f-785e-41ed-993c-4fac1134b356" />

---

# First Scenario: 

  There is a common folder that has full control to everyone in the network. The common subfolders has inherited full control permissions. The Project group Needs access   to only one subfolder called “Project”. What you can do is apply NTFS on the subfolder to restrict access to the project group.

  First you have to disable inheritance from the project subfolder. Then remove the everyone permissions. So the project folder won’t have full control permissions and     give a modify or full control permissions to project group for project subfolder.

**Step 1:** Disable inheritance. Right click on project → Select properties → Go to security tab → Click on Advance → select Disable inheritance → Convert inherited permissions into explicit permissions on this object. 

<img width="817" height="626" alt="8 Disable inheritance" src="https://github.com/user-attachments/assets/bd3bc448-bc47-4faa-9b4a-24d47f41e407" />


This takes all the currently inherited permissions ands makes them explicit permissions on the object. For Example, if you want to keep the current permissions like the everyone full control but stop feature inheritance from the parent folder or object, this is what you will choose and then  for remove all inherited permissions from this object.

This completely removes all inherited permissions from this object. you will use this if you want full control and don’t want the object to have any permissions from the parent and if you want to start with a clean slate. 

if we select remove all inherited permissions from this object, it will lose all of the inherited access from our common folder earlier but if we select the convert inherited permissions, it will still keep the everyone full control permissions. It will not get the future permissions that will be set for common if there’s more permissions that will be added from the parent folder.



**Step 2:** Now if you check the “Everyone” Group should be removed.

**Step 3:** Click on Advance → Select Add → CLick select principle → Type in Project name “Project” - Click OK

**Step 4:** Select full control → click on OK → Verify group project is added and select click ok and apply.

<img width="986" height="700" alt="9 Select principal - Select Project folder - full control " src="https://github.com/user-attachments/assets/1a9fb680-46a5-4bb9-93e1-c8001c0b92df" />

---

Scenario Two: 

# Implementing Explicit Deny in NTFS Permissions.

You are an IT administrator at a company where all employees have access to a shared project folder. However the confidential folder contains sensitive financial data that should not be accessed by John, a member of the employees group. The project folder has read and write permissions for all the employees. You need to block John from opening or reading confidential folder without affecting other employees access. 

**Step 1:** Create a new folder → name it “Project” 

**Step 2:** Create subfolders: Confidential and Materials.
<img width="1000" height="742" alt="10 Create folder - Projects - Create subfolders - Confidential and Materials" src="https://github.com/user-attachments/assets/a382f1a9-1f68-4a08-9650-aea5e6fcd844" />


**Step 3:** In active directory create the user “John Doe” under User
<img width="1003" height="744" alt="11 Create New User - John Doe " src="https://github.com/user-attachments/assets/6edf9bc2-a4cc-494f-893f-81e4450af439" />


**Step 4:** Right click → Sharing Tab→ Share the folder → select Everyone → Select Read/Write
<img width="793" height="535" alt="12 Network access sharing - Select everyone - Read and write" src="https://github.com/user-attachments/assets/c3acdc19-d306-4681-a5ae-6e44c76be0b6" />

**Step 5:** Verify under advance sharing → select permissions → then verify

**Step 6:** Verify the permissions are inherited to the subfolders.

**Step 7:** Right click → Sharing Tab → Share the folder → select Everyone → Verify its selected as Read/Write

**Step 8:** Next we want to explicitly Deny John on this confidential folder. Right Click → select Sharing Tab → select Edit → Click on Add → Type in John Doe → Select Deny Read

<img width="820" height="691" alt="13 Give John doe Permission only " src="https://github.com/user-attachments/assets/8e3bd781-e88e-4bb3-b44b-0610f26e5e36" />


<img width="1025" height="792" alt="14 Successful Test - Denied John Doe to Read folder but can access confidential" src="https://github.com/user-attachments/assets/5380fc90-27d8-406f-9903-61b2dd9706f6" />

**Step 9:** Select yes on the Warning Window. This means if a user is a member of two groups, one that is allowed a permission and another that is denied the same permission, the user is denied that permission.

**Step 10:** Verify the settings are the same for Everyone under materials while under confidential it has an explicit deny permission for John Doe.

**This is how you deny a person or a Specific group.**

---
