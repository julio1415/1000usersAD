# 1000usersAD
Creating 1,000 bulk users using a script in Active Directory

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
![Active Directory Dashboard](images/ActiveDirectory.png)

### Step 2: Running the Bulk User Creation Script
![Running PowerShell Script](images/Runningcode.png)

### Step 3: Verifying Users in Active Directory
![Verifying Users](images/verifyingusers.png)

---

## Active Directory Organizational Units (OUs)

The following organizational units (OUs) are present in the Active Directory structure:

- **mydomain.com**
  - **Builtin** (Default system OU)
  - **Computers** (Default system OU)
  - **Domain Controllers** (Default system OU for domain controllers)
  - **ForeignSecurityPrincipals** (Default system OU for foreign security principals)
  - **Managed Service Accounts** (Default system OU for managed service accounts)
  - **Users** (Default system OU for default and manually added users)
  - **_EMPLOYEES** (Custom OU for employee accounts)
  - **_ADMINS** (Custom OU for administrative accounts)

---

## Demonstration

### Step 1: Installing Active Directory
1. Installed the AD DS role on Windows Server 2022.
2. Configured a new domain `example.local`.

### Step 2: Preparing the User Creation Script
1. Used the PowerShell script below to generate 1,000 users with random names.

**PowerShell Script (`Create-Users.ps1`):**
```powershell
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 10000
# ------------------------------------------------------ #

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $firstName = generate-random-name
    $lastName = generate-random-name
    $username = $firstName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
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
- **Script Location**: The PowerShell script and related assets are located in the `scripts` folder of this repository.
- **Customization**: The script can be adapted for specific user attributes, such as department or group membership.

---

## Repository Structure

```
ActiveDirectory-1000Users/
|-- README.md
|-- images/
|   |-- ActiveDirectory.png
|   |-- Runningcode.png
|   |-- verifyingusers.png
|-- scripts/
    |-- Create-Users.ps1
```

---

## How to Use
1. Clone the repository.
2. Modify the script as needed for your environment.
3. Run the PowerShell script (`Create-Users.ps1`) on a Windows Server with Active Directory installed.
4. Verify the users in the Active Directory Users and Computers (ADUC) console.
