# Demo deployment

**Status: prepared, not connected.** No hosting service has been provisioned and
no deployment has been performed. Automatic deployment remains disabled until the
repository variable `DEMO_DEPLOY_ENABLED` is set to `true`.

The prepared workflow targets one Git-backed Render web service. It is called by
CI only for `main`, after both required check jobs succeed. It requests the exact
tested commit, waits for that deployment to become `live`, verifies the deployed
commit ID, and then checks that `/api/health` returns `{"status":"ok"}`. Merely
accepting a deployment request does not count as a successful deployment.

## Before connecting a host

Provision and validate the demo application first. This repository currently has
a Vite frontend and a separate FastAPI backend; the Vite development proxy is not
a production server. The hosting setup must serve `frontend/dist` with an SPA
fallback and route `/api/*` to FastAPI under the same HTTPS origin. The existing
cookie, CSRF, and Google login flows rely on that arrangement. A backend-only
Render service does not constitute a complete HireLens deployment.

Preserve `HIRELENS_STORAGE_DIR` on persistent storage, including account and history
subdirectories and the default SQLite database. Set a stable
`HIRELENS_ENCRYPTION_KEY` in the hosting service's secret settings before the first
start. Do not generate a new key on every deployment. Keep the backend on one
instance with one worker while account files and analysis jobs use local state.

Run Alembic migrations against the actual runtime database before starting the
API. With a Render persistent disk, migrations must run where that disk is mounted,
not in its separate build/pre-deploy environment. Set up TLS and trusted proxy
headers, keep secure cookies enabled, and configure the exact Google callback URL
if Google login is used. Validate login, resume upload, history persistence, and
frontend deep links before enabling automatic deployment.

Render persistent disks require a paid service and do not support multiple
instances or zero-downtime deployments. No paid service is created by these files.
See [Render disks](https://render.com/docs/disks).

## Connect the prepared workflow

1. Connect the Render service to this repository and `main`. Disable Render's
   independent auto-deploys so only successful GitHub CI starts deployments.
2. In **GitHub Settings → Environments → demo**, allow deployments only from the
   `main` branch.
3. Add `RENDER_API_KEY` as an environment secret for `demo`, with only the Render
   permissions needed to trigger and read deployments. Keep the key out of source,
   PRs, Jira tickets, and chat.
4. Add environment variables `RENDER_SERVICE_ID` (the `srv-...` ID) and
   `DEMO_HEALTH_URL` (the full HTTPS URL ending in `/api/health`).
5. Set the **repository** Actions variable `DEMO_DEPLOY_ENABLED` to `true` last.
6. Run **Actions → CI → Run workflow**, selecting `main`. Both checks must pass
   before the reusable deployment workflow starts.

An unset enable flag intentionally skips deployment; it does not indicate that a
service was deployed. Enabling the flag with missing configuration fails the job.
PRs never invoke the deployment workflow. A deployment is serialized, and newer
commits do not cancel an in-flight deployment.

The reusable workflow reads the environment secret directly in its deployment
job; a repository secret of the same name can also be passed by CI if needed.

## Failure and rollback

If triggering a deployment times out, inspect Render before retrying: the request
may have succeeded. Failed or timed-out deployments leave a failed Actions job.
Use the previous successful deploy in Render to roll back, and verify its health
and application flows. Assess migration compatibility separately: rolling back
application code does not reverse database migrations or restore deleted data.

For a production environment, add an explicit reviewer gate and a separate
deployment job. The current configuration is only for the demo environment.

References: [Render deployment API](https://api-docs.render.com/reference/create-deploy),
[deployment status](https://api-docs.render.com/reference/retrieve-deploy), and
[GitHub environments](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments).
