# quick setup guide for linux. 

## prerequisites are Linux

* Install curl, wget git, and tar (if they're not already installed):

      sudo apt-get install curl git wget tar

## Install Docker

## 1) Set up Docker's apt repository.
* Add Docker's official GPG key:

      sudo apt-get update"
      sudo apt-get install ca-certificates curl"
      sudo install -m 0755 -d /etc/apt/keyrings"
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc"
      sudo chmod a+r /etc/apt/keyrings/docker.asc"

* Add the repository to Apt sources:
  
      echo \
        deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
       sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update.
    * Install the Docker packages

           sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin.
    
    * Verify that the Docker Engine installation is successful by running the hello-world image
    
          sudo docker run hello-world

Install kubectl on Linux 
## Download the latest release

       curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl").  2)Install kubectl  command(sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl).  3)Test to ensure the               version you installed is up-to-date: command(kubectl version --client
   
## Open terminal and Install k3d(v4.4.8) on Linux using the below command 

    wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | TAG=v4.4.8 bash 
  
## after downloading kubectl install minikube

      curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
        && chmod +x minikube
        sudo mv minikube /usr/local/bin/

* use the command "minikube start" when kubectl not working
![Screenshot from 2024-03-25 23-30-31](https://github.com/sgnhyperion/egov_Digit/assets/140370637/f0241702-4d54-4900-916d-22501cc50027)

    
  
### then in the root directory create a kube folder an go into that using these commands 

        cd ~
        mkdir kube
        chmod 777 kube
        cd kube 

### then create a cluster using this command  

    k3d cluster create --k3s-server-arg "--no-deploy=traefik" --agents 2 -v "/home/<user_name>/kube:/kube@agent[0,1]" -v "/home/<user_name>/kube:/kube@server[0]" --port "80:80@loadbalancer

 * NOTE: Update "/home/<user_name>/kube" path in the above cmd with your respective absolute path.
 
 ## Once the cluster creation is successful, get the kubeconfig file, that enables you to connect to the cluster.

    k3d kubeconfig get k3s-default > myk3dconfig
    export KUBECONFIG=<path-to-your-kube_config>
    kubectl config use-context k3d-k3s-default --kubeconfig=myk3dconfig

* then 

## Verify the cluster creation by running the following commands from your local machine where the kubectl is installed. It provides the sample output as below if everything works fine.

    kubectl cluster-info
  ![Screenshot from 2024-03-25 23-31-50](https://github.com/sgnhyperion/egov_Digit/assets/140370637/56314fbc-601e-47ca-8041-f1c0f4788259)
    

### if cluster not working properly we can delete the cluster using this command "k3d cluster delete k3s-default" and recreate it .

### then  Verify the cluster creation by running the following commands from your local machine where the kubectl is installed. It provides the sample output as below if everything works fine.

the output should be like Kubernetes control plane is running at https://0.0.0.0:33931
CoreDNS is running at https://0.0.0.0:33931/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:33931/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy 
* the adress should be 0.0.0.0 and the port can be different .
  
### if address is different then the Apache may be running , stop it using these commands

      sudo systemctl status apache2
      sudo systemctl stop apache2  
      sudo systemctl disable apache2

### if still the same the then delete the cluster then in the kube folder restart minikube using "minikube start" command the recreate cluster and run the given three commands after that , then check the cluster list using this command "k3d cluster list" , the output should be like this

![Screenshot from 2024-03-25 23-28-16](https://github.com/sgnhyperion/egov_Digit/assets/140370637/d42c047d-cea5-4974-9d4b-5026b27e52eb)

 

## then the next step is deployement

## for that we have to clone the give repository using the command  

    git clone -b quickstart https://github.com/egovernments/DIGIT-DevOps

* You may have to install git and then run git clone on your machine.  After cloning the repo CD into the folder DIGIT-DevOps, type the "code ." command. This opens the visual editor and the DIGIT-DevOps repo files.

      cd DIGIT-DevOps
      code .

## Create a deployment config file. Use the following template to update any custom values to be changed (if you know the impact). Run it as is for the quickstart. Navigate to the following file in your local repo:

    DIGIT-DevOps/deploy-as-code/helm/environments

## install helm if not already installed using the commands  

      curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
      chmod 700 get_helm.sh  
      ./get_helm.sh 

* check the helm version using command "helm version".  then   Add the following entries in your host file /etc/hosts , "sudo nano /etc/hosts" then add  "127.0.0.1 quickstart.local.digit"  in it .

## then Run the deployer go script from the following directory:

    export KUBECONFIG=<path-to-your-kube_config>
    cd DIGIT-DevOps/deploy-as-code/egov-deployer
    go run digit_setup.go

## Be prepared for the following questions

1. Are you good to proceed?
2. Please enter the fully qualified path of the kubeconfig file
3. Which DIGIT Version You would like to install, Select below
4. Select the DIGIT modules that you want to install, choose Exit to complete selection
5. Choose the target env files that are identified from your local configs
6. Are we good to proceed with the actual deployment?

All Done.   
  ![Screenshot from 2024-03-25 23-36-44](https://github.com/sgnhyperion/egov_Digit/assets/140370637/b9588991-522a-47dc-9f6d-26a7c0fa1c98)
  ![Screenshot from 2024-03-25 23-37-36](https://github.com/sgnhyperion/egov_Digit/assets/140370637/d4e7d36a-b799-4862-a6be-b7c8f22ff5f3)

### then Check all the pods are running and ready using the below command

    kubectl get pods -A

 ![Screenshot from 2024-03-25 23-38-59](https://github.com/sgnhyperion/egov_Digit/assets/140370637/e2837b92-682a-4c0a-8d3e-cbf2c9dd37d3)
   
  

### then  If Kafka and zookeeper pods go into crashloopbackoff state due to a permission issue, use the below commands 

      cd ~
      sudo chmod -R 777 kube
      kubectl delete pod kafka-0 -n kafka-cluster --kubeconfig=<your_kubeconfig_path>
      kubectl delete pod zookeeper-0 -n zookeeper-cluster --kubeconfig=<your_kubeconfig_path>

### then  Test the Digit application status in the command prompt/terminal using the command below:

    curl -Is http://quickstart.local.digit/employee/login |  head -n 1

* OutPut should be like
HTTP/2 200
![Screenshot from 2024-03-25 23-40-14](https://github.com/sgnhyperion/egov_Digit/assets/140370637/991c57cc-3eea-4296-9cb3-9a49ecfa783a)



### then  Perform the kubectl port-forwarding of the egov-user service running from Kubernetes cluster to your localhost. This provides access to the egov-user service directly and allows the service to interact with the API.

    kubectl port-forward svc/egov-user 8080:8080 -n egov  

* output should be like  Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
![Screenshot from 2024-03-25 23-40-56](https://github.com/sgnhyperion/egov_Digit/assets/140370637/71799f55-14a9-4611-91c3-0ac46ced1517)



## Seed the sample data
* install postman
* import this link there "https://raw.githubusercontent.com/egovernments/DIGIT-DevOps/quickstart/deploy-as-code/bootstrap_scripts/seed_data.json"  after importing run collection.
* to get the pods running in the cluster use the command "kubectl get pods -n egov"
  ![Screenshot from 2024-03-25 23-42-26](https://github.com/sgnhyperion/egov_Digit/assets/140370637/f93a4492-607d-442e-8d02-4b5e58a936be)


  ###then  Paste the below link in the browser.
    "quickstart.local.digit/employee"
  ![Screenshot from 2024-03-25 23-43-53](https://github.com/sgnhyperion/egov_Digit/assets/140370637/388a7b62-93c0-40ae-8c25-bc08299e545a)

    
### Use the below credentials to login into the complaint section:

   * Username: GRO
  
  * Password: eGov@4321
  
  * City: CITYA
