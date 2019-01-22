FROM golang:1.10.1
WORKDIR /go/src/github.com/pritidesai/experiments-with-knative/sample-app/
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -v -o app

FROM scratch
COPY --from=0 /go/src/github.com/pritidesai/experiments-with-knative/sample-app/app .
ENTRYPOINT ["/app"]
