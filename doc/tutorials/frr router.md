# Useful commands for frr router
## Start reload etc...

this command will follow what it is indicated in /etc/frr/frr.conf
```
systemctl <start|restart|reload|stop> frr
```

## Open shell

```
vtysh
```

## BGP Debugging

<details>
<summary> List of useful commands</summary>

shows neighbor status and number of prefixes received from peers
```
show bgp ipv4 unicast summary
```

shows bgp routing table and best path selections
```
show bgp ipv4 unicast
```

Indicates why peering is not coming up, look in log file
```
debug bgp neighbor events
```

Indicates what is happening for routes received, look in log file
```
debug bgp updates
```
</details>


