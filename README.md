# Lab 2 – CST8915 Full-stack Cloud-native Development

**Student:** Dhruvansh Zala
**ID:** 041214130

## Demo Video
https://youtu.be/LdDb1Z-DAVM

## Service Repositories
- order-service: https://github.com/Dzala02/order-service
- product-service: https://github.com/Dzala02/product-service
- store-front: https://github.com/Dzala02/store-front

## Deployment
| Component | VM | Region | Port |
|---|---|---|---|
| RabbitMQ | rabbitmq-vm | Sweden Central | 5672 |
| order-service | order-vm | Sweden Central | 3000 |
| product-service | product-vm | Sweden Central | 3030 |
| store-front | store-vm | Belgium Central | 8080 |

## Reflection Questions

### 1. What changes did you make to the order-service and product-service to comply with the Configuration and Backing Services factors?
In the order-service, the RabbitMQ connection string and port are no longer hard-coded. They are read from `process.env.RABBITMQ_CONNECTION_STRING` and `process.env.PORT`, loaded from a `.env` file using the `dotenv` package, which I added to package.json and package-lock.json. In the product-service, the port is read with `env::var("PORT")` (default 3030), and the `dotenv` crate was added to Cargo.toml and Cargo.lock. The `.env` files are excluded with `.gitignore`, and only a `.env.example` with placeholders is committed. For Backing Services, RabbitMQ runs on its own VM and is treated as an attached resource. The order-service only knows it through a URL with a dedicated `orderapp` account, so the broker could be swapped by changing one environment variable without changing any code.

### 2. Why is it important to use environment variables instead of hard-coding configurations?
Environment variables let the same code run in development, testing and production with different settings, without editing or rebuilding the code. They keep secrets such as passwords out of source control, which lowers the risk of leaking credentials. They also make deployments less error-prone, because each environment provides its own values instead of someone changing constants in the code.

### 3. Why is it important to have separate repositories for each microservice?
Separate repositories give each service its own version history, releases and deployment pipeline, so teams can work and deploy independently. A change or broken build in one service does not block the others. Each service can also be scaled, redeployed or even rewritten in another language on its own, which keeps the microservices loosely coupled and easier to maintain.

## Notes / Challenges
- The Azure for Students subscription has a 6-vCPU limit and a region policy, so store-vm was deployed in Belgium Central while the other VMs are in Sweden Central. The services communicate through public IPs, so this works fine.
- The RabbitMQ password contains `@`, so it had to be percent-encoded as `%40` in the connection string.
- SSH sessions dropped several times, so I ran the services inside `tmux` to keep them running.
