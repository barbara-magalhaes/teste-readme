<h1 align="center">Pelando API</h1>

<div align="center">
   API for Backoffice and Pelando website.
</div>

## Prerequisite

Start at [https://github.com/pelando/docs/wiki/Summary](github.com/pelando/docs/wiki/Summary)

## Usage

1. Clone project code:

```bash
git clone git@github.com:pelando/api.git
cd api
```

2. [Download](https://pelandobr.atlassian.net/wiki/spaces/EN/pages/2654319/API) the development `.env` file for this repository.

3. Create `pelando` network, can ignore errors:
    ```bash
    #Install docker /perform step 1 from the link
    ```
     [Download](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-pt) the `docker` link 

```bash
later:
        sudo docker network create pelando
```

4. Build and start the application:
```bash
step 1:
        sudo apt install buil-essential 
```
```bash
    later:
            #install yarn
            sudo apt install yarn
```
[Download](https://linuxize.com/post/how-to-install-yarn-on-ubuntu-20-04/) the `yarn` link

```bash
step 2:
     #install nodejs/version needs to be above 16
    -curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```
 [Download](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04-pt) the  `nodejs` link


```bash
step 3:
    #install cmake 
    -sudo snap install cmake --classic
```
[Download](https://linuxhint.com/install-cmake-on-ubuntu) the `cmake` link

```bash
step 4:
    #install docker-compose 
    -sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
``` 
[Download](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04-pt) the `docker-compose` link


```bash
later:
        -sudo make up

```

or (detached)

```bash
make up-silent
```

5. After the startup is complete, open a browser and visit [http://localhost:3201/status](http://localhost:3201/status).

## Updating dependencies

Always use yarn commands (e.g. `yarn add`, `yarn install`) to change/install dependencies instead of directly in package.json. You don't need to re-run `make build` in this case.

Your local `node_modules` directory will override the one built in the Docker image, so it's important to keep it updated!

## Other commands

Run unit tests:

```bash
make test
```

Run all tests (unit and integration):

```bash
make test-all
```

Run linters:

```bash
make lint
```

Run code generator (GraphQL schema):

```bash
make generate
```

Build the container:

```bash
make build
```

Inspect the container:

```bash
make shell
```

Run pending database migrations:

```bash
make migrate
```

# Tooling

There are some useful tools included in this project that aim to aid the development process.

## Mongo Express

### What

Mongo Express is a web-based MongoDB admin.
It is acessible under [http://localhost:8081/](http://localhost:8081/).

### Why

It is meant to help to inspect the databases, collections, and documents. It is an easy to use GUI in which you may query the collections, check index size and get data regarding the databases statuses as well as some server info.
Works nicely to delete the database or edit/delete documents when you don't want to go to the trouble of connecting to the mongo shell.

### Docs

[https://github.com/mongo-express/mongo-express](https://github.com/mongo-express/mongo-express)

---

### Bull board

### What

Bull Dashboard is a web-based UI that helps us visualize our queues and their jobs.
It is accessible under [http://localhost:3201/admin/queues/](http://localhost:3201/admin/queues/).

### Why

It is meant to help us see how the queue and jobs are behaving at a glance. It allows us to check the state of the queues (jobs waiting, failed, delayed, running, etc...). It also provides some small capabilities to relaunch failed jobs, etc...
Use it as a tool to monitor how worker jobs are behaving and debug jobs that have failed.

### Docs

https://github.com/vcapretz/bull-board#readme

## How to

### TDD

Our test runner (`jest`) has a nice feature to watch tests and rerun only the affected changes. Just use `make test-watch` to start it.

:bulb: **Pro-tip**: If you want to run specific test cases, you may pass a regex like this `make test-file-pattern TEST=OfferController`.

### Docs

For more info about how to use Jest, refer to it's [docs](https://jestjs.io/docs/en/getting-started)
For some nice resources about testing, refer to:

- [Unit testing in JavaScript (fun and nice videos)](https://www.youtube.com/watch?v=Eu35xM76kKY&list=PL0zVEGEvSaeF_zoW9o66wa_UCNE3a7BEr)
- [Google testing blog (interesting blog posts)](https://testing.googleblog.com/)
- [Fowler's Pyramid of testing (long but gold)](https://martinfowler.com/articles/practical-test-pyramid.html)

## Project Structure

| Name                           | Description                                                                 |
|--------------------------------|-----------------------------------------------------------------------------|
| **.circleci**/                 | CircleCI pipelines configuration                                            |
| **.coverage**/                 | Code coverage reports                                                       |
| **.data**/                     | Docker volume for MongoDB database files                                    |
| **.github**/                   | Github configuration for workflows, PR templates and others                 |
| **src/app**/                   | REST api modules. Each folder is a module                                   |
| **src/backoffice**/            | Resolvers and methods that are used on Backoffice app                       |
| **src/external**/              | GraphQl server inicialization and configuration                             |
| **src/external/graphql**/      | GraphQL server specification. It should only depend on the REST api         |
| **src/internal**/              | REST API Inicalization and configuration                                    |
| **src/migrations**/            | Database versioning migrations                                              |
| **src/tests**/                 | Application unit and integration tests                                      |
| **src/scripts**/               | Self Invoking functions that are called from scripts in package.json        |
| **src/shared**/                | Shared methods and modules between all roles                                |
| **src/shared/config**/         | Centralized configuration file for app variables                            |
| **src/shared/events**          | Domain Events definitions and subscriptions configuration                   |
| **src/shared/helpers**         | Global Helpers                                                              |
| **src/shared/infrastructure**/ | External services configuration and setup. Databases, Queue, etc...         |
| **src/shared/loaders**/        | Split the startup process into modules                                      |
| **src/shared/types**/          | TypeScript type definitions                                                 |
| **src/shared/utils**/          | Auxiliary functions                                                         |
| **src**/index.ts               | The main entrypoint. Starts the project using the specified role            |
| **scripts/**                   | Responsible to notifying monitoring systems and downloads the mongod binary |
| **k8s**/                       | Responsible to configure Kubernetes                                         |
| graphql-codegen.yml            | Configuration file used to generate the GraphQL schema      |
| .dockerignore                  | Folder and files ignored by docker usage                                    |
| .env.template                  | Your API keys, tokens, passwords and database URI                           |
| .eslintrc                      | Rules for eslint linter                                                     |
| .gitignore                     | Folder and files ignored by git                                             |
| .prettierrc                    | Prettier rules configuration                                                |
| docker-compose.yml             | Docker compose configuration file                                           |
| dockerfiles/                   | Dockerfile image blueprint                                                  |
| jest.config.js                 | Jest configuration file                                                     |
| jest.setup.ts                  | Script that setups the test environment                                     |
| Makefile                       | Helpers and shortcuts to start the application                              |
| package.json                   | Project configuration and dependencies list                                 |
| README.md                      | This very file. It is meant for being read                                  |
| ts-config-paths-bootstrap.js   | Workaround bindings to use TS's path aliases in node                        |
| tsconfig.json                  | TypeScript configuration                                                    |
| yarn.lock                      | Contains exact versions of NPM dependencies in package.json                 |

### src/app

Here is where our REST API lives. Each folder should be a working module on its own. Folders here are not separated by entities but rather by functionality, although these can coincide in some cases.

Each module should respect the following folder structure

| Name              | Description                                                 |
|-------------------|-------------------------------------------------------------|
| **controllers**/  | Bridge between the HTTP layer and our services              |
| **cronjobs**/     | Scheduled Jobs that are executed periodically               |
| **data**/         | Seed data and BigQuery-related code                         |
| **events**/       | Domain Events definitions and subscriptions configuration   |
| **errors**/       | Custom JS Errors definitions                                |
| **helpers**/      | Auxiliary functions related to the entity                   |
| **jobs**/         | Job definitions for background processing                   |
| **middlewares**/  | Custom express middlewares                                  |
| **repositories**/ | Data access layer                                           |
| **services**/     | Business logic                                              |
| **validators**/   | Schema definition for HTTP transports                       |
| **views**/        | Transformers for HTTP responses                             |
| context.ts        | Defines the context used between the layers of each module  |
| app.ts            | HTTP routes definition                                      |

#### controllers

Controllers should only have "glue code" with minimal business logic. They should be boring. Their job is to connect the routes inputs (`body`, `query strings`, `URL params`, etc...) to the respective use case service.

They should not have access to `repositories`. They are meant to be a bridge between the HTTP layer and our `services`.

#### data

We are using this folder to hold seed data for the module. Either for testing purposes or business requirements.

#### events

Events published by the module are defined in the `names.ts` file and subscriptions to events in the `subscribers.ts` file.
Events here work in a Pub/Sub interface but because of the distributed nature of our system, they have a _at least once_ delivery strategy. This is important because

#### errors

Custom JS error definitions. Each file is a custom error.
Follow the example: [here](./src/app/offer/errors/CategoryDoesNotExist.ts) to be able to use Error subclassing.

#### helpers

Collection of auxiliary functions used at this context level.
It might be the place to define some contextless functionalities which can be used by other parts of the code.

#### jobs

Job handler definitions. Each file should contain a job. Ideally, this should be a glue code that binds the job payload to a service function that has been unit tested.

Use the `JobHandler` type and be sure that the job is present either in the export of the module's `jobs/index.ts` or in the application-level `src/jobs/index` export.

Refer to our [job definition policy](#Job/Workers-definition) for more information.

#### middlewares

Custom middlewares. In the end, Express-based applications are just a series of middlewares. We are using this folder to define shared middlewares.

Note: Be sure that the middleware will be used in more than one place. If it is not, maybe try a different approach.

For more info: http://expressjs.com/en/guide/using-middleware.html#using-middleware

#### repositories

Repositories follow the [Repository Pattern](https://martinfowler.com/eaaCatalog/repository.html) and aim to be a layer that isolates the details on **how** to deal with persistence models from our domain code.

We should have at least two repositories:

- `InMemory` repositories that have a simplified logic and are meant for testing purposes
- `MongoDB` to connect to our MongoDB database

#### services

Services are the heart and brains of our application. Our business logic should be contained here. Each file should contain a testable feature that will declare and receive the required dependencies (repositories, parameters, etc...) and use those to accomplish something.

Things that are considered use cases, like [flagging a user's first offer](./src/app/offer/services/FlagFirstOffer.ts), should be here.
Complex "queries", with pagination and such [can also be placed here](./src/app/store/services/TopStores.ts).

Services can use other services but before doing that, make sure that the same result could not be achieved with a job in the queue or some other low-coupling strategy. If you are sure about it, just keep in mind the good old Clean Code rule of [single level of abstraction](http://www.principles-wiki.net/principles:single_level_of_abstraction) and watch out for circular dependencies.

#### validators

Schema validation for the HTTP incoming requests.

#### views

Transformers that map the services' output to the controller's response

#### routes.ts

HTTP routes definitions. It should define a `Router` and point each route to a controller function.

### src/shared/config

Centralized configuration file for app variables.

This folder has only one file: `index.ts` that is responsible for building the application configuration object. It is the only file under `src/` that may directly access the `process.env` property to read environment variables and it should map those variables to properties in the configuration object.

Any kind of input sanitization or falling back to default values should be done here.

### src/external/graphql

Here is where our GraphQL API lives. Currently, we are using it as the facade that specifies and fulfills the communication contract between our frontend web client and the REST API.

As an architecture decision, we try to maintain the GraphQL server as an isolated module that works by itself, communicating with the other parts of the application using only the REST HTTP protocol (by using the data sources). This provides us with the flexibility to deploy and scale each part with different strategies.

Project Structure:

| Name            | Description                                                           |
| --------------- | --------------------------------------------------------------------- |
| **config**/     | Interface between `src/config` that filters out the unused properties |
| **datasource**/ | Data access layer. They are very similar to repositories              |
| **generated**/  | Auto generated Schema from GraphQL                                    |
| **resolver**/   | GraphQL Resolvers definition                                          |
| **sanitizer**/  | Helpers that cleans some data structure                               |
| **schema**/     | GraphQL Schema definition                                             |
| **types**/      | TypeScript types definitions                                          |
| **utils**/      | Auxiliary functions                                                   |
| server.ts       | Compose and export a function with the ApolloServer                   |

#### config

The interface between `src/config` that filters out the unused properties.

#### datasource

Datasources here refer to [Apollo Data Sources](https://www.apollographql.com/docs/apollo-server/data/data-sources/). By quoting the docs they "encapsulate fetching data from a particular service, with built-in support for caching, deduplication, and error handling".

Each sub-folder here represents the communication mechanism (REST, Redis, MongoDB, etc...). Currently, we only have REST data sources.
Inside the folder, we have one file per entity.

#### resolver

Code that specifies how the GraphQL resolvers will, well, resolve. In the end, resolvers are just functions that know how to fetch specific data but they allow us great build very complex queries.

We follow the GraphQL resolver specification. The [Apollo Docs's Resolver section](https://www.apollographql.com/docs/apollo-server/data/resolvers) is a great material that explains the resolver chain.

#### schema

GraphQL schema specification. GraphQL has a basic yet flexible Schema Definition Language (SDL) that we use to define our Domain Specific Language (DSL).

Each file here represents an "entity". This is an entity in the domain sense not in a data-persistence context. We must aim to develop our schema in a way that will [closely map our business domain](https://graphql.org/learn/thinking-in-graphs/). Strictly, this means moving away from the REST "resource-based" approach in favor of modeling queries and mutations in a format that is easily consumed by the API clients. A very contrived example in the context of `offer moderation` would be to have `approveOffer`, `rejectOffer` and `submitOffer` instead of just a `updateOffer` mutation.

Refer to the [Apollo documentation](https://www.apollographql.com/docs/apollo-server/schema/schema/).

#### server.ts

The GraphQL-server entry point.

This file is responsible for creating the executable schema, bundling the required dependencies and passing the configuration object to the ApolloServer instance.

### src/shared/infrastructure

External services configuration and setup. Databases, Queue, etc..

### src/shared/loaders

Split the startup process into modules.

Loaders are responsible to start the application. Code here will either configure or boot up a module. The Clean Code association for that would be "Separate Constructing a System from Using It".

### src/tests

Application unit and integration tests

Tests are responsible for the quality of the code, whenever a Controller or Services are created, unit and integration tests must be carried out. It is essential that repositoryInMemory is used for this case, that way, the database will not be affected.
### src/shared/types

TypeScript type definitions

Files here should be type definitions. Nothing else. Type definitions could either be a `.d.ts` file or a `.ts` file with our SDL types.

TODO: Types are very important. We should improve ours very soon.

Currently, we are using the file `index.ts` as a general definition file that will contain any shared type for our system. A single file is useful to provide a boundary for type annotations and is easily removable from dependencies diagrams. What this mean is that we don't need to import `UserId from ../../../somewhere/very/far-away` and just import the type definition from `~/types`. With that being said, we could use some refactor inside this module in order to have separated files for specific types but a general export that will have the same result as a big blob of definitions.

### src/shared/utils

Auxiliary functions

Every project has it's utils folder. Code should be placed here if it is a nice (and small) utility that could be useful in many places (e.g: string manipulation, data structures, etc...) or if it does not fit anywhere else. That second case should be rare and we must try to avoid it.

## Queue

### Job/Workers definition

In order to create new job/workers you should create a new file and define an object that respects the `JobHandler<T>` type.
After that, you must be sure that this object is exported and that it is present either in the exported object of the module's `jobs/index.ts` or in the application-level `src/jobs/index.ts`.

Note: We are currently using [bullmq](https://github.com/taskforcesh/bullmq) a Redis-based queue in a manner that we will create a specific queue for each job name. Because of that, the job name is also the queue name.