# :mortar_board:**TUTORIEL POUR KUBERNETES**:mortar_board:
![](/KubernetesDockerpic2.png)
## :zzz:**1. INSTALLATIONS**:zzz:

### :whale2:**Installation de Docker pour Linux**:whale2:
1. Installer Docker : `apt install docker.io`
2. Vérifier la version de Docker installée : ` docker --version `
3. Vérifier que la virtualisation est supportée : ` grep -E --color 'vmx|svm' /proc/cpuinfo `. Si la réponse n’est pas vide alors c’est supporté.

### :game_die:**Installation de Minikube**:game_die:
1. Installation de Minikube : ` curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \  && chmod +x minikube `
2. Ajouter minikube en tant que variable d'environnement
* ` mkdir -p /usr/local/bin/ `
* ` install minikube /usr/local/bin/ `
3. Lancer le Minikube avec la machine virtuelle de votre choix : ` minikube start --vm-driver= driver_name   `
![](/minikube_start.png)
4. Vérification de la réussite de l’installation : ` minikube status `

## :books:**2. Familiarisation avec Kubernetes**:books:

* Pour voir les déploiements : ` kubectl get deployment `
* Pour voir les noeuds : ` kubectl get nodes `
* pour voir les services : ` kubectl get service `
* Pour voir les informartions sur les cluster : ` kubectl cluster-info `

## :boom:**3. Déployer une image**:boom:

Lancer Minikube (permet de lancer une machine virtuelle qui héberge Kubernetes) : ` minikube start --memory 8000 --cpus 2 --kubernetes-version v1.6.0 `

### :whale2:**Déploiement avec une image Docker**:whale2:	
1. Pour lancer une image trouvée sur DockerHub : ` kubectl run my-nginx --image=nginx --port=80 `
![](/Déploiement_image.png)
2. Pour exposer l'image créée : ` kubectl expose deployment my-nginx --port=80 --type=LoadBalancer `. Le loadbalancer sert à répartir les tâches.
3. Vérification : connaître son port en tapant la commande ` get service ` et taper ` localhost:port ` sur votre navigateur.
![](/nginx_running.png)

### :pencil:**Déploiement avec un fichier yaml**:pencil:
* Pour lancer et exposer une image : ` kubectl apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml `

### :crystal_ball:**Vérification du bon fonctionnement du déploiement** :crystal_ball:
* Fonctionnement général : ` kubectl get deployments `
* Détails du déploiement : ` kubectl describe deployments `
![](/verif_deployement.png)
* Nous pouvons également vérifier les ReplicaSet : ` kubectl get rs `
* Nous pouvons également voir les pods avec leur label : ` kubectl get pods --show-labels `
![](/ReplicatSetAndPods.png)

### :arrows_counterclockwise:**Exemple de mise à jour d'une image**:arrows_counterclockwise:
* Pour mettre à jour l'image dans le déploiement nginx : ` kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1 --record `
* Pour voir le statut du déploiement de la mise à jour : ` kubectl rollout status deployment.v1.apps/nginx-deployment `
![](/statut_deployement.png)

## :bar_chart:**4. Dashboard**:bar_chart:
La dashboard permet une meilleure gestion du déploiement pour des personnes moins familières avec les commandes de Kubernetes.
1. Pour mettre en place la dashboard : ` sudo kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml `
2. Pour pouvoir changer l'accessibilité de la dashboard : ` sudo kubectl proxy ` 
3. Rentrer l'URL suivante dans un navigateur pour accéder à votre dashboard : [http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/deployment?namespace=default](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/deployment?namespace=default)
4. Trouver un token permettant de se connecter à la dashboard : ` sudo kubectl get secret `. (Celle-ci va retourner une liste de *secret*).
5. Mettre le nom du secret dans la commande suivante : ` sudo kubectl describe secret  default-name-secret `. (Celle-ci va retourner une liste d'information dont le token, que l'on peut utiliser pour se connecter dans la dashboard).
![](/secret.png)
![](/dashboardCo.png)

## :checkered_flag:**5. Arrêter un déploiement et un service**:checkered_flag:
* Pour arrêter un déploiement : ` kubectl delete deployment/nginx-deployment `
* Pour arrêter un service : ` kubectl delete service/nginx-deployment `

## :computer:**Quelques liens utiles**:computer:
* https://kubernetes.io/fr/docs/home/
* https://hub.docker.com/ 
