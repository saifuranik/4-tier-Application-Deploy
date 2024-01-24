# use multistage build 
#start with node image
#finally use nginx image

#node image
#COPY source dest
#install
#build
#nginx image
#COPY build folder from previous stage to this stage
#copy nginx conf file to the container
#start nginx


# Stage 1: Build React App
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

