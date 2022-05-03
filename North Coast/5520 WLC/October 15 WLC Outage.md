# October 15th 2020 WLC Outage
#cisco #hardware #day 

## Austins Theory:
1. Rehydn does wlc firmware upgrade and Vista WLC sucessfuly does the firmware upgrade triggerin SME to do the same.
2. Vista Comes up first but does not see SME WLC (possibly latency issue), vista decides to enter maintnance mode because it can not talk to SME at all.
3. SME comes up and sees Vista in maintnacne mode, assumes the role of the primary but cant sync any licenses since Vista (License Holder) is in maintnance mode.
> AP failover works and AP's flip over to SME nobody notices anything went wrong.
4. Power Outage at PV brings their AP's off line, they come back up and attempt to talk to the SME WLC but it has no licenses. Later further breif comunication outages between sites brought AP's down which also did not come back up on account of missing licenses.