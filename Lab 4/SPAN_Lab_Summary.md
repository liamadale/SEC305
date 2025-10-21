# SPAN Configuration Lab Summary

## Network Topology
```
[PC-A Client]----Fa0/1----[S1 Switch]----Fa0/5----[PC-B Server]
                              |
                            Fa0/24
                              |
                         [PC-C Monitor]
```

## IP Addressing
| Device | Interface | IP Address    | Subnet Mask     | Default Gateway |
|--------|-----------|---------------|-----------------|-----------------|
| PC-A   | NIC       | 192.168.1.10  | 255.255.255.0   | 192.168.1.1     |
| PC-B   | NIC       | 192.168.1.20  | 255.255.255.0   | 192.168.1.1     |
| PC-C   | NIC       | 192.168.1.30  | 255.255.255.0   | 192.168.1.1     |
| S1     | VLAN 1    | 192.168.1.2   | 255.255.255.0   | 192.168.1.1     |

## Key SPAN Commands

### Basic SPAN Configuration
```cisco
! Configure basic SPAN session (monitor Fa0/1, destination Fa0/24)
monitor session 1 source interface fastethernet 0/1
monitor session 1 destination interface fastethernet 0/24

! Remove SPAN session
no monitor session 1
```

### Multiple Source Ports
```cisco
! Monitor multiple ports
monitor session 1 source interface fa0/1
monitor session 1 source interface fa0/5
monitor session 1 destination interface fa0/24
```

### Traffic Direction Filtering
```cisco
! Monitor only ingress (RX) traffic
monitor session 1 source interface fa0/1 rx
monitor session 1 destination interface fa0/24

! Monitor only egress (TX) traffic
monitor session 1 source interface fa0/1 tx
monitor session 1 destination interface fa0/24
```

### VLAN Monitoring
```cisco
! Monitor entire VLAN
monitor session 1 source vlan 1
monitor session 1 destination interface fa0/24
```

### Interface Range Monitoring
```cisco
! Monitor range of interfaces
monitor session 1 source interface range fa0/1 - 5
monitor session 1 destination interface fa0/24
```

## Verification Commands

### SPAN Status
```cisco
! Show all SPAN sessions
show monitor

! Show specific session details
show monitor session 1

! Check interface status
show ip interface brief

! View interface counters
show interface fa0/24
```

## Wireshark Setup & Filters

### Installation
- Download from: https://www.wireshark.org
- Install with WinPcap/Npcap for packet capture
- Launch and select network interface

### Useful Display Filters
```
icmp                           # Show only ping traffic
arp                           # Show only ARP traffic
ip.addr == 192.168.1.20       # Show traffic to/from specific IP
tcp                           # Show only TCP traffic
eth.dst == ff:ff:ff:ff:ff:ff  # Show broadcast traffic
```

### Testing Commands
```cmd
# From PC-A to generate traffic
ping 192.168.1.20
ping 192.168.1.20 -n 10
ping 192.168.1.20 -t          # Continuous (Ctrl+C to stop)
```

## Lab Parts Overview

### Part 1: Basic Switch Configuration ✅ (Already Complete)
- Hostname: S1
- Management IP: 192.168.1.2/24
- Ports Fa0/1, Fa0/5, Fa0/24 configured as access ports

### Part 2: Configure and Verify SPAN
1. Install Wireshark on PC-C
2. Capture baseline traffic (without SPAN)
3. Configure basic SPAN session
4. Capture mirrored traffic
5. Verify SPAN statistics

### Part 3: Advanced SPAN Configurations
1. Multiple source ports monitoring
2. Traffic direction filtering (RX/TX only)
3. VLAN monitoring
4. Interface range monitoring

### Part 4: Wireshark Analysis
- Protocol hierarchy analysis
- Conversation analysis
- IO graphs
- ARP, ICMP, TCP analysis

## SPAN Best Practices

### Do's
- Match destination port bandwidth to source ports
- Monitor only necessary ports/VLANs
- Document SPAN sessions
- Remove unused sessions
- Monitor switch CPU usage

### Don'ts
- Don't use destination port for normal traffic
- Don't oversubscribe destination port bandwidth
- Don't leave unnecessary SPAN sessions running

## Common Issues & Troubleshooting

### No Traffic in Wireshark
- Verify SPAN configuration: `show monitor`
- Check interface status: `show ip interface brief`
- Ensure correct Wireshark interface selected

### Packet Loss
- Check destination port bandwidth
- Verify source port traffic levels
- Monitor switch CPU usage

### Missing Traffic Types
- Check SPAN direction (RX/TX/Both)
- Verify source port configuration
- Check Wireshark display filters

## Quick Reference Commands
```cisco
! Essential verification commands
show monitor
show monitor session 1
show ip interface brief
show interface fa0/24

! Clean up
no monitor session 1
```

## Lab Completion Checklist
- [ ] Wireshark installed on PC-C
- [ ] Basic SPAN session configured and tested
- [ ] Multiple source ports tested
- [ ] Direction filtering tested (RX/TX)
- [ ] VLAN monitoring tested
- [ ] Traffic analysis completed in Wireshark
- [ ] All SPAN sessions removed
- [ ] Running config saved and submitted
