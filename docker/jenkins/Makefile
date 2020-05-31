login:
	docker login --username=zioalex --email=zioalex@gmail.com

build:
	docker build -t jenkins-automated-setup .
	docker tag jenkins-automated-setup zioalex/jenkins-automated-setup

push:
	docker push zioalex/jenkins-automated-setup

backup:
	java -jar jenkins-cli.jar -s http://admin:admin@172.17.0.3:30000 get-job hello_world > hello_world.job
	java -jar jenkins-cli.jar -s http://admin:admin@172.17.0.3:30000 get-job create_app_container > create_app_container.job
	java -jar jenkins-cli.jar -s http://admin:admin@172.17.0.3:30000 get-job hello_world_test > hello_world_test.job
	java -jar jenkins-cli.jar -s http://admin:admin@172.17.0.3:30000 get-job deploy_to_k8s > deploy_to_k8s.job
