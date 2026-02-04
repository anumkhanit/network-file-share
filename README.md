<p align="center">
<img src="https://i.imgur.com/7Xz5vES.jpeg" alt="Active Directory" width=500 height=300/> 
</p>

<h1>🔐 Network File Shares & Permissions: A Hands-On Guide to Group-Based Access Control</h1>
<p>In corporate networks, file sharing isn’t just about convenience—it’s about controlling who can access what. By setting up permissions properly, you can protect sensitive information, streamline collaboration, and ensure compliance with security policies.</p>

<h1>🧠 Overview</h1>
<p>In this lab, we’ll walk through how to create shared folders with specific access levels, test permissions as different users, and use Active Directory (AD) groups to manage access. All steps are based on a hands-on lab using Microsoft Azure virtual machines and Active Directory.</p>

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of them)***

-----

### Part 1: Creating File Shares with Different Access Levels

1. First, we’ll log into our VMs:
   - On `DC-1`: log in as `mydomain.com\jane_admin`
   - On `Client-1`: log in as a regular domain user, such as `mydomain.com\testuser4`

2. Create 4 Folders on `DC-1`
   - On `DC-1`, open File Explorer and navigate to `C:\`
    - Create these 4 new folders:
       - `C:\read-access`
       - `C:\write-access`
       - `C:\no-access`
       - `C:\accounting` ***(leave unshared for now)***

3. Now, share each folder with specific permissions:

***For each folder, do the following:***
	- Right-click `folder` > `Properties`
	- Go to the `Sharing` tab > Click `Advanced Sharing`
	- Check `Share this folder`
	- Click `Permissions` and apply the correct group & access level:

Folder | Group | Permission

`read-access` | Domain Users | Read
`write-access` | Domain Users | Read/Write
`no-access` | Domain Admins | Read/Write
`accounting` | Skip for now | (leave unshared)

- Click `OK` and then `Apply` to confirm

***✅ At this stage, you have shared folders with different access levels.***

-----

### Part 2: Testing Access as a Normal User

1. Switch to `Client-1` as testuser4:
2. Press `Windows` + `R` → type `\\dc-1` → hit `Enter`
3. Try opening each share:
    - `read-access` → Can open and view, but cannot edit
    - `write-access` → Can create/edit/delete files
    - `no-access` → Access Denied
    - `accounting` → Won’t appear yet (not shared)

***This demonstrates how access is tied to group membership.***

-----

### Part 3: Creating a Security Group for Granular Access

<p>Now, let’s give access to accounting only for the ACCOUNTANTS group.</p>

1. On `DC-1`:
2. Open `Active Directory Users and Computers (ADUC)`
3. Right-click `domain` → `New` → `Group`
4. Name: `ACCOUNTANTS`
5. Scope: `Global` | Type: `Security`
6. Click `OK`

7. Then, share `C:\accounting`:
8. Advanced Sharing → Share this folder
9. Permissions → Remove `Everyone`
10. Add `ACCOUNTANTS` → Give `Read/Write`
11. Apply and confirm

***✅ You now have a role-based access-controlled folder.***

-----

### Part 4: Testing Access Before Group Membership

1. From `Client-1` as testuser4:
  - Try opening `\\dc-1\accounting` → Access `Denied`

***This confirms the user is not yet authorized.***

-----

### Part 5: Adding a User to the Group & Retesting

1. Back on `DC-1`:
  - In `ADUC`, open testuser4 → Member Of
  - Add `ACCOUNTANTS` → Apply changes

2. On `Client-1`:
  - Log out & back in as testuser4
  - Open `\\dc-1\accounting` → Now accessible with full read/write permissions

-----

<h2>📌 Why This Matters in the Real World</h2>

This lab demonstrates:
	
 - How to set up shared folders with varying access levels
 - The importance of group-based access control over individual permissions
 - How to test and validate permissions as different users
 - The security benefits of denying access by default

<p>In a production environment, group-based permissions are the gold standard—they scale as your organization grows and keep access consistent and easy to manage.</p>

-----

### 🎯 Conclusion

By the end of this exercise, you’ve:
	
 - Configured network file shares
 - Assigned permissions based on security groups
 - Tested and verified access restrictions
 - Learned how to integrate file sharing with Active Directory

***Pro Tip: Always follow the principle of least privilege—give users the minimum access they need, nothing more.***
