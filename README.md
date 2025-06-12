# Local Gitea Setup with Docker Compose

This setup runs Gitea with a PostgreSQL database.

## Prerequisites

- Docker
- Docker Compose

## How to Run

1.  **Clone or download this `docker-compose.yml` file into a directory (e.g., `gitea-local`).**

2.  **Navigate to the directory:**
    ```bash
    cd gitea-local
    ```

3.  **Start the services:**
    ```bash
    docker-compose up -d
    ```

4.  **Access Gitea:**
    Open your web browser and go to [http://localhost:3000](http://localhost:3000).

5.  **Initial Configuration:**
    On the first visit, Gitea will present an installation page.
    -   **Database Type:** Select `PostgreSQL`.
    -   **Host:** `db:5432`
    -   **User:** `gitea`
    -   **Password:** `gitea`
    -   **Database Name:** `gitea`
    -   **Application General Settings:**
        -   **Site Title:** Your desired title (e.g., "Local Gitea")
        -   **Gitea Base URL:** `http://localhost:3000/`
        -   **SSH Server Domain:** `localhost` (if using SSH on port 2222, ensure this is correct or use the server's IP if accessing from other machines)
        -   **SSH Port:** `2222` (as mapped in `docker-compose.yml`)
    -   Fill in the administrator account settings.
    -   Click "Install Gitea".

## Data Persistence

-   Gitea data is stored in the `./gitea` directory on your host machine.
-   PostgreSQL data is stored in the `./postgres` directory on your host machine.

## Stopping the Services

To stop the services:

```bash
docker-compose down
```

To stop and remove volumes (deletes all data):

```bash
docker-compose down -v
```

## Accessing Gitea via SSH

To clone/push via SSH, you'll use port 2222:

```bash
git clone ssh://git@localhost:2222/your-user/your-repo.git
```
