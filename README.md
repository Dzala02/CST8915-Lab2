# Lab 2 – CST8915 Full-stack Cloud-native Development

**Student:** Dhruvansh Zala  
**ID:** 041214130

## Demo Video
https://youtu.be/LdDb1Z-DAVM

## My Service Repositories
- order-service: https://github.com/Dzala02/order-service
- product-service: https://github.com/Dzala02/product-service
- store-front: https://github.com/Dzala02/store-front

## Reflection Questions

### 1. What changes did I make to follow the Configuration and Backing Services factors?
I removed the hard-coded values from the code. The order-service now gets the RabbitMQ connection string and the port from a `.env` file, and the product-service gets its port the same way. I added the `dotenv` package to both services to load these values. I also added `.env` to `.gitignore` so my password is never uploaded to GitHub. For backing services, RabbitMQ runs on its own VM, and the order-service connects to it using only a URL. If I want to use a different RabbitMQ server, I just change the `.env` file, not the code.

### 2. Why use environment variables instead of hard-coding?
Environment variables let me use the same code in different places, like testing and production, by changing only the settings. They also keep passwords and secrets out of the code, so they don't end up on GitHub. And if something changes, like an IP address, I can update it without touching the code.

### 3. Why have a separate repository for each microservice?
Each service can be updated and deployed on its own without affecting the others. If one service has a problem, the other services keep working. It also makes it easier to scale one service, or
