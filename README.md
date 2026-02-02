Step 1. Install Docker & Docker Compose
# Update packages
sudo apt update && sudo apt upgrade -y
 
# Install Docker
sudo apt install -y docker.io
 
# Enable & start Docker
sudo systemctl enable docker
sudo systemctl start docker
 
# Add your user to Docker group (so no need for sudo later)
sudo usermod -aG docker $USER
newgrp docker
 
# Install Docker Compose
sudo apt install -y docker-compose

Step 2. Prepare Project Structure
In /home/ubuntu, create folders:
mkdir frontend backend
Move your files into them:
# Move frontend zip
mv PIP_Tracker.zip frontend/
 
# Move backend jar
mv PipReviewSystem-0.0.1-SNAPSHOT.jar backend/
Step 3. Setup Frontend (Angular + Nginx)
Unzip and create Dockerfile + Nginx config:
cd frontend
unzip PIP_Tracker.zip
mv 'PIP_Tracker - Copy' pip
cd pip/dist/pip-tracker/browser

 Access Application-
Frontend (Angular) → http://<EC2-Public-IP>
Backend (Spring Boot) → http://<EC2-Public-IP>:8080
MySQL (if needed externally) → mysql -h <EC2-Public-IP> -P 3306 -u pipuser -p

Folder Structure
-----------------
demo/
│
├── backend/
│   └── Dockerfile
│
├── frontend/
│   ├── Dockerfile(dist/pip-tracker/browser/)
│   └── default.conf
│
├── docker-compose.yml
└── .env


Demo – pip tracker-
=====================

-> when provided files not git link then using Filezilla
copy files local to EC2

-> Demo – backend -> .Jar
 	  frontend -> pip-tracker

-> unzip file in frontend folder -> Jar -xvf "file-name"

-> write Dockerfile where .Jar present both need in same folder
-> The JAR file should NOT be unzipped.

A Spring Boot JAR is a single executable file.
Docker needs that single .jar file, not the extracted folders inside it.
-> we need to mention Jar file name in Dockerfile automatic all src/dependency pull by
Docker in Dockerfile

How to build and Dockerize Spring Boot JAR (Step-by-step)
------------------------------------------------------------

1) Check if JAR already exists
--------------------------------
Run:

    ls target/*.jar

If you see a file like:

    PipReviewSystem-0.0.1-SNAPSHOT-1.jar

then the JAR exists and you can Dockerize directly.

If you see “No such file or directory” or nothing, continue to step 2.


2) Build the JAR from source
--------------------------------
You must be in the folder that contains:

    pom.xml
    src/

Example:

    cd ~/Pip-Tracker--Docker-containerization--Demo/backend

Then run:

    mvn clean package

If build succeeds, you will see:

    [INFO] BUILD SUCCESS


3) Verify the JAR is created
--------------------------------
Run:

    ls target

You should see a JAR file like:

    PipReviewSystem-0.0.1-SNAPSHOT.jar
    (or similar)


4) If JAR is missing because source is missing
--------------------------------
If you do not have pom.xml and src/, you cannot build.

You must re-clone the repository from GitHub:

    git clone https://github.com/USERNAME/REPO_NAME.git

Then go into the correct folder and build again.
6) Notes
--------------------------------
- The JAR must NOT be unzipped.
- If you only see folders like BOOT-INF, META-INF, org, it means the JAR was extracted.
  You need the actual JAR file to run in Docker.
- If you have source code (pom.xml + src), build with Maven first.

------------------------------------------------------------
# **BACKEND (SPRING BOOT) FOLDER STRUCTURE - IMPORTANT FOCUS**

backend/
├── pom.xml            <-- IMPORTANT (build configuration)
└── src/               <-- IMPORTANT (source code)
    └── main/
        ├── java/      <-- IMPORTANT (Java application code)
        └── resources/ <-- IMPORTANT (application.properties, configs)

**Build output (after `mvn clean package`):**
target/               <-- IMPORTANT (contains the .jar file)


# **FRONTEND FOLDER STRUCTURE - IMPORTANT FOCUS**

frontend/
├── package.json       <-- IMPORTANT (build configuration)
└── src/               <-- IMPORTANT (source code)

**Build output (after `npm run build`):**
build/   (React)
dist/    (Vue/Angular)



* vim Dockerfile

If Jarfile given check BOOT-INF/lib -> where mention dependency / library

-> IF spring boot = Java (backend)

*how to understand which   Img use

Demo ->
-> Backend ->-Boot-INF -> src code present

             -org -> library present

	     -Meta-INF

{ Above These 3 folder present means looks spring boot use as framework }

For more details go meta-INF & check

*main-class : org.springframework.boot ...( this confirm use springboot)

*Boot-INF/classes/apth.properties

aplication.properties is Imp file where mention port / DB / env

-only port Imp & other DB related pass/userid/URL
we can rewrite in yml compose.yml don’t worry

*Frontend
-----------

-> Inside Frontend I got Angular.json so Fix it is
angular base

-> package.json -> This is imp folders

-> Angular is Frontend workfram

-> go inside package.json check resouces & dependancy

*Why use nginx
-> nginx is server help to host aplication
   Angular build & static aplication & nginx serve /host on browser & routing

-> nginx is server

-> every Frontend need server to host aplication 90% nginx use is fast / lightweight

 * why need default.conf

default.conf is nginx conf file this help to nginx
how to serve aplication & how to make routing & give info where is angular build

* Dockerfile for frontend-

-> FROM nginx:1.23.3-alpine – base image
-> COPY dist/pip-tracker/browser /usr/share/nginx/html
here store apliction after build
 -/usr/share/nginx/html->this is nginx root folder /contr here we copy angular build file from broweser folder 

-> COPY. <angular-build-path>-->This path we can see in angular.json & search "outputpath"

 

-> COPY. /default.conf  /etc/nginx/conf.d/ default.conf

COPY. /default.conf->copy from config file             /etc/nginx/conf.d/ default.conf->  puh inside nginx container

