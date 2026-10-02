# BigKain30

Deployment configuration for the IBM Cloud Code Engine backend app `bigkain-ibm-backend`.

## Deploy to IBM Code Engine

The deployment workflow is deliberately **manual-only** until the app source and IBM project settings are verified. From the repository's `main` branch, run **Actions → Deploy backend to IBM Code Engine → Run workflow**.

Configure these GitHub Actions settings first:

| Setting | Type | Required | Purpose |
| --- | --- | --- | --- |
| `IBM_CLOUD_API_KEY` | Secret | Yes | IBM Cloud IAM API key with only the access needed for the target Code Engine project |
| `IBM_CE_REGION` | Variable | Yes | Region containing the Code Engine project, such as `us-south` |
| `IBM_CE_PROJECT` | Variable | Yes | Name of an existing Code Engine project |
| `IBM_CE_VISIBILITY` | Variable | Yes | Explicit app exposure choice: `public`, `private`, or `project` |
| `IBM_CE_SOURCE_DIR` | Variable | No | Repository-relative Node.js app directory; defaults to `.` |
| `IBM_CE_RESOURCE_GROUP` | Variable | No | IBM Cloud resource group; omit to use the account default |
| `IBM_CE_PORT` | Variable | No | App container port, if the app needs an explicit port |
| `IBM_CE_HEALTH_PATH` | Variable | No | Unauthenticated HTTP health path returning 2xx; defaults to `/` |

The workflow validates the configuration and source, confirms that the existing Code Engine project can be selected, creates or updates the app, then retries an HTTP health check. It runs only when manually dispatched from `main`; it does not deploy on pushes or pull requests. The selected visibility is applied to the app during every deployment, so choose it deliberately.

**Current repository limitation:** `main` does not contain a Node.js app or `package.json`. The workflow will stop at preflight until the service source is committed and `IBM_CE_SOURCE_DIR` points to the directory containing its `package.json`. The configured health path must respond successfully without authentication. Do not add the API key or other credentials to tracked files.
