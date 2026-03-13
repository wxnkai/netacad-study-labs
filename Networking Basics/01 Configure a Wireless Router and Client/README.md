# Configure a Wireless Router and Client

A hands-on networking lab completed in Cisco Packet Tracer that walks through setting up a home network from scratch: cabling devices, configuring a wireless router via its GUI, securing a Wi-Fi network, and verifying internet connectivity across wired and wireless clients.

## 🎯 Objectives

- Connect coaxial and Ethernet cables to the appropriate devices
- Configure a home wireless router (DHCP, admin credentials, SSID, wireless security)
- Assign IP addressing to clients and verify end-to-end internet connectivity

## 📋 Lab Steps

### Part 1 – Connect the Devices

#### Step 1: Connect Coaxial Cables

The coaxial wall outlet feeds into a **Cable Splitter**, which separates internet and video signals.

1. In **Network Components**, click **Connections** (lightning bolt icon).
2. Select the **Coaxial** cable (blue zigzag icon).
3. Connect **Cable Splitter → Coaxial1** to **Cable Modem → Port 0**.
4. Connect **Cable Splitter → Coaxial2** to **TV → Port 0**.
5. Click the **TV**, set **Status** to **ON** and a TV program image should appear, confirming correct coaxial wiring.

#### Step 2: Connect Ethernet Cables

Use **Copper Straight-Through** cables for all wired Ethernet connections.

1. Select **Connections → Copper Straight-Through** (solid black line).
2. Connect **Cable Modem → Port 1** to **Home Wireless Router → Internet** port.
3. Connect **Office PC → FastEthernet0** to **Home Wireless Router → GigabitEthernet 1**.
4. Connect **Bedroom PC → FastEthernet0** to **Home Wireless Router → GigabitEthernet 2**.

---

### Part 2 – Configure the Wireless Router

All router configuration is done through the router's web-based GUI, accessed via the **Office PC's** browser.

#### Step 1: Access the Router GUI

1. Click **Office PC → Desktop → IP Configuration**.
2. Set to **DHCP** and the PC will automatically receive a `192.x.x.x` address from the router.
   > If the IP doesn't update, click **Fast Forward Time** to speed up the DHCP process.
3. Note the **Default Gateway** IP address, which is the router's IP.
4. Close **IP Configuration**, open **Web Browser**, enter the default gateway IP, and click **Go**.
5. Log in with:
   - **Username:** `admin`
   - **Password:** `admin`

   > ⚠️ Default credentials should always be changed immediately on real-world devices.

#### Step 2: Configure Basic Router Settings

1. Under the **Setup** tab → **Network Setup**, locate **Maximum Number of Users** and set it to `10`.
2. Click **Save Settings**.
   > **Note:** If the connection drops, reload the browser and re-authenticate as needed.
3. Click the **Administration** tab.
4. Change the admin password:
   - **New Password:** `MyPassword1!`
   - **Confirm Password:** `MyPassword1!`
5. Click **Save Settings**, then re-login with the new credentials.

#### Step 3: Configure the Wireless LAN (2.4 GHz)

1. Navigate to the **Wireless** tab.
2. For the **2.4 GHz** band, click **Enable**.
3. Change the **Network Name (SSID)** from `Default` to `MyHome`.
4. Click **Save Settings**.
5. Click **Wireless Security** (under the Wireless tab).
6. For the **2.4 GHz** network, set the security mode to **WPA2 Personal**.
7. Enter the passphrase: `MyPassPhrase1!`
   > **Note:** Passphrase is case-sensitive.
8. Click **Save Settings** and close the browser.

---

### Part 3 – Configure IP Addressing and Test Connectivity

#### Step 1: Connect the Laptop via Wi-Fi

1. Click **Laptop → Desktop → PC Wireless**.
2. Click the **Connect** tab and the `MyHome` network should appear.
3. Select **MyHome** and click **Connect**.
4. Enter the passphrase `MyPassPhrase1!` in the **Pre-shared Key** field and click **Connect**.
5. Click **Link Information**, then confirm the message: _You have successfully connected to the access point._
6. Click **More Information**, then verify the IP address begins with `192`.
7. Close **PC Wireless**, open **Web Browser**, and navigate to `skillsforall.srv`.
   > **Note:** Click **Fast Forward Time** as needed until the page loads, confirming internet connectivity.

#### Step 2: Test Connectivity – Office PC

1. Click **Office PC → Desktop → Web Browser**.
2. Navigate to `skillsforall.srv` and click **Go**.
3. Confirm the webpage loads, verifying wired internet connectivity.

#### Step 3: Configure the Bedroom PC

1. Click **Bedroom PC → Desktop → IP Configuration**.
2. Set to **DHCP** and confirm an IP starting with `192` is assigned.
3. Close **IP Configuration**, open **Web Browser**, and navigate to `skillsforall.srv`.
4. Confirm the page loads, verifying the Bedroom PC has internet access.

## 🔑 Key Learnings

### Physical Cabling
The correct cable type must be used for each connection. Coaxial cable carries the signal from the provider into the home, a splitter separates that signal for the modem and TV, and copper straight-through Ethernet cables link the modem, router, and wired PCs. Getting the physical layer right is a prerequisite for everything that follows.

### Router
Before any wireless or wired client can communicate with the internet, the router must first receive an internet connection from the modem, then be configured with DHCP settings, secure admin credentials, and wireless parameters. Each setting change must be saved before moving to the next step, otherwise changes are lost when navigating away from the page.

### Dynamic Host Configuration Protocol
DHCP removes the need to manually assign IP addresses to each device. When a client is set to DHCP mode, the router automatically provides it with an IP address, subnet mask, and default gateway. All three clients in this lab (Office PC, Bedroom PC, and Laptop) received their network configuration automatically this way. The IP address beginning with `192` confirmed that the address came from the router and that the device was correctly placed on the home network.

### Default Gateway
The default gateway is what connects a local device to the outside world. Every client on the home network forwards traffic destined for external addresses to the router's IP, which serves as the default gateway. This is also the address used to access the router's GUI from a browser, which makes understanding this address essential for both connectivity and administration.

### Wi-Fi Protected Access
WPA2 Personal secures the wireless network using a shared passphrase. Without wireless security enabled, any nearby device could join the network. Setting the security mode to WPA2 Personal and configuring a strong passphrase ensures that only users who know the key can connect. The SSID identifies the network publicly, but the passphrase controls access to it.

## 📌 Points to Remember

- Always change the default admin password on a router immediately, because default credentials are publicly known and leave the device open to unauthorized access.
- DHCP is a network management protocol that automatically assigns IP addresses, subnet masks, default gateways, and DNS server details to devices
- WPA2 is a widely used, secure, and mandatory wireless encryption standard launched in 2004 that replaced WPA and WEP, but has been succeeded by WPA3.