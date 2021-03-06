# Module mechanism

Service center(SC) support an extend modules mechanism that developers can new some features in SC easily. 

## Just 4 steps, you can add a module in service center
1. Create a module(package) under the github.com/ServiceComb/service-center/server package.
1. Here you just need to implement the controller and service interfaces in your module.
1. And register service to SC when the module initializes.
1. Import the package in github.com/ServiceComb/service-center/server/bootstrap/bootstrap.go

## Quit start for the RESTful module

Implement the [ROAServantService](/pkg/rest/roa.go) interface.

```go
package hello

import (
	"net/http"
	"github.com/ServiceComb/service-center/pkg/rest"
)

type HelloService struct {
}

func (s *HelloService) URLPatterns() []rest.Route {
	return []rest.Route{
		{
		    rest.HTTP_METHOD_GET, // Method is one of the following: GET,PUT,POST,DELETE
		    "/helloWorld", // Path contains a path pattern
		    s.SayHello, // rest callback function for the specified Method and Path
        },
	}
}

func (s *HelloService) SayHello(w http.ResponseWriter, r *http.Request) {
    // say Hi
}
```

Register the service in SC ROA framework when the module initializes.

```go
package hello

import roa "github.com/ServiceComb/service-center/pkg/rest"

func init() {
    roa.RegisterServent(&HelloService{})
}
```

Modify [bootstarp.go](/server/bootstrap/bootstrap.go) file to import your module.

```go
// module
import _ "github.com/ServiceComb/service-center/server/hello"
```

## About GRPC module

To see [govern](/server/govern) module for help.