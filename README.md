<p align="center">
<img src="https://i.imgur.com/7Xz5vES.jpeg" alt="Active Directory" width=500 height=300/> 
</p>

<h1>Network File Shares and Permission</h1>
<p>File Sharing Permissions & Group-Based Access Control</p>

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of them)***

-----

### Part 1: Create File Shares with Different Access Levels

- Log into the VMs
  - On `DC-1`: log in as `mydomain.com\jane_admin`
	- On `Client-1`: log in as a regular domain user, such as `mydomain.com\testuser4`

- Create 4 Folders on `DC-1`
	 - On `DC-1`, open File Explorer and navigate to `C:\`
   - Create these 4 new folders:
       - `C:\read-access`
       - `C:\write-access`
       - `C:\no-access`
       - `C:\accounting`

- Share the Folders with Specific Group Permissions

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

***You’ve now created shared folders with specific group permissions.***

-----

### Part 2: Test File Share Access as a Normal User

- Explore the Shares from `Client-1`
- On `Client-1`, logged in as `testuser4`:
  - Press `Windows` + `R` to open the `Run window`
	- Type: `\\dc-1`
- Press `Enter` to view available shares

- Try Opening Each Folder

Folder | What Should Happen
`read-access` | Can open folder and view files, but cannot save or edit
write-access
  - Can open folder, create/edit/delete files

`no-access` | You’ll see Access Denied because you’re not a Domain Admin
accounting
  - Folder doesn’t appear (not shared yet)

***🧠 Does it make sense? Yes! Access depends on your group membership.***

-----

### Part 3: Use a Security Group to Manage Folder Access

- Create the `ACCOUNTANTS` Group in `Active Directory`

- On `DC-1`:
  - Open `Active Directory Users and Computers (ADUC)`
	- Navigate to the `_EMPLOYEES` or root of your domain
	- Right-click > `New` > `Group`
	   - Name: `ACCOUNTANTS`
	   - Scope: `Global`
	   - Type: `Security`
- Click `OK`

***✅ Now you have a group to manage access to the “accounting” folder.***

- Share the `accounting` Folder with the `ACCOUNTANTS` Group
- On `DC-1`, go to `C:\accounting`
- Right-click > `Properties` > `Sharing` tab > `Advanced Sharing`
- Check `Share this folder`
  - Share name: `accounting`
- Click `Permissions`
  - Remove `Everyone` (if present)
	- Click `Add` > `ACCOUNTANTS`
  - Grant `Read/Write` access
- Click `OK` > `Apply`

***✅ The `accounting` folder is now shared and permissioned for the `ACCOUNTANTS` group.***

-----

### Part 4: Test Access Before Group Membership

- Try to Access `accounting` Share as Test User
- On `Client-1`, still logged in as `testuser4`:
- Open `Run` (Windows + R) > type: `\\dc-1`
- Try opening the accounting share

***🛑 You should see `Access Denied` – the user isn’t in the `ACCOUNTANTS` group yet.***

-----

### Part 5: Add User to `ACCOUNTANTS` Group & Re-Test

- Add User to Group
- Back on `DC-1`:
- In `ADUC`, find `testuser4`
- Right-click > `Properties` > `Member Of tab`
- Click `Add` > Type: `ACCOUNTANTS` > `Check Names` > `OK`
- Click `Apply` > `OK`

- Re-Test Access
- On `Client-1`:
- Log out of `testuser4`
- Log back in as `testuser4`
- Open `Run` (Windows + R) > `\\dc-1`
- Click on the `accounting` folder

***✅ You should now be able to access the folder and create/edit/delete files — because the user is now in the group with permission.***

-----

### Conclusion

<p>Hooray! You've finally reached to the end of the Active Directory lessons and with these task and the skills you've put into practice. You've lerned how to create shared folders, understood the factor of this file server structure setup and why it is setup like it.</p>
	
<p>You have also applied permissions, controlled access with groups, tested access as user in Client-1, using real-world permission validation, used AD Security Group, and finally, how you can best practice using the scalable access control.</p>
