# AWS System Manager Configuration
#aws #sysadmin #powershell 

yyyy-mm-dd-Thh:mm-08:00

2019-08-28T19:20:30-08:00

I never have seem to get the powershell code working correctly to deploy this to windows, through either ISE or regular powershell, I should write a new code that uses bitstransfer to grab the installer instead of the newobject commandlet.

```
You have successfully created a new activation. Your activation code is listed below. Copy this code and keep it in a safe place as you will not be able to access it again.
Activation Code   y9pHoPKLsnYh0iIPP3UF
Activation ID   75367d2d-e821-4451-950a-70350a4177cb
```

Powershell Script to deploy on windows VM's

```powershell
$code = "y9pHoPKLsnYh0iIPP3UF"
$id = "75367d2d-e821-4451-950a-70350a4177cb"
$region = "us-west-2"
$dir = $env:TEMP + "\ssm"
New-Item -ItemType directory -Path $dir -Force
cd $dir
(New-Object System.Net.WebClient).DownloadFile("https://amazon-ssm-$region.s3.amazonaws.com/latest/windows_amd64/AmazonSSMAgentSetup.exe", $dir + "\AmazonSSMAgentSetup.exe")
Start-Process .\AmazonSSMAgentSetup.exe -ArgumentList @("/q", "/log", "install.log", "CODE=$code", "ID=$id", "REGION=$region") -Wait
Get-Content ($env:ProgramData + "\Amazon\SSM\InstanceData\registration")
Get-Service -Name "AmazonSSMAgent"
```

```powershell
$code = "/fvGKqeKBCoJr0mgIkvm"
$id = "9da99443-395e-48de-a0dd-49f239a479ef"
$region = “us-west-2"
$dir = $env:TEMP + "\ssm"
New-Item -ItemType directory -Path $dir -Force
cd $dir
(New-Object System.Net.WebClient).DownloadFile("https://amazon-ssm-$region.s3.amazonaws.com/latest/windows_amd64/AmazonSSMAgentSetup.exe", $dir + "\AmazonSSMAgentSetup.exe")
Start-Process .\AmazonSSMAgentSetup.exe -ArgumentList @("/q", "/log", "install.log", "CODE=$code", "ID=$id", "REGION=$region") -Wait
Get-Content ($env:ProgramData + "\Amazon\SSM\InstanceData\registration")
Get-Service -Name "AmazonSSMAgent"
```

Bash Script to Deploy on CentOS7

```bash
mkdir /tmp/ssm
curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm -o /tmp/ssm/amazon-ssm-agent.rpm
sudo yum install -y /tmp/ssm/amazon-ssm-agent.rpm
sudo systemctl stop amazon-ssm-agent
sudo amazon-ssm-agent -register -code "/fvGKqeKBCoJr0mgIkvm" -id "9da99443-395e-48de-a0dd-49f239a479ef" -region "us-west-2"
sudo systemctl start amazon-ssm-agent
```