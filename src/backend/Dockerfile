FROM node:20.11.0-alpine as build
WORKDIR /build
COPY package*.json ./
RUN npm ci --only=production

FROM node:20.11.0-alpine
WORKDIR /app
COPY --from=build /build/node_modules ./node_modules
COPY . .

EXPOSE 5050

CMD ["node", "server.js"]
