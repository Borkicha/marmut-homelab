# marmut-homelab

Production self-hosted homelab featuring Zero-Trust mesh network, self-hosted security & media stack, custom Python web apps, and an isolated KVM lab for penetration testing.

## Network Architecture & Topology

```mermaid
graph TD
    subgraph "Your Laptop (Attacker / Admin)"
        Laptop[Personal Laptop / Kali / Tools]
    end

    subgraph "MarmutServer (Host OS)"
        subgraph "Production Stack (Docker)"
            TS[Tailscale Interface] --> Proxy[Nginx Proxy Manager]
            Proxy --> Services[Vaultwarden / Immich / Jellyfin / Python App]
            DNS[Pi-hole DNS]
            CrowdSec[CrowdSec IPS] --> Proxy
        end

        subgraph "Isolated Sandbox (KVM / QEMU)"
            Bridge[Isolated Lab Switch / Bridge]
            Bridge --> VulnVM1[Metasploitable]
            Bridge --> VulnVM2[VulnHub Target]
        end
    end

    %% Access flows
    Laptop -->|Zero-Trust Admin Access| TS
    Laptop -->|Encrypted Lab Tunnel / Dedicated Access| Bridge
