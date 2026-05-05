Run Docker hygiene to reclaim disk space. Execute these three commands in order:

1. `docker system prune -f` — removes stopped containers, dangling images, and unused networks
2. `docker image prune -a -f` — removes all images not tied to a running container
3. `docker volume prune --filter "label!=keep" -f` — removes unused volumes, but SKIP this step if the n8n container is currently stopped (it would delete the n8n_data volume and wipe workflow data)

Before running step 3, check whether n8n is running with `docker compose ps`. Only prune volumes if n8n shows as "running".

After all commands complete, report how much disk space was reclaimed.
