# 🔍 Identify MAC and IP Addresses

A hands-on networking lab completed in Cisco Packet Tracer that examines how MAC and IP addresses behave differently depending on whether traffic is destined for a local or remote network. Using Simulation mode, PDU headers are inspected at each hop to observe how Layer 2 addressing changes while Layer 3 addressing remains constant end-to-end.

## 🎯 Objectives

- Gather PDU addressing information for local network communication
- Gather PDU addressing information for remote network communication
- Observe how MAC addresses change at each network boundary while IP addresses remain unchanged
- Understand the role of the default gateway and router in cross-network communication

## 📋 Lab Steps

### Part 1 – Gather PDU Information for Local Network Communication

This part demonstrates that devices on the same subnet communicate directly without involving a default gateway.

1. Click host **172.16.31.3** and open the **Command Prompt**.
2. Run the following command to confirm basic connectivity:

   ```
   ping 172.16.31.2
   ```

3. Switch to **Simulation** mode using the **Simulation** tab in the lower right corner.
4. Repeat the ping command. An envelope icon (PDU) will appear next to host `172.16.31.3`.
5. Click the PDU envelope and open both the **OSI Model** tab and the **Outbound PDU Details** tab.
6. Record the following addressing details for the first step:

   | Field | Value |
   |---|---|
   | At Device | `172.16.31.3` |
   | Source MAC Address | `0060：7036.2849` |
   | Destination MAC Address | `000C:85CC:1DA7` |
   | Source IP Address | `172.16.31.3` |
   | Destination IP Address | `172.16.31.2` |
   > Cisco MAC addresses are commonly represented in a dotted-quad format (`xxxx.xxxx.xxxx`).

7. Click **Capture / Forward** (right arrow with vertical bar) to advance the PDU to the next hop. At each step, record the inbound and outbound MAC and IP addresses into a tracking table.
8. Continue clicking **Capture / Forward** until the PDU reaches its destination at `172.16.31.2`.
9. When the echo reply returns, open the **Outbound PDU Details** tab and observe that the source and destination addresses are reversed. This is the ping echo-reply being sent back to `172.16.31.3`.
10. Return to **Realtime** mode when finished.

---

### Part 2 – Gather PDU Information for Remote Network Communication

This part demonstrates how MAC addressing must change at the router boundary when traffic crosses between two different IP networks.

1. Return to the **Command Prompt** on host **172.16.31.3**.
2. Run the following command:

   ```
   ping 10.10.10.2
   ```

   > The first one or two pings may time out while ARP resolves. This is expected.

3. Switch to **Simulation** mode and repeat the same ping command. A PDU will appear next to `172.16.31.3`.
4. Click the PDU and record the following addressing details:

   | Field | Value |
   |---|---|
   | At Device | `172.16.31.3` |
   | Source MAC Address | `0060.7036.2849` |
   | Destination MAC Address | `00D0:BA8E:741A` |
   | Source IP Address | `172.16.31.3` |
   | Destination IP Address | `10.10.10.2` |

   > The destination MAC address belongs to the router's FastEthernet1/0 interface, not to the final destination host. The sending device addresses its frame to the router (gateway) because the destination IP is on a different network.

5. Click **Capture / Forward** to advance the PDU to the router. Open the **Inbound PDU Details** and **Outbound PDU Details** tabs and record both sets of MAC and IP addresses.
6. Observe that as the PDU exits the router toward the `10.10.10.0/24` network, the source and destination MAC addresses change to reflect the router's outbound interface and the destination host on that network. The source and destination IP addresses remain unchanged throughout.
7. Continue clicking **Capture / Forward** until the PDU reaches `10.10.10.2`, recording the addressing at each hop.
8. Repeat the process for the echo-reply traveling from `10.10.10.2` back to `172.16.31.3`, recording both inbound and outbound PDU details at each device.

---

### Reflection – Key Observations from the Lab

The following questions summarise the key observations from inspecting the PDU data across both parts:

- **Cable and media types** (copper, fiber, and wireless) were used to connect devices, but the media type did not affect how PDUs were handled at the IP or MAC layer.
- **The wireless Access Point** repackaged frames as 802.11 wireless frames but did not change the MAC or IP addressing within those frames. It operates at **Layer 1** only.
- **MAC addresses in PDU Details** always list the destination address first, followed by the source address.
- **PDUs marked with a red X** were not accepted by a device because the destination MAC address did not match that device's own MAC. This is normal and expected switching behavior.
- **MAC addresses changed at the router** every time the PDU crossed between the `10.10.10.0/24` and `172.16.31.0/24` networks. MAC addresses are local to each network segment.
- **IP addresses never changed** at any hop from source to destination. Layer 3 addressing is end-to-end.
- **The router belongs to both IP networks** simultaneously. This is its fundamental purpose: to interconnect different IP networks by being a member of each one.

## 🔑 Key Learnings

### MAC Addresses Are Local and Change at Every Network Boundary

A MAC address is a Layer 2 identifier that is only meaningful within a single network segment. Each time a packet crosses a router, the router strips the incoming Ethernet frame and builds a brand new one with updated source and destination MAC addresses for the next segment. In this lab, the MAC addresses changed completely at the router, while the IP addresses inside the packet remained identical from source to destination.

### IP Addresses Are End-to-End and Never Change in Transit

Unlike MAC addresses, the source and destination IP addresses embedded in the IP packet header remain the same from the originating host all the way to the final destination, regardless of how many routers the traffic passes through. This separation of concerns between Layer 2 and Layer 3 is what makes scalable internetworking possible.

### When Reaching a Remote Network, the Frame is Addressed to the Gateway, Not the Final Host

When a host sends traffic to a destination on a different network, it cannot address the Ethernet frame directly to the remote host because MAC addresses do not span network boundaries. Instead, the host addresses the frame to the MAC address of its default gateway (the router). The router then forwards the packet toward the destination, readdressing the frame at each hop until delivery.

### The Router Is the Boundary Between Layer 2 Domains

The router is the only device in the path that decapsulates the incoming frame, reads the Layer 3 IP header to make a forwarding decision, and then builds a new Layer 2 frame for the outbound segment. Switches and access points forward frames without modifying addressing. The router is where both the MAC addresses and the network segment change simultaneously.

### Access Points Operate at Layer 1 and Do Not Modify Addressing

The wireless access point in this lab translated between wired Ethernet frames and wireless 802.11 frames but left all MAC and IP addressing intact. It is a physical layer device from the perspective of the PDU addressing: the frame payload, source MAC, and destination MAC passed through unchanged. This is an important distinction from a router, which actively rewrites Layer 2 headers.

---

### Full Hop-by-Hop Breakdown Using a Similar Example

**Step 1** — Host A checks its routing table. Host A sees that `10.0.0.20` is not on its local subnet (`192.168.0.0/24`), so it knows it can't deliver this directly. It must send the packet to its default gateway — the router's LAN interface at `192.168.0.1`.

**Step 2** — Host A ARPs for the router. Host A needs the router's MAC address. It checks its ARP cache; if there's no entry, it broadcasts an ARP request for `192.168.0.1`. The router replies with `RR:00:RR:00:RR:00`. This is the direct link between DHCP, ARP, and routing — Host A only knows the router's IP because DHCP told it.

**Step 3** — Host A builds the frame. The IP packet has `src: 192.168.0.10 → dst: 10.0.0.20` (the true end-to-end addresses). The Ethernet frame wraps it with `src MAC: AA:AA → dst MAC: RR:00` — local delivery instructions for this single hop.

**Step 4** — The switch forwards without thinking. The switch operates at Layer 2 only. It reads the destination MAC `RR:00`, looks it up in its CAM table, and forwards the frame out the correct port. It never touches the IP header. It never touches the MACs either — it just moves the frame as-is.

**Step 5** — The router receives and strips the frame. The router's `eth0` interface accepts the frame (because the dst MAC matches its own). It strips the Ethernet header entirely — that header has done its job. Now the router looks at the IP packet inside and performs a routing table lookup for `10.0.0.20`.

**Step 6** — The router builds a brand new frame. The router ARPs on the `10.0.0.0/24` network for Host B's MAC if it's not already cached. It then wraps the original IP packet in a new Ethernet frame: `src MAC: RR:11 → dst MAC: BB:BB`. The IP packet inside is untouched — same source and destination IPs.

**Step 7** — Host B receives it. Host B accepts the frame, strips the Ethernet header, and sees an IP packet addressed to itself. Done.

>The security implication worth noting here: since MAC addresses are rewritten at every router hop, you cannot track a device's MAC address across routed network boundaries. MAC-based controls like port security and 802.1X are strictly local to the Layer 2 segment. An attacker on `10.0.0.0/24` only ever sees the router's MAC as the source — they can never see Host A's real MAC. This is why Layer 3 identity (IP + higher-layer auth) matters for cross-network security, while MAC-based controls are purely a local-segment concern.

## 📌 Points to Remember

- MAC addresses operate at Layer 2 and are local to a single network segment, so they are replaced at every router hop along the path.
- IP addresses operate at Layer 3 and remain unchanged from the original source to the final destination across any number of hops.
- When sending to a remote network, the frame is addressed to the default gateway's MAC address, not to the destination host's MAC address.
- The router is the only device in this lab that modifies frame addressing because it is the only device that operates at Layer 3 and connects two separate IP networks.
- An access point repackages frames between wired and wireless formats but does not alter MAC or IP addresses, confirming it operates at Layer 1.