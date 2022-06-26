# Lab 1: Frontend

planning for component Post allocations:
  * frontend: 9080
  * broker service: 8080
  * authentication service 9090


```
cd go-micro/frontend
go mod init frontend
go run ./cmd/web
```

* visit [visit: http://localhost:9080](visit: http://localhost:9080)

# Lab 2: Broker microservice

```
mkdir go-micro/broker-service
cd go-micro/broker-service
go mod init broker
mkdir -p cmd/api
go get github.com/go-chi/chi/v5
go get github.com/go-chi/cors
```

```
cd go-micro/broker-service
go run ./cmd/api
```

# Lab3: Run Broker microservice in Docker

## run without Docker

* making sure the frontend web is calling backend Broker microservice at the correct url, see `go-micro/front-end/cmd/web/templates/test.page.gohtml` fetch statement.

```
cd go-micro/front-end
go run ./cmd/web/

cd go-micro/broker-service
go run ./cmd/api
```

* start the frontend web
* start the backend Broker microservice
* from [the frontend](http://localhost:9080), click the **Test Broker** button.

## Build multistage Docker image for the Broker microservice

* making sure that your port mapping `localport:dockerport` are correctly specified, e.g. 8080:8080

  * the first port if to be exposed on the local server, it should not conflict with the frontend port (e.g. 9080)
  * and the 2nd port is from the `go-micro/broker-service/cmd/api/main.go` webPort.
  * you may have to delete the existing image if you are making changes to the Broker service

```
mkdir go-micro/project
touch go-micro/broker-service/broker-service.dockerfile
touch go-micro/project/docker-compose.yml

cd go-micro/front-end
go run ./cmd/web/
cd go-micro/project
docker image rm project_broker-service
docker-compose down && docker-compose up -d
```

* start the frontend web
* start the docker Broker microservice image
* from [the frontend](http://localhost:9080), click the **Test Broker** button.

# Lab 4: Add JS Helpers

```
touch go-micro/broker-service/cmd/api/helpers.go

cd go-micro/front-end
go run ./cmd/web/
cd go-micro/project
docker-compose down 
docker image rm project_broker-service
docker-compose down && docker-compose up -d
```

# Lab 5: Using make to build

* using make to build
* remove build stage from the `go-micro/broker-service/broker-service.dockerfile` multistage docker file


```
cd go-micro/project
make stop
make down
make up_build
make start
```

* from [the frontend](http://localhost:9080), click the **Test Broker** button.

# Lab 6: Add Authentication Service

Port: 9090

```
mkdir go-micro/authentication-service
cd go-micro/authentication-service

go mod init authentication
go get github.com/go-chi/chi/v5
go get github.com/go-chi/cors
go get github.com/go-chi/chi/v5/middleware
go get github.com/jackc/pgconn
go get github.com/jackc/pgx/v4/stdlib

mkdir -p cmd/api
touch cmd/api/main.go
touch cmd/api/route.go
mkdir data
touch data/models.go
```

* modify Makefile to build_auth
* go run ./cmd/api to run auth service


# Lab 7: Add PostgreSQL 14 to Docker 

* modify docker-compose.yml
  * add authentication service
  * add postgresql

```
make up_build
```

* authentication servicer log should show Connected to Postgres!

# Lab 8: Create the users sql schema

# Lab 9: Add routes to authentication service

* re-use the json helpers (or github.com/tsawler/toolbox)
* add routes
* add route handlers

# Lab 10: Update frontend to call Authentication Service

```
project_authentication-service
make stop
make down
docker image rm project_authentication-service
docker image rm project_broker-service
make up_build
make start
```

* from [the frontend](http://localhost:9080), click the **Test Broker** button.

# Lab 11: Add MongoDB for logger service

```
mkdir -p logger-service/cmd/api
go mod init log-service
go mod tidy
touch logger-service/cmd/api/main.go
go get go.mongodb.org/mongo-driver/mongo
```

# Lab 12: Setting up routes & handlers for logger service

* add routes
* add handlers
* add app and serve

# Lab 13: Add MongoDB to docker-compose.yml

```
cd go-micro/project
make up
mongo admin -u admin -p password
cd go-micro/logger-service
go run ./cmd/api/
```

# Lab 14: Add logger build scripts & add logger to docker-compose.yml

```
make stop
make down
docker image rm project_authentication-service
docker image rm project_broker-service
docker image rm project_logger-service
make up_build
make start
```

# Lab 15: Connect Broker microservice to the logger service

```
```

# Lab 16: Connect Authentication service to the logger service

```
make stop
make down
docker image rm project_authentication-service
docker image rm project_broker-service
docker image rm project_logger-service
make up_build
make start
```

browse to frontend [at http://localhost:9080](http://localhost:9080)

# Lab 17: Add mailhog to containers

- Add mailhog 'mailhog/mailhog:latest' to docker

```
cd project
make stop
make down
docker image rm project_authentication-service
docker image rm project_broker-service
docker image rm project_logger-service
make up_build
make start
```

Browse to mailhot web page [http://localhost:8025](http://localhost:8025)

# Lab 18: Add mail service

```
mkdir -p mail-serivce/cmd/api
cd mail-service
go mod init mailer-service
go mod tidy
```

- define the mailer interface: type Mail struc{}
- define the mail message structure.

```
cd project
make stop
make down
docker image rm project_broker-service
docker image rm project_postgres
docker image rm project_authentication-service
docker image rm project_mongo
docker image rm project_logger-service
docker image rm project_mailhog
docker image rm project_mailer-service
make up_build
make start
```

# Lab 19: Modifying the Broker service to handle mail

# Lab 20: Updating the front end to send mail

```
cd project
make stop
make down
docker image rm project_broker-service
docker image rm project_postgres
docker image rm project_authentication-service
docker image rm project_mongo
docker image rm project_logger-service
docker image rm project_mailhog
docker image rm project_mailer-service
make up_build
make start
```

- Browse to the frontend web page [http://localhost:9080](http://localhost:9080) to test send mail
- Browse to the mailhot web page [http://localhost:8025](http://localhost:8025) to view mail received

# Lab 21: Add RabbitMQ Stub Listener

- add rabbitmq to docker-compose.yml

```
mkdir listener-service
touch main.go
go mod init listener
```

```
docker-compose up -d
cd listener-service
go mod tidy
go run .
```

- the listener service should connect to the RabbitMQ

# Lab 22: Make Listener Service to run in the container

- Create docker file
- update Makefile
- update 

```
cd project
make stop
make down
docker image rm project_broker-service
docker image rm project_postgres
docker image rm project_authentication-service
docker image rm project_mongo
docker image rm project_logger-service
docker image rm project_mailhog
docker image rm project_mailer-service
docker image rm project_listener-service
make up_build
make start
```

# Lab 23: Update Broker & Frontend to handle listener


```
cd project
make stop
make down
docker image rm project_broker-service
docker image rm project_postgres
docker image rm project_authentication-service
docker image rm project_mongo
docker image rm project_logger-service
docker image rm project_mailhog
docker image rm project_mailer-service
docker image rm project_listener-service
make up_build
make start
```

Logging is now via RabbitMQ, check logged documents in MongoDB.

# Lab 24: Create logger RPC server

- define an interface (type)
- define a message struct
- add interface methods
  * LogInfo()

# Lab 25: Convert loggger service to use RPC

- be sure to rpc.Register(new(RPCServer)) before listening for rpc server.

```
cd project
make stop
make down
docker image rm project_broker-service
docker image rm project_postgres
docker image rm project_authentication-service
docker image rm project_mongo
docker image rm project_logger-service
docker image rm project_mailhog
docker image rm project_mailer-service
docker image rm project_listener-service
make up_build
make start
```

Logging is now via gRPC, check logged documents in MongoDB.

# Enable gRPC for logger

## Lab 26: Convert Logger to use gRPC 

[protoc-gen-go-grpc](https://grpc.io/docs/languages/go/quickstart/)

[protoc-gen-go](https://pkg.go.dev/github.com/golang/protobuf/protoc-gen-go)

```
go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2
brew install protobuf
protoc --version
```

```
mkdir -p logger-service/logs
touch mkdir -p logger-service/logs/logs.proto
protoc --go_out=. --go_opt=paths=source_relative --go-grpc_out=. --go-grpc_opt=paths=source_relative logs.proto
```

```
touch logger-service/cmd/api/grpc.go
go mod tidy
```

## Lab 27 Convert Broker service to use gRPC Logger

```
mkdir -p broker-service/logs
cp logger-service/logs/logs.proto broker-service/logs/logs.proto
protoc --go_out=. --go_opt=paths=source_relative --go-grpc_out=. --go-grpc_opt=paths=source_relative logs.proto
```

```
go mod tidy
```

## Lab 28: Convert frontend to use gRPC endpoint

```
cd project
make stop
make down
docker image rm project_broker-service
docker image rm project_postgres
docker image rm project_authentication-service
docker image rm project_mongo
docker image rm project_logger-service
docker image rm project_mailhog
docker image rm project_mailer-service
docker image rm project_listener-service
make up_build
make start
```