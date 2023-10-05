# glpi

Glpi is an Opensource Service Management solution which offers Helpdesk, CMDB, Asset Management, and Project Management on one platform.

Glpi Deployment on Kubernetes with persistent volumes and SSL certificate

Glpi Prerequisites - WebServer, PHP, Mysql/Mariadb Database.
Glpi Key Directories - /var/www/html/glpi/{config, plugins, marketplace.Files}.
MariadDB - /var/lib/mysql


Prerequisites for deploying GLPI:-

Kubernetes Cluster 

Shared Storage for PV (You may also use local storage, but it is not recommended.)

1. Clone above repo - https://github.com/Dineshk1205/glpi.git

$ git clone https://github.com/Dineshk1205/glpi.git

2. Crate MariaDB root password as secret – 

$ kubectl create secret generic mariadb --from-literal=mariadb-root-password=xxxxx

<img width="468" alt="image" src="https://github.com/Dineshk1205/glpi/assets/93421112/bb414d27-c74b-419b-9bf4-d756890cffe9">


3. Create a glpi namespace. 

$ Kubectl create ns glpi 

4. Extract controller-v1.9.0.tar.gz file
   
$ tar -xvf controller-v1.9.0.tar.gz

6. Installing the Nginx controller helm is required. 

Helm installation – 

$curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh


6. Nginx deployment: - 

Switch to ingress-controller directory. 

$ cd ingress-nginx-controller-v1.9.0/charts/ingress-controller

7. Create a ingress-controller namespace 

$ kubectl create namespace ingress-nginx

8. Run the following command to install the nginx ingress – 

$ helm install -n ingress-nginx ingress-nginx  -f values.yaml.

9. SSL Certificate Configuration – 

Generate a self-signed certificate or use ca certified certificate. 

Crate Secret using Certificate key and cert file in glpi namespace 

$ kubectl create secret tls glpi-tls  --key xx.key  --cert xx.crt -n glpi

 <img width="468" alt="image" src="https://github.com/Dineshk1205/glpi/assets/93421112/d069ab1e-396e-4e4c-a2ba-01114b20accc">


10. Update the IP address Range in metallb-pool.yaml file based on your network
11. Update host name in ingress.yaml based on your requiremnts 

12. Finally, Install MariaDB And glpi 


Switch to cloned Directory glpi and run the below command 

$ kubectl apply -f .

Check pod status 

$ kubectl get pods -A

Once pods up, check glpi IP to access the GLPI GUI.

$ kubectl get ingress -n glpi

 <img width="366" alt="image" src="https://github.com/Dineshk1205/glpi/assets/93421112/6e845f0a-22ac-46fe-9073-3f9015ae3019">


Create DNS records for the above IP ( IP is above ingress IP, and Hostname will be matched with ingress.yaml file hostname).Access GLPU using FQDN.

 <img width="468" alt="image" src="https://github.com/Dineshk1205/glpi/assets/93421112/ed3a84a8-942d-4b9d-bd9b-041a4e62208c">












DB for Configuring GLPI 

In the Kubernetes cluster, check MariaDB IP 

$ kubectl get svc 
 
<img width="468" alt="image" src="https://github.com/Dineshk1205/glpi/assets/93421112/24f5449a-a792-4946-b64d-d6cd80da9faf">

Note down above mariadbsv cluster IP; use IP for connected Glpi.SQL user use root as user and enter a password. (Password created as secret in previous steps)

<img width="468" alt="image" src="https://github.com/Dineshk1205/glpi/assets/93421112/3c0fd8aa-34ad-47d6-890a-70919a3d071a">

 
<img width="468" alt="image" src="https://github.com/Dineshk1205/glpi/assets/93421112/05a52cfb-ddf2-473a-bfab-2a109927c420">

 


<img width="468" alt="image" src="https://github.com/Dineshk1205/glpi/assets/93421112/f59c958d-9e77-4de1-b86f-9073dd5617ac">




 










