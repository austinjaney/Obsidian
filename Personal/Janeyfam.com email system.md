# Janeyfam.com email system
#projects #janeyfam #personal 

I used this guide: https://medium.com/@ashan.fernando/forwarding-emails-to-your-inbox-using-amazon-ses-2d261d60e417

Later found out it probably would have been easier to have used this: https://github.com/alikian/SESEmailForward

Used personal Amazon account to set it up Route 53 in AWS for routing.
Janeyfam.com is registered at godaddy,com

In order for all this to work I still need to get everyone to verify their addresses

One quirk of this is that because the system ingests the message, then forwards it on it looks like the email is both from and to yourself.  When you go to reply it does put the original senders address in the box. 