# Firefly III DBT Models

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)

This repository contains dbt (data build tool) models for integrating Firefly III data with Lightdash for analytics and visualization.

## Overview

[Firefly III](https://www.firefly-iii.org/) is a free and open-source personal finance manager. This repository provides dbt models that transform Firefly III database tables into analytics-ready views that can be used with [Lightdash](https://lightdash.com/), an open-source BI tool built for dbt.

Note, currently, the it's just basic models as I do not need a more complex structure with metrics right now. (I just jump into SQL and do what I need.); If someone wants to contribute more complex models, please feel free to do so.

## Prerequisites

- [dbt-core](https://docs.getdbt.com/docs/core/installation) installed. (Might be optional, do it if it doesn't work without installing dbt-core.)
- A running Firefly III instance with database access
- Lightdash instance (self-hosted or cloud)
- PostgreSQL database credentials for your Firefly III database
- Node & npm (or pnpm) installed (for Lightdash CLI commands)

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/atb00ker/firefly-iii-dbt.git
   cd firefly-iii-dbt
   ```

2. Create a `profiles.yml` file in your `.dbt/` directory (usually at `~/.dbt/profiles.yml`) with your Firefly III database credentials:

   ```yaml
   firefly_iii:
     outputs:
       dev:
         type: postgres
         host: [your_database_host]
         user: [your_database_user]
         password: [your_database_password]
         port: 5432
         dbname: [your_firefly_database_name]
         schema: public
         threads: 4
     target: dev
   ```

### Connecting to Lightdash

_Note:_ It expects the machine pushing the dbt models to have access to the database.
In my case, that's not possible, so I just created a fake database with no tables or data in my host machine (and updated the `profiles.yml` file to point to that fake database) and pushed the models.
Then, I went the Lightdash UI and changed the database connection parameters.

1. If you're using a self-signed certificate in your Lightdash instance, you may need to set the following environment variable:

   ```bash
   export NODE_TLS_REJECT_UNAUTHORIZED=0
   ```

2. Install Lightdash CLI:

   ```bash
   pnpm install @lightdash/cli
   ```

3. Login to your Lightdash instance:

   ```bash
   pnpm lightdash login YOUR_URL --token YOUR_TOKEN
   ```

4. Deploy your dbt project to Lightdash (if already configured in Lightdash):

   ```bash
   pnpm lightdash deploy --create
   # Update the project using
   pnpm lightdash deploy
   ```

## Available Models

The following models are available in this project:

- `transactions`: Financial transactions from Firefly III
- `budgets`: Budget information
- `account_types`: Types of accounts in Firefly III
- `accounts`: All financial accounts
- `categories`: Transaction categories
- `tags`: Transaction tags
- `fake_accounts`: This is not related to Firefly III, feel free to delete it.

## Customization

You can customize the models by modifying the SQL files in the `models/firefly/` directory. Each model is currently set up as a simple view that pulls data directly from the corresponding table in the Firefly III database.

## Troubleshooting

- If you encounter issues with SSL certificates when connecting to Lightdash, use the `NODE_TLS_REJECT_UNAUTHORIZED=0` environment variable.
- Make sure your database user has read access to all the required Firefly III tables.
- If models fail to run, check your database connection details in the profiles.yml file.
