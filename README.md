# AaravLabs Frontend

This is the frontend for AaravLabs. AaravLabs is a fun, educational website for middle schoolers. It combines tough but intresting questions with AI analysis to helps kids know what they got right or where they went wrong.

## Building

To build the frontend:
1. Build and deploy [the backend](https://github.com/AaravLabsOffical/Backend) and update the url in `.env` to match your backend url.
2. Go to `package.json` and replace the `--push` part of the `docker` script `yourusername/aaravlabs`
3. Run `npm i` to install required dependencies
4. Run `npm run docker` to build and push to Docker Hub

## Usage

To use this, you need [the backend](https://github.com/AaravLabsOffical/Backend) too.
It is reccomended that you use the `docker-compose.yaml` given in [the main repository](https://github.com/AaravLabsOffical/AaravLabs) for the best experience.
