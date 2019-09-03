# Django, uWSGI and Nginx in a container, using Supervisord
 
 * https://github.com/dockerfiles/django-uwsgi-nginx


### Build and run

#### Env-file

.stageleft.env
```
DJANGO_DEBUG=True
RDS_DB_NAME=ebdb
RDS_USERNAME=*
RDS_PASSWORD=*
RDS_HOSTNAME=*.*.us-west-2.rds.amazonaws.com
RDS_PORT=3306
BASE_URL=http://localhost:8000/
AWS_SERVER_PUBLIC_KEY=*
AWS_SERVER_PRIVATE_KEY=*
```

#### Build with python3
* `docker build -t app .`
* `docker run -d -p 80:80 --env-file ./.stageleft.env app`
* `docker run -p 80:80 --env-file ./.stageleft.env app` #attached
* go to 127.0.0.1 to see if works



##### attach w console

* docker exec -i -t 4afcf1a5b50e /bin/bash 
* docker attach 4afcf1a5b50e

#### View Logs

nginx:

* `$ more /var/log/nginx/error.log`

uWSGI:

* `$ more /tmp/errlog.log`

## AWS ECS and ECR 

[Docker Push ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)


Ensure you have installed the latest version of the AWS CLI and Docker. For more information, see the ECR documentation .
Retrieve the login command to use to authenticate your Docker client to your registry.
 1. Use the AWS CLI:
   
   `$(aws ecr get-login --no-include-email --region us-west-2)`

    Note: If you receive an "Unknown options: --no-include-email" error when using the AWS CLI, ensure that you have the latest version installed. Learn more 
    
 2. Build your Docker image using the following command. For information on building a Docker file from scratch see the instructions here . You can skip this step if your image is already built:
 
  `docker build -t truthlab/truthlab-server .`

 3. After the build completes, tag your image so you can push the image to this repository:
 
  `docker tag truthlab/truthlab-server:latest 202652978704.dkr.ecr.us-west-2.amazonaws.com/truthlab/truthlab-server:latest`

 4. Run the following command to push this image to your newly created AWS repository:
 
  `docker push 202652978704.dkr.ecr.us-west-2.amazonaws.com/truthlab/truthlab-server:latest`



[Getting Started w ECS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_GetStarted_Fargate.html)

[Task Secret Env](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data.html)
