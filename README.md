# 23-1 DHCP Configuration – Lab Exercise

## Overview

This lab demonstrates how to configure and troubleshoot **Dynamic Host Configuration Protocol (DHCP)** in a small campus network using Cisco Packet Tracer.

You will configure DHCP in three stages:

1. Configure the router's outside interface as a **DHCP client**.
2. Configure **R1 as a DHCP server** for the campus LAN.
3. Configure the network to use an **external DHCP server** located at `10.10.20.10`.

## Lab Objectives

By completing this lab, you will learn how to:

- Configure a Cisco router interface as a DHCP client.
- Verify a dynamically assigned IP address.
- Identify the DHCP server providing an address.
- Configure a Cisco router as a DHCP server.
- Configure DHCP excluded addresses.
- Configure PCs as DHCP clients.
- Verify DHCP-assigned IP addressing.
- Test DNS name resolution.
- Remove a Cisco DHCP configuration.
- Configure DHCP relay using `ip helper-address`.
- Troubleshoot DHCP communication between different subnets.

## Lab Topology

The lab consists of:

- **R1** – Campus router
- **PCs** – DHCP clients
- **DNS Server** – `10.10.20.10`
- **External DHCP Server** – `10.10.20.10`
- **Service Provider** – Provides DHCP addressing to R1's outside interface

> Open the `23-1 DHCP Configuration.pkt` file in Cisco Packet Tracer to load the startup configuration.

---

# Part 1 – Cisco DHCP Client

### Task 1 – Configure R1's Outside Interface

Configure **FastEthernet0/0** on R1 to obtain its IP address dynamically from the ISP.

The ISP has already been configured, and you do not have access to it.

### Task 2 – Verify the DHCP Address

Verify that R1 received its public IP address through DHCP.

You may need to wait a few minutes for the DHCP server to assign the address.

Useful command:

```cisco
show ip interface brief
```

You can also check the DHCP information with:

```cisco
show dhcp lease
```

### Task 3 – Identify the DHCP Server

Determine the IP address of the DHCP server that assigned the address to R1.

---

# Part 2 – Cisco DHCP Server

In this section, R1 will act as the DHCP server for the campus LAN.

### Task 4 – Configure DHCP on R1

Configure R1 to provide IP addresses for the:

```text
10.10.10.0/24
```

network.

The following addresses must be reserved for servers and printers:

```text
10.10.10.1 – 10.10.10.10
```

The DNS server is:

```text
10.10.20.10
```

### Task 5 – Configure PCs as DHCP Clients

In recent versions of Packet Tracer, manually configure each PC as a DHCP client:

1. Open the PC.
2. Select the **Desktop** tab.
3. Select **IP Configuration**.
4. Select **DHCP**.

### Task 6 – Verify DHCP Information

Verify that the PCs successfully received their network configuration from R1.

Check that each client received:

- IP address
- Subnet mask
- Default gateway
- DNS server

### Task 7 – Test DNS Resolution

From the PCs, ping the DNS server using its hostname:

```text
DNSserver
```

It may take some time for Packet Tracer's DNS service to resolve the hostname.

### Task 8 – Verify DHCP Clients on R1

On R1, verify that both PCs received IP addresses through DHCP.

Useful command:

```cisco
show ip dhcp binding
```

### Task 9 – Remove the DHCP Configuration

Remove the DHCP server configuration from R1.

The external DHCP server will be used in the next section.

### Task 10 – Release the DHCP Addresses

On each PC, open the Command Prompt and enter:

```text
ipconfig /release
```

This releases the DHCP-assigned IP address.

### Task 11 – Attempt to Renew the Address

Run:

```text
ipconfig /renew
```

Verify that the PCs can no longer obtain an IP address because R1 is no longer providing DHCP service.

---

# Part 3 – External DHCP Server

The external DHCP server is located at:

```text
10.10.20.10
```

It has already been configured with a DHCP scope for:

```text
10.10.10.0/24
```

However, the PCs are not receiving IP addresses from it.

### Task 12 – Troubleshoot the Problem

Determine why the PCs cannot obtain DHCP addresses from the external DHCP server.

Consider the following:

- Are the clients and DHCP server in the same subnet?
- Can DHCP broadcasts cross a router?
- Does R1 need to forward DHCP requests?

### Task 13 – Configure DHCP Relay

Configure R1 to forward DHCP requests from the `10.10.10.0/24` network to the external DHCP server at:

```text
10.10.20.10
```

Use the Cisco IOS command:

```cisco
ip helper-address 10.10.20.10
```

The command must be configured on the router interface that receives DHCP broadcasts from the clients.

### Task 14 – Verify DHCP

Verify that the PCs successfully receive their IP configuration from the external DHCP server.

Confirm that the clients receive:

- IP address from the `10.10.10.0/24` subnet
- Correct subnet mask
- Correct default gateway
- DNS server information

Useful PC command:

```text
ipconfig /all
```

Useful router command:

```cisco
show ip interface brief
```

---

# Key Concepts

## DHCP Client

A DHCP client automatically obtains network configuration information from a DHCP server instead of requiring manual IP configuration.

The information can include:

- IP address
- Subnet mask
- Default gateway
- DNS server

## DHCP Server

A DHCP server dynamically assigns IP addresses and other network parameters to clients.

In this lab, DHCP is demonstrated using:

- R1 as a DHCP server
- An external server at `10.10.20.10`

## DHCP Relay

DHCP clients initially send their DHCP requests as broadcasts.

Routers normally do not forward broadcasts between interfaces. Therefore, when the DHCP server is located on another network, a router must relay the DHCP request.

Cisco IOS uses:

```cisco
ip helper-address <DHCP-server-IP>
```

For this lab:

```cisco
ip helper-address 10.10.20.10
```

## Important Addresses

| Device / Network | Address |
|---|---|
| Campus LAN | `10.10.10.0/24` |
| Reserved addresses | `10.10.10.1 – 10.10.10.10` |
| External DHCP/DNS Server | `10.10.20.10` |
| DHCP Relay Destination | `10.10.20.10` |

---

# Verification Commands

### Check Interface Status

```cisco
show ip interface brief
```

### Check DHCP Bindings

```cisco
show ip dhcp binding
```

### Check DHCP Pool

```cisco
show ip dhcp pool
```

### Check DHCP Configuration

```cisco
show running-config
```

### Check DHCP Lease on a DHCP Client Interface

```cisco
show dhcp lease
```

### Check PC DHCP Information

```text
ipconfig /all
```

### Release a DHCP Address

```text
ipconfig /release
```

### Renew a DHCP Address

```text
ipconfig /renew
```

---

# Lab Completion Checklist

- [ ] Configure R1 FastEthernet0/0 as a DHCP client.
- [ ] Verify R1 received a public IP address.
- [ ] Identify R1's DHCP server.
- [ ] Configure R1 as a DHCP server.
- [ ] Reserve `10.10.10.1 – 10.10.10.10`.
- [ ] Configure the PCs as DHCP clients.
- [ ] Verify DHCP addressing on the PCs.
- [ ] Verify DNS resolution using `DNSserver`.
- [ ] Verify DHCP bindings on R1.
- [ ] Remove the DHCP configuration from R1.
- [ ] Release the PCs' DHCP addresses.
- [ ] Verify the PCs cannot renew their addresses.
- [ ] Configure R1 as a DHCP relay.
- [ ] Configure `ip helper-address 10.10.20.10`.
- [ ] Verify the PCs receive addresses from the external DHCP server.

## Expected Result

At the end of the lab, the PCs on the `10.10.10.0/24` network should successfully obtain their IP configuration from the **external DHCP server at `10.10.20.10`** through R1 acting as a **DHCP relay agent**.