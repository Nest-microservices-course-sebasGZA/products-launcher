# Products App Microservices

## Dev
1. Create a file .env based on .env.template
2. To start the submodules into this repo run
```git submodule update --init --recursive```
3. Run ```docker compose up --build```

## Note
To remove all containers and volumes run ```docker compose down -v```

## Prod

1. Clone repository
2. Create a .env based on the .env.template file
3. Execute command to build images ```docker compose -f docker-compose.prod.yml build```