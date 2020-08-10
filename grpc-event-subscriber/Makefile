.PHONY: tidy test run event help
all: help

tidy: ## Updates the go modules and vendors all dependencies 
	go mod tidy

test: mod ## Tests the entire project 
	go test -count=1 -race ./...

run: tidy ## Runs uncompiled code in Dapr
	dapr run --app-id event-subscriber \
		 --app-port 50001 \
		 --protocol grpc \
		 --port 3500 \
         --components-path ./config \
         go run main.go

event: ## Publishes sample JSON message to Dapr pubsub API 
	curl -d '{ "from": "John", "to": "Lary", "message": "hi" }' \
     -H "Content-type: application/json" \
     "http://localhost:3500/v1.0/publish/events"

help: ## Display available commands
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk \
		'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'