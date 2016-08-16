#### psiisadminfunda
######4 install iis powershell
open powershell ISE
```
Get-WindowsFeature -Name web-*
```
create a file install.ps1
edit
```
Import-Module ServerManager
Add-WindowsFeature Web-Commom-Http, Web-Default-Doc, Web-Dir-Browing, 
Web-Static-Content, Web-Stat-Compression, Web-CertProvider, Web-ASP, Web-Asp-Net45

Remove-Item C:\ke\wwwroot\* -Recurse

Copy-Item -Path C:\temp\webfiles\* -Destination C:\ke\wwwroot -Recurse

Backup-Webconfiguration -Name IISBackup
```

######5 Install .Net
```
$url = "https://"
$output="C:\exe"
Invoke-WebRequest -Uri $url -Outfile $output
& "$output" /passive /norestart
```
######6 create permission
```
$Right = "ReadAndExecute"
$Principal = "IIS_IUERS"
$StartingDir = "C:\ke\wwwroot"

foreach ($fie=le in $(Get-ChildItem $StartingDir - recurse)){
  $rule = new-object
  System.Security.AccessControl.FileSystemAccessRule($Principal, $Right, "Allow")
  $acl = Get-Acl $file.FullName
  Write-Output $file.FullName
  $acl.SetAccessRule($rule)
  Set-Acl $file.Fullname $acl
}
```
```
