# learn-nestjs-rest-api

Rest API server with Nestjs

## Article

- [How to Build a CRUD REST API](https://www.freecodecamp.org/news/build-a-crud-rest-api-with-nestjs-docker-swagger-prisma/)

## Steps

- [NestJS](https://docs.nestjs.com/)
- Generate a project:
  ```bash
  npx @nestjs/cli new recipe-api
  ```
- npm run start:dev
- Create ProstgreSQL docker-compose file
- docker-compose up -d

## Setup Prisma

- npm install prisma -D
- npx prisma init
- Setup .env: DATABASE_URL="postgres://recipe:RecipePassword@localhost:5432/recipe"
- Setup prisma/schema.prisma
- Migrate: npx prisma migrate dev --name init
- Seed 1: prisma/seed.ts
- Seed 2: add prisma/seed to the package.json

```json
"prisma": {
    "seed": "ts-node prisma/seed.ts"
  }
```
- npx prisma db seed

## Create a Prisma service
- npx nest g module prisma
- npx nest g service prisma
- Update prisma.service.ts and prisma.module.ts

## Setup Swagger
- npm install @nestjs/swagger swagger-ui-express
- Update src/main.ts

------------------------------
## How to Implement CRUD Operations for the Recipe Model
- Generate resource: npx nest generate resource recipe

## How to Add PrismaClient to the Recipe Module
- recipe.module.ts: import { PrismaModule } from './prisma/prisma.module';
- recipe.module.ts: imports: [PrismaModule]

- recipe.service.ts: import { PrismaService } from './prisma/prisma.service';
- recipe.service.ts: constructor(private prisma: PrismaService) {}

## How to Define the GET /recipes Endpoint
- recipe.controller.ts: import { Controller, Get } from '@nestjs/common';
- recipe.controller.ts: @Controller('recipes')
- recipe.controller.ts: @Get()

- recipe.service.ts: 
```typescript
async findAll() {
    return this.prisma.recipe.findMany();
}
```

## Conclusion
