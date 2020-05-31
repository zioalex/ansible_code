Vagrant
===

  cd vagrant_code
  vagrant up minikube

Moved in Vagrant
--------------------------------
Ansible
===
  cd ansible_code/minikube
  ansible-playbook setup.yml
--------------------------------

Inside the Vagrant host
===


From the host level
===
To add the proper ssh config and start the port-forwarding

  vagrant ssh-config >> ~/.ssh/config
  ssh minikube -L 30000:172.17.0.3:30000

Custom jenkins image
===
  eval "$(minikube docker-env -p Newyorker)"
  docker build -t jenkins-automated-setup .
  docker tag jenkins-automated-setup zioalex/jenkins-automated-setup
  docker push zioalex/jenkins-automated-setup

  # test
  docker run -p 8080:8080 -p 50000:50000 jenkins-automated-setup

Enable kubectl auto compleation

  source <(kubectl completion bash)

Backup Jobs
===
  wget localhost:30000/jnlpJars/jenkins-cli.jar

# backup
  java -jar jenkins-cli.jar -s http://admin:admin@localhost:30000 get-job create_app_contaier > create_app_container.job
  java -jar jenkins-cli.jar -s http://admin:admin@localhost:30000 get-job create_app_container > create_app_container.job
  java -jar jenkins-cli.jar -s http://admin:admin@localhost:30000 get-job hello_world_test > hello_world_test.job

# restore
  java -jar jenkins-cli.jar -s http://admin:admin@localhost:30000 create-job hello_world < hello_world.job
  java -jar jenkins-cli.jar -s http://admin:admin@localhost:30000 create-job create_app_container < create_app_container.job
  java -jar jenkins-cli.jar -s http://admin:admin@localhost:30000 create-job hello_world_test < hello_world_test.job


Default kubernetes route
  minikube -p Newyorker ssh "ip r s| grep '^default via'| awk '{ print \$3 }'"

Generate a Kube config files with the embedded crt to be use in Jenkins
===

  kubectl config set-cluster Newyorker --server https://172.17.0.3:8443 --certificate-authority=/home/vagrant/.minikube/ca.crt  --embed-certs=true --kubeconfig ~/.kube/config_embed
  kubectl config set-credentials Newyorker --client-certificate=/home/vagrant/.minikube/profiles/Newyorker/client.crt --client-key=/home/vagrant/.minikube/profiles/Newyorker/client.key  --embed-certs=true --kubeconfig ~/.kube/config_embed
  kubectl config set-context Newyorker --cluster=Newyorker --user=Newyorker --kubeconfig ~/.kube/config_embed
  kubectl config use-context Newyorker --kubeconfig ~/.kube/config_embed

Add the config_embed in /var/jenkins_home/kube_config

How to create a Jenkins Secret unattanded?
===
Creted a secret manually and fetched the config
  java -jar jenkins-cli.jar  -s http://admin:admin@172.17.0.3:30000 get-credentials-as-xml system::system::jenkins _ kube-config > kube-config-secrets.xml

Set it with
  java -jar jenkins-cli.jar  -s http://admin:admin@172.17.0.3:30000 create-credentials-by-xml system::system::jenkins _ < kube-config-secrets.xml


Test the app
===
  ssh minikube -L 30001:172.17.0.3:30001
  curl localhost:30001



Not Needed anymore
---
Jenkins pw
---

  vagrant ssh
  eval "$(minikube docker-env -p Newyorker)"
  kubectl logs -lapp=jenkins --namespace jenkins --tail=20


Reference
===
https://rancher.com/blog/2018/2018-11-27-scaling-jenkins/
https://technologyconversations.com/2017/06/16/automating-jenkins-docker-setup/
https://github.com/jenkinsci/docker/blob/master/README.md
https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/
https://stackoverflow.com/questions/8424228/export-import-jobs-in-jenkins
https://kubernetes.io/docs/concepts/configuration/secret/
https://kubectl.docs.kubernetes.io/pages/app_management/secrets_and_configmaps.html#configmaps-from-files
https://unofficial-kubernetes.readthedocs.io/en/latest/tasks/administer-cluster/share-configuration/
https://kubernetes.io/docs/concepts/containers/images/

