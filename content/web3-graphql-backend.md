---
title: "How to Implement a Simple Back-End for Web3.js Using GraphQL and TypeScript"
description: "Learn how to set up a simple back-end for Web3.js using GraphQL and TypeScript, including setting up a Node.js server, defining GraphQL schemas and resolvers, and integrating Web3.js."
tags: ["Web3.js", "GraphQL", "TypeScript", "Node.js", "Blockchain"]
---

## How to Implement a Simple Back-End for Web3.js Using GraphQL and TypeScript

### Prerequisites

1. **Node.js (v22 or later)**
2. **Basic understanding of TypeScript, GraphQL, and Web3.js**

### Step 1: Set Up Your Project

1. **Initialize the project:**

   ```bash
   mkdir web3-graphql-backend
   cd web3-graphql-backend
   yarn init -y
   ```

2. **Install dependencies:**

   ```bash
   yarn add express express-graphql graphql type-graphql reflect-metadata web3
   yarn add --dev typescript ts-node @types/node @types/express @types/graphql
   ```

3. **Initialize TypeScript configuration:**

   ```bash
   yarn tsc --init
   ```

   Update `tsconfig.json` to include:

   ```json
   {
     "compilerOptions": {
       "target": "ES6",
       "module": "commonjs",
       "outDir": "./dist",
       "rootDir": "./src",
       "strict": true,
       "esModuleInterop": true,
       "experimentalDecorators": true,
       "emitDecoratorMetadata": true
     }
   }
   ```

### Step 2: Set Up Web3.js

1. **Create a `.env` file** to store your testnet RPC URL and other environment variables:

   ```env
   INFURA_PROJECT_ID=your_infura_project_id
   ```

2. **Install dotenv for environment variables:**

   ```bash
   yarn add dotenv
   ```

### Step 3: Create the GraphQL Server

1. **Create the folder structure:**

   ```bash
   mkdir -p src/graphql
   touch src/index.ts src/graphql/schema.ts src/graphql/resolvers.ts
   ```

2. **Configure Web3.js in `src/index.ts`:**

   ```typescript
   import 'reflect-metadata';
   import express from 'express';
   import { ApolloServer } from 'apollo-server-express';
   import { buildSchema } from 'type-graphql';
   import { Web3Resolver } from './graphql/resolvers';
   import dotenv from 'dotenv';
   import Web3 from 'web3';

   dotenv.config();

   const app = express();
   const web3 = new Web3(`https://mainnet.infura.io/v3/${process.env.INFURA_PROJECT_ID}`);

   async function startServer() {
     const schema = await buildSchema({
       resolvers: [Web3Resolver],
     });

     const server = new ApolloServer({ schema });

     await server.start();
     server.applyMiddleware({ app });

     app.listen(4000, () => {
       console.log('Server is running on http://localhost:4000/graphql');
     });
   }

   startServer().catch((error) => {
     console.error('Error starting server:', error);
   });
   ```

### Step 4: Define GraphQL Schema and Resolvers

1. **Create the GraphQL schema in `src/graphql/schema.ts`:**

   ```typescript
   import { buildSchema } from 'type-graphql';
   import { Web3Resolver } from './resolvers';

   export const schema = buildSchema({
     resolvers: [Web3Resolver],
   });
   ```

2. **Create the resolver in `src/graphql/resolvers.ts`:**

   ```typescript
   import { Resolver, Query, Arg } from 'type-graphql';
   import Web3 from 'web3';
   import dotenv from 'dotenv';

   dotenv.config();

   const web3 = new Web3(`https://mainnet.infura.io/v3/${process.env.INFURA_PROJECT_ID}`);

   @Resolver()
   export class Web3Resolver {
     @Query(() => String)
     async getBlockNumber(): Promise<string> {
       const blockNumber = await web3.eth.getBlockNumber();
       return blockNumber.toString();
     }

     @Query(() => String)
     async getBalance(@Arg('address') address: string): Promise<string> {
       const balance = await web3.eth.getBalance(address);
       return web3.utils.fromWei(balance, 'ether');
     }
   }
   ```

### Step 5: Run and Test Your Server

1. **Compile the TypeScript code:**

   ```bash
   yarn tsc
   ```

2. **Run the server using `ts-node`:**

   ```bash
   yarn ts-node src/index.ts
   ```

3. **Access GraphQL Playground:**

   Open [http://localhost:4000/graphql](http://localhost:4000/graphql) in your browser.

### Example Queries

To get the current block number:

```graphql
query {
  getBlockNumber
}
```

To get the balance of an Ethereum address:

```graphql
query {
  getBalance(address: "0x742d35Cc6634C0532925a3b844Bc454e4438f44e")
}
```

### Conclusion

You have successfully implemented a simple back-end for Web3.js using GraphQL and TypeScript. This setup allows you to extend your GraphQL API with more Web3.js functionalities as needed.
