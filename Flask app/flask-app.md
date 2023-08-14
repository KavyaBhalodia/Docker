# Flask-app using Docker (on EC2)
### Create flask app that displays random gifs.
1. Create an EC2 instance on AWS
	 -Instance type: t2micro
	 -AMI: Amazon linux 2 AMI
	 -Create a new key-pair (choose ppk for putty)
	 -Network settings: Allow ssh, http traffic
2. Launch instance 
 
3. Connect to EC2 trough putty
	-Download putty 
	-Open putty gen and click on 'Load' and load the created key
	-Save that key 
	-Open Putty and paste the public IP address of EC2 instance and then go to SSH, then in Auth and load the saved private key
	-Then the CLI loads , enter 'ec2-user' in login
	
4. Perform the following commands to install docker 
  To gain superuser or root privileges temporarily. `sudo su` <br>
  	Install docker<br>
	  ````
 	  yum install docker -y
   ````
   To start docker 
   ````
    service docker start
   ````
   TO check if docker is running
   ````
    service docker status
   ````
		
6. Create a directory flask app and create following files in it
		````mkdir flask-app````
		
7. Make following files in flask-app
		app.py

    ````

    from flask import Flask, render_template
    import random
    app = Flask(__name__)
    # list of images
    images = [ "Enter gif URL" , 
           "Enter another gif URL" ,
           .....
           ]

    @app.route('/')
    def index():
      url = random.choice(images)
      return render_template('index.html', url=url)

    if __name__ == "__main__":
      app.run(host="0.0.0.0")
  

    ````
    requirements.txt
  
    ````
    Flask==0.10.1
    ````
    Make a directory templates `mkdir templates` and a file inside it `touch index.html`
  
    ````
    <html>
      <head>
        <style type="text/css">
          body {
            background: black;
            color: white;
          }
        div.container {
          max-width: 500px;
          margin: 100px auto;
          border: 20px solid white;
          padding: 10px;
          text-align: center;
        }
        h4 {
          text-transform: uppercase;
        }
        </style>
      </head>
      <body>
        <div class="container">
          <h4>Cat Gif of the day</h4>
          <img src="{{url}}" />
        </div>
      </body>
    </html>
    ````
    Create a Dockerfile `touch Dockerfile`
    
    ````
    # base image
      FROM alpine:3.5

    # Install python and pip
      RUN apk add --update py2-pip

    # install Python modules needed by the Python app
      COPY requirements.txt /usr/src/app/
      RUN pip install --no-cache-dir -r /usr/src/app/requirements.txt

    # copy files required for the app to run
      COPY app.py /usr/src/app/
      COPY templates/index.html /usr/src/app/templates/

    # Define port the container should expose
      EXPOSE 5000

    # Command for running the application
      CMD ["python", "/usr/src/app/app.py"]
    ````
7. Build docker image
    ````
    docker build -t <YOUR_USERNAME>/myfirstapp .
    ````
    >'-t' for tag
    
    Make an account in docker hub
    If you are not able to run the command run `docker login` first and then run build command

8. Run docker container
    ````
    docker run -p 80:5000 --name myfirstapp YOUR_USERNAME/myfirstapp
    ````
    >'-p' for publish (or expose) a container's port to the host system
    >'--name' to give name to the container
    
    copy the public IP address of EC2 instance and paste in new tab to see the application running

   Output:
   ![Screenshot (978)](https://github.com/KavyaBhalodia/Docker/assets/87963890/9d0018e2-047d-4007-88ee-9e557dd8d351)
