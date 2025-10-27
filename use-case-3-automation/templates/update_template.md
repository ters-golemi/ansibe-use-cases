# Device Update Template
# This file shows what configuration will be applied during updates

## Security Hardening Updates

### Password Policy
```
service password-encryption
security passwords min-length 14
login block-for 300 attempts 5 within 120
```

### SSH Hardening
```
ip ssh version 2
ip ssh time-out 60
no ip http server
no ip http secure-server
```

### SNMP Security
```
no snmp-server community public
snmp-server community SecureComm<timestamp> RO
snmp-server group SecureGroup v3 priv
```

### NTP Authentication
```
ntp authenticate
ntp authentication-key 1 md5 <key>
ntp trusted-key 1
ntp server 10.0.0.1 key 1
ntp server 10.0.0.2 key 1
```

### Management ACL
```
access-list 99 remark Management Access Control
access-list 99 permit 10.0.0.0 0.255.255.255
access-list 99 deny any log
```

### Enhanced Logging
```
logging buffered 100000
logging console critical
logging monitor informational
logging trap notifications
logging facility local6
logging source-interface Loopback0
```

## Update Process

1. **Pre-Update Phase**
   - Check device reachability
   - Backup current configuration
   - Log backup status

2. **Update Phase**
   - Apply security configurations
   - Update SNMP settings
   - Configure NTP authentication
   - Apply ACL updates
   - Enhance logging
   - Verify changes

3. **Post-Update Phase**
   - Verify connectivity
   - Run health checks
   - Generate reports
   - Create summary
