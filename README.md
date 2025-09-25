# Pupero-Matrix

A self-contained Matrix (Synapse) homeserver setup for local/dev use. No TLS, no DNS, not federated. Optional Element Web client included.

Services
- Postgres 16 (for Synapse)
- Synapse (matrixdotorg/synapse)
- Element Web (optional) — convenient web client

Quick start
1) From this folder:
   - docker compose up -d
2) Wait until Synapse is healthy:
   - docker compose ps
   - or curl http://localhost:8008/_matrix/client/versions
3) Create users (three options):
   - Use Element: open http://localhost:8080, homeserver http://localhost:8008, and self-register (registration enabled by default for convenience).
   - Or CLI inside the container:
     - docker exec -it pupero-matrix-synapse register_new_matrix_user -c /data/homeserver.yaml http://localhost:8008
   - Or call the admin API using the registration_shared_secret defined in synapse.overrides.yaml

URLs
- Synapse client API: http://localhost:8008
- Element Web: http://localhost:8080 (optional service)

Notes
- Configuration uses synapse.overrides.yaml layered over the generated /data/homeserver.yaml.
- Federation is disabled (no 8448 listener and send_federation: false).
- This setup is intended for local development only. Do not expose it to the Internet without TLS and proper reverse proxying.
- Data persistence is via Docker volumes: synapse_data and synapse_db.

Common tasks
- View logs: docker logs -f pupero-matrix-synapse
- Reset everything (DANGER — deletes data): docker compose down -v && docker compose up -d
- Change server name: edit synapse.overrides.yaml (server_name) before the first run, or wipe volumes and re-init.

Troubleshooting
- If Element at http://localhost:8080 cannot connect, verify Synapse is healthy on http://localhost:8008/_matrix/client/versions
- Browsers block https->http mixed content; use the local Element (http) with the local Synapse (http).
