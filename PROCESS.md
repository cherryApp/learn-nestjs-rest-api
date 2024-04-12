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
- recipe.controller.ts: 
```typescript
@Get()
async findAll() {
  return await this.recipeService.findAll();
}
```

- recipe.service.ts: 
```typescript
async findAll() {
    return this.prisma.recipe.findMany();
}
```

## How to Define the POST, PATCH and DELETE Endpoints
- At the same way of GET, but we have to define DTO's.

## Secure the application
- [Documentation](https://docs.nestjs.com/recipes/passport)
- Install necessary dependencies:
```bash
npm install @nestjs/passport passport passport-local
npm install -D @types/passport-local
```

- Generate the module and the service:
```bash
npx nest g module auth
npx nest g service auth
npx nest g module users
npx nest g service users
```

- Update users/user.service.ts
- Update users/user.module.ts: export the UsersService

- Update auth/auth.service.ts
- Update auth/auth.module.ts: import UsersModule

- Implementing the Passport's local authentication strategy.
- Create the descriptor file: auth/local.strategy.ts
- Update auth/auth.module.ts: 
  - add LocalStrategy to the providers
  - add PassportModule to the imports

- __Setup built-in Guards__
- Update app.controller.ts: add a guarded post request to login
- Test it from the browser:
```javascript
fetch ('/auth/login', {
    method: 'POST',
    headers: {
        'Content-type': 'application/json',
    },
    body: '{"username": "john", "password": "changeme"}'
}).then( r => r.json() )
.then( data => console.log(data) );
```

- __Create LocalAuthGuard__
- Create new file: auth/local-auth.guard.ts
- Define the LocalAuthGuard class.
- Apply the guard in the AppController: @UseGuards(LocalAuthGuard)

- __JWT Functionality__
- Add dependencies:
```bash
npm install @nestjs/jwt passport-jwt
npm install -D @types/passport-jwt
```
- Update auth/auth.service.ts: implementing the login method.
- Create auth/constants.ts: add jwtContants.secret, export AuthService.
- Update auth/auth.module.ts: setup JWT.
- Update app.controller.ts: the login function returns the AuthService login method.
- Test it with a fetch request.

- __Implementing Passport JWT__
- Create auth/jwt.strategy.ts: a strategy for JWT based guards.
- Update auth/auth.module.ts: add the new JwtStrategy to the providers.
- Create auth/jwt-auth.guard.ts

- __Implement protected routes__
- Update app.controller.ts: use JwtAuthGuard for desired routes.

## Nestjs + Prisma Authentication
- [Documentation](https://www.prisma.io/blog/nestjs-prisma-authentication-7D056s1s0k3l)
- 
## Conclusion
