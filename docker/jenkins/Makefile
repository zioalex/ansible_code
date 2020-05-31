login:
	docker login --username=zioalex --email=zioalex@gmail.com

build:
	docker build -t jenkins-automated-setup .
	docker tag jenkins-automated-setup zioalex/jenkins-automated-setup

push:
	docker push zioalex/jenkins-automated-setup
