# Users REST API — Azure deployment

A Node.js, Express, and Mongoose REST API for managing users. This repository currently contains the backend only; it does not include a React frontend, so there is no `npm run build` step or frontend folder to publish.

## Run locally

1. Install Node.js 20 or later and run MongoDB locally, or create a MongoDB Atlas cluster.
2. Copy `config/.env.example` to `config/.env` and set `MONGODB_URI` to your database connection string.
3. Install dependencies and start the API:

   ```sh
   npm install
   npm start
   ```

The API listens on port 3000 by default. It uses the `PORT` environment variable when provided by a host such as Azure App Service.

## API routes

| Method | Path | Action |
| --- | --- | --- |
| GET | `/api/users` | Return all users |
| POST | `/api/users` | Create a user |
| PUT | `/api/users/:id` | Update a user |
| DELETE | `/api/users/:id` | Delete a user |

POST and PUT accept JSON such as:

```json
{
  "name": "Alex Example",
  "email": "alex@example.com",
  "age": 25
}
```

## Deploy the API to Azure App Service

1. Create a MongoDB Atlas cluster and database user. In Atlas Network Access, allow connections from the App Service outbound IP addresses. Keep database credentials private.
2. In Azure Portal, create a **Web App** using the **Node** runtime on Linux. Choose a name, region, and pricing tier for your needs.
3. In the Web App's **Deployment Center**, connect this GitHub repository and select the `main` branch. Enable continuous deployment.
4. In **Settings → Environment variables** (called **Configuration** in some portal views), add `MONGODB_URI` with the Atlas connection string. Add `PORT` only if your setup requires it; App Service supplies the port. Save and restart the app.
5. Open the app's default domain and check `/` for the API status. Use Postman to try the routes above.

The project has an `npm start` script and reads configuration from environment variables, which App Service exposes to Node.js. No `.env` file or credentials should be committed. `config/.env.example` is only a local setup template.

## References

- [Configure a Node.js app for Azure App Service](https://learn.microsoft.com/azure/app-service/configure-language-nodejs)
- [Configure App Service environment variables](https://learn.microsoft.com/azure/app-service/configure-common)
- [Deploy to App Service with GitHub Actions](https://learn.microsoft.com/azure/app-service/deploy-github-actions)
