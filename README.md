# SSO Protobuf Contracts (`protos`)

This repository contains **Protocol Buffers** definitions and generated **gRPC Go code** for the Single Sign-On (SSO) authentication microservice.

> **Credits & Acknowledgements:**  
> This project was developed as an educational project following the YouTube tutorial series by **Nikolay Tuzov** ([Николай Тузов - Golang](https://www.youtube.com/@NikolayTuzov)).

---

## Tech Stack

- **Schema Definition:** Protocol Buffers v3 (`proto3`)
- **Protocol:** [gRPC](https://grpc.io/)
- **Target Language:** Go (Golang)
- **Compilers & Plugins:** `protoc`, `protoc-gen-go`, `protoc-gen-go-grpc`
- **Automation:** [Taskfile](https://taskfile.dev/)

---

## Services & RPC Methods

Defined in `proto/sso/sso.proto` (`package auth`):

### `service Auth`
- **`Register`**: Registers a new user with email and password, returning `user_id`.
- **`Login`**: Authenticates user credentials for a specific `app_id` and returns a signed authentication token.
- **`IsAdmin`**: Validates whether a specific `user_id` possesses administrative privileges.

---

## Project Structure

```text
.
├── gen/
│   └── go/
│       └── sso/             # Generated Go code (do not edit manually)
│           ├── sso.pb.go
│           └── sso_grpc.pb.go
├── proto/
│   └── sso/
│       └── sso.proto        # Protocol Buffers source schema
├── Taskfile.yaml            # Code generation automation
├── go.mod                   # Go module definition
└── go.sum
```

---

## Generating Code

### Prerequisites
Make sure you have `protoc` and the Go plugins installed:
```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

### Build / Generate Command

Run the generation task via [Task](https://taskfile.dev/):
```bash
task generate
```
*(Or the equivalent `protoc` command configured in your `Taskfile.yaml`).*

---

## Usage as a Dependency

To import and use these generated contracts in your Go microservices:

```bash
go get -u github.com/loundxr/protos
```

### In Go Code:
```go
import ssov1 "github.com/loundxr/protos/gen/go/sso"

// Example: Calling the gRPC Auth Client
client := ssov1.NewAuthClient(conn)
resp, err := client.Login(ctx, &ssov1.LoginRequest{
    Email:    "user@example.com",
    Password: "password",
    AppId:    1,
})
```