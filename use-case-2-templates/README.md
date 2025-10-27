# Use Case 2: Template-Based Configuration Customization

## Overview
This use case demonstrates how to use Jinja2 templates to customize network device configurations based on device type, location, and specific requirements.

## Problem Statement
Different network devices need different configurations:
- **Location-specific**: NTP, DNS, syslog servers vary by region
- **Device-type-specific**: Routers need BGP, switches need VLANs
- **Role-specific**: Core vs. edge vs. access layer have different needs
- **Instance-specific**: Each device has unique IP addresses and interfaces

Managing hundreds of unique configuration files manually is impractical.

## Solution
Use Ansible templates with Jinja2 to:
- Define configuration structure once in templates
- Automatically populate values from inventory variables
- Generate custom configurations for each device
- Maintain consistency while allowing customization

## Components

### Inventory Structure
```
inventory/hosts.yml          # Device definitions with metadata
group_vars/
  ├── nyc.yml               # NYC location variables
  ├── london.yml            # London location variables
  └── tokyo.yml             # Tokyo location variables
host_vars/
  ├── core-router-nyc-01.yml    # Host-specific variables
  └── access-switch-nyc-01.yml  # Host-specific variables
```

### Templates
- `templates/router_config.j2`: Template for routers (core and edge)
- `templates/switch_config.j2`: Template for access switches

### Playbook
- `playbooks/deploy_templated_configs.yml`: Generates and deploys configurations

## Key Features

### Location-Based Customization
Each location has unique settings:
- **NYC**: Timezone America/New_York, servers 10.10.x.x, VLANs 100-199
- **London**: Timezone Europe/London, servers 10.20.x.x, VLANs 200-299
- **Tokyo**: Timezone Asia/Tokyo, servers 10.30.x.x, VLANs 300-399

### Device-Type Customization
Templates include conditional sections:
```jinja2
{% if device_role == 'core' %}
! BGP Configuration
router bgp {{ bgp_as }}
{% endif %}
```

### Host-Specific Customization
Individual devices get unique settings:
- Interface configurations
- IP addresses
- BGP router IDs
- Static routes

## Usage

### Prerequisites
```bash
# Install Ansible
pip install ansible

# Install Cisco IOS collection
ansible-galaxy collection install cisco.ios
```

### Generate Configurations
```bash
# Generate all device configurations
ansible-playbook -i inventory/hosts.yml playbooks/deploy_templated_configs.yml

# Generate only router configurations
ansible-playbook -i inventory/hosts.yml playbooks/deploy_templated_configs.yml --tags routers

# Generate only switch configurations
ansible-playbook -i inventory/hosts.yml playbooks/deploy_templated_configs.yml --tags switches

# Generate configurations and summaries
ansible-playbook -i inventory/hosts.yml playbooks/deploy_templated_configs.yml --tags summary,report
```

### View Generated Configurations
```bash
# View generated configs
ls -l configs/
cat configs/core-router-nyc-01_config.txt
```

## Benefits

1. **DRY Principle**: Single template for all devices of same type
2. **Scalability**: Add new locations by creating new group_vars file
3. **Maintainability**: Update template once, affects all devices
4. **Consistency**: Guaranteed structure across all configurations
5. **Flexibility**: Location, device, and host-level customization
6. **Documentation**: Templates serve as configuration documentation

## Real-World Scenarios

### Scenario 1: New Location Deployment
To add a new Singapore datacenter:
1. Create `group_vars/singapore.yml` with location settings
2. Add Singapore devices to inventory
3. Run playbook - configurations auto-generated!

### Scenario 2: Organization-Wide Policy Change
To update NTP servers globally:
1. Update NTP servers in each location's group_vars
2. Re-run playbook
3. All device configs updated with new NTP servers

### Scenario 3: New Device Type
To support new device type:
1. Create new template (e.g., `firewall_config.j2`)
2. Add conditional logic to playbook
3. Define device-specific variables

## Template Variables Reference

### Group Variables (Location-Specific)
- `location_name`: Full location name
- `location_code`: Short code (NYC, LON, TYO)
- `timezone`: IANA timezone string
- `ntp_servers`: List of NTP servers
- `syslog_servers`: List of syslog servers
- `dns_servers`: List of DNS servers
- `snmp_location`: SNMP location string
- `snmp_contact`: SNMP contact email
- `network_prefix`: IP prefix for location
- `vlan_range_start`: Starting VLAN ID
- `vlan_range_end`: Ending VLAN ID

### Host Variables (Device-Specific)
- `bgp_as`: BGP AS number (routers)
- `bgp_router_id`: BGP router ID (routers)
- `interfaces`: List of interface configurations
- `static_routes`: List of static routes (routers)
- `vlans`: List of VLANs (switches)
- `spanning_tree_priority`: STP priority (switches)
- `floor`: Floor number (switches)
