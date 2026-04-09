# Use the ipconfig Command

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates how to use `ipconfig /all` to audit IP configurations across multiple PCs, identify a misconfigured host, and correct the static addressing error through the IP Configuration interface.

## 🎯 Objective

- Use the `ipconfig` command to identify incorrect IP configuration on a PC in a small office network.

## Background

A small office has four PCs, all statically addressed on the `192.168.1.0/24` network. One PC cannot reach the internet or the `www.cisco.pka` web server. The task is to inspect every PC using `ipconfig /all`, determine which one has an incorrect configuration, and fix it.

## 📋 Lab Steps

### Part 1 – Verify Configurations

1. Click the first PC, open the **Desktop** tab, and click **Command Prompt**.
2. Run the extended ipconfig command:

   ```
   ipconfig /all
   ```

3. Record the following values from the output for that PC:

   | Field           | Value |
   |-----------------|-------|
   | IP Address      |       |
   | Subnet Mask     |       |
   | Default Gateway |       |

4. Close the Command Prompt and repeat steps 1 through 3 on every remaining PC.

   All four PCs should show addresses on the `192.168.1.0` network with a subnet mask of `255.255.255.0`. Compare the recorded values across all PCs. The misconfigured PC will have at least one value that is inconsistent with the others, such as a wrong IP address, an incorrect subnet mask, or a missing or wrong default gateway.

---

### Part 2 – Correct Any Misconfigurations

1. Click the PC identified as incorrectly configured.
2. Open the **Desktop** tab and click **IP Configuration**.
3. Update the incorrect field or fields to match the correct `192.168.1.0/24` network settings used by the other PCs.
4. Close the IP Configuration window.
5. Return to the **Command Prompt** on the corrected PC and run `ipconfig /all` again to confirm the new values are applied.
6. Verify the fix by opening **Web Browser** from the **Desktop** tab and navigating to `www.cisco.pka` to confirm internet access is restored.

---

### Reflection – Key Observations from the Lab

- Running `ipconfig /all` on every host and recording each result side by side is the fastest way to spot a configuration that does not match the expected pattern. One address, mask, or gateway that stands out immediately identifies the problem device.
- Static addressing errors are silent at the host level. The PC does not warn the user that its configuration is wrong. The only symptom is failed connectivity, which is why a systematic audit with `ipconfig /all` is the correct first step.
- Fixing the IP configuration through the desktop GUI takes effect immediately without requiring a reboot or a DHCP renewal, because the address is static and the change is applied directly to the interface.

## 🔑 Key Learnings

### ipconfig /all Exposes the Full Layer 3 Configuration of a Host

The basic `ipconfig` command shows the IP address, subnet mask, and default gateway. The `/all` flag adds the physical MAC address, DHCP status, DNS server addresses, and whether DHCP is enabled or the address is statically assigned. In a network where all hosts should share the same subnet, mask, and gateway, comparing `ipconfig /all` output side by side across machines is a reliable and fast method for spotting a misconfiguration.

### Static Addressing Requires Careful Manual Entry

DHCP automates address assignment and reduces the chance of misconfiguration. Static addressing puts the responsibility for correct entry entirely on the person configuring the host. A single digit transposed in the IP address, subnet mask, or default gateway is enough to break connectivity silently. This lab reinforces why verification with `ipconfig /all` should follow every manual address change.

### The Default Gateway Is the Exit Point for All Remote Traffic

If a host has the correct IP address and subnet mask but a wrong or missing default gateway, it can communicate with other devices on the same local subnet but cannot reach anything outside it, including web servers and remote networks. The default gateway is the router interface on the local segment, and every statically configured host must have it set correctly to reach the internet.

### Troubleshooting Connectivity Starts with the Local Host

Before checking cables, switches, or routers, confirming that the local host is correctly configured is the most efficient first step. If `ipconfig /all` shows a valid address, mask, and gateway that match the expected network, the problem lies elsewhere in the path. If any of those values are wrong, the fault is local and can be fixed immediately without touching any other device.

## 📌 Points to Remember

- `ipconfig /all` shows the complete IP configuration including MAC address, DHCP status, DNS servers, and gateway, while `ipconfig` alone shows only the address, mask, and gateway.
- On a statically addressed network, record the expected IP address, subnet mask, and default gateway before auditing hosts so deviations are immediately obvious.
- A wrong subnet mask causes the host to misidentify which destinations are local and which require the gateway, breaking connectivity in ways that are not always obvious from the symptom alone.
- A missing or incorrect default gateway breaks all communication outside the local subnet, including internet access, while leaving local host-to-host communication intact.
- IP configuration changes applied through the desktop IP Configuration panel take effect immediately on statically addressed hosts.