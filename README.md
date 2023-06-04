# VPN over SSH – sshuttle
sshuttle is a powerful program that enables you to establish a VPN connection from your local machine to a remote server accessible via SSH. This connection creates a tunnel through which all your traffic, including DNS requests, can be routed from your client machine to the remote server. Essentially, sshuttle acts as a proxy server on your client machine, forwarding all traffic through an SSH tunnel. Being open-source, sshuttle offers numerous advantages over conventional VPN solutions, making it a versatile and attractive option for secure networking.
[For more info visit this website](https://terminalbytes.com/sshuttle-vpn-over-ssh-vpn-alternative)
```plaintext
https://terminalbytes.com/sshuttle-vpn-over-ssh-vpn-alternative/
```
## Install an SSHD server on your server-side
*(debian family distributions with apt packet manager)*
```plaintext
sudo apt update
sudo apt install openssh-server
```
*Check if SSH server is ready to accept connections*

To check if the SSH server is running and ready to accept connections, run the following command:

```plaintext
sudo systemctl status ssh
or
sudo systemctl stop ssh
sudo systemctl start ssh
sudo systemctl status ssh
```
Location of SSHD config
```plaintext
/etc/ssh/sshd_config
```
* change Loglevel of SSHD config(Optional)
```plaintext
sudo vim /etc/ssh/sshd_config
```
* change from LogLevel(Optional)
```plaintext
LogLevel INFO to LogLevel VERBOSE
```
Location of SSHD Logfiles
```plaintext
/var/log/auth.log
```

## SSHuttle Server Configuration

This guide provides instructions for setting up SSHuttle on the server side.
* Installation
```plaintext
sudo apt update
sudo apt install sshuttle
```
## Usage
1. Run SSHuttle to route traffic through SSH:
```plaintext
sshuttle -r [username]@[SSH_server] [subnets_to_route]
```
Example for `subnets_to_route= 0/0`:
```plaintext
sshuttle -r username@ssh_server 0/0
```
2. Run SSH with Dynamic Port Forwarding:
```plaintext
ssh -D 0.0.0.0:1080 [username]@[SSH_server]
```
If you have another SSH port, such as `1980`:
```plaintext
ssh -D 0.0.0.0:1080 -p 1980 [username]@[SSH_server]
```
## Example
- Run SSHuttle with DNS and specific subnet:
```plaintext
sudo sshuttle --dns -r [username]@[SSH_server]:1983 192.168.0.0/16
```
Or run
- SSHuttle with DNS and all subnets:
```plaintext
sudo sshuttle --dns -r [username]@[SSH_server]:1983 0/0
```
If you want to use an extended IP:
```plaintext
sudo sshuttle --dns -r [username]@[SSH_server]:1983 0/0 -X [your-ip]
```
Please note that the commands and options mentioned above are specific to the installation and usage of SSHuttle on the server side.
## Server Side Configuration for udpgw
1. Download and install `badvpn-udpgw`:
```plaintext
sudo wget -O /usr/bin/badvpn-udpgw "https://raw.githubusercontent.com/daybreakersx/premscript/master/badvpn-udpgw64"
sudo touch /etc/rc.local
sudo vim /etc/rc.local
```
2. Add the following lines to  `/etc/rc.local`:
```plaintext
#!/bin/sh -e
screen -AmdS badvpn badvpn-udpgw --listen-addr 127.0.0.1:7300
exit 0
```
3. Set permissions and start   `badvpn-udpgw`:
```plaintext
sudo chmod +x /etc/rc.local
sudo systemctl status rc-local.service
sudo chmod +x /usr/bin/badvpn-udpgw
```
If `screen` is not available, install it using:
```plaintext
sudo apt install screen
```
4. Start `badvpn-udpgw` using `screen`:
```plaintext
sudo screen -AmdS badvpn badvpn-udpgw --listen-addr 127.0.0.1:7300
```
5. Reboot your system to apply the changes.
```plaintext
sudo reboot
```
6. Verify that `badvpn-udpgw` is running:
```plaintext
sudo lsof -i -P -n | grep LISTEN | grep 7300
```
Please note that the commands and steps mentioned above are specific to the configuration of `badvpn-udpgw` on the server side.
# Client Side
* NapsternetV Configuration for Android Smartphone
1. **Install NapsternetV:**
   - Visit the app store on your smartphone.
   - Search for `NapsternetV` and install the app.
   - Launch the NapsternetV app.
2. **Create a Config File:**
   - Tap on the `Config` tab in the bottom navigation menu.
   - Tap on the `+` button to create a new config file.
3. **Add SSH Configuration:**
   - Select `SSH-Direct` as the protocol for your config.
   - Enter a name for your config, such as `MySSH-VPN`.
   - Set the SSH host to the IP address or DDNS name of your SSH server.
   - Specify the SSH port. By default, it is `22`, but if you have configured a different port on your server (e.g., `1980`), enter that port.
   - Set the Udpgw port. The default is `7300`, but if you have already configured it on your SSH server, keep it as default.
   - Enable the `Use UDPGW Transparent DNS` option.
4. **SSH Server Credentials:**
   - Enter your SSH server username in the designated field.
   - Provide the username's password for authentication.

5. **Select your Config:**
   - Tap on the config you just created from the list of available configurations.

6. **Start the VPN Connection:**
   - Go back to the home screen of NapsternetV.
   - Tap on the play button (▶️) to start the VPN connection.

By following these steps, you should be able to configure NapsternetV on your smartphone and establish a VPN-like connection using SSH. Please note that the specific steps may vary slightly depending on the version of NapsternetV and your smartphone's operating system.
## command to check public ip
```plaintext
curl ipconfig.io
```
