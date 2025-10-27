# Use Case 3: Large-Scale Automated Updates

## Overview
This use case demonstrates automating configuration updates across hundreds of network devices simultaneously, saving time and reducing human error.

## Problem Statement
Organizations need to update network device configurations for:
- **Security patches**: Critical vulnerabilities require immediate fixes
- **Policy changes**: New security requirements must be applied everywhere
- **Feature rollouts**: New services need consistent configuration
- **Compliance updates**: Regulatory changes affect all devices

Manual updates are:
- **Time-consuming**: 20+ minutes per device × 200 devices = 66+ hours
- **Error-prone**: Typos, missed steps, inconsistencies
- **Risky**: No systematic backup or verification
- **Unscalable**: Can't keep up with infrastructure growth

## Solution
Ansible automates large-scale updates with:
- **Parallel execution**: Update multiple devices simultaneously
- **Automatic backups**: Safe rollback if issues occur
- **Error handling**: Skip failed devices, continue with others
- **Comprehensive logging**: Track every change
- **Post-update validation**: Verify successful deployment

## Key Features

### Intelligent Batching (Serial Execution)
```yaml
serial: 10  # Process 10 devices at a time
```
- Controls parallelism to avoid overwhelming network
- Balances speed with safety
- Different batch sizes for different phases

### Automatic Backup
```yaml
- name: Backup current configuration
  cisco.ios.ios_config:
    backup: yes
```
- Every device backed up before changes
- Timestamped backups for easy recovery
- No changes without backup

### Error Resilience
```yaml
ignore_errors: yes
meta: end_host
```
- Unreachable devices skipped automatically
- Failed devices don't stop entire update
- Comprehensive error logging

### Real-Time Logging
```yaml
- name: Log update status
  lineinfile:
    path: "logs/update_log.txt"
    line: "{{ timestamp }} | {{ hostname }} | STATUS"
```
- Every action logged with timestamp
- Success/failure tracking per device
- Audit trail for compliance

## Execution Phases

### Phase 1: Pre-Update (Serial: 10)
1. Create backup directories
2. Check device reachability
3. Backup configurations
4. Log backup status

### Phase 2: Deploy Updates (Serial: 20)
1. Check current version
2. Apply security patches
3. Update SNMP configuration
4. Configure NTP authentication
5. Apply ACL updates
6. Enhance logging
7. Verify changes
8. Log update status

### Phase 3: Post-Update (Serial: 30)
1. Verify connectivity
2. Run health checks
3. Generate device reports
4. Create summary

## Updates Deployed

This playbook applies multiple security and operational updates:

1. **Enhanced Password Policies**
   - Minimum length: 14 characters
   - Account lockout: 5 attempts in 120 seconds
   - Lockout duration: 300 seconds

2. **SSH Hardening**
   - SSH version 2 only
   - 60-second timeout
   - Disabled HTTP/HTTPS services

3. **SNMP Security**
   - Removed default community strings
   - Timestamped secure community
   - SNMPv3 group configuration

4. **NTP Authentication**
   - Enabled NTP authentication
   - Configured authentication keys
   - Trusted key configuration

5. **Management ACLs**
   - Restricted management access
   - Logging for denied attempts

6. **Enhanced Logging**
   - Increased buffer size
   - Appropriate logging levels
   - Source interface configuration

## Usage

### Prerequisites
```bash
# Install Ansible
pip install ansible

# Install Cisco IOS collection
ansible-galaxy collection install cisco.ios
```

### Running the Update

```bash
# Full update with all phases
ansible-playbook -i inventory/large_scale_inventory.yml playbooks/large_scale_update.yml

# Dry run (check mode)
ansible-playbook -i inventory/large_scale_inventory.yml playbooks/large_scale_update.yml --check

# Update only specific components
ansible-playbook -i inventory/large_scale_inventory.yml playbooks/large_scale_update.yml --tags security,snmp

# Limit to specific device group
ansible-playbook -i inventory/large_scale_inventory.yml playbooks/large_scale_update.yml --limit dc1_routers

# Verbose output for troubleshooting
ansible-playbook -i inventory/large_scale_inventory.yml playbooks/large_scale_update.yml -vvv
```

### Monitoring Progress

```bash
# Watch update log in real-time
tail -f logs/update_log_$(date +%Y-%m-%d).txt

# Check summary after completion
cat logs/executive_summary_$(date +%Y-%m-%d).txt

# Review individual device reports
ls -l logs/*_update_report.txt
```

## Performance Metrics

### Example: 200 Device Update

**Manual Approach:**
- Time per device: 20 minutes
- Total time: 66.7 hours
- Error rate: 5-10%
- Consistency: Variable

**Automated Approach:**
- Time per batch: ~5 minutes
- Total batches: ~10 (serial: 20)
- Total time: ~50 minutes
- Error rate: <1%
- Consistency: 100%

**Time Savings: ~65 hours (98% faster)**

## Safety Features

### 1. Pre-Update Backups
Every device configuration backed up before any changes.

### 2. Batched Execution
Serial processing prevents overwhelming the network or control plane.

### 3. Reachability Checks
Unreachable devices skipped automatically.

### 4. Change Verification
Post-update commands verify successful application.

### 5. Health Checks
Comprehensive validation ensures devices are functioning.

### 6. Detailed Logging
Complete audit trail for troubleshooting and compliance.

## Real-World Scenarios

### Scenario 1: Emergency Security Patch
**Situation**: Critical vulnerability discovered, must patch 500 devices immediately.

**Solution**:
```bash
# Update security configurations on all devices
ansible-playbook -i production_inventory.yml playbooks/large_scale_update.yml --tags security

# Results: 500 devices patched in ~2 hours instead of 166 hours manually
```

### Scenario 2: Quarterly Compliance Update
**Situation**: New compliance requirements for password policies and logging.

**Solution**:
```bash
# Apply compliance updates
ansible-playbook -i inventory.yml playbooks/large_scale_update.yml --tags password,logging

# Results: Consistent compliance across all devices, complete audit trail
```

### Scenario 3: Infrastructure Modernization
**Situation**: Migrate from SNMPv2 to SNMPv3 across entire network.

**Solution**:
```bash
# Update SNMP configuration
ansible-playbook -i inventory.yml playbooks/large_scale_update.yml --tags snmp

# Results: Seamless migration with zero manual configuration
```

## Benefits Summary

| Aspect | Manual | Automated | Improvement |
|--------|--------|-----------|-------------|
| **Time** | 66 hours | 1 hour | 98% faster |
| **Errors** | 5-10% | <1% | 90%+ reduction |
| **Consistency** | Variable | 100% | Perfect |
| **Audit Trail** | Manual notes | Automatic logs | Complete |
| **Rollback** | Difficult | Simple | Quick recovery |
| **Scalability** | Limited | Unlimited | Grows with infrastructure |

## Best Practices

1. **Test in Lab First**: Always test updates on non-production devices
2. **Use Check Mode**: Verify changes before applying
3. **Start Small**: Begin with small batches, increase as confidence grows
4. **Monitor Closely**: Watch logs during initial deployments
5. **Keep Backups**: Retain backups for rollback capability
6. **Document Changes**: Maintain change log for all updates
7. **Schedule Wisely**: Run during maintenance windows when possible
8. **Verify Thoroughly**: Always run post-update validation

## Troubleshooting

### Device Unreachable
Check logs for connectivity issues:
```bash
grep "FAILED" logs/update_log_*.txt
```

### Update Failed on Specific Device
Review device-specific report:
```bash
cat logs/<hostname>_update_report.txt
```

### Rollback Required
Use pre-update backup:
```bash
# Copy backup to device
scp backups/<date>/<hostname>_pre_update.cfg admin@<device>:
```

## File Structure

```
use-case-3-automation/
├── inventory/
│   └── large_scale_inventory.yml    # 200 device inventory
├── playbooks/
│   └── large_scale_update.yml       # Main update playbook
├── templates/
│   └── update_template.md           # Configuration reference
├── logs/                            # Generated logs
│   ├── update_log_YYYY-MM-DD.txt   # Main update log
│   ├── *_update_report.txt         # Device reports
│   └── executive_summary_*.txt      # Summary reports
├── backups/                         # Configuration backups
│   └── YYYY-MM-DD/                 # Dated backup directory
└── README.md                        # This file
```
