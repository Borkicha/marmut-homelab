# marmut-homelab

Production self-hosted homelab featuring Zero-Trust mesh network, self-hosted security &amp; media stack, custom Python web apps, and an isolated KVM lab for penetration testing.

## Network Architecture & Topology

```mermaid
graph TD
    Client[Remote Workstation / Mobile] -->|Tailscale Encrypted Mesh| TS[Tailscale Interface]
    
    subgraph Host OS - Production Stack (Docker)
        TS --> Proxy[Reverse Proxy - Nginx Proxy Manager]
        
        Proxy --> Vault[Vaultwarden - Passwords]
        Proxy --> Immich[Immich - Photo Management]
        Proxy --> Jelly[Jellyfin - Media Server]
        Proxy --> PyApp[Custom Python App - Calendar/Todo]
        
        DNS[Pi-hole - Network DNS & Adblocking] --> TS
        CrowdSec[CrowdSec IPS] -->|Parses Access Logs| Proxy
        Proxy -->|Security Hardening| UFW[UFW Firewall / Fail2ban]
    end

    subgraph Host OS - Isolated Sandbox (KVM / QEMU Virtual Machines)
        Attacker[Pentest Workstation] -->|Isolated Internal Virtual Bridge| VulnVM[Vulnerable VMs / VulnHub / Metasploitable]
    end
