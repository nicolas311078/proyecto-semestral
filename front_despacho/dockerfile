# Etapa de Build
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Etapa de Producción (Nginx)
FROM nginx:alpine
# Copiamos los archivos construidos a la carpeta que usa Nginx para servir la web
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]