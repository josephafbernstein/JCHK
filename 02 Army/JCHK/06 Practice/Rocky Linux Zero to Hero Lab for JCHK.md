---
tags: [jchk, rocky-linux, linux, lab, learning]
created: 2026-09-23
status: ready-to-follow
jchk_reference: v0.6.1 training guide
---

# Rocky Linux Zero to Hero Lab for JCHK

An individual learning lab: start on your Zorin OS 18.1 laptop, build a Rocky Linux 9 virtual machine, learn to operate and troubleshoot it, then practice Kubernetes skills that transfer to JCRS-D.

**Your laptop:** Zorin OS 18.1 (Core/Pro), 27 GiB RAM, approximately 711 GB available disk space, as reported on 2026-09-23. Use **4 GiB RAM, 2 virtual CPUs, and a 30 GiB disk for Rocky**, then **4 GiB RAM, 2 virtual CPUs, and a 30 GiB disk for minikube**. Your reported memory and storage comfortably accommodate both; confirm the CPU and virtualization checks below before installation. Leave the remaining memory available for Zorin and your other applications.

Related: [[JCHK Start Here]] · [[JCHK Software Map and Baseline]] · [[JCHK JCRS-D and Kubernetes]] · [[JCHK Troubleshooting and Recovery]]

## What this course prepares you to do

By the end, you should be able to explain and investigate “the tool page is down,” “the host is slow,” “the disk is full,” and “the workload will not start.” You will collect evidence, identify the failing layer, make a specific repair, and verify recovery.

The guide places Rocky Linux 9 underneath JCRS-D on the SN 7100 servers. Security Onion collection sensors use Oracle Linux. Linux fundamentals transfer between them, but the images and management procedures differ. This lab uses stock Rocky 9 and a small learning cluster; it does not install JCRS-D or reproduce the kit's secured image, distributed storage, or operational baseline.

**Time budget:** roughly 30–45 hours of initial practice, spread across 4–6 weeks. Repeat the troubleshooting exercises until you can explain the results without following the answers.

**How to follow the commands:** work through sections in order. Run one code block at a time and inspect its result. Commands are labeled **ZORIN**, **ROCKY**, or **ZORIN / Kubernetes**. Zorin is your real laptop; Rocky is the guest VM. Do not type a shell prompt such as `$` or `#`. Password entry does not display characters. Press `q` to leave a pager such as `less`; press `Ctrl+C` to stop a foreground command. In a multiline `cat <<'EOF'` block, paste the complete block, including its final `EOF` line.

> [!important] Lab boundary
> Fault injection, firewall changes, formatting, and service stops in this note are for these disposable lab systems. Actual JCHK work follows the deployed baseline and its recovery process. Keep SELinux and firewalld enabled while learning to diagnose them.

## Course checklist

- [x] 1. Check the laptop and install virtualization tools
- [x] 2. Download and verify Rocky Linux 9
- [x] 3. Install and initialize the VM
- [x] 4. Learn checkpoints and recovery
- [ ] 5. Use SSH and the command line
- [ ] 6. Work with files, permissions, and packages
- [ ] 7. Operate a web service and read its logs
- [ ] 8. Diagnose networking and DNS
- [ ] 9. Diagnose firewall and file-access failures
- [ ] 10. Understand time and certificates
- [ ] 11. Diagnose storage and resource pressure
- [ ] 12. Capture and interpret lab traffic
- [ ] 13. Automate a repeatable health report
- [ ] 14. Understand containers
- [ ] 15. Build a Kubernetes learning cluster
- [ ] 16. Diagnose Kubernetes workloads and persistent storage
- [ ] 17. Complete the JCHK troubleshooting capstone

## 1. Prepare your Zorin laptop

### 1.1 Inventory the laptop

Open Terminal with `Ctrl+Alt+T`.

**ZORIN:**

```bash
cat /etc/os-release
uname -m
lscpu
free -h
df -h / /home /var/lib
```

Record the Zorin version, CPU model, RAM, and free disk space in your lab journal. The commands below assume an Intel/AMD laptop where `uname -m` returns `x86_64`. The package instructions use Zorin's Ubuntu-based `apt` environment. If your system is no longer supported or package sources fail, resolve that before installing the lab.

Use these as planning allocations, not formal product requirements:

| Laptop resources | Recommended plan |
|---|---|
| 8 GB RAM | One Rocky VM with 2 GB RAM and 2 virtual CPUs; shut it down before Kubernetes. Close memory-heavy apps. |
| 16 GB RAM | Rocky with 4 GB RAM and 2 virtual CPUs; Kubernetes with 4 GB RAM, preferably run separately at first. |
| Your 27 GiB laptop, or larger | Use 4 GiB for each lab VM; room for both and an optional second Rocky VM later, subject to CPU and other application use. |
| Under 8 GB RAM | Start with the Linux sections; expect tight memory and defer Kubernetes. |

Have about **60 GB free for the initial lab**; plan around **100 GB free** if keeping the ISO, growing VM disks, checkpoints, and the later Kubernetes VM. Virtual disks grow as data is written. Check free space periodically, especially before checkpoints.

### 1.2 Check hardware virtualization

**ZORIN:**

```bash
grep -E 'vmx|svm' /proc/cpuinfo | head -n 1
```

Output containing `vmx` or `svm` indicates that CPU virtualization extensions are visible. If there is no output, restart into your laptop's firmware settings and look for Intel Virtualization Technology/VT-x or AMD-V/SVM. Enable that setting, save, and boot Zorin again. Firmware entry keys vary by laptop. Do not change unrelated storage or boot settings.

### 1.3 Install KVM and Virtual Machine Manager

KVM runs the VM, QEMU supplies virtual hardware, libvirt manages VMs/networks, and Virtual Machine Manager supplies the graphical interface.

**ZORIN:**

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients virt-manager cpu-checker curl openssh-client
sudo usermod -aG libvirt,kvm "$USER"
```

Log out of Zorin completely, then log back in. Opening another terminal is insufficient to refresh every desktop process's group membership.

**ZORIN:**

```bash
id
kvm-ok
virsh -c qemu:///system list --all
virsh -c qemu:///system net-list --all
```

**Pass:** your groups include `libvirt` and `kvm`, KVM acceleration is available, and `virsh` connects without a permissions error. An empty VM list is normal.

If the `default` virtual network exists but is inactive:

```bash
virsh -c qemu:///system net-start default
virsh -c qemu:///system net-autostart default
```

If it is absent, first check that the distribution supplied this definition:

```bash
ls /usr/share/libvirt/networks/default.xml
```

Only if that file exists and the network is absent:

```bash
virsh -c qemu:///system net-define /usr/share/libvirt/networks/default.xml
virsh -c qemu:///system net-start default
virsh -c qemu:///system net-autostart default
```

If KVM reports permission problems, recheck logout/login and `/dev/kvm` permissions with `ls -l /dev/kvm`. If libvirt cannot connect, inspect `systemctl status libvirtd.socket libvirtd.service`; service organization varies with the Ubuntu base. If a network start reports an address conflict, record the error and compare `ip route` with `virsh -c qemu:///system net-dumpxml default`; do not delete your laptop's routes to make it start.

The default network uses NAT: the guest can normally get out through your laptop, and your laptop can reach the guest. NAT is not an air gap. For this lab, do not bridge the guest directly onto your physical Wi-Fi network.

Reference: [Ubuntu libvirt setup](https://ubuntu.com/server/docs/how-to/virtualisation/libvirt/) and [Virtual Machine Manager](https://ubuntu.com/server/docs/how-to/virtualisation/virtual-machine-manager/).

## 2. Download Rocky Linux 9

1. On Zorin, open [Rocky Linux downloads](https://rockylinux.org/download).
2. Choose the **9.x** release family, **x86_64**, and the **Minimal ISO**. Use the latest supported 9.x minor release offered there; the point is matching the guide's major-version family. Do not accidentally choose Rocky 10.
3. Save it into `Downloads`. The Minimal ISO includes installation packages; the Boot ISO depends more on network installation sources.
4. From the same official release directory, download its `CHECKSUM` file. You can find the 9.x x86_64 images at the [official image directory](https://download.rockylinux.org/pub/rocky/9/isos/x86_64/).
5. Put the ISO and matching checksum file together in a dedicated folder so you do not confuse releases.

**ZORIN:**

```bash
mkdir -p ~/Downloads/rocky9-lab
```

Use the file manager to move the downloaded ISO and its `CHECKSUM` file into that folder. Then:

```bash
cd ~/Downloads/rocky9-lab
ls -lh
sha256sum -c CHECKSUM --ignore-missing
```

**Pass:** the exact ISO filename reports `OK`. Do not proceed if it reports a mismatch or no file was verified. Confirm the filenames, matching release, and completed download; download again if needed. This compares against the checksum you retrieved through the official HTTPS source; it is not an independent signature-verification procedure.

Reference: [Rocky 9 release and verification guidance](https://docs.rockylinux.org/releases/release_notes/9_0/). Its old minor-version examples explain the procedure; use the current 9.x image you selected.

## 3. Create and initialize the Rocky VM

### 3.1 Create its virtual hardware

1. Launch **Virtual Machine Manager** from Zorin's application menu.
2. Use the **QEMU/KVM system connection**. If missing, choose File → Add Connection → QEMU/KVM → local connection. Avoid the user-session connection for this course.
3. Choose **Create a new virtual machine** → **Local install media (ISO)**.
4. Browse to your Rocky ISO. If Rocky 9 is not detected, choose Rocky Linux 9 or the closest RHEL 9 entry. Do not select Windows or Ubuntu for the guest type.
5. Allocate **4096 MiB RAM and 2 CPUs**, or **2048 MiB** on an 8 GB laptop.
6. Create a **30 GiB virtual disk**, preferably qcow2, in the default storage pool.
7. Name it **rocky-lab**. Select **Customize configuration before install**.
8. Confirm its network uses **Virtual network default: NAT** and the network device model is **virtio**.
9. Use the normal virtual disk, not physical-disk passthrough. No laptop partitions should be offered to this VM.
10. Leave the firmware default and start installation. The console is your recovery access if networking fails.

If QEMU cannot read an ISO in your home folder, use the storage browser to place a copy in the default libvirt storage pool, or accept the narrowly scoped access adjustment offered by the application. Do not make your entire home folder world-writable.

### 3.2 Install Rocky

1. At the VM boot menu, choose **Test this media & install Rocky Linux 9**.
2. Select language and keyboard settings.
3. Open **Installation Destination**. Select only the approximately 30 GiB virtual disk and choose automatic partitioning. You are installing inside the VM window.
4. Under software selection, choose **Minimal Install** if the installer offers choices.
5. Under **Network & Host Name**, turn on the virtual Ethernet connection and set the hostname to `rocky-lab.test`.
6. Choose your actual time zone and enable network time if available.
7. Create a user named **student**. Select **Make this user administrator**, require a password, and save that password securely. Keep direct root login locked if the installer allows it.
8. Start installation. When finished, reboot the VM.
9. If it returns to the installer, remove/eject the ISO from the virtual CD-ROM or put the virtual disk first in boot order.
10. Log in as `student`. A text login is expected; this VM does not need a desktop.

### 3.3 Initialize the guest

Everything in this subsection runs **inside Rocky**, in the VM console.

```bash
hostnamectl
cat /etc/rocky-release
ip -br address
ip route
sudo dnf upgrade -y
sudo dnf install -y vim-enhanced nano man-db man-pages bash-completion bind-utils tcpdump httpd firewalld chrony policycoreutils-python-utils audit openssh-server qemu-guest-agent sysstat lsof tar e2fsprogs openssl
sudo systemctl enable --now sshd firewalld chronyd qemu-guest-agent
sudo reboot
```

Internet access is needed for package downloads. If `dnf` fails, inspect `ip route` and `getent hosts download.rockylinux.org`; do not disable the firewall or certificate verification as a first response. If the guest-agent unit reports a missing device, confirm Virtual Machine Manager has a channel named `org.qemu.guest_agent.0`; the rest of the lab can work without the agent.

Log back in and check:

```bash
hostname
sudo -v
getenforce
systemctl is-active sshd firewavlld chronyd
ip -4 -br address
```

**Pass:** Rocky 9 is installed, the hostname is correct, `sudo` works, SELinux reports `Enforcing`, and the three services are active. Record the VM's non-loopback IPv4 address. It is often in `192.168.122.0/24`, but use what your machine actually displays.

## 4. Create a checkpoint and learn recovery

Before experimenting, make a known-good restore point.

1. **ROCKY:** run `sudo poweroff`.
2. In Virtual Machine Manager, confirm `rocky-lab` is shut off.
3. Open its snapshot view/menu and create a snapshot named **01-clean-rocky9**. UI wording varies. A powered-off snapshot avoids capturing a running filesystem mid-write.
4. Start the VM and confirm you can log in.

Some disk/firmware combinations do not support the offered snapshot operation. If the tool rejects it, use the powered-off **Clone** operation instead: create `rocky-lab-clean-copy` with a separate copied disk. Keep that copy powered off. For recovery, power off the broken VM and use the copy; do not run both together before changing identity/network settings. A clone is a fallback, not an instruction to alter storage formats while the VM is running.

**Snapshot restore procedure:** shut the VM down; choose the checkpoint; choose revert/restore; then start it. Everything changed since that checkpoint inside the captured disks can be lost. Keep your journal in Obsidian on Zorin, outside the guest.

Create another checkpoint named **02-working-web** after section 7. Snapshots and clones on the same laptop are convenient rollback tools, not protection from losing the laptop's drive.

## 5. SSH and terminal basics

### 5.1 Connect from Zorin

**ZORIN:** replace the example address with the guest address you recorded. Repeat this variable assignment in any new Zorin terminal where you need it.

```bash
ROCKY_IP=192.168.122.100
ssh student@"$ROCKY_IP"
```

Before accepting the first SSH host-key prompt, compare its fingerprint with the guest console:

**ROCKY console:**

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

After SSH login, `hostname` should report `rocky-lab.test`. Type `exit` to return to Zorin. Keep the VM console available throughout network exercises.

**ZORIN:** create a dedicated key; if the target file already exists, do not overwrite it—reuse the intended key or choose a new lab-specific filename.

```bash
ssh-keygen -t ed25519 -f ~/.ssh/rocky_lab_ed25519 -C rocky-lab
ssh-copy-id -i ~/.ssh/rocky_lab_ed25519.pub student@"$ROCKY_IP"
ssh -i ~/.ssh/rocky_lab_ed25519 student@"$ROCKY_IP"
```

Use a passphrase. The `.pub` file is public; the file without `.pub` is private. Leave password login available during these initial labs so recovery remains simple.

### 5.2 Learn navigation and command help

**ROCKY:**

```bash
whoami
id
pwd
mkdir -p ~/lab/{notes,evidence,backups,scripts}
cd ~/lab
ls -la
printf 'Host: rocky-lab.test\nPurpose: JCHK practice\n' > notes/inventory.txt
cat notes/inventory.txt
cp notes/inventory.txt backups/inventory.txt
grep 'Purpose' notes/inventory.txt
find ~/lab -type f
man ls
```

Press `q` to leave the manual. Use Tab for completion and the up-arrow for history. Learn `/etc` for configuration, `/var/log` for logs, `/home/student` for your files, and `/run` for runtime state.

**Pass:** you can explain your current directory, absolute versus relative paths, and what `>` does. It replaces a file's contents; `>>` appends. Practice only on your own lab files.

**JCHK connection:** distinguish the laptop, physical host, VM guest, and container before interpreting a command's output.

## 6. Files, permissions, packages, and recoverable edits

**ROCKY:**

```bash
printf 'practice only\n' > ~/lab/notes/private.txt
chmod 600 ~/lab/notes/private.txt
ls -l ~/lab/notes/private.txt
stat ~/lab/notes/private.txt
rpm -q httpd
rpm -ql httpd | head
dnf info httpd
dnf history list
```

`600` gives the owner read/write access and nobody else access. In `ls -l`, identify the owner, group, and permission bits. Learn that directory execute permission means the ability to traverse that directory.

Practice an edit and restore:

```bash
cp -a ~/lab/notes/inventory.txt ~/lab/backups/inventory.before-edit.txt
nano ~/lab/notes/inventory.txt
diff -u ~/lab/backups/inventory.before-edit.txt ~/lab/notes/inventory.txt
cp -a ~/lab/backups/inventory.before-edit.txt ~/lab/notes/inventory.txt
```

In nano, save with `Ctrl+O`, press Enter, and exit with `Ctrl+X`. A `diff` exit code of 1 means differences were found, not that the command malfunctioned.

**Pass:** you can inspect a package, make a backup before editing, explain a permission string, and restore your own file. Do not use recursive `chmod 777` to fix unexplained access problems.

## 7. Build and troubleshoot a real service

### 7.1 Start the web service

**ROCKY:**

```bash
printf 'Rocky JCHK practice service is healthy\n' | sudo tee /var/www/html/index.html
sudo restorecon -v /var/www/html/index.html
sudo systemctl enable --now httpd
curl --fail http://127.0.0.1/
sudo ss -lntp
sudo systemctl status httpd --no-pager
sudo journalctl -u httpd -b --no-pager -n 30
```

Now identify the guest's network profile:

```bash
nmcli -t -f NAME,DEVICE connection show --active
```

Choose the Ethernet profile associated with the guest's NAT interface, not `lo`. In the **Rocky console**, replace the example with that exact profile name:

```bash
LAB_CON='Wired connection 1'
sudo nmcli connection modify "$LAB_CON" connection.zone public
sudo nmcli connection up "$LAB_CON"
sudo firewall-cmd --zone=public --add-service=ssh --permanent
sudo firewall-cmd --zone=public --add-service=http --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --zone=public --list-services
```

Reactivating the profile may interrupt SSH; this is why the console is used. Confirm your NAT interface is assigned to `public`. Record the profile name in your journal. Recheck the IP in case DHCP changed it.

**ZORIN:**

```bash
curl --fail --max-time 5 "http://$ROCKY_IP/"
```

Also open `http://YOUR-ROCKY-IP/` in Zorin's browser. You should see your message. Create checkpoint **02-working-web** using section 4.

### 7.2 Fault: the service stops

**ROCKY:**

```bash
sudo systemctl stop httpd
curl --max-time 5 http://127.0.0.1/
systemctl is-active httpd
sudo ss -lntp
sudo journalctl -u httpd --since '10 minutes ago' --no-pager
```

**Expected:** HTTP fails, the service is inactive, and the port-80 listener is absent. The logs should show shutdown, not necessarily a crash.

Recover and verify:

```bash
sudo systemctl start httpd
curl --fail http://127.0.0.1/
```

Repeat the Zorin request. Explain the difference between `start`, `enable`, `restart`, and `status`. `enable` affects boot behavior; it does not by itself start the service.

### 7.3 Fault: invalid configuration

**ROCKY:** create only this dedicated lab file:

```bash
printf 'ThisIsNotAnApacheDirective yes\n' | sudo tee /etc/httpd/conf.d/zz-lab-broken.conf
sudo apachectl configtest
```

The configuration check should fail and name the file/line. The running service may still work because this bad configuration has not been loaded. This is why a preflight configuration check is useful.

Recover:

```bash
sudo rm /etc/httpd/conf.d/zz-lab-broken.conf
sudo apachectl configtest
sudo systemctl reload httpd
curl --fail http://127.0.0.1/
```

**Pass:** record the error, identify its cause, and repair it without rebooting the VM.

## 8. Networking and DNS

Related: [[JCHK Network Fundamentals]] · [[JCHK Service Portal and Identity]]

### 8.1 Trace the path

**ROCKY:**

```bash
ip -br link
ip -4 -br address
ip route
nmcli device status
nmcli device show
cat /etc/resolv.conf
getent hosts download.rockylinux.org
dig download.rockylinux.org
curl -I --max-time 10 https://download.rockylinux.org/
```

Write down: interface name, address/prefix, default gateway, DNS server, and the difference between a DNS answer and an HTTP response. `getent` uses the system's configured name-service lookup path; `dig` queries DNS. A ping failure alone does not establish that a web service is unreachable.

### 8.2 Fault: use an unresponsive DNS server

Run this exercise from the **Rocky VM console**, not only SSH. This fresh lab uses DHCP for IPv4. Before changing anything, verify the selected profile and save its settings:

```bash
LAB_CON='Wired connection 1'
nmcli connection show "$LAB_CON" | tee ~/lab/backups/network-before-dns.txt
nmcli -g ipv4.method,ipv4.dns,ipv4.ignore-auto-dns connection show "$LAB_CON"
```

Replace the profile name as before. Proceed with the provided rollback only if `ipv4.method` is `auto`, DNS is empty, and `ignore-auto-dns` is `no`. Otherwise record and restore your actual values rather than assuming the defaults.

```bash
sudo nmcli connection modify "$LAB_CON" ipv4.ignore-auto-dns yes ipv4.dns 192.0.2.53
sudo nmcli connection up "$LAB_CON"
dig +time=2 +tries=1 @192.0.2.53 download.rockylinux.org
dig +time=2 +tries=1 download.rockylinux.org
curl --fail http://127.0.0.1/
ip route
```

The direct query to the deliberately unusable documentation address should time out. Normal DNS may still succeed if another resolver, IPv6 DNS, or a cache is available; inspect `nmcli device show` and `/etc/resolv.conf` and explain that result. Do not treat an unexpectedly successful query as evidence that DNS settings never matter.

Restore the original DHCP DNS behavior:

```bash
sudo nmcli connection modify "$LAB_CON" ipv4.ignore-auto-dns no ipv4.dns ''
sudo nmcli connection up "$LAB_CON"
dig +time=2 +tries=1 download.rockylinux.org
```

**Pass:** DNS is restored, local HTTP kept working during the exercise, and you can explain why DNS failure does not necessarily mean link or service failure.

**Extension:** read `ip route get 192.0.2.10` and explain which next hop the OS would choose. This checks the route decision; it does not contact that destination or prove end-to-end reachability.

## 9. Firewall, Unix permissions, and SELinux

These are separate controls. Practice each fault independently and recover before moving on.

### 9.1 Fault: the firewall blocks HTTP

**ROCKY:**

```bash
sudo firewall-cmd --zone=public --remove-service=http
curl --fail http://127.0.0.1/
sudo ss -lntp
sudo firewall-cmd --zone=public --list-services
```

**ZORIN:**

```bash
curl --max-time 5 "http://$ROCKY_IP/"
```

Expected: the guest's local request works, the service still listens, but the fresh remote request fails. The failure might be a rejection or timeout. If it succeeds, check the active zone, any explicit port rules, and whether you removed the rule from the actual interface's zone.

**ROCKY recovery:**

```bash
sudo firewall-cmd --zone=public --add-service=http
```

This exercise changed runtime rules only. The permanent HTTP allowance remains. Repeat the Zorin request to prove recovery.

### 9.2 Fault: ordinary file permissions

**ROCKY:**

```bash
sudo chmod 000 /var/www/html/index.html
curl -i http://127.0.0.1/index.html
ls -l /var/www/html/index.html
sudo tail -n 20 /var/log/httpd/error_log
```

Expect an HTTP access error, commonly 403, while `httpd` remains active.

Recover:

```bash
sudo chmod 644 /var/www/html/index.html
curl --fail http://127.0.0.1/index.html
```

### 9.3 Fault: the SELinux label is wrong

**ROCKY:**

```bash
getenforce
ls -lZ /var/www/html/index.html
sudo chcon -t user_home_t /var/www/html/index.html
curl -i http://127.0.0.1/index.html
ls -lZ /var/www/html/index.html
sudo ausearch -m AVC,USER_AVC -ts recent
```

With SELinux enforcing, the deliberately inappropriate label should cause access denial even though the Unix permissions still allow reading. The audit output is evidence; absence of a matching event is a reason to investigate, not to assume SELinux caused the error.

Restore the standard path's expected label:

```bash
sudo restorecon -v /var/www/html/index.html
curl --fail http://127.0.0.1/index.html
```

**Pass:** distinguish a closed port, stopped process, permission denial, and SELinux denial. Explain why changing a label with `chcon` is not a durable policy definition for a custom directory. Later, study `semanage fcontext` for persistent mappings.

References: [Rocky firewalld basics](https://docs.rockylinux.org/guides/security/firewalld-beginners/) and [Rocky SELinux](https://docs.rockylinux.org/guides/security/learning_selinux/).

## 10. Time synchronization and certificate trust

Related: [[JCHK Service Portal and Identity]]

### 10.1 Check time without intentionally corrupting it

**ROCKY:**

```bash
date -Is
timedatectl
systemctl status chronyd --no-pager
chronyc tracking
chronyc sources -v
sudo journalctl -u chronyd -b --no-pager -n 30
```

Find the selected time source, if one is available, and the synchronization state. In `chronyc sources`, `*` marks the selected source. A newly booted or disconnected VM may need time or network access before synchronizing. A running daemon alone does not establish synchronization. Do not manually set an incorrect clock for this lab.

**JCHK connection:** timestamp correlation, certificate validity, and some authentication mechanisms depend on correct time. A stock VM's public time sources are not the kit's deployed time configuration.

### 10.2 Create a local TLS practice endpoint

This uses a temporary OpenSSL server on loopback port 8443. It does not change Apache or install a new CA into your laptop's trust store.

**ROCKY terminal A:**

```bash
mkdir -p ~/lab/tls
cd ~/lab/tls
openssl req -x509 -newkey rsa:2048 -sha256 -days 7 -nodes -keyout lab.key -out lab.crt -subj '/CN=rocky-lab.test' -addext 'subjectAltName=DNS:rocky-lab.test'
chmod 600 lab.key
openssl x509 -in lab.crt -noout -subject -issuer -dates
openssl s_server -accept 127.0.0.1:8443 -cert lab.crt -key lab.key -www
```

Leave that server running. Its unencrypted private key is lab-only and protected by the file's permissions.

**ROCKY terminal B:** open a second SSH connection from Zorin, then run:

```bash
curl --noproxy '*' --resolve rocky-lab.test:8443:127.0.0.1 https://rocky-lab.test:8443/
curl --noproxy '*' --cacert ~/lab/tls/lab.crt --resolve rocky-lab.test:8443:127.0.0.1 https://rocky-lab.test:8443/
curl --noproxy '*' --cacert ~/lab/tls/lab.crt https://127.0.0.1:8443/
```

Expected: first request fails because the self-signed certificate is not trusted; second works because it explicitly trusts that certificate and uses the matching hostname; third fails hostname validation because the certificate lacks an IP-address SAN.

`--resolve` supplies an address for this request while preserving the URL hostname used in TLS validation. Do not use `curl -k` as your solution. Stop the server with `Ctrl+C` in terminal A. If repeating after seven days, regenerate the lab certificate.

**Pass:** explain trust, hostname matching, and validity dates as different checks.

Reference: [RHEL 9 chrony guidance](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/configuring-time-synchronization_configuring-basic-system-settings).

TLS command references: [OpenSSL certificate creation](https://docs.openssl.org/3.0/man1/openssl-req/) and [OpenSSL test server](https://docs.openssl.org/3.0/man1/openssl-s_server/).

## 11. Storage and resource troubleshooting

Related: [[JCHK Storage Capacity and Retention]] · [[JCHK Sensor Health and Packet Loss]]

### 11.1 Read the machine's storage layout

**ROCKY:**

```bash
lsblk -f
findmnt
df -h
df -i
du -sh ~/lab
free -h
uptime
vmstat 1 5
iostat -xz 1 3
```

Explain the differences between a disk, partition, filesystem, and mount point. `df -h` shows filesystem space; `df -i` shows inode availability; `du` measures file usage under a path. Available memory and load averages need context, not arbitrary universal passing thresholds.

If the automatic installation used LVM, inspect it:

```bash
sudo dnf install -y lvm2
sudo pvs
sudo vgs
sudo lvs
```

A physical volume contributes storage to a volume group; logical volumes allocate it. Empty output is valid if your installation did not use LVM. Do not issue formatting or `pvcreate` commands against disks merely to make this output nonempty.

### 11.2 Fault: a small disposable filesystem fills

Use a **256 MiB file-backed filesystem** so the failure is bounded. All commands below run in **ROCKY**, never against a Zorin disk. Start with at least 1 GiB free in the guest and adequate space on Zorin.

```bash
df -h ~/lab
test -e ~/lab/scratch.img && echo 'STOP: scratch.img already exists; inspect it before continuing'
```

If the file already exists, skip its creation/formatting and inspect your previous exercise. For the first run:

```bash
fallocate -l 256M ~/lab/scratch.img
mkfs.ext4 -F ~/lab/scratch.img
sudo mkdir -p /mnt/lab-scratch
sudo mount -o loop ~/lab/scratch.img /mnt/lab-scratch
findmnt /mnt/lab-scratch
sudo chown student:student /mnt/lab-scratch
```

**Stop unless `findmnt` confirms `/mnt/lab-scratch` is mounted from a loop device.** Otherwise the next write would target the guest's root filesystem directory.

Create bounded synthetic data; the guard prevents the write if the mount is absent:

```bash
mountpoint -q /mnt/lab-scratch && dd if=/dev/zero of=/mnt/lab-scratch/fill.bin bs=1M count=300 status=progress
df -h /mnt/lab-scratch
df -i /mnt/lab-scratch
du -sh /mnt/lab-scratch
```

Expect `dd` to stop with **No space left on device** before 300 MiB. That is the intended fault. Compare used bytes and inodes. Your guest's root filesystem should still have free space.

Recover:

```bash
rm /mnt/lab-scratch/fill.bin
printf 'write succeeds after recovery\n' > /mnt/lab-scratch/proof.txt
cat /mnt/lab-scratch/proof.txt
df -h /mnt/lab-scratch
cd ~
sudo umount /mnt/lab-scratch
```

Keep `scratch.img` for later practice. No `/etc/fstab` changes were made, so it will not mount automatically after reboot. To repeat, mount the existing image again; do not reformat it unless you intend to erase its lab contents.

### 11.3 Observe a bounded CPU load

**ROCKY terminal A:**

```bash
timeout 20s yes > /dev/null
```

**ROCKY terminal B**, while it runs:

```bash
top
```

Find the `yes` process, observe its CPU use, and watch it disappear after about 20 seconds. Press `q`. This creates short CPU activity without creating a large file. Compare with `vmstat 1 5` when idle.

**Pass:** identify the exhausted resource from evidence and verify a successful write or request after recovery. Do not delete captures or databases merely because a filesystem is full.

## 12. Capture your own lab traffic

This exercise captures only your synthetic HTTP request. It teaches the difference between a listener, a connection, and actual observed traffic.

**ROCKY terminal A:**

```bash
sudo timeout 20s tcpdump -i any -nn -s 0 -w /tmp/rocky-lab-http.pcap 'tcp port 80'
```

While it runs, **ZORIN terminal B:**

```bash
curl --fail "http://$ROCKY_IP/index.html"
```

After the capture ends, **ROCKY:**

```bash
sudo tcpdump -nn -r /tmp/rocky-lab-http.pcap
sudo cp /tmp/rocky-lab-http.pcap ~/lab/evidence/http.pcap
sudo chown student:student ~/lab/evidence/http.pcap
sha256sum ~/lab/evidence/http.pcap > ~/lab/evidence/http.pcap.sha256
```

Identify the client/server IPs and TCP handshake. If zero packets were captured, check whether the request happened during the capture, the destination was correct, and HTTP was healthy. The timeout may return exit status 124 even though the capture completed as designed.

**ZORIN:** copy your evidence into a dedicated folder:

```bash
mkdir -p ~/Documents/rocky-lab-evidence
scp -i ~/.ssh/rocky_lab_ed25519 student@"$ROCKY_IP":lab/evidence/http.pcap ~/Documents/rocky-lab-evidence/
scp -i ~/.ssh/rocky_lab_ed25519 student@"$ROCKY_IP":lab/evidence/http.pcap.sha256 ~/Documents/rocky-lab-evidence/
```

The checksum file contains the guest's path, so compare the digest with `sha256sum ~/Documents/rocky-lab-evidence/http.pcap` rather than blindly checking the guest path on Zorin.

**JCHK connection:** observing packets at one interface does not establish successful parsing, forwarding, indexing, or retention. A hash supports integrity comparison; it is not a complete evidence-handling process.

## 13. Create a repeatable health report

**ROCKY:** create a small script that observes the host and records a basic HTTP test.

```bash
cat > ~/lab/scripts/health-report.sh <<'EOF'
#!/usr/bin/env bash
set -u
printf '=== Identity and time ===\n'
hostname
date -Is
printf '\n=== OS ===\n'
cat /etc/rocky-release
printf '\n=== Addresses and routes ===\n'
ip -br address
ip route
printf '\n=== Services ===\n'
for service in sshd firewalld chronyd httpd; do
  printf '%s: ' "$service"
  systemctl is-active "$service" || true
done
printf '\n=== Resources ===\n'
free -h
df -h
printf '\n=== Local HTTP ===\n'
if curl --fail --silent --show-error --max-time 5 http://127.0.0.1/; then
  printf '\nHTTP check: PASS\n'
else
  printf '\nHTTP check: FAIL\n'
  exit 1
fi
EOF
chmod 700 ~/lab/scripts/health-report.sh
bash -n ~/lab/scripts/health-report.sh
~/lab/scripts/health-report.sh > ~/lab/evidence/health-report.txt 2>&1
cat ~/lab/evidence/health-report.txt
```

Run it before and after stopping `httpd`; then recover the service. Capture the exit code immediately after running the script with `echo $?`, before any other command changes it. Exit 0 here means only that the local HTTP test passed; the report is not a comprehensive readiness certification.

**Pass:** you can explain every line, add a useful read-only check, and avoid logging passwords or private keys. This is a foundation for understanding JAKD automation output, not a replacement for its playbooks.

## 14. Run a container on Rocky

First understand that a container shares its host's kernel; it is not another full guest operating system.

**ROCKY, logged in as student:**

```bash
sudo dnf install -y podman
podman info
podman run -d --name lab-nginx -p 127.0.0.1:8080:80 docker.io/library/nginx:stable
podman ps
curl --fail http://127.0.0.1:8080/
podman logs lab-nginx
podman inspect lab-nginx
```

This runs rootless under your user. Port 8080 is bound only to Rocky's loopback interface. Its image downloads from a public registry; save the image digest shown by `podman image inspect docker.io/library/nginx:stable` because the `stable` tag can change over time.

Fault and recovery:

```bash
podman stop lab-nginx
curl --max-time 5 http://127.0.0.1:8080/
podman ps -a
podman start lab-nginx
curl --fail http://127.0.0.1:8080/
```

Cleanup after recording results:

```bash
podman stop lab-nginx
podman rm lab-nginx
```

If a pull fails, inspect connectivity, DNS, registry response, and disk capacity. If rootless setup fails, inspect `podman info` and the `student` entries in `/etc/subuid` and `/etc/subgid`; a rootless account needs subordinate ID ranges. Do not switch randomly between `sudo podman` and `podman`, because they use different container stores.

**Pass:** distinguish image, container, process, published port, host filesystem, and container filesystem. Defer advanced container building until these distinctions are comfortable.

Reference: [Podman run and rootless containers](https://docs.podman.io/en/latest/markdown/podman-run.1.html).

## 15. Build a separate Kubernetes practice VM

This section runs **on Zorin**. Minikube uses KVM to create a separate learning node. It is not installed inside Rocky, so nested virtualization is unnecessary. The node's OS is minikube's own image; these exercises teach Kubernetes operations that you will later apply in the Rocky-based JCRS-D environment.

On an 8 GB laptop, shut down Rocky first. The starting allocation below is 4 GB RAM, 2 CPUs, and a 30 GB virtual disk. Check `free -h` and disk capacity before proceeding.

### 15.1 Install minikube

**ZORIN:**

```bash
mkdir -p ~/Downloads/minikube-lab
cd ~/Downloads/minikube-lab
curl -fLO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install -m 0755 minikube-linux-amd64 /usr/local/bin/minikube
minikube version
minikube start -p jchk-lab --driver=kvm2 --cpus=2 --memory=4096 --disk-size=30g
minikube -p jchk-lab status
minikube -p jchk-lab kubectl -- get nodes
```

Run minikube as your normal Zorin user, not with `sudo`. Record the minikube version and Kubernetes version. The moving download gives the current learning release, not the JCHK version. This lab uses long-established APIs; use the deployed kit's supported versions for operational work.

**Pass:** the profile is running and one node is `Ready`. First launch needs Internet access and may take several minutes. For failure, read the exact message and inspect `minikube -p jchk-lab logs`, available memory/disk, and the libvirt networks. Do not destroy the default libvirt network as a generic repair.

### 15.2 Create a short command helper

**ZORIN:** define this function in each terminal used for the Kubernetes labs:

```bash
k() { minikube -p jchk-lab kubectl -- "$@"; }
k get nodes -o wide
k create namespace practice
```

`k` is a helper defined by this lab, not a standard command. If the namespace already exists, keep using it. Every following `k` command targets the dedicated `jchk-lab` profile.

References: [minikube installation](https://minikube.sigs.k8s.io/docs/start/) and [KVM driver](https://minikube.sigs.k8s.io/docs/drivers/kvm2/).

## 16. Kubernetes workloads and storage

### 16.1 Deploy a service

**ZORIN / Kubernetes:**

```bash
k -n practice create deployment lab-web --image=nginx:stable
k -n practice rollout status deployment/lab-web --timeout=180s
k -n practice expose deployment lab-web --port=80 --target-port=80
k -n practice get pods -o wide
k -n practice get services
k -n practice describe deployment lab-web
k -n practice logs deployment/lab-web --tail=20
k -n practice port-forward service/lab-web 8081:80
```

Leave port-forward running. In another **Zorin** terminal:

```bash
curl --fail http://127.0.0.1:8081/
```

Pass: the Nginx page is returned. Port-forward is a temporary local access method, not proof that an ingress controller, external route, or firewall path works. Press `Ctrl+C` in the first terminal to end it.

### 16.2 Fault: an image cannot be pulled

```bash
k -n practice set image deployment/lab-web nginx=nginx:does-not-exist-jchk-lab
k -n practice get pods
k -n practice get events --sort-by=.metadata.creationTimestamp
```

Wait briefly and inspect the new failing pod. Copy its exact name from `get pods`, then replace the example:

```bash
BAD_POD='paste-the-failing-pod-name-here'
k -n practice describe pod "$BAD_POD"
```

Expect image-pull failures such as `ErrImagePull` or `ImagePullBackOff`. Container logs may not exist because the container never started. The old healthy replica may keep serving during the failed rollout; a bad update does not automatically imply total outage.

Recover:

```bash
k -n practice set image deployment/lab-web nginx=nginx:stable
k -n practice rollout status deployment/lab-web --timeout=180s
k -n practice get pods
```

### 16.3 Observe controller reconciliation

```bash
k -n practice delete pod -l app=lab-web
k -n practice get pods -w
```

Watch the Deployment create a replacement. Press `Ctrl+C` after the new pod is running. This deliberate deletion is limited to the lab's `practice` namespace. Explain why deleting a managed pod is not equivalent to uninstalling the application.

### 16.4 Create a persistent claim and use it

Confirm a default storage class is present:

```bash
k get storageclass
```

On ordinary minikube, its host-path storage provisioner supplies the default. If none exists, inspect `minikube -p jchk-lab addons list` and enable the lab add-ons:

```bash
minikube -p jchk-lab addons enable storage-provisioner
minikube -p jchk-lab addons enable default-storageclass
k get storageclass
```

Create an original practice manifest:

```bash
mkdir -p ~/Documents/rocky-k8s-lab
cat > ~/Documents/rocky-k8s-lab/storage.yaml <<'EOF'
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: evidence
  namespace: practice
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: v1
kind: Pod
metadata:
  name: evidence-reader
  namespace: practice
spec:
  containers:
    - name: reader
      image: busybox:1.37
      command: ["sh", "-c", "sleep 86400"]
      volumeMounts:
        - name: evidence
          mountPath: /evidence
  volumes:
    - name: evidence
      persistentVolumeClaim:
        claimName: evidence
EOF
k apply -f ~/Documents/rocky-k8s-lab/storage.yaml
k -n practice wait --for=condition=Ready pod/evidence-reader --timeout=180s
k -n practice get pvc
k get pv
k -n practice exec evidence-reader -- sh -c 'echo synthetic-lab-evidence > /evidence/proof.txt'
k -n practice exec evidence-reader -- cat /evidence/proof.txt
```

Now remove only the pod and recreate it from the same manifest:

```bash
k -n practice delete pod evidence-reader
k apply -f ~/Documents/rocky-k8s-lab/storage.yaml
k -n practice wait --for=condition=Ready pod/evidence-reader --timeout=180s
k -n practice exec evidence-reader -- cat /evidence/proof.txt
```

**Pass:** the file remains after pod recreation. The PVC remained bound while the pod was removed. If it is Pending, use `describe pvc`, `describe pod`, and namespace events to identify why. The sleeping practice container lasts one day per start; recreate it if returning later and it has stopped.

A PVC is namespaced; a PV is cluster-scoped. This minikube storage is local to its node and is not Rook/Ceph, redundancy, or backup. Deleting the cluster can remove its data. Learn Ceph and KubeVirt architecture next using [[JCHK JCRS-D and Kubernetes]]; a single-node laptop lab does not demonstrate their production recovery behavior.

### 16.5 Stop and resume

**ZORIN:**

```bash
minikube stop -p jchk-lab
```

Resume with `minikube start -p jchk-lab`. Preserve the profile while learning. `minikube delete -p jchk-lab` removes the practice cluster and can remove its stored data; use it only when intentionally discarding that lab after saving your journal and manifests.

References: [Kubernetes basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/), [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/), and [minikube local storage](https://minikube.sigs.k8s.io/docs/handbook/persistent_volumes/).

## 17. JCHK-focused capstone

Related: [[JCHK Capstone Practice]] · [[JCHK Troubleshooting and Recovery]] · [[JCHK Post-Deployment Readiness]]

Return to the Rocky VM. Pick one fault from sections 7–11, or have a study partner choose one while you look away. Use only the bounded lab faults already described. Start from a working checkpoint, introduce one fault, and hide the recovery instructions while diagnosing.

### Scenario A: “The tool page will not open”

1. Record which client, destination, time, and exact error are involved.
2. Check whether the address and route are correct.
3. Compare name lookup with address-level reachability.
4. Compare HTTP on Rocky loopback with HTTP from Zorin.
5. Inspect the listener, service status, and recent logs.
6. If access is denied, examine Unix permissions and SELinux evidence separately.
7. State your diagnosis before changing anything.
8. Make one specific repair and repeat both local and remote tests.

### Scenario B: “The application cannot save evidence”

1. Use the scratch filesystem, not actual captured data, to reproduce the full-disk condition.
2. Confirm the target is mounted where expected.
3. Compare byte and inode availability.
4. Identify the synthetic file consuming space.
5. Remove only that known test file and prove a new write succeeds.
6. Explain why a successful write does not prove redundancy or an independent backup.

### Scenario C: “The node is Ready, but the workload is broken”

1. Use the minikube invalid-image exercise.
2. Verify the node state separately from pod and rollout state.
3. Inspect the correct namespace and object events.
4. Explain why logs may be unavailable for a container that never started.
5. Correct the image and verify the rollout and an actual HTTP response.

### Your incident record

Copy this template into a fresh Obsidian note for each attempt:

```text
Date/time and timezone:
Host / VM / namespace:
Observed symptom and exact error:
Last known working state:
Recent change:
Hypothesis:
Check performed:
Observation:
What this proves / does not prove:
Corrective action and its scope:
Rollback available:
Verification from the affected client:
Remaining uncertainty:
Relevant JCHK guide note:
```

**Completion standard:** diagnose all three scenarios, explain each command you used, and recover without blanket firewall disabling, SELinux disabling, or an unexplained reboot. Repeat on a different day without copying the repair commands.

## Applying this to the actual JCHK

Use the guide's layers to choose your next check:

| Observation | Useful next evidence |
|---|---|
| Several tool URLs fail together | Client addressing, routes, DNS, time, shared identity dependencies |
| One Rocky host is unhealthy | Host journals, storage, memory, interfaces, relevant services |
| Host seems healthy but application fails | Namespace, pod/VM status, events, application logs, storage claims |
| Sensor has no data | Capture path, TAP/interface counters, sensor services; check the actual Oracle Linux/Security Onion host |
| Local capture exists but search is empty | Forwarding, ingestion, indexing, search time range, role-specific health |

The guide introduces `kubectl get nodes`, `kubectl get all -n jcrsd`, and `kubectl get vmis --all-namespaces`. Read those as targeted observations. `get all` is not every resource type; `vmis` requires KubeVirt and will not exist in this plain minikube cluster. Security Onion commands such as `so-status` belong on the appropriate Security Onion installation, not stock Rocky.

Next topics after completing this course: network bridges/bonds/VLANs; durable mounts and LVM growth on an additional disposable virtual disk; FreeIPA/Kerberos concepts; reading Ansible tasks and YAML; KubeVirt VM lifecycles; Rook/Ceph health and failure domains. Learn those against the actual platform's version and role. This course establishes operational foundations, not authority to reconfigure the kit.

## Common snags

| Symptom | First check |
|---|---|
| `sudo` says student is not allowed | The installer account must be an administrator. Use another configured administrator to add it to `wheel`, or correct the clean lab installation. |
| SSH times out | VM is running, current IP, NAT network, `sshd`, guest firewall; use the console. |
| SSH host key changed | A reinstall/clone may explain it. Verify the console fingerprint first; only then remove the old entry with `ssh-keygen -R "$ROCKY_IP"` on Zorin. |
| Guest IP changed | Read it with `ip -4 -br address` in the console and update `ROCKY_IP` in Zorin terminals. |
| Guest has no Internet | Check libvirt NAT state, guest route and DNS, then laptop connectivity/VPN policy. |
| Rocky installer rejects CPU | Confirm Rocky 9's x86-64-v2 support and that the VM exposes an appropriate host CPU model. Old CPUs may not meet the requirement. |
| `LAB_CON` is empty after reconnecting | Shell variables do not persist between sessions; assign the actual connection name again. |
| `k: command not found` | Redefine the helper from section 15 in that Zorin terminal. |
| Kubernetes pod is Pending or cannot pull | Read `describe` and events; inspect capacity, storage, network/registry errors. Do not infer the cause from the status label alone. |
| Laptop becomes sluggish | Stop the unused VM/profile, inspect available memory and disk, then reduce concurrent workloads. |

## Source and verification notes

This note was written for the JCHK v0.6.1 guide in this vault, especially [[JCHK Software Map and Baseline]], [[JCHK JCRS-D and Kubernetes]], [[JCHK Service Portal and Identity]], [[JCHK Sensor Health and Packet Loss]], and [[JCHK Troubleshooting and Recovery]]. Their deployment claims describe that training snapshot, not verification of a current kit.

External documentation was checked on 2026-09-23. The linked official references support the setup and platform concepts; the staged fault exercises are an original learning sequence. UI labels and package versions can vary. All 68 Bash command blocks and the embedded health script passed shell syntax checks; the Kubernetes YAML parsed successfully and its claim/namespace references were checked. The instructions were also reviewed for host/guest boundaries, dependencies, expected results, and rollback. These are static checks, not an end-to-end execution on your Zorin laptop. Record actual versions and unexpected output as you work.
