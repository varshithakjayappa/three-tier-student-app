FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
ARG REACT_APP_API_BASE_URL
ENV REACT_APP_API_BASE_URL=$REACT_APP_API_BASE_URL
RUN npm run build
#--second stage----
FROM nginx:alpine
RUN addgroup -S frontend \
    && adduser -S frontendUser -G frontend
RUN chown -R frontendUser:frontend /usr/share/nginx/html
RUN mkdir -p /var/cache/nginx /run/ \
   && chown -R frontendUser:frontend /var/cache/nginx /run/
RUN apk add --no-cache curl    
COPY --from=build /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
HEALTHCHECK --interval=30s --timeout=5s --start-period=15s --retries=3 \
  CMD curl -f http://localhost:3000/ || exit 1
EXPOSE 3000
USER frontendUser
CMD ["nginx", "-g", "daemon off;"]
