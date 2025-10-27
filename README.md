# Ansible Network Configuration Management Use Cases

This repository demonstrates practical use cases of configuration management with Ansible in real network environments. Each use case shows how Ansible solves common network operations challenges with automation, consistency, and efficiency.

## 📋 Use Cases Overview

### [Use Case 1: Compliance Configuration Enforcement](use-case-1-compliance/)
**Deploy consistent configurations across many devices to maintain compliance**

Demonstrates how to:
- Apply standardized security configurations across all network devices
- Enforce organizational policies (passwords, SSH, banners, logging)
- Maintain compliance with regulatory requirements
- Generate compliance reports and audit trails

**Key Benefits:**
- ✅ Eliminates configuration drift
- ✅ Ensures 100% policy compliance
- ✅ Reduces manual errors
- ✅ Provides complete audit trail

[📖 View Use Case 1 Details →](use-case-1-compliance/README.md)

---

### [Use Case 2: Template-Based Configuration Customization](use-case-2-templates/)
**Use templates to customize settings based on device type or location**

Demonstrates how to:
- Create reusable Jinja2 templates for different device types
- Customize configurations based on location (NYC, London, Tokyo)
- Define device-role-specific configurations (core, edge, access)
- Manage host-specific variables (interfaces, IPs, VLANs)

**Key Benefits:**
- ✅ Single template supports multiple locations
- ✅ DRY principle - define once, use everywhere
- ✅ Easy to add new locations or device types
- ✅ Consistent structure with flexible values

[📖 View Use Case 2 Details →](use-case-2-templates/README.md)

---

### [Use Case 3: Large-Scale Automated Updates](use-case-3-automation/)
**Automate large-scale updates to save time and reduce errors**

Demonstrates how to:
- Update hundreds of devices simultaneously with parallel execution
- Apply security patches and configuration changes at scale
- Implement automatic backups before changes
- Use comprehensive logging and error handling
- Perform post-update validation and health checks

**Key Benefits:**
- ✅ 98% faster than manual updates (66 hours → 1 hour for 200 devices)
- ✅ Error rate reduced from 5-10% to <1%
- ✅ Complete automation with safety measures
- ✅ Real-time logging and monitoring

[📖 View Use Case 3 Details →](use-case-3-automation/README.md)

---

### [Use Case 4: Quick Configuration Rollback](use-case-4-rollback/)
**Roll back changes quickly to fix problems and maintain uptime**

Demonstrates how to:
- Create automated configuration backups (running & startup)
- Perform quick rollback to previous configurations
- Implement safety backups before rollback
- Verify connectivity and configuration post-rollback
- Maintain comprehensive audit trail of all changes

**Key Benefits:**
- ✅ 80-90% faster recovery (15-30 min → 2-3 min per device)
- ✅ Safety backups prevent data loss
- ✅ Complete documentation of all actions
- ✅ Repeatable and reliable recovery process

[📖 View Use Case 4 Details →](use-case-4-rollback/README.md)

---

## 🚀 Getting Started

### Prerequisites

1. **Install Ansible**
   ```bash
   pip install ansible
   ```

2. **Install Cisco IOS Collection** (required for network modules)
   ```bash
   ansible-galaxy collection install cisco.ios
   ```

3. **Configure Inventory**
   - Update inventory files with your actual device IPs and credentials
   - Consider using Ansible Vault for sensitive data:
     ```bash
     ansible-vault create secrets.yml
     ```

### Quick Start Examples

```bash
# Use Case 1: Enforce compliance on all devices
cd use-case-1-compliance
ansible-playbook -i inventory/hosts.yml playbooks/enforce_compliance.yml

# Use Case 2: Generate templated configurations
cd use-case-2-templates
ansible-playbook -i inventory/hosts.yml playbooks/deploy_templated_configs.yml

# Use Case 3: Execute large-scale update
cd use-case-3-automation
ansible-playbook -i inventory/large_scale_inventory.yml playbooks/large_scale_update.yml

# Use Case 4: Create backup of all devices
cd use-case-4-rollback
ansible-playbook -i inventory/hosts.yml playbooks/backup_configurations.yml
```

## 📁 Repository Structure

```
ansibe-use-cases/
├── README.md                          # This file
├── use-case-1-compliance/             # Compliance enforcement
│   ├── README.md
│   ├── inventory/
│   │   └── hosts.yml
│   ├── playbooks/
│   │   └── enforce_compliance.yml
│   ├── templates/
│   └── configs/                       # Generated reports
├── use-case-2-templates/              # Template-based configs
│   ├── README.md
│   ├── inventory/
│   │   └── hosts.yml
│   ├── group_vars/                    # Location-specific variables
│   │   ├── nyc.yml
│   │   ├── london.yml
│   │   └── tokyo.yml
│   ├── host_vars/                     # Device-specific variables
│   ├── playbooks/
│   │   └── deploy_templated_configs.yml
│   ├── templates/                     # Jinja2 templates
│   │   ├── router_config.j2
│   │   └── switch_config.j2
│   └── configs/                       # Generated configs
├── use-case-3-automation/             # Large-scale updates
│   ├── README.md
│   ├── inventory/
│   │   └── large_scale_inventory.yml  # 200 devices
│   ├── playbooks/
│   │   └── large_scale_update.yml
│   ├── templates/
│   ├── logs/                          # Update logs
│   └── backups/                       # Pre-update backups
└── use-case-4-rollback/               # Rollback capabilities
    ├── README.md
    ├── inventory/
    │   └── hosts.yml
    ├── playbooks/
    │   ├── backup_configurations.yml
    │   └── rollback_configuration.yml
    ├── backups/                       # Configuration backups
    └── configs/                       # Rollback reports
```

## 🎯 Use Case Selection Guide

Choose the appropriate use case based on your needs:

| Scenario | Recommended Use Case |
|----------|---------------------|
| Need to apply security policies across all devices | Use Case 1: Compliance |
| Managing multiple datacenters with different settings | Use Case 2: Templates |
| Emergency security patch for 100+ devices | Use Case 3: Automation |
| Configuration change failed, need quick recovery | Use Case 4: Rollback |
| New device deployment with standard config | Use Case 1 + 2 |
| Quarterly compliance audit preparation | Use Case 1 + 4 |
| Infrastructure modernization project | Use Case 2 + 3 |
| Change management with safety net | Use Case 3 + 4 |

## 💡 Best Practices

### 1. Version Control
- Keep all playbooks, inventories, and templates in Git
- Use meaningful commit messages
- Tag releases for important milestones

### 2. Testing Strategy
- Always test in lab environment first
- Use `--check` mode for dry runs
- Start with small device groups before scaling

### 3. Security
- Use Ansible Vault for sensitive data
- Implement role-based access control
- Audit all configuration changes
- Maintain secure backup storage

### 4. Documentation
- Document all customizations
- Keep inventory up-to-date
- Maintain runbooks for common tasks
- Track lessons learned

### 5. Change Management
- Create backup before changes (Use Case 4)
- Apply changes during maintenance windows
- Monitor devices during updates
- Have rollback plan ready

### 6. Continuous Improvement
- Review automation regularly
- Optimize playbooks based on experience
- Gather feedback from team
- Expand automation coverage

## 🔒 Security Considerations

### Credential Management
```bash
# Create encrypted vault for credentials
ansible-vault create group_vars/all/vault.yml

# Add credentials
vault_password: YourSecurePassword
vault_snmp_community: SecureSNMPString

# Use in playbooks
ansible_password: "{{ vault_password }}"
```

### SSH Key Authentication (Preferred)
```yaml
# In inventory
ansible_connection: network_cli
ansible_network_os: ios
ansible_user: automation
ansible_ssh_private_key_file: ~/.ssh/network_automation_key
```

### Audit Trail
All use cases include comprehensive logging:
- Timestamped actions
- Success/failure tracking
- Configuration checksums
- Change documentation

## 📊 Performance & Scalability

### Execution Time Comparison

| Devices | Manual (hours) | Automated (minutes) | Time Saved |
|---------|---------------|-------------------|------------|
| 10      | 3.3           | 5-10              | 95%        |
| 50      | 16.7          | 15-20             | 97%        |
| 100     | 33.3          | 25-30             | 98%        |
| 200     | 66.7          | 45-60             | 98%        |
| 500     | 166.7         | 120-150           | 98%        |

### Scaling Considerations
- **Serial Execution**: Control parallelism with `serial: N`
- **Batching**: Process devices in manageable groups
- **Network Bandwidth**: Consider network capacity
- **Control Node**: Ensure adequate resources for large deployments

## 🛠️ Troubleshooting

### Common Issues

**Connection Failures**
```bash
# Test connectivity
ansible -i inventory/hosts.yml all -m ping

# Verbose output for debugging
ansible-playbook playbook.yml -vvv
```

**Module Not Found**
```bash
# Install required collections
ansible-galaxy collection install cisco.ios
ansible-galaxy collection install ansible.netcommon
```

**Permission Denied**
```bash
# Verify credentials
# Check enable password if using privilege escalation
ansible_become: yes
ansible_become_method: enable
ansible_become_password: "{{ vault_enable_password }}"
```

**Slow Execution**
```bash
# Increase parallelism (with caution)
ansible-playbook playbook.yml -f 50  # 50 parallel forks

# Or in playbook
serial: 20  # Process 20 at a time
```

## 📚 Additional Resources

### Ansible Documentation
- [Ansible Network Guide](https://docs.ansible.com/ansible/latest/network/index.html)
- [Cisco IOS Platform Guide](https://docs.ansible.com/ansible/latest/network/user_guide/platform_ios.html)
- [Ansible Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)

### Network Automation
- [Network to Code](https://www.networktocode.com/)
- [Ansible Network Automation](https://www.ansible.com/use-cases/network-automation)
- [Network Automation Community](https://networkautomation.forum/)

## 🤝 Contributing

To add new use cases or improve existing ones:

1. Fork the repository
2. Create a feature branch
3. Add your use case following the existing structure
4. Include comprehensive README documentation
5. Test thoroughly in lab environment
6. Submit pull request with detailed description

## 📝 License

This repository is provided as-is for educational and demonstration purposes.

## ⚠️ Disclaimer

These examples are for demonstration purposes. Always:
- Test thoroughly in non-production environments
- Customize for your specific network requirements
- Follow your organization's change management procedures
- Ensure proper authorization before making changes
- Maintain appropriate backups and rollback capabilities

## 🎓 Learning Path

**Beginners:**
1. Start with Use Case 1 (Compliance) - simple, straightforward
2. Move to Use Case 4 (Rollback) - understand safety mechanisms
3. Progress to Use Case 2 (Templates) - learn customization
4. Master Use Case 3 (Automation) - large-scale operations

**Advanced Users:**
- Combine multiple use cases for comprehensive solutions
- Customize templates for your infrastructure
- Integrate with CI/CD pipelines
- Build custom modules for specific requirements

## 📞 Support

For questions or issues:
- Review the README in each use case directory
- Check Ansible documentation
- Consult network automation communities
- Review example logs and reports

---

**Remember**: Network automation is powerful. Use responsibly, test thoroughly, and always have a rollback plan! 🚀