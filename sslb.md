# Repository
https://github.com/eduardonunesp/sslb

# Run

# Basic code path 1 : main - Start Listenner
main.go#main
- cli/app.go#CreateAPP()
  - cli/commands#NewServer()
    - lb/pool.go#NewWorkerPool
      - lb/pool.go#createPool
        - for i := 0; i <= wp.Configuration.WorkerPoolSize; i++ {
          - wp.Workers = append(wp.Workers, NewWorker())
        - }
  - cli/commands#RunServer(c \*cli.Context)
    - configuration := cfg.Setup(filename)
    - server := lb.NewServer(configuration)
    - sslbRPC.StartServer(server)
      - server := rpc.NewServer()
      - server.HandleHTTP(rpc.DefaultRPCPath, rpc.DefaultDebugPath)
      - listener, e := net.Listen("tcp", address)
      - for {
        - if conn, err := listener.Accept(); err == nil {
          - go server.ServeCodec(jsonrpc.NewServerCodec(conn))
        - }
      - }
    - server.Run()
    - signal.Notify(ch, syscall.SIGINT, syscall.SIGTERM)
    - server.Stop()

# Basic code path 2 : Server.Run
lb/server.go
func (s \*Server) Run()
- s.setup()
- for \_, frontend := range s.Frontends {
  - go s.RunFrontendServer(frontend)
- }

# Basic code path 3 : Server.RunFrontendServer
for \_, backend := range frontend.Backends {
  // Before start the backend let's set a monitor
  backend.HeartCheck()
}
.
.
.


# For each http request how frontend server select one backend server.
```
// Search for backend with the less score
func preProcessWorker(frontend *Frontend) *Backend {
	frontend.Lock()
	defer frontend.Unlock()

	var backendWithMinScore *Backend

	for idx, backend := range frontend.Backends {
		backend.RLock()
		if idx == 0 {
			backendWithMinScore = backend
		} else {
			if backend.Score < backendWithMinScore.Score {
				backendWithMinScore = backend
			}
		}
		backend.RUnlock()
	}

	return backendWithMinScore
}

```

# coroutines

- Server
  - source: rpc/listener.go#StartServer
  - invoked by: main coroutine
  - description: invoke one coroutine

- ServeCodec
  - source: rpc/listener.go
  - invoked by: Server coroutine
  - description: invoke one when accept one rpc request 

- RunFrontendServer
  - source: lb/server.go#Run
  - invoked by: main coroutine
  - description: invoke ones for each frontend configurations

- HeartCheck
  - lb/backend.go#HeartCheck
  - invoked by: RunFrontendServer coroutine
  - description: invoke ones for each backend configurations of the frontend

- worker in pool
  - lb/worker.go#
  - invoked by: RunFrontendServer coroutine
  - description: invoke one when one http request for FrontendServer arrives

- websocket frontend & websocket backend
  - lb/request.go#
  - invoked by: RunFrontendServer coroutine
  - description: HijackWebSocket

# What is basic roll

Server RPC:
  Host/Port:
    GeneralConfig.RPCHost/GeneralConfig.RPCPort
  Protocol:
    HTTP  net.rpc.HandleHTTP


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
