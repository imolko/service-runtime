
# Construimos Dlv y algun otra dependencia.
FROM golang:1.12-stretch as dependencies

# Buscamos git y curl.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        git \
        curl \
    && rm -rf /var/lib/apt/lists/*
    
# Instalamos dlv para debug.
RUN go get -v -u -d github.com/derekparker/delve/cmd/dlv && \
    cd $GOPATH/src/github.com/derekparker/delve && \
    go install github.com/derekparker/delve/cmd/dlv && \
    dlv version


#####
# Costruimos ubuntu

FROM ubuntu:bionic as ubuntu

RUN apt-get update -y \
    && apt-get install -y ca-certificates wget \
    && wget -O /usr/local/share/ca-certificates/cacert.crt https://imolko-dev.nyc3.digitaloceanspaces.com/certs/certs/ca-dev/cacert.crt \
    && update-ca-certificates \
    && apt-get purge -y wget \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean -y

# Creamos la carpeta tools/bin
RUN mkdir -p /imolko/tools/bin

# Agregamos los tools al PATH.
ENV PATH="${PATH}:/imolko/tools/bin/"

# Copiamos dlv.
COPY --from=dependencies /go/bin/dlv /imolko/tools/bin/
