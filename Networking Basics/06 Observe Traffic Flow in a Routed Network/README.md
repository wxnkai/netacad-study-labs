# 🌐 Observe Traffic Flow in a Routed Network

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates the operational difference between a flat, unrouted LAN and a segmented, routed network. Using Simulation mode, ARP broadcast behavior is observed before and after network segmentation to illustrate why routing between subnets improves efficiency at scale.

## 🎯 Objectives

- Observe how ARP broadcasts propagate across an unrouted single-subnet LAN
- Reconfigure the network so each department operates on its own dedicated IPv4 network
- Observe how routing contains ARP traffic within the relevant network segment
- Understand the efficiency benefits of segmenting a network with a router

## 📋 Lab Steps

### Part 1 – Observe Traffic Flow in an Unrouted LAN

In this part, all departments share a single flat IPv4 network. ARP broadcasts are issued to every device on the LAN, demonstrating the inefficiency of this design at scale.

#### Step 1: Clear the ARP Cache on Sales 1

1. Hover over the **Sales 1** host to note its IP address.
2. Click **Sales 1**, open the **Desktop** tab, and select **Command Prompt**.
3. Run the following command to check the ARP cache:

   ```
   arp -a
   ```

4. If any entries exist, clear them with:

   ```
   arp -d
   ```

#### Step 2: Observe Traffic Flow Across the Flat Network

1. Click the **Simulation** tab in the lower right corner to switch from Realtime to Simulation mode.
2. Open the **Command Prompt** on **Sales 2** and enter:

   ```
   ping <IP address of Sales 1>
   ```

3. Click **Capture then Forward** (triangle with vertical bar) in the Simulation Panel to step the PDU through the network one hop at a time.
4. Click the PDU envelope that appears next to Sales 2 and inspect its contents.

   **Observe the following in the first PDU:**

   | Field | Value |
   |---|---|
   | Source MAC | MAC address of Sales 2 |
   | Destination MAC | `FFFF.FFFF.FFFF` (broadcast) |
   | Source IP | IP address of Sales 2 |
   | Destination IP | IP address of Sales 1 |

   > The destination MAC is the broadcast address because Sales 2 has no ARP entry for Sales 1. It must first broadcast an ARP request to discover the MAC address before it can send the ping.

5. Continue clicking **Capture then Forward** until a new PDU of a different color appears at Sales 2.

   >**Observe:** Every host on the network and the router interface must process the ARP broadcast, even those in unrelated departments. On a network of 150+ devices, frequent ARP broadcasts consume significant bandwidth and processing resources that would otherwise serve productive traffic.

6. Click the new (differently colored) PDU and open **Outbound PDU Details**. This is the first ICMP echo request issued by the ping after ARP has resolved the destination MAC.
7. Return to **Realtime** mode.

---

### Part 2 – Reconfigure the Network to Route Between LANs

In this part, each department switch is connected directly to a dedicated router interface, placing each department on its own isolated IPv4 network.

#### Step 1: Reconnect the Department Switches to the Router

The three department switches are currently connected to each other in a chain. Reconnect them to the Edge router instead.

1. Click the green triangle on the **Accounting switch** side of the cable linking it to the **Finance switch**.
2. Drag that cable end to the **Edge router** and connect it to **GigabitEthernet 1/0**.
3. Repeat this process for the cable linking the **Finance switch** to the **Sales switch**. Connect that end to the next available **GigabitEthernet** port on the Edge router.

Each switch is now connected directly to a dedicated router interface, and the Edge router has been pre-configured to serve each department on a separate IPv4 network.

#### Step 2: Renew IP Addresses on Finance and Sales Hosts

The Finance and Sales hosts need to obtain new addresses from the router's DHCP server on their new subnets. The Accounting network remains on `192.168.1.0/24` unchanged.

1. Open the **Command Prompt** on each of the four hosts in the **Finance** and **Sales** networks.
2. Run the following command on each host to force a DHCP renewal:

   ```
   ipconfig /renew
   ```

3. Confirm the new subnet assignments:

   | Department | New Network |
   |---|---|
   | Finance | `192.168.2.0/24` |
   | Sales | `192.168.3.0/24` |

---

### Part 3 – Observe Traffic Flow in the Routed Network

With each department now on its own subnet, ARP broadcasts are contained within their respective network segments.

#### Step 1: Ping Sales 1 from Sales 2

1. Return to the **Command Prompt** on **Sales 2** and verify the ARP cache is empty. Clear it with `arp -d` if needed.
2. Switch to **Simulation** mode.
3. Ping **Sales 1** from **Sales 2**.
4. Click **Capture then Forward** to step through the PDUs and observe the ARP broadcast behavior.

   **Observe:** This time, only **Sales 1** and the **router interface connected to the Sales network** receive and process the ARP broadcast. Hosts in Accounting and Finance are completely unaffected.

#### Step 2: Ping Other Hosts and the Internet Server

1. Repeat the ping process targeting hosts in other departments and the internet server.
2. Use **Capture then Forward** to observe how ARP requests are now contained within each subnet boundary.

   >**Key observation:** ARP broadcasts no longer propagate across the entire network. Each subnet handles its own ARP resolution independently, and only inter-network traffic is forwarded through the router.

## 🔑 Key Learnings

### ARP Broadcasts Are the Hidden Cost of a Flat Network

Every time a host needs to communicate with another host whose MAC address is not yet in its ARP cache, it sends a broadcast that every single device on the network must receive and process. On a small network this is negligible, but on a network of 150 or more devices, ARP broadcasts accumulate constantly as cache entries expire and new devices join. This consumes CPU cycles on every host and eats into the available bandwidth for actual work traffic.

### Routing Confines Broadcasts to the Relevant Subnet

When the network is segmented into three separate subnets, ARP broadcasts sent within the Sales department stay within the Sales subnet. The router does not forward Layer 2 broadcasts across its interfaces. This means that a host in Accounting is completely shielded from broadcast traffic generated in the Finance or Sales departments, and vice versa. The total broadcast domain for each department shrinks dramatically, which is the primary efficiency gain of network segmentation.

### The Router Interface Is the MAC Address Boundary Between Networks

When a host in the Sales department needs to reach a host in Accounting or access the internet, it addresses its Ethernet frame to the MAC address of the router interface on its subnet, not to the final destination. The router accepts the frame, reads the IP header, and forwards it out the appropriate interface with new Layer 2 addressing for the next segment. This is identical to the behavior observed in the MAC and IP Addresses lab: IP addressing is end-to-end while MAC addressing is local to each segment.

### DHCP Enables Fast Reassignment When the Network Topology Changes

When the switches were reconnected from a chain to individual router interfaces, the hosts on Finance and Sales needed new IP addresses to match their new subnets. Running `ipconfig /renew` forced each host to issue a fresh DHCP request to the router, which responded with the correct address for the newly assigned network. This demonstrates how DHCP simplifies reconfiguration at scale: no manual IP changes were required on any individual host.

### Network Segmentation Is a Foundation of Both Performance and Security

Separating departments into distinct subnets not only reduces broadcast traffic but also creates natural enforcement boundaries for access control. A router or firewall can apply policies between subnets to restrict which departments can communicate with which, making segmentation a foundational practice in both network design and cybersecurity. Traffic that never crosses a boundary cannot be intercepted or misused across that boundary.

## 📌 Points to Remember

- ARP broadcasts are sent to every device on the same subnet every time a host needs to resolve a MAC address it does not have cached, which becomes a significant overhead on large flat networks.
- Routers do not forward Layer 2 broadcast frames, so placing a router between network segments confines ARP traffic to the subnet where it originated.
- When a host communicates across subnets, it addresses its frame to the router's MAC address on that subnet, not to the destination host's MAC address directly.
- Running `ipconfig /renew` forces a host to request a fresh DHCP lease immediately, which is the fastest way to apply new addressing after a topology change without manually reconfiguring each device.
- Subnet segmentation shrinks the broadcast domain for each department, reducing unnecessary traffic on unrelated parts of the network and improving overall efficiency.
- Network segmentation also creates access control boundaries, making it a practice that serves both network performance and security architecture simultaneously.