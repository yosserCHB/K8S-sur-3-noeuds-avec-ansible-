"# K8S-sur-3-noeuds-avec-ansible-" 


1/ Configuration de l'environnement SSH : 
* Sur master :
ssh-keygen -t rsa -b 2048
ssh-copy-id triweb-test-worker@192.168.40.157
ssh triweb-test-worker@192.168.40.157

* Sur worker :
ssh-keygen -t rsa -b 2048
ssh-copy-id triweb-test@192.168.40.156
ssh triweb-test@192.168.40.156

* Vérification des permissions SSH : 
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 700 ~/.ssh

mkdir -p ~/ansible-k8s && cd ~/ansible-k8s

2/ Vérification de la connexion Ansible: 
sudo ansible all -i inventory.ini -m ping

3/ Exécution du Playbook : 
sudo ansible-playbook -i inventory.ini install_k8s.yml


**************************************
4/ Créer un déploiement Nginx (sans fichier YAML) : 
sudo kubectl create deployment nginx-deployment --image=nginx

- Vérifier si l'image est bien téléchargée : 
docker images | grep nginx

- Si l’image nginx n’apparaît pas, essaie de la télécharger manuellement :
docker pull nginx

- Vérifier le déploiement : 
kubectl get deployments

- Vérifier les pods : 
kubectl get pods

- Exposer le service pour Nginx : 
kubectl expose deployment nginx-deployment --type=NodePort --port=80

- Vérifier le service exposé : 
kubectl get services

- communication entre master et worker : 
sur le worker : sudo ufw allow 10250/tcp
		  sudo ufw reload

************************************
Apache:

sudo kubectl apply -f  httpd-deployment.yml
kubectl apply -f service.yml