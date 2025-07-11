# Imagem golang que servirá como base da imagem final https://hub.docker.com/_/golang
FROM golang:alpine AS build

# Diretório de trabalho dentro do container
WORKDIR /app

# Copia o arquivo main.go para o diretório de trabalho no container (/app)
COPY main.go .

# Faz constrói o binário do go para ser execultado https://go.dev/doc/tutorial/getting-started
# GO111MODULE=off força o go não utilizar módulos
# -ldflags="-s -w" remove símbolos de depuração para reduzir o tamanho do binário.
RUN GO111MODULE=off go build -ldflags="-s -w" -v -o fullcycle

# Usa a imagem especial do docker para contextos de imagens minimas https://hub.docker.com/_/scratch
FROM scratch

# Copia o binário buildado no estágio anterior para a nova imagem mínima
COPY --from=build /app/fullcycle /fullcycle

# Execulta o binário ao iniciar o container baseado nesta imagem
ENTRYPOINT [ "/fullcycle" ]



