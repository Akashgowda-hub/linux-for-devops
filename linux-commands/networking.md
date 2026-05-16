# Networking Commands

## ping

Check network connectivity to remote hosts.

### Basic Example
```bash
ping -c 4 google.com
```

**Output:**
```
PING google.com (142.251.32.46) 56(84) bytes of data.
64 bytes from 142.251.32.46: icmp_seq=1 ttl=57 time=15.4 ms
64 bytes from 142.251.32.46: icmp_seq=2 ttl=57 time=14.2 ms
64 bytes from 142.251.32.46: icmp_seq=3 ttl=57 time=15.1 ms
64 bytes from 142.251.32.46: icmp_seq=4 ttl=57 time=14.8 ms
--- google.com statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3028ms
rtt min/avg/max/stddev = 14.2/14.9/15.4/0.5 ms
```

### Common Use Cases

**Ping with specific packet count:**
```bash
ping -c 5 192.168.1.1
```

**Ping with timeout:**
```bash
ping -w 10 google.com  # Stop after 10 milliseconds
```

**Ping and measure response time:**
```bash
ping -c 1 8.8.8.8 | grep time=
```

### Best Practices
- Use `-c` flag to limit packet count (prevents infinite pinging)
- Check connectivity before troubleshooting network issues
- Use ping to verify DNS is working (if domain resolves)

---

## curl

Make HTTP/HTTPS requests and retrieve web content.

### Basic Example - GET Request
```bash
curl https://api.github.com/users/github
```

**Output (JSON response):**
```json
{
  "login": "github",
  "id": 1,
  "node_id": "MDEyOk9yZ2FuaXphdGlvbjE=",
  "avatar_url": "https://avatars.githubusercontent.com/u/1?v=4",
  ...
}
```

### Common Use Cases

**Save output to file:**
```bash
curl https://example.com/file.zip -o downloaded.zip
```

**Follow redirects:**
```bash
curl -L https://example.com/redirect
```

**POST request with data:**
```bash
curl -X POST https://api.example.com/data \
  -H "Content-Type: application/json" \
  -d '{"name":"devops","role":"engineer"}'
```

**Include headers in response:**
```bash
curl -i https://google.com
```

**Show response headers only:**
```bash
curl -I https://google.com
```

**Add custom headers:**
```bash
curl -H "Authorization: Bearer token123" https://api.example.com
```

**Check HTTP status code:**
```bash
curl -s -o /dev/null -w "%{http_code}" https://google.com
```

### Best Practices
- Use `-s` for silent mode (no progress bar)
- Use `-i` to see headers when debugging API responses
- Use `-H` to add authentication headers for secure endpoints

---

## wget

Download files from the web using HTTP/HTTPS/FTP.

### Basic Example
```bash
wget https://example.com/file.zip
```

### Common Use Cases

**Save with different filename:**
```bash
wget https://example.com/archive.zip -O myfile.zip
```

**Download multiple files:**
```bash
wget https://example.com/file1.zip https://example.com/file2.zip
```

**Download directory recursively:**
```bash
wget -r https://example.com/docs/
```

**Continue incomplete download:**
```bash
wget -c https://example.com/largefile.iso
```

**Set download limit:**
```bash
wget --limit-rate=100k https://example.com/largefile.zip
```

**Retry failed downloads:**
```bash
wget --tries=5 https://example.com/file.zip
```

**Background download:**
```bash
wget -b https://example.com/largefile.zip
tail -f wget-log
```

### Best Practices
- Use `-c` flag for resuming interrupted downloads
- Use `--limit-rate` to avoid overwhelming network
- Use `-b` for background downloads of large files

---

## ssh

Securely connect to remote servers.

### Basic Connection
```bash
ssh user@192.168.1.100
```

### Common Use Cases

**Connect with specific port:**
```bash
ssh -p 2222 user@example.com
```

**Connect with specific key file:**
```bash
ssh -i ~/.ssh/id_rsa user@example.com
```

**Execute remote command:**
```bash
ssh user@example.com "ls -la /home"
```

**Run interactive shell commands:**
```bash
ssh user@example.com << 'EOF'
cd /var/log
tail -f syslog
EOF
```

**Port forwarding (local to remote):**
```bash
ssh -L 3306:localhost:3306 user@remotehost
# Now access remote MySQL at localhost:3306
```

**Port forwarding (remote to local):**
```bash
ssh -R 8080:localhost:80 user@remotehost
# Tunnel traffic from remote port 8080 to local port 80
```

**Keep connection alive:**
```bash
ssh -o ServerAliveInterval=60 user@example.com
```

### Best Practices
- Always use key-based authentication instead of passwords
- Use non-standard ports for better security
- Enable `ServerAliveInterval` for long-lived connections

---

## scp

Securely copy files between local and remote systems.

### Copy File to Remote Server
```bash
scp myfile.txt user@192.168.1.100:/home/user/
```

### Common Use Cases

**Copy file from remote to local:**
```bash
scp user@example.com:/var/log/app.log ./app.log
```

**Copy entire directory:**
```bash
scp -r user@example.com:/home/user/project ./project
```

**Copy with specific SSH key:**
```bash
scp -i ~/.ssh/id_rsa file.txt user@example.com:/tmp/
```

**Copy with custom SSH port:**
```bash
scp -P 2222 myfile.txt user@example.com:/home/
```

**Copy between two remote servers:**
```bash
scp -3 user1@server1.com:/tmp/file.txt user2@server2.com:/tmp/
```

**Preserve file permissions:**
```bash
scp -p user@example.com:/home/script.sh ./script.sh
```

### Best Practices
- Use `-r` for copying directories recursively
- Use `-p` to preserve file permissions and timestamps
- Use `-P` (capital P) to specify custom SSH port

---

## ip addr (ip a)

Display and manage IP addresses and network interfaces.

### Show All Interfaces
```bash
ip addr show
# or short form:
ip a
```

**Output:**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 08:00:27:a1:b2:c3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.100/24 brd 192.168.1.255 scope global eth0
    inet6 fe80::a00:27ff:fea1:b2c3/64 scope link
       valid_lft forever preferred_lft forever
```

### Common Use Cases

**Show specific interface:**
```bash
ip addr show eth0
```

**Add IP address to interface:**
```bash
sudo ip addr add 192.168.1.50/24 dev eth0
```

**Remove IP address from interface:**
```bash
sudo ip addr del 192.168.1.50/24 dev eth0
```

**Show only IPv4 addresses:**
```bash
ip -4 addr
```

**Show only IPv6 addresses:**
```bash
ip -6 addr
```

### Best Practices
- Use to quickly verify IP configuration
- Use before troubleshooting network connectivity issues

---

## ss -tulpn

Display socket statistics and listening ports (modern replacement for netstat).

### Show All Listening Ports
```bash
ss -tulpn
```

**Output:**
```
Netid   State   Recv-Q Send-Q Local Address:Port    Peer Address:Port Process
tcp     LISTEN  0      128    0.0.0.0:22           0.0.0.0:*        users:(("sshd",pid=1234,fd=3))
tcp     LISTEN  0      511    0.0.0.0:80           0.0.0.0:*        users:(("nginx",pid=5678,fd=6))
tcp     LISTEN  0      511    0.0.0.0:443          0.0.0.0:*        users:(("nginx",pid=5678,fd=7))
udp     UNCONN  0      0      0.0.0.0:53           0.0.0.0:*        users:(("systemd-resolve",pid=890,fd=11))
```

### Common Use Cases

**Show only TCP listening ports:**
```bash
ss -tlnp
```

**Show only UDP listening ports:**
```bash
ss -ulnp
```

**Show established connections:**
```bash
ss -tnp | grep ESTAB
```

**Check if specific port is listening:**
```bash
ss -tulpn | grep :8080
```

**Show all connections with process info:**
```bash
ss -tupn
```

**Count connections per port:**
```bash
ss -tn | grep ESTAB | awk '{print $4}' | cut -d: -f2 | sort | uniq -c
```

### Flag Meanings
- `-t`: TCP sockets
- `-u`: UDP sockets
- `-l`: Listening sockets
- `-p`: Show process name and PID
- `-n`: Show numeric ports

### Best Practices
- Use `ss` instead of deprecated `netstat`
- Use `-p` to identify which process uses which port
- Helpful for troubleshooting port conflicts

---

## nslookup

Query DNS to resolve domain names and IP addresses.

### Basic Domain Lookup
```bash
nslookup google.com
```

**Output:**
```
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
Name:   google.com
Address: 142.251.32.46
Name:   google.com
Address: 2607:f8b0:4004:80f::200e
```

### Common Use Cases

**Reverse DNS lookup:**
```bash
nslookup 142.251.32.46
```

**Query specific DNS server:**
```bash
nslookup google.com 8.8.8.8
```

**Query A record:**
```bash
nslookup -type=A google.com
```

**Query MX records (mail servers):**
```bash
nslookup -type=MX example.com
```

**Query NS records (nameservers):**
```bash
nslookup -type=NS example.com
```

**Query CNAME records:**
```bash
nslookup -type=CNAME www.example.com
```

### Best Practices
- Use for quick DNS verification
- Good for checking if DNS is resolving correctly

---

## dig

Advanced DNS query tool with detailed output.

### Basic Query
```bash
dig google.com
```

**Output (abbreviated):**
```
; <<>> DiG 9.16.1-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12345
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             300     IN      A       142.251.32.46

;; Query time: 15 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
;; WHEN: Mon Jan 10 12:00:00 UTC 2024
;; MSG SIZE  rcvd: 55
```

### Common Use Cases

**Query specific record type:**
```bash
dig google.com MX
```

**Short output (answer only):**
```bash
dig google.com +short
```

**Query all records:**
```bash
dig google.com ANY
```

**Query specific DNS server:**
```bash
dig @8.8.8.8 google.com
```

**Trace DNS hierarchy:**
```bash
dig google.com +trace
```

**Reverse DNS lookup:**
```bash
dig -x 142.251.32.46
```

**Check SOA record:**
```bash
dig google.com SOA
```

**Multiple queries:**
```bash
dig google.com gmail.com github.com
```

### Best Practices
- Use `+short` for clean output when you just need the answer
- Use `+trace` to diagnose DNS resolution issues
- More powerful than `nslookup` for advanced DNS debugging