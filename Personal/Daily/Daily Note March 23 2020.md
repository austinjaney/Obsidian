# Daily Note March 23 2020
#day #northcoast #sysadmin

These Queues / JQL Queries were changed to be more basic so that who is assigned to what is more clear, eventualy we should probably change them back.

Austins Queue:

```
statusCategory in (2, 4) AND assignee in (admin) OR (summary ~ 5d55e3f6c4be040db2d55e1f OR description ~ 5d55e3f6c4be040db2d55e1f OR comment ~ 5d55e3f6c4be040db2d55e1f) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
```

Westons Queue:

```
statusCategory in (2, 4) AND assignee in (wjones) OR (summary ~ 5d66a2acec4c1d0c15861d64 OR description ~ 5d66a2acec4c1d0c15861d64 OR comment ~ 5d66a2acec4c1d0c15861d64) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
```

Alans Queue:

```
statusCategory in (2, 4) AND assignee in (atsunekawa) OR (summary ~ 5d6ea87694e3580d923b24e6 OR description ~ 5d6ea87694e3580d923b24e6 OR comment ~ 5d6ea87694e3580d923b24e6) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
```

Rehydns Queue:

```
statusCategory in (2, 4) AND assignee in (rmontes) OR (summary ~ 5d55fb82c58ad70d9964939b OR description ~ 5d55fb82c58ad70d9964939b OR comment ~ 5d55fb82c58ad70d9964939b) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC
```

Erikas Queue:

```
statusCategory in (2, 4) AND assignee in (ehester) OR (summary ~ 5d66a2a8388d780dac241147 OR description ~ 5d66a2a8388d780dac241147 OR comment ~ 5d66a2a8388d780dac241147) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
```

Kentons Queue:

```
statusCategory in (2, 4) AND assignee in (krussell) OR (summary ~ 5d61667be09baa0d7487b084 OR description ~ 5d61667be09baa0d7487b084 OR comment ~ 5d61667be09baa0d7487b084) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
```

Martys Queue:

```
statusCategory in (2, 4) AND assignee in (mhudacko) OR (summary ~ 5d5af29b64b6510d56c40caa OR description ~ 5d5af29b64b6510d56c40caa OR comment ~ 5d5af29b64b6510d56c40caa) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
```

Ricks Queue:

```
statusCategory in (2, 4) AND assignee in (rgarza) OR (summary ~ 5d69497b9eb3300c0f6f819c OR description ~ 5d69497b9eb3300c0f6f819c OR comment ~ 5d69497b9eb3300c0f6f819c) AND statusCategory in (2, 4) AND updatedDate >= -9d ORDER BY priority DESC, "Time to done" ASC
```