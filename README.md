# Praxisaufgabe 1305 - CI/CD Pipeline mit Slack

Zwei GitHub Actions Workflows für automatisierte Deployments auf EC2 mit Slack-Benachrichtigung.

## Was ist hier drin

```
.github/workflows/deploy-staging.yml   -> Staging-Pipeline
.github/workflows/deploy-prod.yml      -> Production-Pipeline
frontend/index.html                    -> die Webseite
```

## Wie funktioniert es

**Staging** läuft automatisch bei jedem Push auf `main`. Der Workflow schickt eine Startnachricht an Slack, kopiert `frontend/index.html` per SCP auf den Server und meldet dann Erfolg oder Fehler.

**Production** wird nur ausgelöst, wenn ein Tag wie `v1.0.0` gepusht wird. Dann läuft ein Bash-Loop über alle Production-IPs und deployed auf jeden Server einzeln.

Production manuell starten:
```bash
git tag v1.0.0
git push origin v1.0.0
```

## Secrets die gesetzt werden müssen

| Secret | Wozu |
|--------|------|
| `SSH_PRIVATE_KEY` | SSH-Zugriff auf die Server |
| `EC2_IP_STAGING` | IP vom Staging-Server |
| `EC2_IPS_PROD` | IPs der Prod-Server, leerzeichen-getrennt |
| `SLACK_WEBHOOK_URL` | für die Slack-Nachrichten |
