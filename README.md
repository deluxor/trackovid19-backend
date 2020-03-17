# Trackovid19-Backend
### Guidelines:
- YARN as PKG Manager
- ASYNC/AWAIT Promise Style
- DB is PostgreSQL
- Sequelize as ORM using ASYNC Client
- FASTIFY as base framework (we need something simple yet speedy)
- FASTIFY based scaffolding (with some minor adjustments)
- JWT as Authentication and Authorization System
- JSON requests & responses
- This is to be run at AWS Lambda (via wrapper):
--> Your code is running in a serverless environment. You cannot rely on your server being 'up' in the sense that you can/should not use in-memory sessions, web sockets, etc. You are also subject to provider specific restrictions on request/response size, duration, etc.
### Installation
You will need atleast nodejs version 12 LTS to run this server.
### 1 - Generate Private and Public keys to be used with JWT:
```sh
$ openssl genrsa -out private.key 2048
$ openssl rsa -in private.key -outform PEM -pubout -out public.key
$ mv private.key public.key plugins/certs
```
### 2 - Install the dependencies and devDependencies and start the server:
```sh
$ yarn
$ yarn start
```
### 3 - Generate DB Models from Remote deployed PGSQL DB at AWS:
(Yeah yeah magic stuff, but keep in mind you need to do step 2 first)
```sh
$ yarn generate-db
```
After the task is done, you will see the models inside models folder with the current definition from the remote DB.
### Side Notes
You will see a weird snippet inside package.json:
```json
"peerDependencies": {
    "sequelize": "^5.21.5"
  }
```
This is to handle external dependencies for 3rd party modules, otherwise we will receive security warnings from deprecated / insecure modules.

### Documentation
1º - Login using your facebook account on: http://localhost:3000/login/facebook;
2º - You will receive a JWT token, use it as a Bearer Token in all the authenticated requests;
3º- Please load the `insomnia.json` file into your Insomnia Rest Client and call the routes or consult https://trackcovid19.docs.apiary.io;
PS: Do not forget to set the Bearer token on insomnia!
# Serverless usage

Install the framework:

```sh
npm install -g serverless
serverless plugin install --name serverless-stage-manager
```

Verify its installation:

```sh
serverless --version
```

In dev, use:


```sh
yarn start

```

Configure your environment:

Credentials were given before, use 'lambda' user.

```sh
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_PROFILE="default"
```

```sh
touch ~/.aws/config
```

Paste it inside:

```sh
[default]
region=eu-west-1
```

For dev deploy, use:

```sh
sls deploy --stage dev --db_pass 'PASSWORD'

```

For prd deploy, use:

```sh
sls deploy --stage dev --db_pass 'PASSWORD'
```

You should see

```sh
Serverless: Uploading artifacts...
Serverless: Uploading service trackovid19-backend.zip file to S3 (2.78 MB)...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
..............
Serverless: Stack update finished...
Service Information
service: trackovid19-backend
stage: dev
region: eu-west-1
stack: trackovid19-backend-dev
resources: 12
api keys:
  None
endpoints:
  ANY - https://2ceesuwr5l.execute-api.eu-west-1.amazonaws.com/dev/
  ANY - https://2ceesuwr5l.execute-api.eu-west-1.amazonaws.com/dev/{proxy+}
functions:
  server: trackovid19-backend-dev-server
layers:
  None
Serverless: Removing old service artifacts from S3...
Serverless: Run the "serverless" command to setup monitoring, troubleshooting and testing.

```

# Debug

```sh
sls logs --stage prd -f app -t
sls logs --stage dev -f app -t
```

# Local DB

A docker compose yml file was added for launching a local postgres database and a web based admin app (adminer). 

You can check/change all db parameters by editing the docker compose file:

```sh
docker/database/tracovid.yml
```

You can use the convenience start/stop scripts for starting/stopping the database, but first you need to add execute permissions:

```sh
chmod +x docker/database/*.sh
```

Then for starting the local database:

```sh
docker/database/start.sh
```

You can then go to your browser and access the database via adminer at http://localhost:8080


Stopping the local database:

```sh
docker/database/stop.sh
```


### Authors
Special thanks to @victorfern91 @hugoduraes @ludwig801 @palminha @lcfb91 @zepcp @jcazevedo @cchostak for the work and efforts to bootstrap this service!
Feel free to change / improve / delete everything you want!
