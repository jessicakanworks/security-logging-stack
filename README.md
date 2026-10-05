# security-logging-stack

Centralized logging and security monitoring: **Wazuh** (host intrusion detection, file integrity,
vulnerability detection, compliance checks) and **OpenSearch** (syslog, NetFlow and application log
aggregation with dashboards). Deployable with Docker Compose; Ansible enrols hosts and network devices.

```
Linux hosts ── wazuh-agent ─┐
Network devices ── syslog ──┼──► wazuh-manager ──► wazuh-indexer (OpenSearch) ──► wazuh-dashboard
Routers ── NetFlow (2055) ──┤                                   ▲
App servers ── Filebeat ────┴──► logstash/opensearch pipeline ──┘
```

* `docker-compose.yml` — manager, indexer, dashboard, Filebeat/Logstash pipeline, NetFlow collector
* `ansible/enrol-hosts.yml` — installs and registers the Wazuh agent on Linux hosts
* `ansible/network-syslog.yml` — points Cisco/Juniper devices at the collector
* `rules/local_rules.xml` — custom detections (SSH brute force burst, sudo abuse, config change on network device)
* `retention/ism-policy.json` — 90-day hot / 365-day cold index lifecycle

```bash
cp .env.example .env && docker compose up -d
ansible-playbook -i ../inventory ansible/enrol-hosts.yml
```
