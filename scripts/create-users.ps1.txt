PowerShell script to create AD users in bulk
Run on Domain Controller as Administrator


$users = @(
@{FirstName="John"; LastName="Smith"; Username="jsmith"; Department="IT"},
@{FirstName="Terry"; LastName="Smith"; Username="tsmith"; Department="IT"}
)
foreach ($user in $users) {
New-ADUser         -Name "$($user.FirstName) $($user.LastName)"
-GivenName $user.FirstName         -Surname $user.LastName
-SamAccountName $user.Username         -UserPrincipalName "$($user.Username)@demodomain.local"        -Path "OU=((
(user.Department),DC=demodomain,DC=local"         -AccountPassword (ConvertTo-SecureString "Password1234!" -AsPlainText -Force)        -PasswordNeverExpires $true `
        -Enabled $true
    Write-Host "Created user: ((
(user.Username)"
}