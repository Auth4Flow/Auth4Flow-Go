# Warrant Go Library

Use [Forge4Flow](https://github.com/Forge4Flow) in server-side Go projects.

## Installation

```shell
go get github.com/forge4flow/forge4flow-go
```

## Usage

You can use the Forge4Flow SDK with or without a client. Instantiating a client allows you to create different client instances each with their own config (API key, API endpoint, etc).

### Without a Client

```go
import "github.com/forge4flow/forge4flow-go"

// Setup
warrant.ApiKey = "api_test_f5dsKVeYnVSLHGje44zAygqgqXiLJBICbFzCiAg1E="

// Create warrant
warrant, err := warrant.Create(&warrant.WarrantParams{})

// Create tenant
tenant, err := tenant.Create(&tenant.TenantParams{})
```

### With a Client

Instantiate the Warrant client with your API key to get started:
```go
import "github.com/forge4flow/forge4flow-go"
import "github.com/forge4flow/forge4flow-go/config"

client := warrant.NewClient(config.ClientConfig{
    ApiKey: "api_test_f5dsKVeYnVSLHGje44zAygqgqXiLJBICbFzCiAg1E=",
	ApiEndpoint: "https://api.warrant.dev",
	AuthorizeEndpoint: "https://api.warrant.dev",
	SelfServiceDashEndpoint: "https://self-serve.warrant.dev",
})
```

## Configuring Endpoints
The API, Authorize, and Self-Service endpoints the SDK makes requests to are configurable via the `warrant.ApiEndpoint`, `warrant.AuthorizeEndpoint`, `warrant.SelfServiceDashEndpoint` attributes:

```go
import "github.com/forge4flow/forge4flow-go"
import "github.com/forge4flow/forge4flow-go/config"

// Without client initialization
// Set api and authorize endpoints to http://localhost:8000
warrant.ApiEndpoint = "http://localhost:8000"
warrant.AuthorizeEndpoint = "http://localhost:8000"

// With client initialization
// Set api and authorize endpoints to http://localhost:8000 and self-service endpoint to http://localhost:8080
client := warrant.NewClient(config.ClientConfig{
    ApiKey: "api_test_f5dsKVeYnVSLHGje44zAygqgqXiLJBICbFzCiAg1E=",
	ApiEndpoint: "http://localhost:8000",
	AuthorizeEndpoint: "http://localhost:8000",
	SelfServiceDashEndpoint: "http://localhost:8080",
})
```

## Examples

### Users

```go
// Create
createdUser, err := user.Create(&warrant.UserParams{
    UserId: "userId",
})

// Get
user, err := user.Get("userId")


// Delete
err = user.Delete("userId")
```

### Warrants

```go

// Create
createdWarrant, err := warrant.Create(&warrant.WarrantParams{
	ObjectType: "tenant",
	ObjectId:   "1",
	Relation:   "member",
	Subject: warrant.Subject{
		ObjectType: "user",
		ObjectId:   "1",
	},
})

// Delete
err = warrant.Delete(&warrant.WarrantParams{
	ObjectType: "tenant",
	ObjectId:   "1",
	Relation:   "member",
	Subject: warrant.Subject{
		ObjectType: "user",
		ObjectId:   "1",
	},
})

// Check access
isAuthorized, err := warrant.Check(&warrant.WarrantCheckParams{
	Object: warrant.Object{
		ObjectType: "tenant",
		ObjectId:   "1",
	},
	Relation: "member",
	Subject: warrant.Subject{
		ObjectType: "user",
		ObjectId:   "1",
	},
})
```


We’ve used a random API key in these code examples. Replace it with your
[actual publishable API keys](https://app.warrant.dev) to
test this code through your own Warrant account.

For more information on how to use the Warrant API, please refer to the
[Warrant API reference](https://docs.warrant.dev).

Note that we may release new [minor and patch](https://semver.org/) versions of this library with small but backwards-incompatible fixes to the type declarations. These changes will not affect Warrant itself.

## Warrant Documentation

- [Warrant Docs](https://docs.warrant.dev/)
