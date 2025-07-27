# Runbook: Database Failover Procedure

## Purpose

This runbook provides step-by-step instructions for performing a manual failover of the primary PostgreSQL database to a standby replica in case of primary database failure or during planned maintenance.

## Scope

- Applies to: Production PostgreSQL database cluster
- Services affected: All customer-facing applications, internal admin tools, and reporting systems
- Prerequisites: Admin access to database servers and monitoring systems

## Prerequisites

- SSH access to database servers
- PostgreSQL admin credentials
- Access to monitoring dashboard
- Slack access to #db-ops and #incident-response channels
- PagerDuty access for managing alerts

## Procedure

### 1. Assess the Situation

1. Verify that a failover is necessary by checking:
   - Database health metrics in Grafana dashboard: [DB Health Dashboard](https://grafana.example.com/dashboards/db-health)
   - Recent alerts in PagerDuty
   - Error rates in application logs

2. If the primary database is:
   - **Unresponsive**: Proceed with emergency failover (Section 3)
   - **Degraded but functional**: Proceed with controlled failover (Section 2)

3. Create an incident ticket in Jira with the template "DB Failover" and note the incident ID: `INC-XXXXX`

4. Notify stakeholders in Slack #incident-response channel:
   ```
   @here Initiating database failover procedure due to [reason]. 
   Incident ticket: INC-XXXXX
   Expected downtime: 3-5 minutes
   Updates will be posted in this thread.
   ```

### 2. Controlled Failover Procedure

1. **Prepare the standby**
   ```bash
   # Connect to standby server
   ssh db-standby.example.com
   
   # Check replication status
   sudo -u postgres psql -c "SELECT pg_last_wal_receive_lsn(), pg_last_wal_replay_lsn(), pg_last_xact_replay_timestamp();"
   ```
   
   Verify that:
   - The standby is not lagging (both LSN values should be close)
   - Last replay timestamp is within 30 seconds of current time

2. **Pause application traffic**
   ```bash
   # Connect to application server
   ssh app-server.example.com
   
   # Enable maintenance mode
   cd /opt/application
   ./scripts/maintenance-mode.sh enable
   ```

3. **Promote the standby to primary**
   ```bash
   # Connect to standby server
   ssh db-standby.example.com
   
   # Promote standby to primary
   sudo -u postgres pg_ctl promote -D /var/lib/postgresql/data
   ```

4. **Verify promotion success**
   ```bash
   # Check if server is in primary mode
   sudo -u postgres psql -c "SELECT pg_is_in_recovery();"
   ```
   
   Expected output: `f` (false)

5. **Update connection strings**
   ```bash
   # Connect to configuration server
   ssh config-server.example.com
   
   # Update database pointer
   cd /opt/config-service
   ./scripts/update-db-pointer.sh --primary db-standby.example.com
   ```

6. **Resume application traffic**
   ```bash
   # Connect to application server
   ssh app-server.example.com
   
   # Disable maintenance mode
   cd /opt/application
   ./scripts/maintenance-mode.sh disable
   ```

7. **Configure old primary as new standby** (once it's available)
   ```bash
   # Connect to old primary server
   ssh db-primary.example.com
   
   # Stop PostgreSQL
   sudo systemctl stop postgresql
   
   # Reinitialize as standby
   sudo -u postgres pg_basebackup -h db-standby.example.com -D /var/lib/postgresql/data -P -U replication -R
   
   # Start PostgreSQL
   sudo systemctl start postgresql
   ```

### 3. Emergency Failover Procedure

1. **Promote the standby immediately**
   ```bash
   # Connect to standby server
   ssh db-standby.example.com
   
   # Promote standby to primary
   sudo -u postgres pg_ctl promote -D /var/lib/postgresql/data
   ```

2. **Update connection strings**
   ```bash
   # Connect to configuration server
   ssh config-server.example.com
   
   # Update database pointer
   cd /opt/config-service
   ./scripts/update-db-pointer.sh --primary db-standby.example.com --emergency
   ```

3. **Restart application services**
   ```bash
   # Connect to application server
   ssh app-server.example.com
   
   # Restart services
   cd /opt/application
   ./scripts/restart-services.sh --all
   ```

4. **Follow up with controlled procedure steps** 4-7 once immediate service is restored

### 4. Verification

1. **Check database connectivity**
   ```bash
   # From application server
   ssh app-server.example.com
   
   # Test connection
   cd /opt/application
   ./scripts/db-connectivity-test.sh
   ```

2. **Verify application functionality**
   - Navigate to [Health Check Dashboard](https://monitor.example.com/health)
   - Verify all services show "Connected" status for database
   - Perform a test transaction through the admin interface

3. **Check replication** (if old primary is now standby)
   ```bash
   # Connect to new standby
   ssh db-primary.example.com
   
   # Check replication status
   sudo -u postgres psql -c "SELECT pg_last_wal_receive_lsn(), pg_last_wal_replay_lsn();"
   ```

### 5. Post-Failover Tasks

1. **Update documentation**
   - Update the database inventory document with new primary/standby roles
   - Document the reason for failover in the incident ticket

2. **Adjust monitoring**
   ```bash
   # Connect to monitoring server
   ssh monitor.example.com
   
   # Update monitoring configuration
   cd /opt/monitoring
   ./scripts/update-db-targets.sh
   ```

3. **Notify stakeholders of completion**
   ```
   @here Database failover completed successfully.
   - New primary: db-standby.example.com
   - New standby: db-primary.example.com
   - Total downtime: X minutes
   - All services operational
   ```

## Troubleshooting

### Standby Won't Promote

**Symptoms**: `pg_ctl promote` command fails or standby remains in recovery mode

**Resolution**:
1. Check PostgreSQL logs:
   ```bash
   sudo -u postgres cat /var/lib/postgresql/data/log/postgresql-*.log | tail -100
   ```

2. Verify standby is properly configured:
   ```bash
   sudo -u postgres cat /var/lib/postgresql/data/postgresql.conf | grep hot_standby
   ```
   Expected: `hot_standby = on`

3. If promotion still fails, try restarting PostgreSQL:
   ```bash
   sudo systemctl restart postgresql
   ```

### Applications Can't Connect After Failover

**Symptoms**: Applications report database connection errors after failover

**Resolution**:
1. Verify connection string updates were applied:
   ```bash
   # On application server
   cat /opt/application/config/database.yml
   ```

2. Check if PostgreSQL is accepting connections:
   ```bash
   # On database server
   sudo -u postgres psql -c "SELECT count(*) FROM pg_stat_activity;"
   ```

3. Verify firewall rules:
   ```bash
   # On database server
   sudo iptables -L | grep 5432
   ```

## Related Resources

- [PostgreSQL High Availability Documentation](https://www.postgresql.org/docs/current/high-availability.html)
- [Internal Wiki: Database Architecture](https://wiki.example.com/dba/architecture)
- [Monitoring Dashboard](https://grafana.example.com/dashboards/db-performance)
- [Incident Response Playbook](https://wiki.example.com/incidents/playbooks/database)

## Revision History

| Date | Author | Changes |
|------|--------|---------|
| 2023-05-15 | Jane Smith | Initial version |
| 2023-06-22 | Carlos Rodriguez | Added emergency procedure |
| 2023-07-10 | Priya Patel | Updated verification steps |
| 2023-08-03 | Jane Smith | Added troubleshooting section |

[Back to Table of Contents](/README.md)