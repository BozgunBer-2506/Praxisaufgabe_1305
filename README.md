# Praxisaufgabe 1305 - CI/CD Pipeline mit Slack-Benachrichtigung

Dieses Repository enthält zwei GitHub Actions Workflows für automatisierte Deployments auf EC2-Server mit Slack-Benachrichtigungen.

## Projektstruktur

```
.github/
  workflows/
    deploy-staging.yml   # Staging-Pipeline
    deploy-prod.yml      # Production-Pipeline
frontend/
  index.html             # Statische Webseite
```

## Workflows

### Staging (`deploy-staging.yml`)

- **Trigger:** Jeder Push auf den `main`-Branch
- **Ablauf:**
  1. Slack-Startmeldung mit `github.actor` und `github.workflow`
  2. Berechtigungen auf dem Server setzen (`sudo chown`)
  3. `frontend/index.html` per SCP auf den Staging-Server übertragen
  4. Slack-Erfolgsmeldung oder Slack-Fehlermeldung je nach Ergebnis

### Production (`deploy-prod.yml`)

- **Trigger:** Push eines Git-Tags mit `v*` (z.B. `v1.0.0`)
- **Ablauf:**
  1. Slack-Startmeldung
  2. Bash-Schleife über alle IPs in `EC2_IPS_PROD`
  3. SSH-Key schreiben, `ssh-keyscan` und `scp` für jeden Server
  4. Slack-Erfolgsmeldung oder Slack-Fehlermeldung je nach Ergebnis

### Production-Deployment auslösen

```bash
git tag v1.0.0
git push origin v1.0.0
```

## GitHub Secrets

| Secret | Beschreibung |
|--------|-------------|
| `SSH_PRIVATE_KEY` | Privater SSH-Schlüssel für den Zugriff auf die EC2-Instanzen |
| `EC2_IP_STAGING` | IP-Adresse des Staging-Servers |
| `EC2_IPS_PROD` | IP-Adressen der Production-Server (leerzeichen-getrennt) |
| `SLACK_WEBHOOK_URL` | Webhook-URL für Slack-Benachrichtigungen |
