# 1000usersAD
Creating 1,000 bulk users using a script in active directory
# Active Directory Implementation with Bulk User Creation (1,000 Users)

## Project Summary

This project demonstrates the implementation of an Active Directory environment and the addition of 1,000 users using PowerShell scripting. The goal is to showcase the ability to efficiently manage large-scale user provisioning in a Windows Server environment.

### Details:
- **Type**: Technology Implementation & Scripting
- **Languages Used**: PowerShell
- **Environments Used**: 
  - Windows Server 2022 (Active Directory Domain Controller)
  - Windows 10 (Client Environment)
- **Technology/Applications/Services Used**: 
  - Active Directory Domain Services (AD DS)
  - PowerShell Scripting

---

## Media

### Step 1: Installing Active Directory Domain Services
![AD Installation](images/ad-installation.png)

### Step 2: Running the Bulk User Creation Script
![Bulk User Script](images/powershell-script.png)

### Step 3: Verifying Users in Active Directory
![User List](images/user-list.png)

### Video Walkthrough
[![Active Directory User Creation Demo](images/video-thumbnail.png)](https://youtu.be/examplelink)

---

## Demonstration

### Step 1: Installing Active Directory
1. Installed the AD DS role on Windows Server 2022.
2. Configured a new domain `example.local`.

### Step 2: Preparing the User Creation Script
1. Created a CSV file with 1,000 user details (`users.csv`).
2. Developed a PowerShell script to read the CSV and create users in Active Directory.

**Sample CSV (`users.csv`):**
```csv
FirstName,LastName,Username,Password
John,Doe,john.doe,Password123!
Jane,Doe,jane.doe,Password123!
...
```

**PowerShell Script (`Create-Users.ps1`):**
```powershell
# Import the CSV file
$users = Import-Csv -Path "C:\Users.csv"

# Loop through each user and create them in Active Directory
foreach ($user in $users) {
    New-ADUser `
        -GivenName $user.FirstName `
        -Surname $user.LastName `
        -Name "$($user.FirstName) $($user.LastName)" `
        -SamAccountName $user.Username `
        -UserPrincipalName "$($user.Username)@example.local" `
        -AccountPassword (ConvertTo-SecureString $user.Password -AsPlainText -Force) `
        -Enabled $true
    Write-Host "Created user: $($user.FirstName) $($user.LastName)"
}
```

### Step 3: Running the Script
1. Executed the script on the domain controller.
2. Verified that all 1,000 users were successfully added.

### Step 4: Verifying in Active Directory
1. Opened Active Directory Users and Computers (ADUC).
2. Navigated to the `Users` organizational unit.
3. Verified that all 1,000 users were created.

---

### Additional Notes
- **Script Location**: The PowerShell script and CSV file are located in the `scripts` folder of this repository.
- **Customization**: The script can be adapted for specific user attributes, such as department or group membership.

---

## Repository Structure

```
ActiveDirectory-1000Users/
|-- README.md
|-- images/
|   |-- ad-installation.png
|   |-- powershell-script.png
|   |-- user-list.png
|   |-- video-thumbnail.png
|-- scripts/
    |-- Create-Users.ps1
    |-- users.csv
```

---

## How to Use
1. Clone the repository.
2. Update the CSV file (`users.csv`) with your desired user details.
3. Run the PowerShell script (`Create-Users.ps1`) on a Windows Server with Active Directory installed.
4. Verify the users in the Active Directory Users and Computers (ADUC) console.

---

## Video Demonstration
A detailed walkthrough of the implementation can be found here: [YouTube Video Link](https://youtu.be/examplelink).
