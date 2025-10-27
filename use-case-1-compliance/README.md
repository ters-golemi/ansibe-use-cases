# Use Case 1: Compliance Configuration Management

## Overview
This use case demonstrates how to deploy consistent configurations across many network devices to maintain compliance with security and operational standards.

## Problem Statement
Organizations with large network infrastructures need to ensure all devices adhere to:
- Security standards (passwords, SSH, banners)
- Operational requirements (NTP, logging, SNMP)
- Regulatory compliance (audit trails, access controls)

Manual configuration is error-prone and doesn't scale.

## Solution
Ansible automates the deployment of standardized configurations across all network devices, ensuring:
- **Consistency**: Same configuration applied to all devices
- **Compliance**: Enforces security and operational policies
- **Auditability**: Configuration changes are tracked and documented
- **Scalability**: Works across hundreds or thousands of devices

## Components

### Inventory (`inventory/hosts.yml`)
Defines all network devices organized by type (routers, switches) with connection details.

### Playbook (`playbooks/enforce_compliance.yml`)
Main playbook that enforces compliance standards:
- NTP configuration for time synchronization
- Syslog for centralized logging
- Security banners
- Password policies
- SNMP settings
- Disabling unused services
- SSH hardening

### Configuration Reports (`configs/`)
Generated reports documenting compliance status for each device.

## Usage

### Prerequisites
```bash
# Install required Ansible collections
ansible-galaxy collection install cisco.ios
```

### Running the Playbook
```bash
# Enforce compliance on all devices
ansible-playbook -i inventory/hosts.yml playbooks/enforce_compliance.yml

# Enforce specific compliance items using tags
ansible-playbook -i inventory/hosts.yml playbooks/enforce_compliance.yml --tags ntp,syslog

# Run in check mode (dry run)
ansible-playbook -i inventory/hosts.yml playbooks/enforce_compliance.yml --check

# Verify compliance only
ansible-playbook -i inventory/hosts.yml playbooks/enforce_compliance.yml --tags verify
```

## Benefits
1. **Reduced Errors**: Eliminates manual configuration mistakes
2. **Time Savings**: Configure 100+ devices in minutes instead of days
3. **Consistency**: Guaranteed uniform configuration across all devices
4. **Audit Trail**: All changes documented and traceable
5. **Quick Updates**: Policy changes deployed instantly network-wide

## Real-World Scenarios
- **New Security Policy**: Deploy new password requirements across all devices
- **Compliance Audit**: Verify and enforce standards before audit
- **Post-Breach Recovery**: Quickly restore secure configurations
- **Onboarding New Devices**: Apply full compliance configuration automatically
