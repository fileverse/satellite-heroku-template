# API Heroku Template

One-click deployment template for [Fileverse API](https://github.com/fileverse/api) - a document management system with sync.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://www.heroku.com/deploy?template=https://github.com/fileverse/api-heroku-template)

## Prerequisites

Before deploying, you'll need:

1. **Storage Server API Key** - API key for the storage server authentication

## Environment Variables

| Variable        | Required | Description                       | Default                   |
| --------------- | -------- | --------------------------------- | ------------------------- |
| `API_KEY`       | Yes      | Storage server authentication key | -                         |
| `DATABASE_URL`  | Auto     | PostgreSQL connection string (set by Heroku Postgres addon) | -  |
| `RPC_URL`       | No       | Ethereum RPC endpoint             | `https://rpc.sepolia.org` |
| `INLINE_WORKER` | No       | Run worker in same process        | `true`                    |
| `PORT`          | Auto     | Automatically set by Heroku       | -                         |

## Architecture

This template deploys a single web dyno with **Heroku Postgres** that runs both:

- **API Server**: REST API for document management
- **Worker**: Background process for sync jobs

Both run in the same process (via `INLINE_WORKER=true`). The Heroku Postgres addon is automatically provisioned and `DATABASE_URL` is set by Heroku, so data persists across dyno restarts and deployments.

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
export DATABASE_URL="postgresql://localhost:5432/fileverse"
export API_KEY="your-api-key"
npm start
```

## License

ISC
