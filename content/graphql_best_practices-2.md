---
tags: ["GraphQL", "React", "NestJS", "Prisma", "Senior", "Best Practices"]
description: "Learn the best practices for using GraphQL with React, NestJS, and Prisma, including examples and tools. Understand how to implement signup functionality and use tokens to fetch user information for Yarn users."
title: "GraphQL Best Practices for Newbies: React, NestJS, Prisma with Examples and Tools"
---

# GraphQL Best Practices for Newbies: React, NestJS, Prisma

GraphQL is a powerful query language for your API, and combining it with React, NestJS, and Prisma can create a robust, efficient stack for web development. This guide will walk you through best practices, provide examples, and introduce tools to help you get started. We will also cover how to implement signup functionality and use tokens to fetch user information, tailored for Yarn users.

## Why GraphQL?

GraphQL provides a more efficient and flexible alternative to REST by allowing clients to request exactly the data they need. This reduces over-fetching and under-fetching issues and provides a better experience for developers and users alike.

## Setting Up Your Environment

Before diving into the code, ensure you have the necessary tools installed:

- **Node.js**: Ensure you have Node.js installed. You can download it from [nodejs.org](https://nodejs.org/)

## Creating the Backend with NestJS and Prisma

### Step 1: Initialize NestJS Project

I use Yarn for package management, but you can use npm or pnpm if you prefer.

Start by creating a new NestJS project:

```bash
yarn create @nestjs/core my-nestjs-app
cd my-nestjs-app
yarn add @nestjs/graphql @nestjs/apollo graphql apollo-server-express
```

### Step 2: Set Up Prisma

Prisma is a modern database toolkit that simplifies database access in Node.js and TypeScript applications.

```bash
yarn add prisma @prisma/client
npx prisma init
```

Configure your `prisma/schema.prisma` file to define your database models. For example:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  password  String
  name      String?
  createdAt DateTime @default(now())
}
```

Run the following to migrate your database:

```bash
npx prisma migrate dev --name init
```

### Step 3: Implement GraphQL in NestJS

Create a GraphQL module and resolver in NestJS:

```bash
nest g module graphql
nest g resolver graphql
```

In `graphql/graphql.module.ts`, configure GraphQL:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';
import { join } from 'path';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
    }),
  ],
})
export class GraphqlModule {}
```

In `graphql/graphql.resolver.ts`, define your resolvers:

```typescript
import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
import { PrismaService } from '../prisma/prisma.service';

@Resolver()
export class GraphqlResolver {
  constructor(private prisma: PrismaService) {}

  @Query(() => String)
  async hello() {
    return 'Hello World!';
  }

  // Add signup and fetch user info mutations here
}
```

### Step 4: Add Signup and Fetch User Info

Define GraphQL types and resolvers for signup and fetching user info:

```typescript
import { ObjectType, Field, ID } from '@nestjs/graphql';

@ObjectType()
export class User {
  @Field(() => ID)
  id: number;

  @Field()
  email: string;

  @Field()
  name?: string;
}

@Resolver(() => User)
export class UserResolver {
  constructor(private prisma: PrismaService) {}

  @Mutation(() => User)
  async signup(
    @Args('email') email: string,
    @Args('password') password: string,
    @Args('name') name?: string,
  ): Promise<User> {
    return this.prisma.user.create({
      data: {
        email,
        password, // hash this in real applications
        name,
      },
    });
  }

  @Query(() => User)
  async me(@Args('id') id: number): Promise<User> {
    return this.prisma.user.findUnique({ where: { id } });
  }
}
```

## Setting Up the Frontend with React

### Step 1: Initialize React Project

Create a new React project and install dependencies:

```bash
yarn create react-app my-react-app
cd my-react-app
yarn add @apollo/client graphql
```

### Step 2: Configure Apollo Client

Set up Apollo Client in your React application:

```javascript
import React from 'react';
import { ApolloProvider, InMemoryCache, ApolloClient } from '@apollo/client';

const client = new ApolloClient({
  uri: 'http://localhost:3000/graphql',
  cache: new InMemoryCache(),
});

function App() {
  return (
    <ApolloProvider client={client}>
      <div className="App">
        <h1>Hello GraphQL</h1>
        {/* Add components for signup and fetching user info */}
      </div>
    </ApolloProvider>
  );
}

export default App;
```

### Step 3: Implement Signup and Fetch User Info

Create a component for signup:

```javascript
import React, { useState } from 'react';
import { gql, useMutation } from '@apollo/client';

const SIGNUP = gql`
  mutation Signup($email: String!, $password: String!, $name: String) {
    signup(email: $email, password: $password, name: $name) {
      id
      email
    }
  }
`;

function Signup() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [name, setName] = useState('');
  const [signup] = useMutation(SIGNUP);

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const { data } = await signup({ variables: { email, password, name } });
      console.log('User signed up:', data);
    } catch (error) {
      console.error('Error signing up:', error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        placeholder="Password"
      />
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
      />
      <button type="submit">Signup</button>
    </form>
  );
}

export default Signup;
```

Create a component to fetch user info:

```javascript
import React, { useState } from 'react';
import { gql, useQuery } from '@apollo/client';

const GET_USER = gql`
  query Me($id: Int!) {
    me(id: $id) {
      id
      email
      name
    }
  }
`;

function FetchUser() {
  const [userId, setUserId] = useState('');
  const { data, loading, error } = useQuery(GET_USER, {
    variables: { id: parseInt(userId) },
    skip: !userId,
  });

  const handleFetchUser = (e) => {
    e.preventDefault();
    if (userId) {
      // Trigger query
    }
  };

  return (
    <div>
      <form onSubmit={handleFetchUser}>
        <input
          type="number"
          value={userId}
          onChange={(e) => setUserId(e.target.value)}
          placeholder="User ID"
        />
        <button type="submit">Fetch User Info</button>
      </form>
      {loading && <p>Loading...</p>}
      {error && <p>Error fetching user info: {error.message}</p>}
      {data && (
        <div>
          <p>ID: {data.me.id}</p>
          <p>Email: {data.me.email}</p>
          <p>Name: {data.me.name}</p>
        </div>
      )}
    </div>
  );
}

export default FetchUser;
```

## Conclusion

By following these best practices and examples, you can efficiently use GraphQL with React, NestJS, and Prisma. This stack allows you to build scalable and maintainable applications. Remember to always secure your endpoints and properly handle sensitive data like passwords and tokens.