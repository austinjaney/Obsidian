# Current Jira Queue Queries for Computer Helpdesk
#sysadmin #northcoast #jira 

```sql
statusCategory in (2, 4) AND assignee in (rmontes) OR (summary ~ 5d55fb82c58ad70d9964939b OR description ~ 5d55fb82c58ad70d9964939b OR comment ~ 5d55fb82c58ad70d9964939b) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
statusCategory in (2, 4) AND assignee in (wjones) OR (summary ~ 5d66a2acec4c1d0c15861d64 OR description ~ 5d66a2acec4c1d0c15861d64 OR comment ~ 5d66a2acec4c1d0c15861d64) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
statusCategory in (2, 4) AND assignee in (atsunekawa) OR (summary ~ 5d6ea87694e3580d923b24e6 OR description ~ 5d6ea87694e3580d923b24e6 OR comment ~ 5d6ea87694e3580d923b24e6) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
statusCategory in (2, 4) AND assignee in (ehester) OR (summary ~ 5d66a2a8388d780dac241147 OR description ~ 5d66a2a8388d780dac241147 OR comment ~ 5d66a2a8388d780dac241147) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
statusCategory in (2, 4) AND assignee in (krussell) OR (summary ~ 5d61667be09baa0d7487b084 OR description ~ 5d61667be09baa0d7487b084 OR comment ~ 5d61667be09baa0d7487b084) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
statusCategory in (2, 4) AND assignee in (mhudacko) OR (summary ~ 5d5af29b64b6510d56c40caa OR description ~ 5d5af29b64b6510d56c40caa OR comment ~ 5d5af29b64b6510d56c40caa) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
statusCategory in (2, 4) AND assignee in (mhudacko) OR (summary ~ 5d69497b9eb3300c0f6f819c OR description ~ 5d69497b9eb3300c0f6f819c OR comment ~ 5d69497b9eb3300c0f6f819c) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
statusCategory in (2, 4) AND assignee in (admin) OR (summary ~ 5d55e3f6c4be040db2d55e1f OR description ~ 5d55e3f6c4be040db2d55e1f OR comment ~ 5d55e3f6c4be040db2d55e1f) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC


statusCategory in (2, 4) AND assignee in (admin) OR (summary ~ 5d55e3f6c4be040db2d55e1f OR description ~ 5d55e3f6c4be040db2d55e1f OR comment ~ 5d55e3f6c4be040db2d55e1f) AND updatedDate >= -9d ORDER BY priority DESC
```

abandoned queue:

```
issuetype in "Service Request" AND status = "Waiting for customer" AND statusCategory in (2, 4) And updated < -7d ORDER BY updated
```