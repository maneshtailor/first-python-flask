Built using the help of https://gist.github.com/dineshviswanath/af72af0ae2031cd9949f
ie install Virtualenv and Flask using pip3


in this directory, first run:
>source bin/activate

then in the Flask environment, run:
>python3 app.py



Use this guide to install to AWS Lightsail 
https://aws.amazon.com/getting-started/hands-on/serve-a-flask-app/

Each time we want to redeploy, we need to do the following:

1) Delete the current lightsail deployment
aws lightsail delete-container-service --service-name flask-service

Use the following to get the status:
aws lightsail get-container-services --service-name flask-service

2) Ensure the Dockerfile is configured to copy the right resources and 'start' the right app.py file

3) Do a Docker Build
docker build -t flask-container .

4) Test it locally:
docker run -p 5000:5000 flask-container
Goto: http://localhost:5000/

5) Create a container service on AWS Lightsail
aws lightsail create-container-service --service-name flask-service --power small --scale 1

6) Check that the container is ACTIVE:
aws lightsail get-container-services --service-name flask-service

You can see your lightsail containers here:
https://lightsail.aws.amazon.com/ls/webapp/home/containers 

7) Push the application container to Lightsail
aws lightsail push-container-image --service-name flask-service --label flask-container --image flask-container

8) Get the 'ID'/count/X from the message/payload that looks like:
Refer to this image as ":flask-service.flask-container.X" in deployments.

9) Update the container.json file to include the X in the config from the last step

10) Deploy the container 
aws lightsail create-container-service-deployment --service-name flask-service --containers file://containers.json --public-endpoint file://public-endpoint.json

11) Check its Running:
aws lightsail get-container-services --service-name flask-service

12) Hit the URL you see in that 'Running' payload
