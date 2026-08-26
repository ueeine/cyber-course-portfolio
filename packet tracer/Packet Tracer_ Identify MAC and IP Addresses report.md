# Packet Tracer: Identify MAC and IP Addresses

## Objective

The purpose of this activity is to observe how MAC addresses and IPv4 addresses are used when data travels through a network. The activity compares communication between two devices on the same network with communication between devices on different networks.

## Topology

The topology contains two networks connected by a router:

- `172.16.31.0/24`: Router, Switch 2, `172.16.31.2`, and `172.16.31.3`
- `10.10.10.0/24`: Router, Switch 1, Access Point, `10.10.10.2`, and `10.10.10.3`

The paths used in the activity are:

```text
Local: 172.16.31.3 → Switch 2 → 172.16.31.2

Remote request: 172.16.31.3 → Switch 2 → Router → Switch 1 → Access Point → 10.10.10.2

Reply: 10.10.10.2 → Access Point → Switch 1 → Router → Switch 2 → 172.16.31.3
```

## Procedure

1. Open the Packet Tracer activity file.
2. Click `172.16.31.3` and open **Desktop > Command Prompt**.
3. Use **Realtime** mode to run the first ping.
4. Change to **Simulation** mode and run the ping again.
5. Click the envelope that appears beside the source device.
6. Check the **OSI Model** and **Outbound PDU Details** tabs.
7. Record the device, source MAC, destination MAC, source IPv4, and destination IPv4.
8. Click **Capture / Forward** to move the PDU one step at a time.
9. Record the information at every device on the path.
10. Repeat the process for the reply message.

# Part 1: Local Communication

## Test

At the command prompt on `172.16.31.3`, enter:

```text
ping 172.16.31.2
```

Both devices are on the `172.16.31.0/24` network. The packet travels through Switch 2 and does not need to go through the router.

## Local PDU Information

The first values shown in the activity are:

| At Device | Source MAC | Destination MAC | Source IPv4 | Destination IPv4 |
|---|---|---|---|---|
| `172.16.31.3` | `0060.7036.2849` | `000C:85CC:1DA7` | `172.16.31.3` | `172.16.31.2` |
| Switch 2 | Record from PDU | Record from PDU | N/A | N/A |
| `172.16.31.2` inbound | Record from PDU | Record from PDU | `172.16.31.3` | `172.16.31.2` |
| `172.16.31.2` reply | Record from PDU | Record from PDU | `172.16.31.2` | `172.16.31.3` |

The reply uses the reverse addresses. The device that received the original ping becomes the source of the reply.

# Part 2: Remote Communication

## Test

At the command prompt on `172.16.31.3`, enter:

```text
ping 10.10.10.2
```

The first one or two requests may time out. Run the command again if necessary. Since `10.10.10.2` is on a different network, the packet must pass through the router.

The first PDU should show information similar to this:

| At Device | Source MAC | Destination MAC | Source IPv4 | Destination IPv4 |
|---|---|---|---|---|
| `172.16.31.3` | `0060.7036.2849` | `00D0:BA8E:741A` | `172.16.31.3` | `10.10.10.2` |

The destination MAC address `00D0:BA8E:741A` belongs to the router's interface on the `172.16.31.0/24` side. The activity identifies this interface as **FastEthernet1/0**.

## Remote Request PDU Information

| At Device | Source MAC | Destination MAC | Source IPv4 | Destination IPv4 |
|---|---|---|---|---|
| `172.16.31.3` | Record from PDU | Record from PDU | `172.16.31.3` | `10.10.10.2` |
| Switch 2 | Record from PDU | Record from PDU | N/A | N/A |
| Router (in) | Record from PDU | Record from PDU | `172.16.31.3` | `10.10.10.2` |
| Router (out) | Record from PDU | Record from PDU | `172.16.31.3` | `10.10.10.2` |
| Switch 1 | Record from PDU | Record from PDU | N/A | N/A |
| Access Point | Record from PDU or N/A | Record from PDU or N/A | N/A | N/A |
| `10.10.10.2` | Record from PDU | Record from PDU | `172.16.31.3` | `10.10.10.2` |

At the router, the MAC addresses change because the router creates a new frame for the other network. The IPv4 source and destination remain the same.

## Remote Reply PDU Information

Follow the reply from `10.10.10.2` back to `172.16.31.3` and complete the table.

| At Device | Source MAC | Destination MAC | Source IPv4 | Destination IPv4 |
|---|---|---|---|---|
| `10.10.10.2` | Record from PDU | Record from PDU | `10.10.10.2` | `172.16.31.3` |
| Access Point | Record from PDU or N/A | Record from PDU or N/A | N/A | N/A |
| Switch 1 | Record from PDU | Record from PDU | N/A | N/A |
| Router (in) | Record from PDU | Record from PDU | `10.10.10.2` | `172.16.31.3` |
| Router (out) | Record from PDU | Record from PDU | `10.10.10.2` | `172.16.31.3` |
| Switch 2 | Record from PDU | Record from PDU | N/A | N/A |
| `172.16.31.3` | Record from PDU | Record from PDU | `10.10.10.2` | `172.16.31.3` |

# Reflection Questions

### 1. What types of cables or media were used?

Copper, fiber, and wireless media were used.

### 2. Did the cables change the handling of the PDU?

No. The cables only provided the physical connection.

### 3. Did the wireless access point do anything to the PDUs?

Yes. It converted or repackaged the traffic into wireless 802.11 frames.

### 4. Did the access point change the PDU addressing?

No. It did not change the end-to-end IPv4 addresses.

### 5. What was the highest OSI layer used by the access point in this activity?

Layer 1.

### 6. At what OSI layer do cables and access points operate?

The expected answer for this activity is Layer 1, the Physical layer.

### 7. Which MAC address appeared first in the PDU Details tab?

The destination MAC address appeared first.

### 8. What do the red X and green check marks mean?

A green check means that the device accepted the PDU. A red X means that the device rejected it, usually because the destination MAC address did not match.

### 9. Where did the MAC addresses change between the two networks?

They changed at the router.

### 10. Which device uses MAC addresses beginning with `00D0:BA`?

The router.

### 11. What did the other MAC addresses belong to?

They belonged to the sending device, receiving device, and router interfaces shown in the PDU details.

### 12. Did the IPv4 source and destination addresses change?

No. The IPv4 addresses stayed the same while the request traveled through the router.

### 13. What happens to the addresses in the ping reply?

The source and destination addresses are reversed. The receiving device becomes the source of the reply.

### 14. Why are the router interfaces in two different IP networks?

A router connects different IP networks. Each interface must have an address from the network to which it is connected.

### 15. Which networks are connected by the router?

The router connects `172.16.31.0/24` and `10.10.10.0/24`.

## Conclusion

The local ping traveled from `172.16.31.3` to `172.16.31.2` through Switch 2. Because both devices were on the same network, the router was not needed.

The remote ping traveled from `172.16.31.3` through Switch 2, the router, Switch 1, and the access point before reaching `10.10.10.2`. The router changed the MAC addresses when forwarding the frame to the other network, but the IPv4 source and destination stayed the same. The reply followed the reverse path and used reversed source and destination addresses.
