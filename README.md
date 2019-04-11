#1
Blah


Blurp

AWAIT

STYLEBENDER







Closes #1

Eats #1

### Pup
The Ultimate Boilerplate for Products.

[Read the Documentation](https://cleverbeagle.com/pup)

[![CircleCI](https://circleci.com/gh/cleverbeagle/pup.svg?style=shield)](https://circleci.com/gh/cleverbeagle/pup)

Want to work side-by-side with an experienced, trusted mentor? [Check out Clever Beagle](http://cleverbeagle.com).

---

**NOTE:** The following represents example `README.md` content for your product. **_The information below should be customized for your product._**

---

1. Infrastructure
2. Settings & Configuration
3. Dependencies
4. Commands
5. Git & Branching
6. Testing
7. Releasing

### 1. Infrastructure

The following explains how the `production` and `staging` environments for this app are managed and configured.

#### Where is DNS configured for this app?

DNS for the app is configured and managed via [DNSimple](https://dnsimple.com).

#### Where does the database live?

The database is hosted via [Compose](https://compose.com). A single deployment `cleverbeagle` exists with a `pup` database for the `production` environment and a `pup-staging` database for the `staging` environment. Additionally, the "Oplog Access" add-on has been enabled to [improve the performance of Meteor in `production`](https://galaxy-guide.meteor.com/apm-optimize-your-app-for-oplog.html).

#### Where does this app live?

The app is deployed to [https://galaxy.meteor.com](https://galaxy.meteor.com). It has two versions:

1. `staging` which is accessed via `https://pup-staging.cleverbeagle.com` and is used to test a release in a production environment _before_ being deployed to `production`.
2. `production` which is accessed via `https://pup.cleverbeagle.com` and is the live, customer-facing server.

Deployment to these domains is controlled via NPM scripts defined in the `package.json` file at the root of the project, `npm run staging` and `npm run production`.

#### How does the SSL work?

SSL certificates are generated via the UI at [https://galaxy.meteor.com/cleverbeagle](https://galaxy.meteor.com/cleverbeagle). Each application's certificates are managed via the app's settings page:

- [Staging Server Settings Dashboard](https://galaxy.meteor.com/app/pup-staging.cleverbeagle.com/settings)
- [Production Server Settings Dashboard](https://galaxy.meteor.com/app/pup.cleverbeagle.com/settings)

SSL certificates are auto-generated by Galaxy using the Let's Encrypt Certificate Authority and shouldn't require any maintenance. If maintenance or edits are required, locate the "Domains & Encryption" section of the app's settings page (linked above) and click on the domain you'd like to manage.

### 2. Settings & Configuration

Settings for the app are defined in three files at the root of the project:

- `settings-development.json` contains the settings specific to the `development` environment (i.e., when running the app on your computer).
- `settings-staging.json` contains the settings specific to the `staging` environmnet (i.e., when deploying the app to `pup-staging.cleverbeagle.com`).
- `settings-production.json` contains the settings specific to the `production` environment (i.e., when deploying the app to `pup.cleverbeagle.com`).

Each settings file should **_only be used in conjunction with the environment it's intended for_**. Further, each settings file's contents should be restricted to that specific environment (i.e., don't use an API key intended for the `production` environment in `development` and vice-versa—only break this rule when a given service's API key provisioning makes this prohibitive).

#### How do I load the settings file?

Settings files are automatically loaded for you as part of the NPM commands listed below. It's best to rely on these instead of doing it manually.

#### How do I access the accounts used in the settings files?

If you need to obtain an API key, password, or other secret information, you can find this in the [Clever Beagle 1Password Vault](https://cleverbeagle.1password.com). If you do not have access to this, send a direct message to @rglover in Slack or email him ryan.glover@cleverbeagle.com.

### 3. Dependencies

Dependencies for `pup.cleverbeagle.com` are installed via NPM and Meteor's Atmosphere package system. Atmosphere dependencies are installed automatically on app startup. NPM dependencies can be installed with the following command **before the first startup of the application**:

```
meteor npm install
```

### 4. Commands

The following NPM commands can be used when working on the app.

#### dev

```
$ npm run dev
```

Runs the app development server at `http://localhost:3000` and loads the `settings-development.json` file.

#### staging

```
$ npm run staging
```

Deploys the app to the staging server at `https://pup-staging.cleverbeagle.com` and loads the `settings-staging.json` file.

#### production

```
$ npm run production
```

Deploys the app to the production server at `https://pup.cleverbeagle.com` and loads the `settings-production.json` file.

#### test

```
$ npm run test
```

Runs all [Jest](https://jestjs.io/) test suites in the app once and then quits.

#### test-watch

```
$ npm run test-watch
```

Runs all [Jest](https://jestjs.io/) test suites in the app in watch mode and reruns whenever a test or file in the app changes.

#### test-e2e

```
$ npm run test-e2e
```

Runs all end-to-end tests using [TestCafe](https://github.com/DevExpress/testcafe) once and then quits.

### 5. Git & Branching

[Read the "Managing branches in Git" tutorial in the Pup docs](https://cleverbeagle.com/pup/v2/tutorials/managing-branches-in-git)

### 6. Testing

There are two types of testing performed in relation to the app: manual and automated. Manual testing is any testing where you're _manually_ clicking around the app yourself to test things out. Automated testing is any where you're relying on the automated test suites in the app to test things out.

#### Test Users

When you start the app for the first time in development and staging mode, we create a set of test users to use when testing different permissions:

| Email Address | Password | Roles | Notes |
|:----------------|:--------:|:-------:|:-------------------------------|
| admin@admin.com | password | `admin` | Full access to the application.
| user+1@test.com | password | `user`  | Access to user-only features.
| user+2@test.com | password | `user`  | Access to user-only features.
| user+3@test.com | password | `user`  | Access to user-only features.
| user+4@test.com | password | `user`  | Access to user-only features.
| user+5@test.com | password | `user`  | Access to user-only features.

#### Test Data

When you start the app for the first time, we create test data for all collections in the application. If you ever want to "start over" with fresh data in your `development` environment, stop the app and in your terminal run:

```
meteor reset
```

Upon restarting the app, the databased will be reseeded with the default test data.

**FAIR WARNING**: This command will **PERMANENTLY ERASE** (😈) any data that you've added to the app manually. If you've added something that you will need/want after the reset, make sure to back it up first.

#### Writing Tests

[Read the "Writing and running automated tests" tutorial in the Pup docs](https://cleverbeagle.com/pup/v2/tutorials/writing-and-running-automated-tests)

### 7. Releasing

Releasing the app to both the `staging` and `production` environment should be performed primarily via continuous integration. This is configured via Circle CI and the `.circleci/config.yml` file at the root of the app.

If an emergency deployment is required, the `npm run staging` and `npm run production` commands detailed in the "NPM Commands" section above can be utilized.

#### Performing a release

In order for a release to be pushed to either the `staging` or `production` environment via continuous integration, code must be pushed to:

- The `master` branch when releasing new code into production.
- The `staging` branch when releasing new code to the `staging` server.

When code is pushed, the continuous integration service should pick this change up and automatically deploy per the rules in the `.circleci/config.yml` file in the app.

#### Tagging releases on master

When code has been tested and confirmed ready for production, it's important to tag the release in Git so that it's clear when and where certain code is introduced. In order to tag a release, it's recommended that the [Git Extras library](https://github.com/tj/git-extras/blob/master/Commands.md#git-release) be used, specifically, the `git release <Semantic Version Number>` command be utilized.

This pushed the code to the `master` branch while also creating a tag locally and remotely, all simultaneously.

For more information on Semantic Versioning, [visit the official documentation site](https://semver.org/).
