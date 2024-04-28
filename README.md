# Tile38 Client for Go
[![Go](https://github.com/forrest321/go-tile38-client/workflows/Go/badge.svg)](https://github.com/forrest321/go-tile38-client/actions)
[![Documentation](https://pkg.go.dev/badge/github.com/forrest321/go-tile38-client)](https://pkg.go.dev/github.com/forrest321/go-tile38-client?tab=doc)
[![Go Report Card](https://goreportcard.com/badge/github.com/forrest321/go-tile38-client)](https://goreportcard.com/report/github.com/forrest321/go-tile38-client)
[![codecov](https://codecov.io/gh/forrest321/go-tile38-client/branch/master/graph/badge.svg)](https://codecov.io/gh/forrest321/go-tile38-client)
[![license](https://img.shields.io/github/license/forrest321/go-tile38-client.svg)](https://github.com/forrest321/go-tile38-client/blob/master/LICENSE)

See what [Tile38](https://tile38.com/) is all about.

- [Supported features](TODO.md)
- [Examples](examples)

## Installation

```
go get github.com/forrest321/go-tile38-client@latest
```

## Basic example

```go
package main

import (
	"fmt"

	"github.com/forrest321/go-tile38-client"
)

func main() {
	client, err := t38c.New(t38c.Config{
		Address: "localhost:9851",
		Debug:   true, // print queries to stdout
	})
	if err != nil {
		panic(err)
	}
	defer client.Close()

	if err := client.Keys.Set("fleet", "truck1").Point(33.5123, -112.2693).Do(context.TODO()); err != nil {
		panic(err)
	}

	if err := client.Keys.Set("fleet", "truck2").Point(33.4626, -112.1695).
		Field("speed", 20). // optional
		Expiration(20).     // optional
		Do(context.TODO()); err != nil {
		panic(err)
	}

	// search 6 kilometers around a point. returns one truck.
	response, err := client.Search.Nearby("fleet", 33.462, -112.268, 6000).
		Where("speed", 0, 100).
		Match("truck*").
		Format(t38c.FormatPoints).
		Do(context.TODO())
	if err != nil {
		panic(err)
	}

	// truck1 {33.5123 -112.2693}
	fmt.Println(response.Points[0].ID, response.Points[0].Point)
}
```

## License

Source code is available under the [MIT License](LICENSE).