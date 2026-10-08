# ResQmap

ResQmap is a web app for coordinating safety checks during an emergency. People can share their location and report whether they are safe, need help, or have not responded. Responders can view those reports on a live map and send targeted or area-wide notifications.

## What is included

- **Home:** an overview of the response workflow.
- **Live map:** current safety counts, location markers, status filters, and a priority queue. The map refreshes every 30 seconds.
- **Admin panel:** a user directory, role management, targeted notifications, and broadcasts.
- **Police dispatch:** a map-based radius filter for finding people in a disaster zone and notifying that zone.
- **Safety responses:** signed-in users can share browser location, respond to safety checks, and update their status.

The interface uses FastAPI, Jinja templates, Leaflet, and OpenStreetMap tiles. Data is stored in a local SQLite file (`backend.db`).

## Run locally

You need Python 3.12 and [uv](https://docs.astral.sh/uv/).

```bash
uv sync
export SECRET_KEY="$(python3 -c 'import secrets; print(secrets.token_urlsafe(32))')"
uv run uvicorn main:app --reload
```

Open [http://127.0.0.1:8000](http://127.0.0.1:8000). The API reference is at [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs). The app creates missing database tables at startup.

## Configuration

`SECRET_KEY` is required for login tokens. Settings can be supplied through environment variables or a `.env` file. Email delivery is optional: set `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`, and `EMAIL_FROM` to send notification emails. Without SMTP credentials, in-app safety notifications still work.

Location sharing requires browser permission. When enabled, the app sends location updates every three minutes. The live map uses OpenStreetMap tiles, so its background map needs an internet connection.

## Roles

| Role | Access |
| --- | --- |
| User | View the live map, share location, and respond to safety checks. |
| Police | Use the live map and disaster-zone dispatch tools. |
| Admin | Manage users and roles, send notifications, and use dispatch tools. |

The project includes a development admin seed in `database.py`; it runs only if the database has no admin account.
