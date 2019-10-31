AS471 Assignment

1) Created a Google Cloud.


2) Configure the SSH server to cloud instance.

	export PROJECT_ID='sanka-257610'

	gcloud iam service-accounts create ssh-account --project $PROJECT_ID \ --display-name "ssh-account"

	gcloud compute networks create ssh-example --project $PROJECT_ID

	gcloud compute firewall-rules create ssh-all --project $PROJECT_ID \
   		--network ssh-example --allow tcp:22

	gcloud compute instances create target --project $PROJECT_ID \
   		--zone us-central1-f --network ssh-example \
   		--no-service-account --no-scopes \
   		--machine-type f1-micro --metadata=enable-oslogin=TRUE

	gcloud compute instances add-iam-policy-binding target \
   		--project $PROJECT_ID --zone us-central1-f \
   		--member serviceAccount:ssh-account@$PROJECT_ID.iam.gserviceaccount.com \
   		--role roles/compute.osAdminLogin

	gcloud compute instances create source \
   		--project $PROJECT_ID --zone us-central1-f \
   		--service-account ssh-account@$PROJECT_ID.iam.gserviceaccount.com  \
   		--scopes https://www.googleapis.com/auth/cloud-platform \
   		--network ssh-example --machine-type f1-micro


3) Install docker into your cloud instance.

	gcloud auth login

	gcloud config set project sanka

Created a file named quickstart.sh with the following contents:

	#!/bin/sh
	echo "Hello, world! The time is $(date)."

Created a file named Dockerfile with the following contents

	FROM alpine
	COPY quickstart.sh /
	CMD ["/quickstart.sh"]

Run the following command to make quickstart.sh executable:

	chmod +x quickstart.sh

	gcloud builds submit --tag gcr.io/sanka/quickstart-image .


4) Create a sample application connected to a MYSQL database within docker container
	
	gcloud compute instances create mysql-test

	gcloud compute ssh mysql-test

Install MySQL

	sudo apt-get update

	sudo apt-get -y install mysql-server


creating sample MySql App
	
	mkdir app
	cd app
	nano docker-compose.yml
	mkdir app
	cd app
	nano Dockerfile
	nano requirements.txt
	nano app.py
	mkdir db
	cd db
	nano init.sql
	docker-compose up   //to run the app
	app_1  |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
	{"favorite_colors": [{"Lancelot": "blue"}, {"Galahad": "yellow"}]}      // output on browser
	

5) Create a public Github repository and commit changes.

	echo "# AS471Assignment" >> README.md
	git init
	git add README.md
	git commit -m "assignment"
	git remote add origin https://github.com/sankaSanjeeva/AS471Assignment.git
	git push -u origin master
