# Ethereum Deposit Tracker

A powerful and efficient tool for tracking and recording Ethereum (ETH) deposits to the Beacon Deposit Contract.

## Clone the Repository

```bash
git clone https://github.com/an1ket-s1ngh/luganodes-task
cd luganodes-task
```

## Create a Docker Network

```bash
docker network create grafana-net
```

## Launch the Observation Suite (For Metrics and Alerts with Grafana)

Navigate to the `observation-suite` directory:

```bash
cd observation-suite
```

Start Grafana, Prometheus, and cadvisor:

```bash
docker compose up -d
```

## Launch the Trace Engine (For Tracking)

Navigate to the `trace-engine` directory:

```bash
cd trace-engine
```

Start PostgreSQL and the tracker app:

```bash
ALCHEMY_API_KEY=<YOUR_ALCHEMY_API_KEY> docker compose up -d
```

## Set Up a Dashboard to View the Deposit Table

Go to `localhost:3001/login` and log in:

```
Email: admin
Password: admin
```

- Create a new dashboard
- Add PostgreSQL as a data source:

```
*Connection*
Host URL: postgres:5432
Database name: grafana

*Authentication*
Username: grafana
Password: grafana
```

- To add visualization, select PostgreSQL as the data source
- Choose the `deposits` table, select all columns (`*`), run the query, and switch to table view.
  
  ![image](https://github.com/user-attachments/assets/92034b25-f3de-418b-8c42-a0f38164c70d)

## Import the Cadvisor Dashboard

- Import a dashboard
- Use the dashboard ID `19792`
- For the Prometheus data source, configure a new one with the URL `http://prometheus:9090`.
- Once configured, import the cadvisor dashboard.

## Set Up Telegram Alerts

- Go to `http://localhost:3001/alerting/new/alerting`
- Name your alert "ETH Deposit Tracker BOT"

_Define your query and alert conditions:_

```sql
SELECT
  date_trunc('hour', "blockTimestamp") AS time,
  count(*) AS deposit_count
FROM deposits
GROUP BY 1
ORDER BY 1;
```

Under the section _Configure Labels and Notifications_, click on `View or create contact points`.

- Add a contact point:
  - Name: Telegram
  - Integration: Telegram

Get your Bot API Token by talking to [@BotFather](https://telegram.me/BotFather) on Telegram:
  
![{F8B6A1D5-8B0F-41B9-B998-95D8B08EAC26}](https://github.com/user-attachments/assets/f46e83c6-a04a-4e6d-8f69-d40a47e0b274)


- Create a Telegram group, add your bot to the group, and send a message to the bot.
  ![{7EA172A3-17E5-438E-9799-E9369367E4E1}](https://github.com/user-attachments/assets/9c082ca0-3bc6-4c13-b7c8-f906d0b6746c)
  
- Retrieve your chat ID by visiting:
  ```url
  https://api.telegram.org/bot<YOUR_BOT_API_TOKEN>/getUpdates
  ```
  ![image](https://github.com/user-attachments/assets/ff6d8581-6594-4f1a-afb5-2809dbfe50dc)


- Save the contact point and link it to your alert rule, then save the rule.
  ![image](https://github.com/user-attachments/assets/66c28d0a-c731-4cab-bcc1-09e41cc7c9e8)
