# Satellite Heroku Template

One-click deployment template for [Satellite](https://github.com/fileverse/satellite) - a document management system with sync.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://www.heroku.com/deploy?template=https://github.com/fileverse/satellite-heroku-template)

## Prerequisites

Before deploying, you'll need:

1. **Storage Server API Key** - API key for the storage server authentication

## Environment Variables

| Variable        | Required | Description                       | Default                   |
| --------------- | -------- | --------------------------------- | ------------------------- | --- |
| `API_KEY`       | Yes      | Storage server authentication key | -                         |     |
| `DB_PATH`       | No       | SQLite database path              | `/app/data/satellite.db`  |
| `RPC_URL`       | No       | Ethereum RPC endpoint             | `https://rpc.sepolia.org` |
| `INLINE_WORKER` | No       | Run worker in same process        | `true`                    |
| `PORT`          | Auto     | Automatically set by Heroku       | -                         |

## Architecture

This template deploys a single web dyno that runs both:

- **API Server**: REST API for document management
- **Worker**: Background process for sync jobs

Both run in the same process (via `INLINE_WORKER=true`) to share the SQLite database. This is required on Heroku because each dyno has its own ephemeral filesystem.

## Important Limitations

### Ephemeral SQLite Storage

Heroku uses an ephemeral filesystem. This means:

- SQLite database data is **not persistent** across dyno restarts or deployments
- Data will be lost when the dyno cycles (approximately every 24 hours on free/hobby plans)
- For production use, consider migrating to Heroku Postgres

## Post-Deployment Setup

1. After deployment, verify the dyno is running:

   ```bash
   heroku ps
   ```

2. Check the logs for any startup issues:

   ```bash
   heroku logs --tail
   ```

## Local Development

To run locally:

```bash
npm install
export DB_PATH="./data/satellite.db"
export API_KEY="your-api-key"
npm start
```

## License

ISC
