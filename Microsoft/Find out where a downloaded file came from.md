# Find out where a downloaded file came from
#support 

TIL that all Chromium-based browsers store the URL they were downloaded from in an NTFS alternate data stream. You can use get-content -path <filename> -Stream Zone.Identifier to retrieve the URL. For Mac and Linux users, the data is stored in the extended attributes (getfattr -d <filename> for Linux; Finder/Cmd+I on Mac). One way this could be useful is to trace the source of malicious downloads or otherwise prove a file's origin. Files downloaded via Incognito mode will not have this tag present.

Credit goes to the Veeam Community Forums Digest email that I get weekly.