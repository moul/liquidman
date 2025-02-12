FROM golang:1.24

# Configure environment and rebuild stdlib
ENV CGO_ENABLED=1 WEBDIR=/web GODIR=moul.io/radioman
RUN go install -a std

## Install deps
#RUN go get github.com/tools/godep \
# && go get github.com/golang/lint/golint \
# && go get golang.org/x/tools/cmd/goimports \
# && go get golang.org/x/tools/cmd/stringer

# Install dependencies
RUN apt-get update \
 && apt-get install -y -qq libtagc0-dev pkg-config \
 && apt-get clean


# Run the project
WORKDIR /go/src/${GODIR}/radioman
COPY go.sum go.mod ./
RUN go mod download
COPY pkg ./pkg
COPY cmd ./cmd
RUN go install ./cmd/radiomand
COPY web ${WEBDIR}
ENTRYPOINT ["radiomand"]
