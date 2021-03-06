# Use Typescript with Node.js and Express

Setting up a new project can be overwhelming with the amount of options available.

Here is a basic Express server configured to use Typescript.

Included:

- nodemon (development)
- jest (testing)
- cors (middleware)
- tslint (linting)

## Initial Setup

```sh
mkdir src &&
npm init
```

## Dependencies

```sh
### app dependencies
npm i express cors dotenv &&

### dev dependencies
# typescript + node
npm i -D typescript tslint ts-node nodemon &&

# testing
npm i -D jest ts-jest supertest &&

# types
npm i -D @types/express @types/cors @types/dotenv @types/node @types/jest @types/supertest
```

## Server + Config Files

```sh
# configuration files
touch tsconfig.json tslint.json jest.config.js &&

# express server files
touch ./src/app.ts ./src/index.ts &&

# test file
touch ./src/app.test.ts &&

# .gitignore and .env
touch .gitignore .env
```

## `package.json`

```js
{
  // ...
  "scripts": {
    "start": "ts-node ./src/index.ts",
    "dev": "nodemon src/index.ts",
    "lint": "tslint -c tslint.json -p tsconfig.json",
    "test": "jest"
  }
}
```

## `tsconfig.json`

`lib`, `module`, and `target` use recommended settings for Node 14. [Source](https://github.com/microsoft/TypeScript/wiki/Node-Target-Mapping)

```json
{
  "compilerOptions": {
    "lib": ["ES2020"],
    "module": "commonjs",
    "target": "ES2020",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./src",
    "alwaysStrict": true,
    "noImplicitAny": true,
    "esModuleInterop": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

## `tslint.json`

```json
{
  "defaultSeverity": "error",
  "extends": ["tslint:recommended"],
  "jsRules": {
    "no-unused-expression": true
  },
  "rulesDirectory": []
}
```

## `jest.config.js`

```ts
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['src'],
};
```

## `app.ts`

```ts
import express from 'express';
import cors from 'cors';

const app: express.Application = express();

app.use(express.json());
app.use(cors());

app.get('/', (req: express.Request, res: express.Response) => {
  res.status(200).send('hello world');
});

export default app;

```

## `index.ts`

```ts
import 'dotenv/config';
import app from './app';

const PORT = process.env.PORT || 8080;

app.listen(PORT, () => {
  // tslint:disable-next-line:no-console
  console.log(`server listening on port ${PORT}`);
});
```

## `app.test.ts`

```ts
import request from 'supertest';
import app from './app';

describe('Test the root path', () => {
  test('It should response the GET method', async () => {
    const response = await request(app).get('/');
    expect(response.status).toBe(200);
  });
  test("It should response the with 'hello world'", async () => {
    const response = await request(app).get('/');
    expect(response.text).toEqual('hello world');
  });
});
```

## `.gitignore`

```
node_modules
.env
```

## `.env`

```
PORT=8080
```
