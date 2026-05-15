# FortiSASE Explicit Proxy PAC Flow

::: mermaid
flowchart LR
    subgraph DEVOPS["Configuration / Automation"]
        REPO["Git Repo<br/>proxy.pac"]
        PIPE["Azure DevOps Pipeline"]
    end

    subgraph USER["Remote User"]
        EP["User Endpoint<br/>Browser + FortiSASE Agent"]
    end

    subgraph SASE["FortiSASE Cloud"]
        POP["FortiSASE PoP"]
    end

    subgraph ONPREM["On-Prem Environment"]
        FG["FortiGate Firewall<br/>Explicit Proxy Enabled<br/>PAC Hosting on :8888"]
    end

    INTERNET["Public Websites / Internet"]

    REPO --> PIPE
    PIPE -.->|"REST API PUT<br/>Updates PAC config"| FG

    EP -.->|"Downloads PAC file"| FG
    EP -->|"PAC-matched traffic"| POP
    POP -->|"IPsec / BGP tunnel"| FG
    FG -->|"Explicit proxy / internet breakout"| INTERNET

    EP -->|"Other traffic"| POP
    POP -->|"Normal FortiSASE egress"| INTERNET
:::
