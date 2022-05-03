# Export OVA from VMware Fusion
#vmware #macos #bash 

A quick guide on exporting ova files from vmware fusion since it does not contain that option in the UI.

Navigate to the directory of the OVF tool:
```bash
cd /Applications/VMware\ Fusion.app/Contents/Library/VMware\ OVF\ Tool/
```

Use the OVF tool:

In the example below the users username is pilajaney
```bash
./ovftool /Users/ajaney/Documents/Virtual\ Machines.localized/Windows\ 7\ Pro\ PIL\ Template.vmwarevm/Windows\ 7\ Pro\ PIL\ Template.vmx /Users/ajaney/Desktop/Windows_7_Authoring.ova
```

Before Exporting Windows VM’s to an OVA file:

* Update windows completely (because you mine as well)
* Run the windows disk cleanup tool using the delete system files option
* Delete the continence of C:\\Windows\\SoftwareDistribution\\
* Disable/delete hibernate.sys
```bash
powercfg.exe /hibernate off
```

* Zero the free space of the windows VM using sdelete 
	* <https://docs.microsoft.com/en-us/sysinternals/downloads/sdelete>
```js
Sdelete64.exe -z c:
```

* Do not run Sysprep just shut the system down
* Install the Registry hack to zero out the windows page file on shutdown 
	*  <https://www.howtogeek.com/282049/how-to-make-windows-clear-your-page-file-at-shutdown-and-when-you-should/>
* Run the VMware tool to shrink the size of the disk

Following this process I was able to export an OVA for Windows 7 Enterprise that was only 7.26GB in size, I had tried to do the same process with a windows 7 Pro vm which turned out 1mb larger despite having less updates.

Other Misc information

<https://www.howtogeek.com/312883/how-to-shrink-a-virtualbox-virtual-machine-and-free-up-disk-space/>