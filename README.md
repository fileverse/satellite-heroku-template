# Satellite Heroku Template

One-click deployment template for [Satellite](https://github.com/your-org/satellite) - a document management system with sync.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://www.heroku.com/deploy?template=https://github.com/your-org/satellite-heroku-template)

## Prerequisites

Before deploying, you'll need:

1. **Pimlico API Key** - Sign up at [pimlico.io](https://pimlico.io) to get an API key for account abstraction services
2. **Storage Server API Key** - API key for the storage server authentication

## Environment Variables

| Variable          | Required | Description                         | Default                   |
| ----------------- | -------- | ----------------------------------- | ------------------------- |
| `API_KEY`         | Yes      | Storage server authentication key   | -                         |
| `PIMLICO_API_KEY` | Yes      | Pimlico account abstraction API key | -                         |
| `DB_PATH`         | No       | SQLite database path                | `/app/data/satellite.db`  |
| `RPC_URL`         | No       | Ethereum RPC endpoint               | `https://rpc.sepolia.org` |
| `PORT`            | Auto     | Automatically set by Heroku         | -                         |

## Architecture

This template deploys two processes:

- **Web Dyno**: Runs the API server for document management
- **Worker Dyno**: Processes sync jobs

## Important Limitations

### Ephemeral SQLite Storage

Heroku uses an ephemeral filesystem. This means:

- SQLite database data is **not persistent** across dyno restarts or deployments
- Data will be lost when the dyno cycles (approximately every 24 hours on free/hobby plans)
- For production use, consider migrating to Heroku Postgres

### Worker Dyno Requirements

- The worker dyno requires a **paid Heroku plan** for 24/7 operation
- On free plans, the worker will sleep after 30 minutes of inactivity
- You can manually scale the worker: `heroku ps:scale worker=1`

## Post-Deployment Setup

1. After deployment, verify both dynos are running:

   ```bash
   heroku ps
   ```

2. Check the logs for any startup issues:

   ```bash
   heroku logs --tail
   ```

3. Scale the worker if needed:
   ```bash
   heroku ps:scale worker=1
   ```

## Local Development

To run locally:

```bash
npm install
export DB_PATH="./data/satellite.db"
export API_KEY="your-api-key"
export PIMLICO_API_KEY="your-pimlico-key"
npm start
```

In a separate terminal, start the worker:

```bash
npm run worker
```

## License

ISC
