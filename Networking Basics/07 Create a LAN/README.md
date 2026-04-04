# Create a LAN

A hands-on networking lab completed in Cisco Packet Tracer that covers building a branch office LAN from scratch: powering on devices, cabling them together, configuring IPv4 addressing, and verifying local and internet connectivity using diagnostic commands.

## 🎯 Objectives

- Connect network devices and hosts using appropriate cables
- Configure end devices with IPv4 addressing (DHCP and static)
- Verify connectivity between hosts and to the internet
- Use `ipconfig` and `tracert` to inspect host network information

## 📋 Lab Steps

### Part 1 – Connect Network Devices and Hosts

#### Step 1: Power on the end devices and Office Router

1. Click each device and open its **Physical** tab.
2. Locate the power switch in the Physical Device View window.
3. Click the power switch to turn the device on. A green light near the switch confirms it is powered on.

> There is no power switch on the switch model used in this activity.

#### Step 2: Connect the end devices

Use the connections table below and **Copper Straight-Through** cables for all links. For the internet-to-Office-Router connection, select the device and port from the dropdown menus that appear when clicking the cloud with the connections tool selected.

**Connections Table**

| Device        | Interface/Port | Connected to Device | Connection Interface/Port |
|---------------|----------------|---------------------|---------------------------|
| Office Router | G0/0           | ISP1                | G0/0                      |
| Office Router | G0/1           | Switch              | G0/1                      |
| Admin PC      | NIC (F/0)      | Switch              | F0/1                      |
| Manager PC    | NIC (F/0)      | Switch              | F0/2                      |
| Printer       | NIC (F/0)      | Switch              | F0/24                     |

> Interfaces labeled **G** are GigabitEthernet; interfaces labeled **F** are FastEthernet.

After cabling, wait briefly for all link lights to turn green before proceeding.

---

### Part 2 – Configure Devices with IPv4 Addressing

#### Step 1: Configure the hosts

**Admin PC and Manager PC (DHCP):**

1. Click each PC and open the **Desktop** tab.
2. Open **IP Configuration** and select **DHCP**.
3. The Office Router is pre-configured as a DHCP server and will assign addresses automatically.

**Printer (Static):**

1. Click the **Printer** and open the **Config** tab.
2. Click **FastEthernet0** in the left-hand pane.
3. Enter the IP address and subnet mask from the Addressing Table: `192.168.1.100 / 255.255.255.0`.

> Printers and servers are typically given static addresses so other devices can reliably reach them by a known IP. The Admin and Manager PCs will share the same subnet mask and default gateway because they are on the same network, but each receives a unique IP address from DHCP.

---

### Part 3 – Verify End Device Configuration and Connectivity

#### Step 1: Verify connectivity between the two PCs

1. On each PC, open the **Desktop** tab and check **IP Configuration**. Both should have received addresses on the `192.168.1.0 / 255.255.255.0` network, along with a default gateway and DNS server.
2. From the **Command Prompt** on **Admin PC**, ping the Printer:

   ```
   ping 192.168.1.100
   ```

3. Repeat the ping from **Manager PC**. Successful replies confirm that both PCs and the printer are connected and correctly addressed.

#### Step 2: Verify internet connectivity

1. On either PC, open the **Desktop** tab and launch **Web Browser**.
2. Enter the IP address of the internet server in the URL bar:

   ```
   209.165.200.225
   ```

3. Confirm the webpage loads. Then repeat the process using the URL `www.cisco.pt` instead of the IP address. Both should succeed.

---

### Part 4 – Use Networking Commands to View Host Information

#### Step 1: Use the ipconfig command

From the **Command Prompt** on either PC, run:

```
ipconfig
```

Then run the extended version:

```
ipconfig /all
```

> `ipconfig` displays the IP address, subnet mask, and default gateway. `ipconfig /all` adds the physical MAC address of the NIC, along with the DHCP and DNS server addresses.

#### Step 2: Use the tracert command

From the **Command Prompt** on either PC, trace the path to the web server:

```
tracert www.cisco.pt
```

Review the output to identify each router hop along the path. Two routers appear in the trace: the Office Router on the local network and a second router inside the internet cloud. Each hop is identified by the IP address of its incoming interface.

## 🔑 Key Learnings

### DHCP and Static Addressing Serve Different Use Cases

Hosts like workstations benefit from DHCP because their addresses do not need to be predictable. Devices like printers and servers are given static addresses because other hosts must be able to reach them at a consistent, known IP. Both methods coexist on the same network and are not mutually exclusive.

### The Default Gateway Connects the LAN to the Outside World

Every host on the branch office LAN forwards traffic for remote destinations to the Office Router, which serves as the default gateway. Understanding this address is essential for both connectivity and for accessing the router's management interface.

### ipconfig and tracert Are First-Tier Diagnostic Tools

`ipconfig` confirms that a host has received a valid address, mask, and gateway. `ipconfig /all` extends this by showing the MAC address and DHCP/DNS server details. `tracert` shows the hop-by-hop path a packet takes to a destination, identifying each intermediate router by its IP address. Used together, these two commands narrow down where a connectivity problem exists without requiring access to any network device other than the local host.

### DNS Translates Names to Addresses

Successfully reaching a server by IP but failing by URL isolates the fault to DNS. The IP layer is working correctly; the name resolution layer is not. This is a practical and recurring pattern in real-world network troubleshooting.

## 📌 Points to Remember

- Use Copper Straight-Through cables for all host-to-switch and switch-to-router connections.
- DHCP clients and their statically addressed peers must share the same subnet for local communication to succeed.
- `ipconfig /all` reveals the MAC address and DHCP/DNS server information that `ipconfig` alone does not show.
- `tracert` identifies routers along the path to a destination by their IP addresses, not their hostnames.
- A successful ping to an IP address with a failed URL connection points to a DNS problem, not a routing or connectivity problem.