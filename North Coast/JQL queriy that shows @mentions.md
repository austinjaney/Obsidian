# JQL queriy that shows @mentions
#jira #northcoast 

This JQL string shows @mentions within a certain time frame:

```
resolution = Unresolved AND (summary ~ currentUser() OR description ~ currentUser() OR comment ~ currentUser()) AND updatedDate >= -9d
```

Great to put in colaboritive queues. but if you just want to do comments with @mentions then your better off using uniqui ID's so that you can drop into someone elses queue and see when they had been mentioned as well:

```
resolution = Unresolved AND (summary ~ 5d55e3f6c4be040db2d55e1f OR description ~ 5d55e3f6c4be040db2d55e1f OR comment ~ 5d55e3f6c4be040db2d55e1f) AND updatedDate >= -9d OR 
```
