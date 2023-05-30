# Kubernetes

Ce répertoire contient des playbooks Ansible permettant d'automatiser le processus d'installation et de configuration de Kubernetes.

## Playbooks

- `install-k8s.yml`: Ce playbook désactive le service de pare-feu et le SWAP, installe et démarre Docker. Il désactive ensuite SELinux et configure les paramètres requis pour Kubernetes. En utilisant le référentiel YUM de Kubernetes, le playbook installe les composants spécifiques de Kubernetes (kubelet, kubeadm et kubectl). Dans notre cas, nous avons choisi d'installer la version 1.14.0 de ces composants en raison de sa stabilité et de sa compatibilité avec notre système CentOS 7.

- `masters.yml`: Ce playbook configure le nœud maître en exécutant plusieurs tâches. Il initialise le cluster Kubernetes à l'aide de la commande `kubeadm init`, en spécifiant le réseau de pods CIDR et l'adresse IP d'annonce du serveur API. Ensuite, il crée le répertoire `.kube` dans le répertoire utilisateur du nœud maître et copie le fichier de configuration `admin.conf` dans le répertoire `.kube/config` de l'utilisateur racine. Enfin, il déploie le réseau de pods en utilisant le réseau Flannel.

- `workers.yml`: Ce playbook comporte deux parties. La première partie s'exécute sur le nœud maître et obtient la commande de rejoindre le cluster. Elle génère la commande de rejoindre le cluster à l'aide de la commande `kubeadm token create --print-join-command` et enregistre le résultat dans une variable. La deuxième partie s'exécute sur les nœuds workers et utilise la commande de rejoindre le cluster précédemment obtenue pour se connecter au nœud maître. Les erreurs préliminaires sont ignorées avec l'option `--ignore-preflight-errors all`, et le résultat est enregistré dans le fichier `node_joined.txt`.

N'hésitez pas à utiliser ces playbooks pour automatiser l'installation et la configuration de Kubernetes. Si vous rencontrez des problèmes ou avez des suggestions d'amélioration, n'hésitez pas à ouvrir une issue ou à contribuer au projet.

Happy Kubernetes automation!
