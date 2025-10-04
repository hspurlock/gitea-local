# Local Gitea Setup with Podman Compose

This setup runs Gitea with HTTPS enabled and a PostgreSQL database.

## Prerequisites

- Podman
- Podman Compose

## How to Run

1.  **Clone or download this `docker-compose.yml` file into a directory (e.g., `gitea-local`).**

2.  **Navigate to the directory:**
    ```bash
    cd gitea-local
    ```

3.  **Generate SSL certificates (if you don't have them):**
    ```bash
    openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes \
      -subj "/CN=gitea" \
      -addext "subjectAltName=DNS:gitea,DNS:localhost,IP:127.0.0.1"
    ```

    This certificate is valid for the hostname `gitea`, `localhost`, and `127.0.0.1`. You can add `gitea` to your `/etc/hosts` file to point it to any IP address.

4.  **Start the services:**
    ```bash
    podman-compose up -d
    ```

5.  **Access Gitea:**
    Open your web browser and go to [https://localhost:3000](https://localhost:3000).

    **Note:** You may see a browser warning about the self-signed certificate. This is expected and safe for local development.

6.  **Initial Configuration:**
    On the first visit, Gitea will present an installation page.
    -   **Database Type:** Select `PostgreSQL`.
    -   **Host:** `db:5432`
    -   **User:** `gitea`
    -   **Password:** `gitea`
    -   **Database Name:** `gitea`
    -   **Application General Settings:**
        -   **Site Title:** Your desired title (e.g., "Local Gitea")
        -   **Gitea Base URL:** `https://localhost:3000/`
    -   Fill in the administrator account settings.
    -   Click "Install Gitea".

## Setting up Gitea Actions Runner

To enable CI/CD with Gitea Actions, you need to register the runner:

1.  **Access Gitea admin panel:**
    Go to `https://localhost:3000` and log in as administrator.

2.  **Generate a runner registration token:**
    Navigate to `Site Administration` → `Actions` → `Runners` and click "Create new Runner". Copy the registration token.

3.  **Create a `.env` file with the token:**
    ```bash
    cp .env.example .env
    ```

    Edit `.env` and replace `your_registration_token_here` with your actual token:
    ```
    GITEA_RUNNER_TOKEN=your-actual-token-here
    ```

4.  **Restart the services:**
    ```bash
    podman-compose down
    podman-compose up -d
    ```

The runner will automatically register and start accepting jobs. The runner is configured to trust the self-signed certificate used by Gitea.

## Data Persistence

-   Gitea data is stored in the `./gitea` directory on your host machine.
-   PostgreSQL data is stored in the `./postgres` directory on your host machine.
-   Runner data is stored in the `./runner` directory on your host machine.

## Stopping the Services

To stop the services:

```bash
podman-compose down
```

To stop and remove volumes (deletes all data):

```bash
podman-compose down -v
```

