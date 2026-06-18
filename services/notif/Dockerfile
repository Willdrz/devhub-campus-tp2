# Squelette — étape 3.
# Cible : image FINALE ≤ 50 Mo (Go produit un binaire statique, profitez-en).

# ---- Stage 1 : build du binaire Go ----
FROM golang:1.22-alpine AS build

WORKDIR /src

# TODO : copier go.mod / go.sum et faire `go mod download` AVANT de copier le code.
# On copie go.mod EN PREMIER pour profiter du cache Docker :
# si le code change mais pas les dépendances, cette couche n'est pas reconstruite
COPY go.mod ./
RUN go mod download


# TODO : copier le code et compiler.
# Copie du code source et compilation
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /out/notif ./cmd

# Indice : `CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /out/notif ./cmd`

# ---- Stage 2 : runtime minimal ----
# TODO : choisir une image de runtime ULTRA-minimale (scratch ? distroless ? alpine ?)
# Comparez les tailles dans RAPPORT.md. Quel est le compromis pour chaque option ?
FROM gcr.io/distroless/static:nonroot AS runtime


# TODO : LABEL org.opencontainers.image.source.

LABEL org.opencontainers.image.source="https://github.com/willdrz/devhub-campus"

# TODO : copiez le binaire depuis le stage `build`.

COPY --from=build /out/notif /notif

ENV PORT=8080 \
    LOG_LEVEL=info

EXPOSE 8080

# USER nonroot:nonroot     # déjà fourni par l'image distroless
# TODO : ENTRYPOINT.
ENTRYPOINT ["/notif"]
