
Steps Have to Follow 

for more easy read follow this link: https://saifuranik.medium.com/deploy-4-tier-react-node-mongodb-application-with-nginx-67add197577c



![1_59gHJeRGZilJhZmm46b6eA](https://github.com/saifuranik/4-tier-Application-Deploy/assets/102476507/f8fed9a2-f197-4d30-b215-836ed3c9a7fa)


[ x ] make a Mongodb database (cloud/local container/local server)
[ x ] connect MongoDB from the backend
[ x ] write dockerfile and dockerize backend
[ x ] Write a docker file for frontend
[ x ] use multistage build
[ x ] build a front end at first stage using node lightweight image
[ x ] Run the previously built code in the second stage using light weight nginx image
[ x ] Write a docker file for nginx
[ x ] write docker-compose file
[ x ] name backend server=api, frontend=client, nginx build it
[ x] find where the container puts logs of the application
[ x ] Mount the /logs directory inside the container logs folder,so that we can access the container application logs outside of the container(from the local directory)
[ x ] Successfully run the docker-compose
GitHub Repo: https://github.com/saifuranik/tasks_docker.git

GitHub Repo (ready): https://github.com/saifuranik/4-tier-Application-Deploy.git

Use Chat GPT for writing Dockerfiles but understand properly if you not understand something ask Chat GPT about that Question he will explain you!! Chat Gpt is a great Teacher also can use youtube and blogs in google for better understanding

carefully read every line what i write otherwise you will not understand what have to do how to start

How to connect Database (MongoDB) with the Backend file — step 1
make a MongoDB database (cloud/local container/local server)

There are 3 ways till now i know to connect the database with backend

way 1
if we want we can make MongoDB database in our local PC or AWS Ec2 or Mongodb atlas online website

if you want to connect with you pc installed mongodb then after install it copy the link from top left green bar copy connection string.

see in the image below


make sure you make a database in here by the click option i was make named “ToDo” You can see in the image 1st one in list

Now come…how can we connect it with the Backend NodeJs code?

when you open your code from the backend file in my case its name is app.js in your case might different name maybe index.js or something else

after opening it scroll down or find the line where write code for connect Mongoose and see a blank URL space for giving your Mongodb connection link

mongodb://localhost:27017/ToDo
see the image below so you can understand better


in your case, this section might be at the top or the might developer name a separate file for this. its up to you to find it.

honestly speaking my going to blast when I'm going to find this section for connect MongoDB. you can ask from developer. in my case was asked one of my experienced brother from Kolkata

way 2
no worries come in next if you wanna connect it with the AWS Ec2 Instance

Go to AWS ec2 , & launch a free tier instance and install MongoDB there and make a Database there my using the command “ USE Your_Database_name”

and for connecting it same as previous just use your ec2 instance public ip


i was faced this problem

remember in the case of AWS ec2 instance when you stop your ec2 and re-lounch it again in next day this ec2 come with new public ip. so might be for that your mongodb not connected with backend while you run command “npm start”.

so change your backend code url section new ec2 public ip and run npm you will see a connected message like Avobe.

and if your “npm start” command not working you have to first run this “npm install” command for install npm modules and package.JSON files

Now move another issue my be you face


as you see in this image after put Mongodb connection url in code and you run “npm start”

mybe it will not work and show you this kind of error message written in green line below in image

basically, its says you need to make a folder with the name “logs” and then run the same “npm start” it will automatically create a file in this folder and do the work done

see the image carefully after making logs named folder in backend folder


if still your mongoDb not connected with Ec2 instance might be you have to follow those steps and in my it was worked.

actually, i also troubled so much by that and after the help of chat GPT i figured out this problem


Follow the steps no. 4 & 5 hope it will work properly

Now if you wanna connect with direct Mongodb Atlas website you can follow this below:-

Now Hopefully your mongoDb Connected with your Backend file

way 3
Connecting with MongoDB with backend with MongoDB Atlas website


sign up with Google and you will see this page


click on connect button with green color


cluster 1 name of your database in mongodb. click on Drivers as marked in image


your username was placed here automatically you just have to re-place your Mongodb password in the section where you see this <password>


just paste your coped URL in the nodejs-backed file called app.js you can see in the image Avobe

Now come in the next step — 2
make a Dockerfile for the backend file. you can follow my one


and you are done with your backend file

Now come to the frontend part — step 3 (Multi-stage Dockerfile)
in the frontend file, you will find a config file named nginx.conf


in this file basically developer mapped the port of frontend 3000 with the nginx server

so we have to attach this or copy this nginx.conf file into the frontend Multi-stage Dockerfile
Now write a Multi-stage docker file for frontend and nginx

here important is: to see the image carefully


in this multi-stage Dockerfile in stage 2 of the nginx part, we have to copy the conf folder from the local pc frontend to the frontend multi-stage Dockerfile nginx section “ where I mark in this image”

otherwise, you might see this error below. if you not copy Avobe white color marked file

COPY ./conf /etc/nginx/conf.d

when you copy this (conf) folder/file this error will resolve

careful!! otherwise have to face a lot of problems if we do not copy this conf folder and change the port to 3000. if you write dockerfile with the help of chat gpt he will put 80 which is the nginx port but for this conf file it will be redirected to the frontend html file so we have to expose here the 3000 port which allocates our frontend file declared in the nginx.conf file

Multi-stage Dockerfile for React frontend & nginx with nginx conf folder

FROM node:14 AS build

WORKDIR /app

# Copy package files to leverage Docker cache
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application source code
COPY . .

# Build the React app
RUN npm run build

# Stage 2: Create Nginx Runtime Image
FROM nginx:alpine

# Set working directory to Nginx webroot
WORKDIR /usr/share/nginx/html

# Remove default Nginx static assets
RUN rm -rf ./*

# Copy the built React app from the builder stage
COPY --from=build /app/build /usr/share/nginx/html
#have to copy conf folder from frontend folder in nginx dockerfile for run nginx
COPY ./conf /etc/nginx/conf.d


# Expose port 3000
EXPOSE 3000

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
if you face any issues re-read avobe this frontend section ( step 3 )error topic again and do it carefully

Now comes the next stage NGINX — step 4
Once we are done frontend multistage Dockerfile Move to the Next step NGINX


in the nginx folder, we will find a conf file called default.conf

we have to copy this file in the nginx Dockerfile in the location that what you see in the Avobe image

# Use the official Nginx image as a base image
FROM nginx:alpine

# Copy your custom Nginx configuration file
COPY default.conf /etc/nginx/conf.d/default.conf
“ this path where we copied this default.conf file, this path is by-default already existing path available in the nginx software image ”

we have to replace this pre-existing default.conf file with our own default.conf file which is we have in nginx folder in our local pc project folder

because in this default.conf file developer writes code / re-directs the backend & frontend paths

see this image you will understand


remember port numbers while you creating Dockerfiles. if you take the help of chat gpt he will write their normal default ports. but you have to change those with your port numbers that are given in your frontend, backend,nginx, and other files mentioned

Now most of the work is finished. last thing have to make a Docker-Compose file for all of those Dockerfiles


remember in this image Avobe i mark api & client . you have to change this name with a default name ever you find it in the code snippet

“ normally when you take this docker-compose file template code from anywhere even from chatgpt might be you see here name backend in the place of api and the frontend in the place of the client ”

because those api and client name mentoned in the default.conf nginx file

see this image below you will undestand


so that's why for connecting backend and front file with nginx we have to do this.

otherwise, we might see the error and not come output showing us a black screen


something like this in the Avobe image.

or you might see this page when you enter localhost:3000


or might be like this also


all of those pages are not workable we have to put localhost:80

which shows you this page is everything is fine


final output
& you are Done!

My dear brother and sisters…if you face any issue please comment and knock me at rsaifur985@gmail.com

this project was my DevOps Internship Interview project.

honestly speaking i was take lot of help from my two devops engineer brother who are really like angle for me in that time.

so make connections with good people. they will help you & keep faith on Allah SWT

Have a good day,

carefully read every line what i write otherwise you will not understand what have to do how to start
