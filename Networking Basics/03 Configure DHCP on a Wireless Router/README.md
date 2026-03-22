# Configure DHCP on a Wireless Router

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates how to configure a wireless router's DHCP server, modify its IP addressing range, and verify that multiple clients automatically receive valid network configurations.

## 🎯 Objectives

- Connect three PCs to a wireless router using Ethernet cables
- Observe and modify the router's default DHCP settings
- Configure all clients to obtain IP addresses automatically via DHCP
- Verify end-to-end connectivity between all devices on the network

## 📋 Lab Steps

### Part 1 – Set Up the Network Topology

1. Add three generic PCs to the workspace.
2. Connect each PC to a separate Ethernet port on the wireless router using **Copper Straight-Through** cables.
3. Wait for the link lights to turn from amber to green before proceeding.

---

### Part 2 – Observe the Default DHCP Settings

1. Click **PC0**, open the **Desktop** tab, and select **IP Configuration**.
2. Click **DHCP** to request an address from the router. PC0 will receive an IP in the default `192.168.0.x` range.
3. Note and record the **Default Gateway** address (expected: `192.168.0.1`).
4. Close **IP Configuration**, then open the **Web Browser**.
5. Enter the default gateway IP in the URL bar and log in with:
   - **Username:** `admin`
   - **Password:** `admin`
6. Scroll through the **Basic Setup** page to review the default DHCP settings, including the starting address and the total number of addresses available to clients.

---

### Part 3 – Change the Router's Default IP Address

1. On the **Basic Setup** page, locate the **Router IP Settings** section.
2. Change the router IP address to `192.168.5.1`.
3. Scroll to the bottom of the page and click **Save Settings**.
   > The browser will display an error page after saving. This is expected because the router is now on a different subnet. Close the browser.
4. On PC0, go to **IP Configuration**, click **Static**, then click **DHCP** to force a renewal and receive a new address from the updated router.
5. Reopen the **Web Browser**, enter `192.168.5.1` in the URL bar, and log in again with `admin` / `admin`.

---

### Part 4 – Modify the DHCP Address Range

1. On the **Basic Setup** page, confirm the DHCP Start IP Address has automatically updated to the `192.168.5.x` network.
2. Change the **Starting IP Address** from `192.168.5.100` to `192.168.5.126`.
3. Change the **Maximum Number of Users** to `75`.
4. Scroll to the bottom and click **Save Settings**, then close the browser.
5. On PC0, go to **IP Configuration**, click **Static**, then click **DHCP** to renew the address under the new range.
6. Open the **Command Prompt** and run:

   ```
   ipconfig
   ```

   > PC0 should receive `192.168.5.126` as its IP address, confirming it is the first lease in the updated range.

---

### Part 5 – Enable DHCP on PC1 and PC2

**For PC1:**

1. Click **PC1**, open the **Desktop** tab, and select **IP Configuration**.
2. Click **DHCP** to request an address from the router.
3. Record the assigned IP address (expected: `192.168.5.127`).
4. Close the configuration window.

**For PC2:**

1. Repeat the same steps on **PC2** to enable DHCP.
2. PC2 will receive the next available address in the DHCP range.

---

### Part 6 – Verify Connectivity

1. Click **PC2**, open the **Desktop** tab, and select **Command Prompt**.
2. Run `ipconfig` to confirm PC2 received a valid IP in the `192.168.5.x` range.
3. Ping the wireless router:

   ```
   ping 192.168.5.1
   ```

4. Ping PC1 to verify host-to-host communication:

   ```
   ping 192.168.5.127
   ```

5. All pings should return successful replies, confirming full network connectivity.

## 🔑 Key Learnings

### DHCP Automates IP Address Management
Without DHCP, each device would need a manually assigned IP address, subnet mask, and default gateway. By enabling DHCP on the router and setting each PC to receive its configuration automatically, the router handles all address assignment from a defined pool. This is how virtually every home and enterprise network operates in practice.

### The Router IP Defines the Network DHCP Will Serve
When the router IP was changed from `192.168.0.1` to `192.168.5.1`, the DHCP pool automatically shifted to the `192.168.5.x` network. This illustrates a foundational principle: the DHCP server cannot assign addresses on a subnet it does not belong to. The router IP and the DHCP range must always share the same network.

### Changing the Router IP Disconnects Existing Clients Until They Renew
After saving the new router IP, the browser displayed an error and the existing DHCP lease became invalid. The fix was to toggle the PC between Static and DHCP to force a lease renewal. This is important to understand in real-world troubleshooting because a router IP change silently breaks all connected clients until they request new addresses.

### The Start Address and User Limit Define the Boundaries of the DHCP Pool
Setting the start address to `192.168.5.126` and the user count to `75` means the router will lease addresses from `192.168.5.126` through `192.168.5.200`. PC0 received `.126` as the first lease, PC1 received `.127` as the second, which directly demonstrates how sequential DHCP allocation works.

## 📌 Points to Remember

- The DHCP pool must exist on the same subnet as the router's IP address, so changing the router IP also changes the network range that DHCP will assign.
- After changing the router's IP, existing clients must renew their lease to receive a valid address on the new network.
- Clients receive DHCP leases sequentially starting from the configured start address, so the first PC to request an address gets the lowest available IP in the pool.