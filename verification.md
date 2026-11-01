# Verification guide

## Switch0

```text
show vlan brief
show interfaces trunk
show port-security
show port-security address
show port-security interface fa0/2
```

## Router0

```text
show ip interface brief
show ip dhcp binding
show ip dhcp pool
show ip route
```

## PCs

Use `ipconfig` to check DHCP addresses. From PC0, ping the actual DHCP address of PC2 to test inter-VLAN connectivity. If the first attempt times out due to ARP resolution, retry.

## Security violation test (completed — reported by project author)

Verify a sticky MAC on Fa0/2, replace PC0 with another endpoint on that port, and send traffic. Check `show port-security interface fa0/2` for a violation and shutdown. Reconnect the original PC and recover with `shutdown` followed by `no shutdown` on Fa0/2. Perform this only in the lab simulation.

**Status:** Completed according to the project author. A screenshot of the violation result has not been provided in this documentation package.
