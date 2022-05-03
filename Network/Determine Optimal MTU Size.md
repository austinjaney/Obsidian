# Determine Optimal MTU Size using Ping
#sysadmin #network #support 

1. In Windows, go to Start and select Run.
2. Type in cmd (Windows 2000/XP) or command (Windows 98/ME) into the Open: field. Hit the enter key or click OK. The DOS prompt should open.
3. At the DOS prompt, type in ping www.yahoo.com -f -l 1492 and hit the Enter key: 
4. The results above indicate that the packet needs to be fragmented. Repeat this test, lowering the size the packet in increments of +/-10 (e.g. 1472, 1462, 1440, 1400) until you have a packet size that does not fragment:
5. Begin increasing the packet size from this number in small increments until you find the largest size that does not fragment. Add 28 to that number (IP/ICMP headers) to get the optimal MTU setting. For example, if the largest packet size from ping tests is 1462, add 28 to 1462 to get a total of 1490 which is the optimal MTU setting.
6. Change the MTU on the routers WAN Setup.
