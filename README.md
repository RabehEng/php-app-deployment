Prerequisites:
- install Docker runtime and docker-compse.
- create dockerhub account where to save docker images.
- install Minikube in local computer.
- run kubernetes cluster.
- install kubectl command line (kubectl CLI).
- install Helm v3.x.


Steps:
* Clone the repository. This will clone the sample repository and make it the current directory:
   git clone https://github.com/RabehEng/php-app-deployment
   cd php-app-deployment/php-minikube/
   
In the app-code folder i created a file named phpminiadmin.php. This is a small PHP application for accessing and managing MySQL databases which i will use as an example application.
   
* Build the image using the command below. Remember to replace the "rabehboubakri"  with your Docker ID:
                 docker build . -t  rabehboubakri/phpfpm-app:0.1.0

* Run the docker-compose up command in order to create and start the containers:
                  docker-compose -f app-code/docker-compose.yml up
   
* Check if the application is running correctly by entering http://localhost/phpminiadmin.php in your default browser.

for me i used a VM in microsoft azure cloud and i accessed it with putty using ssh so i used the ip address  http://40.89.164.200/phpminiadmin.php

* To log in to the application, you must connect first to the database. Click the “advanced settings” link and enter the information below. Then, click “Apply”.

DB user name: mini
Password: mini
DB name: mini
MySQL host: mariadb
port: 3306

* Check the images that have been created in your local repository by executing the command below:
                     docker images | grep -E 'fpm|mariadb|nginx'

* Log in to Docker Hub:
                   docker login
	 
* Push the image to your Docker Hub account. Replace the "rabehboubakri" with the username of your Docker Hub account and phpfpm-app:0.1.0 with the name and the version of your Docker image:
                           docker push rabehboubakri/phpfpm-app:0.1.0
			
* move to helm-chart file using this command
                           cd helm-chart/
and change "rabehboubakri" in values.yaml file by your dockerID

* add this repository to helm using this command 
                   helm repo add stable https://kubernetes-charts.storage.googleapis.com
		
* Deploy the Helm chart with the helm install command. This will create three pods within the cluster, one for the Nginx service, another for the MariaDB service, and the third for the PHP-FPM application.
                                                                                                helm install phpfpm stable/mariadb  --set mariadb.mariadbRootPassword=mini,mariadb.mariadbUser=mini,mariadb.mariadbPassword=mini,mariadb.mariadbDatabase=mini
		 
* Run the kubectl get pods command to get a list of running pods:
                  kubectl get pods
		 
* Check the logs of the application pod. POD-NAME is a placeholder; remember to replace it with the right value.
                  kubectl logs -f POD-NAME
* get the application URL:
                               minikube service phpfpm-php-app-nginx --url
                               
* Enter the resulting URL in browser to access the application
