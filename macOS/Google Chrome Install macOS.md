# Scripted Google Chrome Install
#macos #bash 

This script downloads and installs google chrome

```bash
#!/bin/bash

# Download and Install Google Chrome.

################################## VARIABLE DEFINITIONS ################################# 
#########################################################################################

appDownloadURL="https://dl.google.com/chrome/mac/stable/GGRO/googlechrome.dmg"
appDownload="googlechrome.dmg"
appName="Google Chrome.app"

##################################### SCRIPT BEGINS ##################################### 
#########################################################################################

function DownloadAndVerify {
	echo ""
	echo "Step 1: Downloading $appDownload"
	curl -o /private/var/tmp/"$appDownload" "$appDownloadURL" --connect-timeout 10 &>/dev/null
	echo "Step 2: Verifying $appDownload"
	if [ -e /private/var/tmp/"$appDownload" ]; then
		AttachAndInstall
	else
		echo "Error: Missing download."
	fi
}

function AttachAndInstall {
	echo "Step 3: Installing $appDownload"
	# Determine the file type and application type.
	fileType=$(echo "$appDownload" | sed 's/^.*\.//g')
	appType=$(echo "$appName" | sed 's/^.*\.//g')
	# Remove the existing application if present.
	if [ "$appName" != "" ]; then
		rm -Rf /Applications/"$appName"
	fi
	# Open the dmg, tar.gz or zip file.
	if [ "$fileType" = "dmg" ]; then
		attachDMG=$(yes | hdiutil attach -nobrowse /private/var/tmp/"$appDownload" | awk '/Volumes/{$2="";print $0}')
		deviceNode=$(echo $attachDMG | awk '{print $1}')
		volumeName=$(echo $attachDMG | awk '{$1="";print $0}' | sed -e 's/^ *//')
	elif [ "$fileType" = "zip" ]; then
		unzip -oq /private/var/tmp/"$appDownload" -d /private/var/tmp/
	elif [ "$fileType" = "gz" -o "$fileType" = "tgz" ]; then
		tar -zxf /private/var/tmp/"$appDownload" -C /private/var/tmp "$appName"
	fi
	# Install the drag and drop app or pkg.
	if [ "$appType" = "app" ]; then
		if [ "$fileType" = "dmg" ]; then
			cp -Rp "$volumeName/$appName" /Applications
		elif [ "$fileType" = "zip" -o "$fileType" = "gz" -o "$fileType" = "tgz" ]; then
			cp -Rp /private/var/tmp/"$appName" /Applications
		else
			echo "Error: Unknown installer type."
			exit 1
		fi
	elif [ "$appType" = "pkg" ]; then
		if [ "$fileType" = "dmg" ]; then
			installer -pkg "$volumeName/$appName" -tgt /
		elif [ "$fileType" = "zip" -o "$fileType" = "gz" -o "$fileType" = "tgz" ]; then
			installer -pkg /private/var/tmp/"$appName" -tgt /
		else
			echo "Error: Unknown installer type."
			exit 1
		fi
	fi
	echo "Step 4: Cleanup"
	if [ "$fileType" = "dmg" ]; then
		hdiutil detach "$deviceNode" &>/dev/null
		if [ "$appDownload" != "" ]; then
			rm /private/var/tmp/"$appDownload"
		fi
	elif [ "$fileType" = "zip" -o "$fileType" = "gz" -o "$fileType" = "tgz" ]; then
		if [ "$appDownload" != "" ]; then
			rm /private/var/tmp/"$appDownload"
		fi
		if [ "$appName" != "" ]; then
			rm -Rf /Applications/"$appName"
		fi
	fi
}

##################################### FUNCTION CALL ##################################### 
#########################################################################################

DownloadAndVerify
```
