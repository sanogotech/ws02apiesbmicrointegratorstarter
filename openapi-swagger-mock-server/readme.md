# API Mock Server / Swagger UI

This tutorial provide a quick way to setup a Mock Server and its Swagger UI interface trough OpenAPI specification files.

## Installation

Use following link [docker](https://docs.docker.com/docker-for-mac/install/) to install docker.

This is the only requirement. 

## Usage

```bash
% cd $mock_server

% docker-compose up 

or 

% docker-compose up -d (detached mode)
```
The previous commands will run the docker-compose.yml file included in the folder. 

Once completed, the following services will be available:

- http://localhost/swagger -> Swagger UI Implementation / API Documentation
- http://localhost:8080/ -> API mock server.
