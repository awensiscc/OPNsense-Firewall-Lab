# Open-Source Firewall Testing: OPNsense Lab

Group project for the Network Security module at TU Dublin (CA1, Oct to Nov 2025), with Vsevolod Shestopalov and Vladyslav Shkurina.

Each of us set up a different open-source firewall in a VMware lab and attacked it to see how well its rules held up. The three firewalls were OPNsense, nftables and pfSense. My part was OPNsense.

I built a small network with three zones: a Kali attacker on the WAN (RED) side, the OPNsense firewall in the middle, and a Windows 10 victim on the LAN (GREEN) side. WAN to LAN was denied by default; LAN to WAN was allowed. Then I ran three tests against my own rules:

- SSH left open to the WAN on purpose, to play a misconfigured rule. I brute-forced the login with hydra; OPNsense rate-limited the attempts, and I showed a weak password still means full firewall access. Fix: restrict SSH to the LAN address range, so any outside host is refused (least privilege).
- Full Nmap port scan from the attacker, with per-rule logging turned on. The live log showed every WAN probe being dropped in real time.
- Port forwarding to a small web server on the victim, then a Nikto scan of it, to show why the service behind a forwarded port still has to be configured carefully.

For each test I wrote up why I ran it, what the firewall did, and how to remediate it, then compared OPNsense against my teammates' nftables and pfSense setups. We presented and demonstrated the work live in the lab.

Tools: OPNsense, Kali Linux, VMware, hydra, Nmap, Nikto. Topics: stateful firewall rules, network zoning (WAN/LAN), least-privilege access, logging and monitoring.

## Lab topology
![Topology](images/01-topology.png)

## WAN firewall rules
![WAN rules](images/02-wan-firewall-rules.png)
