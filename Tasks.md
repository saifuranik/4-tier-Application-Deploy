# Complete these tasks

 - [ x ] make a mongodb database (cloud/local container/local server)
 - [ x ]  connect mongodb from backend
 - [ x ] write dockerfile and dockerize backend
 - [ ] write docker file for frontend
	 - [ ] use multistage build
	 - [ ] build front end at first stage using node light weight image
	 - [ ] run the previously built code in the second stage using light weight nginx image
 - [ ] write docker file for nginx 
 - [ ] write docker compose file
	 - [ ] name backend server=api, frontend=client, nginx  build it 
	 - [ ] find where does the container put logs of application
	 - [ ] mount the /logs directory inside the container logs folder ,so that we can access the container application logs outside of the container(from local directory)
 - [ ] Successfully run the docker compose 
