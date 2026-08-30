# Subnetting Basics Assignment

## Task 1 - Binary ↔ decimal for a single octet

### 1.1 - Decimal to binary

| Decimal | Binary |
|---|---|
| 10 | 00001010 |
| 210 | 11010010 (Work: 128 + 64 + 16 + 2 = 210) |
| 168 | 10101000 (Work: 168 - 128 = 40. 40 - 32 = 8. 8 - 8 = 0) |
| 16 | 00010000 |
| 255 | 11111111 |
| 128 | 10000000 |
| 192 | 11000000 (Work: 128 + 64 = 192) |
| 248 | 11111000 |
| 0 | 00000000 |

### 1.2 - Binary to decimal

| Binary | Decimal |
|---|---|
| 11000000 | 192 |
| 11111111 | 255 |
| 10101000 | 168 (128 + 32 + 8) |
| 00010000 | 16 |
| 11111000 | 248 (128 + 64 + 32 + 16 + 8) |
| 11010010 | 210 (128 + 64 + 16 + 2) |

### 1.3 - Full-address conversion

- 10.210.168.16 → 00001010.11010010.10101000.00010000
- 192.168.0.1 → 11000000.10101000.00000000.00000001
- 172.16.5.100 → 10101100.00010000.00000101.01100100

Reverse:
- 11000000.10101000.00000001.00000001 → 192.168.1.1
- 00001010.00001010.00000000.01001011 → 10.10.0.75

## Task 2 - Recognize the class and CIDR

### 2.1 - What class is it?

| Address | Class | Default mask (dotted) | Default mask (CIDR) |
|---|---|---|---|
| 10.0.0.5 | A | 255.0.0.0 | /8 |
| 192.168.1.1 | C | 255.255.255.0 | /24 |
| 172.16.4.20 | B | 255.255.0.0 | /16 |
| 8.8.8.8 | A | 255.0.0.0 | /8 |
| 200.100.50.25 | C | 255.255.255.0 | /24 |

### 2.2 - Mask ↔ CIDR ↔ binary

| Dotted-decimal | CIDR | Binary |
|---|---|---|
| 255.255.255.0 | /24 | 11111111.11111111.11111111.00000000 |
| 255.255.0.0 | /16 | 11111111.11111111.00000000.00000000 |
| 255.0.0.0 | /8 | 11111111.00000000.00000000.00000000 |
| 255.255.255.192 | /26 | 11111111.11111111.11111111.11000000 |
| 255.255.248.0 | /21 | 11111111.11111111.11111000.00000000 |
| 255.255.255.128 | /25 | 11111111.11111111.11111111.10000000 |

### 2.3 - Networks and hosts per class

| Class | Default CIDR | Number of possible networks | Number of hosts per network |
|---|---|---|---|
| A | /8 | 128 nets | 16,777,214 hosts |
| B | /16 | 16,384 nets | 65,534 hosts |
| C | /24 | 2,097,152 nets | 254 hosts |

## Task 3 - The five key values

### 3.1 - 172.16.0.0/16
- mask: 255.255.0.0
- network: 172.16.0.0
- gateway: 172.16.0.1
- host range start: 172.16.0.2
- host range end: 172.16.255.254
- broadcast: 172.16.255.255

### 3.2 - 10.10.0.0/26
- mask: 255.255.255.192
- network: 10.10.0.0
- gateway: 10.10.0.1
- host range start: 10.10.0.2
- host range end: 10.10.0.62
- broadcast: 10.10.0.63

### 3.3 - 192.168.5.0/28
- mask: 255.255.255.240
- network: 192.168.5.0
- gateway: 192.168.5.1
- host range start: 192.168.5.2
- host range end: 192.168.5.14
- broadcast: 192.168.5.15

### 3.4 - 10.0.0.0/30
- mask: 255.255.255.252
- network: 10.0.0.0
- gateway: 10.0.0.1
- host range start: 10.0.0.2
- host range end: 10.0.0.2
- broadcast: 10.0.0.3

### 3.5 - 192.168.100.128/25
- mask: 255.255.255.128
- network: 192.168.100.128
- gateway: 192.168.100.129
- host range start: 192.168.100.130
- host range end: 192.168.100.254
- broadcast: 192.168.100.255

## Task 4 - Which subnet does this host belong to?

### 4.1 - 10.10.0.75/26
- Network address: 10.10.0.64
- Broadcast: 10.10.0.127
- Valid host?: Yes. The .64 network goes up to .127, so 75 falls right in the middle.

### 4.2 - 192.168.1.200/26
- Network address: 192.168.1.192
- Broadcast: 192.168.1.255
- Valid host?: Yes, it sits between the network ID (.192) and the broadcast (.255).

### 4.3 - 172.16.5.130/25
- Network address: 172.16.5.128
- Broadcast: 172.16.5.255
- Valid host?: Yes. The /25 splits the last octet in half. This is the top half (.128 block), and 130 is a normal IP inside it.

### 4.4 - 10.0.0.0/30
- Network address: 10.0.0.0
- Broadcast: 10.0.0.3
- Valid host?: No, it's the network address itself so you can't assign it to a device.

## Task 5 - Slicing up a /24

### 5.1 - Four equal /26 subnets

**Subnet 1:**
- Network: 192.168.10.0
- Gateway: 192.168.10.1
- Range: 192.168.10.2 - 192.168.10.62
- Broadcast: 192.168.10.63

**Subnet 2:**
- Network: 192.168.10.64
- Gateway: 192.168.10.65
- Range: 192.168.10.66 - 192.168.10.126
- Broadcast: 192.168.10.127

**Subnet 3:**
- Network: 192.168.10.128
- Gateway: 192.168.10.129
- Range: 192.168.10.130 - 192.168.10.190
- Broadcast: 192.168.10.191

**Subnet 4:**
- Network: 192.168.10.192
- Gateway: 192.168.10.193
- Range: 192.168.10.194 - 192.168.10.254
- Broadcast: 192.168.10.255

### 5.2 - Enough hosts?

| CIDR | Total addresses | Usable hosts |
|---|---|---|
| /24 | 256 | 254 |
| /25 | 128 | 126 |
| /26 | 64 | 62 |
| /27 | 32 | 30 |
| /28 | 16 | 14 |
| /29 | 8 | 6 |
| /30 | 4 | 2 |

A /26 fits up to 62 hosts, so yes it would technically work for everyone, but it wastes a lot of IP space for the smaller groups. Here is a better sizing:

- Dept A (50 hosts): /26 (fits 62)
- Dept B (25 hosts): /27 (fits 30)
- Dept C (10 hosts): /28 (fits 14)
- Dept D (2 hosts): /30 (fits exactly 2)

## Task 6 - IPv6, briefly

### 6.1 - Hex ↔ decimal ↔ binary

| Hex | Decimal | Binary |
|---|---|---|
| 0 | 0 | 0000 |
| 5 | 5 | 0101 |
| a | 10 | 1010 |
| f | 15 | 1111 |

### 6.2 - Compress these IPv6 addresses

- 2001:0df8:23f2:0000:0000:0000:0000:0f11 → 2001:df8:23f2::f11
- 2001:0000:00d0:00f2:0000:0000:0000:0f11 → 2001:0:d0:f2::f11
- fe80:0000:0000:0000:0000:0000:0000:0001 → fe80::1

### 6.3 - A conceptual question

We need IPv6 because we basically ran out of IPv4 addresses. With smartphones, laptops, and smart home devices, there are just way too many things on the internet now for the ~4 billion IPv4 addresses. IPv6 uses a 128-bit address space, which is so huge that we can give every single device its own public IP and finally stop relying on workarounds like NAT.
