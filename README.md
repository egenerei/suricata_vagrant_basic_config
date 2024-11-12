---

# Suricata Setup with ICMP Rules

This repository provides configuration files and a `Vagrantfile` for setting up Suricata with custom ICMP rules in a virtualized environment.

## Files Included

- **`icmp.rules`**: Custom Suricata rules file focused on ICMP traffic (e.g., ping requests, ICMP redirects).
- **`suricata.yaml`**: Configuration file for Suricata, customized to include the provided ICMP rules and optimize monitoring.
- **`Vagrantfile`**: A Vagrant configuration for setting up a virtual machine with Suricata installed, providing a sandbox environment for rule testing and development.

## Prerequisites

- **Vagrant** and **VirtualBox** (or any other compatible provider for Vagrant) are required for running the virtual machine.
- **Suricata** installed on the VM (configured in the Vagrant provisioning steps).

## Setup Instructions

### 1. Clone the Repository

Clone this repository to your local machine:

```bash
git clone https://github.com/egenerei/suricata_vagrant_basic_config.git
cd suricata_vagrant_basic_config
```

### 2. Start the Vagrant Environment

Run the following command to start the virtual machine:

```bash
vagrant up
```

This command uses the `Vagrantfile` to set up a virtual machine and automatically install and configure Suricata, along with copying the `icmp.rules` and `suricata.yaml` files into the VM.

### 3. Access the Virtual Machine

Once the VM is running, connect to it via SSH:

```bash
vagrant ssh
```

### 4. Configure Suricata

Inside the VM, ensure that `suricata.yaml` is configured to include the `icmp.rules` file. Confirm that the `rule-files` section in `suricata.yaml` contains:

```yaml
rule-files:
  - icmp.rules
```

This line tells Suricata to load the custom ICMP rules when starting.

### 5. Start Suricata

Run Suricata with the specified configuration:

```bash
sudo suricata -c /etc/suricata/suricata.yaml -i eth0
```

- `-c`: Specifies the path to the Suricata configuration file.
- `-i`: Specifies the network interface to monitor. (Replace `eth0` if your interface differs.)

### 6. Test ICMP Rules

You can generate ICMP traffic (such as ping requests) to test the rules. For example:

```bash
ping <target-ip>
```

Check Suricata’s alert log (e.g., `/var/log/suricata/fast.log`) to see if the ICMP traffic is detected.

### 7. Check Logs

Suricata logs alerts in the `/var/log/suricata/` directory by default. You can use the following command to view recent alerts:

```bash
tail -f /var/log/suricata/fast.log
```

## Customizing Rules

You can modify `icmp.rules` to detect different types of ICMP traffic. After editing the rules file, reload Suricata:

```bash
sudo systemctl restart suricata
```

## Troubleshooting

- **Suricata Not Starting**: Verify the paths in `suricata.yaml` and ensure Suricata has permission to read the rules file.
- **No Alerts Logged**: Ensure the network interface is correctly specified when starting Suricata and that traffic matches the ICMP rules.

---