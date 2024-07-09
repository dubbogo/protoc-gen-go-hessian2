# protoc-gen-go-hessian2

## Prerequisites
Before using `protoc-gen-go-hessian2`, make sure you have the following prerequisites installed on your system:
- Go (version 1.17 or higher)
- Protocol Buffers (version 3.0 or higher)

## Installation
To install `protoc-gen-go-hessian2`, you can use `go install`:
```shell
go install github.com/dubbogo/protoc-gen-go-hessian2
```

Or you can clone this repository and build it manually:
```shell
git clone github.com/dubbogo/protoc-gen-go-hessian2
cd protoc-gen-go-hessian2
go build
```

## Usage
To generate Hessian2 code using `protoc-gen-go-hessian2`, you can use the following command:
```shell
protoc --go-hessian2_out=. --go-hessian2_opt=paths=source_relative your_file.proto
```
The `--go-hessian2_out` option specifies the output directory for the generated code, 
and `--go-hessian2_opt=paths=source_relative` sets the output path to be relative to the source file.

## Example
To generate Hessian2 code for a simple message, you can create a `.proto` file with the following content:
```protobuf
syntax = "proto3";

package greet;

option go_package = "dubbo.apache.org/dubbo-go/v3/protocol/triple/internal/proto/dubbo3_gen;greet";

import "unified_idl_extend/unified_idl_extend.proto";

message GreetRequest {
  string name = 1;

  option (unified_idl_extend.message_extend) = {
    java_class_name: "org.apache.greet.GreetRequest";
  };
}

message GreetResponse {
  string greeting = 1;

  option (unified_idl_extend.message_extend) = {
    java_class_name: "org.apache.greet.GreetResponse";
  };
}
```
Note that you need to import the `unified_idl_extend` package and extend the message with the `message_extend` option 
and set `java_class_name` to the corresponding Java class name.

Then, you can run the following command to generate the Hessian2 code:
```shell
protoc --go-hessian2_out=. --go-hessian2_opt=paths=source_relative greet/greet.proto
```

This will generate the `greet.hessian2.go` file in the same directory as your `greet.proto` file:
```shell
.
├── greet.hessian2.go
├── greet.proto
└── unified_idl_extend
    ├── unified_idl_extend.pb.go
    └── unified_idl_extend.proto
```

The content of the `greet.hessian2.go` file will be:
```go
// Code generated by protoc-gen-go-dubbo. DO NOT EDIT.

// Source: greet.proto
// Package: greet

package greet

import (
	dubbo_go_hessian2 "github.com/apache/dubbo-go-hessian2"
)

type GreetRequest struct {
	Name string
}

func (x *GreetRequest) JavaClassName() string {
	return "org.apache.greet.GreetRequest"
}

func (x *GreetRequest) String() string {
	e := dubbo_go_hessian2.NewEncoder()
	err := e.Encode(x)
	if err != nil {
		return ""
	}
	return string(e.Buffer())
}

func (x *GreetRequest) GetName() string {
	if x != nil {
		return x.Name
	}
	return ""
}

type GreetResponse struct {
	Greeting string
}

func (x *GreetResponse) JavaClassName() string {
	return "org.apache.greet.GreetResponse"
}

func (x *GreetResponse) String() string {
	e := dubbo_go_hessian2.NewEncoder()
	err := e.Encode(x)
	if err != nil {
		return ""
	}
	return string(e.Buffer())
}

func (x *GreetResponse) GetGreeting() string {
	if x != nil {
		return x.Greeting
	}
	return ""
}

func init() {
	dubbo_go_hessian2.RegisterPOJO(new(GreetRequest))
	dubbo_go_hessian2.RegisterPOJO(new(GreetResponse))
}
```