# Examine NAT on a Wireless Router

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates how Network Address Translation (NAT) operates on a wireless router. The lab covers the distinction between public and private IP addressing, how DHCP assigns internal addresses, and how NAT rewrites packet headers when traffic crosses from a private network to the internet.

## 🎯 Objectives

- Examine the NAT configuration on a wireless router
- Set up four PCs to connect to the wireless router using DHCP
- Differentiate between public (ISP-assigned) and private (internal) IP addresses
- Observe NAT translation in action by inspecting packet headers in Simulation mode

## 📋 Lab Steps

### Part 1 – Examine the Configuration for Accessing the External Network

1. Add one PC to the workspace and connect it to the wireless router using a **Copper Straight-Through** cable.
2. Wait for all link lights to turn green, or click **Fast Forward** to speed up the process.
3. On the PC, open the **Desktop** tab and select **IP Configuration**. Click **DHCP** to obtain an address from the router.
4. Note the **Default Gateway** IP address, then close **IP Configuration**.
5. Open the **Web Browser**, enter the default gateway IP in the URL bar, and log in with:
   - **Username:** `admin`
   - **Password:** `admin`
6. Click the **Status** menu in the upper right corner to open the Router sub-menu page.
7. Scroll down to the **Internet Connection** section. The IP address shown here is the address assigned by the ISP.
   > If the IP shows as `0.0.0.0`, close the window, wait a few seconds, and try again. The router is still obtaining its address from the ISP DHCP server.
8. Observe that this is a **public IP address**, assigned by the ISP for use on the internet.

---

### Part 2 – Examine the Configuration for the Internal Network

1. Within the **Status** sub-menu, click **Local Network**.
2. Scroll down to review the **Local Network** section. This is the private IP address assigned to the router's internal interface.
3. Scroll further to review the **DHCP Server** information, including the range of addresses available for internal clients.
4. Observe that all internal addresses are **private IP addresses**, which cannot route directly across the internet.
5. Close the wireless router configuration window.

---

### Part 3 – Connect Three Additional PCs to the Router

1. Add three more PCs to the workspace and connect each to a separate port on the wireless router using **Copper Straight-Through** cables.
2. Wait for all link lights to turn green, or click **Fast Forward**.
3. On each PC, open the **Desktop** tab, select **IP Configuration**, and click **DHCP** to obtain an address. Close the window when done.
4. On each PC, open the **Command Prompt** and run:

   ```
   ipconfig /all
   ```

   Confirm that each PC has received a private IP address in the router's DHCP range.

   > Private IP addresses cannot cross the internet directly. NAT translation must occur at the router before traffic can reach external destinations.

---

### Part 4 – View NAT Translation Across the Wireless Router

1. Switch to **Simulation** mode by clicking the **Simulation** tab in the lower right corner (stopwatch icon).
2. In the **Simulation Panel**, click **Show All/None** to clear all visible event types.
3. Click **Edit Filters**, go to the **Misc** tab, and check the boxes for **TCP** and **HTTP**. Close the filter window.
4. Click the **Add Complex PDU** icon (open envelope) in the upper menu bar.
5. Click one of the PCs to set it as the source.
6. In the **Create Complex PDU** window, configure the following:
   - **Select Application:** `HTTP`
   - **Destination:** Click the **ciscolearn.nat.com** server
   - **Source Port:** `1000`
   - **Simulation Settings:** Select **Periodic**, set **Interval** to `120` seconds
7. Click **Create PDU**.
8. Double-click the Simulation Panel to detach it from the main window so the full network topology is visible.
9. Click **Play** in the Simulation Panel to begin the animation. Drag the play speed slider to the right to increase the animation speed.
   > Click **View Previous Events** if a Buffer Full message appears.

---

### Part 5 – Inspect Packet Headers to Observe NAT in Action

1. In the **Simulation Panel**, double-click the **3rd event line** in the event list. An envelope icon will appear in the workspace representing that packet.
2. Click the envelope in the workspace to open the packet details window.
3. Click the **Inbound PDU Details** tab and note the **source (SRC) IP address** and **destination IP address**.
4. Click the **Outbound PDU Details** tab and note the same fields.
5. Observe that the source IP address has changed between inbound and outbound. This is NAT replacing the private internal IP with the router's public IP before the packet is forwarded to the internet.
6. Click through additional event lines to trace how packet headers change at each hop throughout the session.
7. When finished, click **Check Results** to verify the lab is complete.

## 🔑 Key Learnings

### NAT Allows Private Networks to Share a Single Public IP Address

Every device on the internal network holds a private IP address that is not routable on the internet. NAT works by having the router replace the private source IP in each outgoing packet with its own public IP before forwarding it. When the response returns, the router reverses the mapping and delivers the packet to the correct internal host. This is how a single ISP-assigned address serves an entire household or office of devices simultaneously.

### Public and Private IP Addresses Serve Fundamentally Different Purposes

The ISP assigns one public IP to the router's WAN interface, and that address is globally unique and reachable from anywhere on the internet. The router's LAN interface uses a private IP from a reserved range (such as `192.168.x.x`), and all internal clients receive addresses from that same private range via DHCP. Private addresses are free to reuse across millions of networks worldwide precisely because NAT prevents them from ever appearing directly on the public internet.

### The Router Operates at the Boundary Between Two Distinct Networks

The wireless router in this lab has two separate IP identities: a public address on its WAN side facing the ISP, and a private address on its LAN side facing internal clients. This boundary is exactly where NAT translation occurs. Understanding this boundary is essential for diagnosing connectivity issues, configuring port forwarding, and understanding why devices behind NAT cannot be reached from the internet without explicit rules.

### The Source Port Is Part of the NAT Mapping Table

When configuring the Complex PDU, a specific source port (`1000`) was assigned. NAT does not only track IP addresses; it also tracks port numbers to maintain a translation table. This allows multiple internal devices to send traffic through the same public IP simultaneously, with each session distinguished by a unique port number. This mechanism is more precisely called **PAT** (Port Address Translation) and is the form of NAT used in most home routers.

## 📌 Points to Remember

- A router performing NAT has two IP addresses: a public address on the WAN interface assigned by the ISP, and a private address on the LAN interface used by internal clients.
- Private IP addresses are not routable on the internet, so NAT must translate them to the router's public IP before any traffic can reach an external destination.
- NAT rewrites the source IP address in outgoing packets and reverses the mapping on return traffic, which is visible by comparing the Inbound and Outbound PDU tabs in Simulation mode.
- NAT tracks both IP addresses and port numbers to manage multiple simultaneous sessions through a single public IP.