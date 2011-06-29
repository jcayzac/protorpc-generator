# ProtoRPC generators
## Requirements
Well, of course, [libprotobuf](http://code.google.com/p/protobuf/).
If you install through [MacPorts](http://www.macports.org/), then this should be enough:

    $ sudo port install protobuf-cpp protobuf-pythonVERSION # protobuf-python25, protobuf-python26 or protobuf-python27

## How to generate the example service
Running the following:

    $ ./proto2rpc hello.proto

...will create these source files:

    gen_rpc_client_cpp:
        hello.pb.cc  hello.pb.h
    
    gen_rpc_client_objc:
        hello.rpc.h  hello.rpc.mm
    
    gen_rpc_services:
        __init__.py  hello.py

## Testing the example service
Assuming you running app.yaml on port 8081,

    $ echo "my_name:'$USER'" | \
        protoc --encode=hello.HelloRequest hello.proto | \
        curl 2>/dev/null -H 'content-type:application/x-google-protobuf' \
            --data-binary @- http://localhost:8081/rpc/HelloService.hello | \
        protoc --decode=hello.HelloResponse hello.proto
    hello: "Hey, hello jcayzac!"

