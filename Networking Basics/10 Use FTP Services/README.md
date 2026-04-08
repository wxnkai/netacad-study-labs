# Use FTP Services

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates how to use the File Transfer Protocol (FTP) to upload, download, rename, and delete files on a remote server. The lab covers connecting to an FTP server from the command line, authenticating with credentials, and executing the core FTP client commands.

## 🎯 Objectives

- Upload a file to an FTP server
- Download a file from an FTP server

## Addressing Table

| Device             | Interface | IP Address      | Subnet Mask     |
|--------------------|-----------|-----------------|-----------------|
| FTP Server (ftp.pka) | NIC     | 209.165.200.226 | 255.255.255.224 |

## 📋 Lab Steps

### Part 1 – Upload a File to an FTP Server

#### Step 1: Locate the file on PC-A

1. Click **PC-A**, open the **Desktop** tab, and click **Command Prompt**.
2. Run the following command to list available commands:

   ```
   ?
   ```

3. Run `dir` to view the files in the local directory:

   ```
   dir
   ```

   Confirm that `sampleFile.txt` is present in the `C:` directory.

#### Step 2: Connect to the FTP server

1. Connect to the FTP server using its IP address:

   ```
   ftp 209.165.200.226
   ```

2. When prompted, enter the following credentials:

   | Field    | Value     |
   |----------|-----------|
   | Username | `student` |
   | Password | `class`   |

   A successful login returns `230- Logged in` and enables passive mode.

#### Step 3: Upload a file to the FTP server

1. At the `ftp>` prompt, enter `?` to list available FTP commands:

   ```
   ?
   ```

2. List the current contents of the FTP server directory:

   ```
   dir
   ```

3. Upload `sampleFile.txt` from PC-A to the server:

   ```
   put sampleFile.txt
   ```

4. Run `dir` again to confirm the file now appears on the server:

   ```
   dir
   ```

---

### Part 2 – Download a File from an FTP Server

#### Step 1: Rename the file on the FTP server

1. At the `ftp>` prompt, rename the uploaded file:

   ```
   rename sampleFile.txt sampleFile_FTP.txt
   ```

2. Verify the rename was successful:

   ```
   dir
   ```

   Confirm `sampleFile_FTP.txt` appears in the directory listing and `sampleFile.txt` no longer does.

#### Step 2: Download the file from the FTP server

1. Download the renamed file to PC-A:

   ```
   get sampleFile_FTP.txt
   ```

2. Exit the FTP session:

   ```
   quit
   ```

3. Back at the regular command prompt, run `dir` to confirm `sampleFile_FTP.txt` now exists in the local `C:` directory:

   ```
   dir
   ```

#### Step 3: Delete the file from the FTP server

1. Reconnect to the FTP server and log in again using the same credentials:

   ```
   ftp 209.165.200.226
   ```

2. Delete the file from the server:

   ```
   delete sampleFile_FTP.txt
   ```

3. Exit the FTP session:

   ```
   quit
   ```

---

### Reflection – Key Observations from the Lab

- FTP uses two separate ports: port 21 for the command channel (authentication and control) and port 20 for the data channel (actual file transfers). This separation means a session can be established and controlled independently of any data movement.
- The `put` command transfers a file from the local client to the remote server. The `get` command transfers a file from the remote server to the local client. These two directions are distinct operations, each confirmed with a `dir` before and after.
- FTP credentials are transmitted in plaintext. In production environments, SFTP or FTPS is used instead to encrypt both the control and data channels.

## 🔑 Key Learnings

### FTP Uses Separate Ports for Control and Data

Port 21 handles the command channel, which is where login, directory listing, rename, and delete commands are sent. Port 20 handles the actual transfer of file data. This dual-port design is a defining characteristic of FTP and distinguishes it from single-port protocols. Passive mode, which was enabled automatically on login in this lab, changes how the data connection is established so it works through firewalls and NAT.

### Core FTP Client Commands Cover the Full File Lifecycle

The FTP client exposes a small, focused command set accessible by typing `?` at the `ftp>` prompt. The commands used in this lab cover the full lifecycle of a file on the server: `dir` to inspect, `put` to upload, `get` to download, `rename` to modify, `delete` to remove, and `quit` to close the session cleanly. Knowing these six commands is sufficient for routine FTP file management.

### Verifying Each Operation with dir Builds Good Habits

Running `dir` before and after each operation confirms that the expected change actually occurred on the server. This practice of verification is directly transferable to real-world file management workflows where silent failures can cause data loss or confusion about the state of remote storage.

### FTP Authentication Is Cleartext by Design

The `student` / `class` credentials entered in this lab are sent over the network without encryption. This is standard FTP behavior. On any network where confidentiality matters, this makes FTP unsuitable for production use. Understanding this limitation explains why SFTP (SSH File Transfer Protocol) and FTPS (FTP over TLS) exist as secure replacements.

## 📌 Points to Remember

- FTP uses port 21 for commands and port 20 for data transfers. Both ports must be reachable for a complete session.
- `put` uploads from the local machine to the server. `get` downloads from the server to the local machine.
- Always run `dir` after an upload, download, rename, or delete to confirm the operation completed as expected.
- `quit` terminates the FTP session cleanly. Closing the terminal without quitting may leave the connection open on the server side until it times out.
- FTP credentials and data travel in plaintext. Use SFTP or FTPS in any environment where security is a concern.