# neon-database-per-tenant-drizzle

The steps outlined below have been covered in more detail on the Neon blog: [Migrating schemas across many Neon projects using DrizzleORM](https://neon.tech/blog/migrating-schemas).

## Create a new Neon Database

Currently all database are created using `aws-us-east-1` under the account the `NEON_API_KEY` was generated for.

```
npm run create -- --name="ACME Corp"
```

## Generate migrations

Creates DrizzleORM config, migration directories, updates GitHub Repository secrets with Neon connection strings and updates GitHub Actions workflow file.

```
npm run generate
```

## Run migrations

Migrations are run from a GitHub Action when a PR is closed and merged is `true`. Environment variables are dynamically added to the GitHub Repository secrets and added in the workflow file under the `env` section.

```
name: Migrate changes

on:
  pull_request:
    types: [closed]
    branches:
      - main

env:
  ...

jobs:
  migrate:
    runs-on: ubuntu-latest
    if: github.event.pull_request.merged == true

    ...
```
