# DecryptingPFXCerts . Copy Paste the belong.
The Following will help to extract vital information from a PFX that has a password


``` Ruby
#Locating Certificate
write-output "Locate our PFX Certificate"
Add-Type -AssemblyName System.Windows.Forms
$FileBrowser = New-Object System.Windows.Forms.OpenFileDialog
[void]$FileBrowser.ShowDialog()


#Adding the password for Cert, unless not given
write-output "Type in the password for this certificate"
$CertificatePassword      = [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.VisualBasic') | Out-Null
$CertificatePassword      = [Microsoft.VisualBasic.Interaction]::InputBox("Enter Password ") 

class PFXDecriptioner{
    [string]$Thumbprint
    [string]$Subject
    [string]$FriendlyName
    [string]$Issuer
    [string]$SubjectName
    [string]$IssueDate
    [string]$ExpirationDate
}

$certficate                = [PFXDecriptioner]::new()
$CertificatePath           = $FileBrowser.FileName

$sSecStrPassword           = ConvertTo-SecureString -String $CertificatePassword -Force –AsPlainText
$certificateObject         = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$certificateObject.Import($CertificatePath, $sSecStrPassword, [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::DefaultKeySet)
$certficate.Thumbprint     = $certificateObject.Thumbprint
$certficate.Subject        = $certificateObject.Subject
$certficate.FriendlyName   = $certificateObject.FriendlyName
$certficate.Issuer         = $certificateObject.Issuer
$certficate.SubjectName    = $certificateObject.SubjectName.Name
$certficate.IssueDate      = $certificateObject.NotBefore
$certficate.ExpirationDate = $certificateObject.NotAfter

clear
$certficate | Out-GridView -PassThru

```
