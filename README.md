# Network Setup Instructions

This repository contains scripts for setting up and testing a custom network using Linux networking features. The network consists of a bridge, a network namespace, and a virtual Ethernet (veth) pair for testing network configuration.

## Prerequisites
- Linux system with root privileges
- `iptables` installed
- `ip` command (usually part of `iproute2` package)
- A basic understanding of network namespaces, bridges, and veth pairs

## Setup Instructions

### Clone the repository
```bash
git clone <repository_url>
cd <repository_directory>
```

### Running the setup

1. **Run the `Makefile`**
   The `Makefile` automates the setup and testing process. You can run the entire process with the following command:

   ```bash
   make
   ```

   This will:
   - Set up a custom network with a bridge and network namespace.
   - Test the network setup by pinging Google's public DNS server (8.8.8.8) from the custom namespace.

2. **Manually running the setup**

   If you prefer running the setup manually, you can execute the scripts directly:
   
   - **Setup the network:**

     ```bash
     bash scripts/setup_network.sh
     ```

     This script will:
     - Create a bridge network (`br0`).
     - Create a custom network namespace (`custom_ns`).
     - Set up a veth pair (`veth0` and `veth1`) and link them to the bridge and the namespace.
     - Assign IP addresses to the interfaces.
     - Set up IP forwarding and NAT for outgoing traffic.

   - **Test the network:**

     After setting up the network, you can test the connectivity from the custom namespace by running the following:

     ```bash
     bash scripts/test_ping.sh
     ```

     This script will test egress networking by pinging `8.8.8.8` from the custom namespace.

### Cleanup

To clean up the network setup and remove all the created resources (bridge, namespace, etc.), you can run the `clean` target from the Makefile:

```bash
make clean
```

Alternatively, you can manually clean up by running the following commands:

```bash
ip netns delete custom_ns || true
ip link delete veth0 || true
ip link delete br0 || true
```

### Troubleshooting

- Ensure you have the necessary privileges (root or `sudo`) to run the network setup commands.
- If `sysctl` or `iptables` commands fail, ensure the required modules are loaded on your system.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
