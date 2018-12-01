# Repository
https://github.com/eduardonunesp/sslb

# Run

# Basic code path 1 : main - 
main.go#main
- cli/app.go#CreateAPP()
  - cli/commands#RunServer(c \*cli.Context)
    - configuration := cfg.Setup(filename)
    - server := lb.NewServer(configuration)
    - sslbRPC.StartServer(server)


# Sources
```
tree -h -P "*.go"
.
└── [ 384]  sslb
    ├── [  96]  Godeps
    ├── [ 160]  cfg
    │   ├── [1.8K]  fileparser.go
    │   ├── [4.2K]  fileparser_test.go
    │   └── [3.7K]  validate.go
    ├── [ 128]  cli
    │   ├── [ 876]  app.go
    │   └── [1.8K]  commands.go
    ├── [ 288]  lb
    │   ├── [2.5K]  backend.go
    │   ├── [ 719]  frontend.go
    │   ├── [ 488]  general.go
    │   ├── [1.2K]  pool.go
    │   ├── [2.1K]  request.go
    │   ├── [3.9K]  server.go
    │   └── [2.7K]  worker.go
    ├── [ 114]  main.go
    └── [ 128]  rpc
        └── [ 996]  listener.go

6 directories, 14 files
```
