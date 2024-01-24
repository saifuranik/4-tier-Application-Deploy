# use multistage build 
#start with node image
#finally use nginx image

FROM node:14 as build

WORKDIR /app


COPY package*.json ./


RUN npm install


COPY . .


RUN npm run build


FROM nginx:1.21


COPY --from=build /app/build /usr/share/nginx/html
COPY ./conf /etc/nginx/conf.d

EXPOSE 3000


CMD ["nginx", "-g", "daemon off;"]

