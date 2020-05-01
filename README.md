# Investigating ESXi Latency

### On ESXi shell,
[esxtop](https://www.virten.net/vmware/esxtop/)

1. Monitoring storage performance per HBA: d, f, b, c, d, e, h, j, s, 2, Enter.
2. Monitoring storage performance per LUN: u, f, b, c, f, h, s, 2, Enter.
3. Monitoring storage performance per VM: v, f, b, d, e, h, j, s, 2, Enter.

### Queue depth
Per-LUN maximum queue depth is 256

Per-adapter maximum queue depth is 4096

By increasing the per LUN queue depth from 32 to 64, it can take fewer LUNs to saturate a port queue. For
example, 4096/32=128 LUNs, but 4096/64=64 LUNs

Compare LOAD,
```
LOAD = ( ACTV + QUED ) / DQLEN
```

If LOAD < 1: no need to change DQLEN

If ( LOAD > 1 ) and ( backend disk or storage has spare IOPS ): can increase DQLEN

Observe CMDS/s and GAVG/cmd, if GAVG/cmd increases more than CMDS/s, maybe DQLEN should not be increased.

### Latency in a nutshell,
![Stack latency](stack_latency.png)
