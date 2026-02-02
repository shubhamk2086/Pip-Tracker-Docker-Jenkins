FROM node:18-alpine AS build

WORKDIR /app
COPY PIP_Tracker/package*.json ./
RUN npm install

COPY PIP_Tracker/ ./
RUN npm run build -- PIP_Tracker --configuration production

FROM nginx:alpine
COPY default.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist/pip-tracker/browser /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

