# Satellite Heroku Template

One-click deploy template for my-satellite.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://www.heroku.com/deploy?template=https://github.com/fileverse/satellite-heroku-template)

## Processes

This template runs two Heroku dynos:

- **web**: API server (`npm run start:api`)
- **worker**: Background worker (`npm run start:worker`)

## Configuration

Update the `env` section in `app.json` with your required environment variables before deploying.

## Setup

1. Add your environment variables to `app.json`
2. Click the Deploy button above
