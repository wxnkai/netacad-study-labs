# Use Telnet and SSH

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates how to establish a remote connection to a router using Telnet and SSH. The lab highlights why Telnet is rejected on a securely configured device and confirms that SSH is the required protocol for encrypted remote management access.

## 🎯 Objectives

- Verify connectivity between a PC and a remote router
- Attempt remote access via Telnet and observe the result
- Establish a successful encrypted remote session using SSH

## Addressing Table

| Device | Interface | IP Address  | Subnet Mask   |
|--------|-----------|-------------|---------------|
| HQ     | G0/0/1    | 64.100.1.1  | 255.255.255.0 |
| PC0    | NIC       | DHCP        | N/A           |
| PC1    | NIC       | DHCP        | N/A           |

## 📋 Lab Steps

### Part 1 – Verify Connectivity

#### Step 1: Verify the IP address on a PC

1. Click a PC, open the **Desktop** tab, and click **Command Prompt**.
2. Run the following command to confirm the PC has received an address from DHCP:

   ```
   ipconfig
   ```

   Confirm that a valid IP address is assigned to the NIC.

#### Step 2: Verify connectivity to HQ

1. Ping the router using its IP address from the Addressing Table:

   ```
   ping 64.100.1.1
   ```

   Successful replies confirm Layer 3 connectivity between the PC and the HQ router.

---

### Part 2 – Access a Remote Device

#### Step 1: Attempt Telnet to HQ

1. At the command prompt, attempt a Telnet connection to the router:

   ```
   telnet 64.100.1.1
   ```

   The connection opens briefly and is then immediately closed by the remote host. This is the expected result. The router is configured to reject Telnet because it transmits all data, including credentials, in plaintext. A securely configured router disables Telnet and requires SSH instead.

#### Step 2: Connect to HQ using SSH

1. At the command prompt, initiate an SSH session using the `-l` flag to specify the username:

   ```
   ssh -l admin 64.100.1.1
   ```

2. When prompted, enter the following password:

   ```
   class
   ```

3. A successful login presents the router privileged EXEC prompt:

   ```
   HQ#
   ```

   The `HQ#` prompt confirms the session is established and the user is in privileged EXEC mode on the remote router.

---

### Reflection – Key Observations from the Lab

- Telnet and SSH both provide remote command-line access to network devices, but only SSH encrypts the session. The router in this lab is configured to close Telnet connections immediately, which is standard practice on any production device.
- The `-l` flag in the SSH command specifies the login username separately from the hostname. The syntax `ssh -l admin 64.100.1.1` is equivalent to `ssh admin@64.100.1.1` in most implementations.
- The `HQ#` prompt after login indicates privileged EXEC mode, meaning the authenticated user has full access to view and modify router configuration. This is why strong credentials and SSH encryption matter: anyone who gains this access can reconfigure the device entirely.

## 🔑 Key Learnings

### Telnet Is Disabled by Design on Secure Devices

Telnet transmits everything in cleartext, including the username and password entered during login. Any device on the network path between the client and the router can capture and read these credentials with basic packet capture tools. A properly hardened router closes Telnet connections and only accepts SSH, which encrypts the entire session from authentication through to command output.

### SSH Provides Encrypted Remote Management

SSH uses public-key cryptography to establish an encrypted tunnel before any credentials are exchanged. The username and password entered at the `Password:` prompt are protected inside that tunnel, unlike Telnet where they travel in the clear. For any remote administration of network infrastructure, SSH is the minimum acceptable standard.

### DHCP Addressing Must Be Confirmed Before Testing Connectivity

The PCs in this lab receive their IP addressing from DHCP. Running `ipconfig` before attempting any ping or remote connection confirms that a valid address was actually assigned. Attempting to ping or SSH without first verifying the local address is a common source of confusion in connectivity troubleshooting: the problem may be the local host, not the remote device.

### The Ping Test Isolates Layer 3 Before Testing Layer 7

Verifying the ping to `64.100.1.1` before attempting Telnet or SSH confirms that IP routing is working and the router is reachable at the network layer. If the ping fails, there is no point attempting a remote management session. Working from the bottom up confirms each layer before testing the one above it.

## 📌 Points to Remember

- Run `ipconfig` first to confirm DHCP assigned a valid address before testing any connectivity.
- Ping the remote device before attempting a remote management session to confirm IP reachability.
- Telnet uses port 23 and sends all data, including passwords, in plaintext. It should not be used on any production network device.
- SSH uses port 22 and encrypts the full session. It is the standard protocol for remote CLI access to routers and switches.
- The `-l` flag in the SSH command specifies the username: `ssh -l <username> <ip-address>`.