![Kubernetes Logo](https://raw.githubusercontent.com/kubernetes-sigs/kubespray/master/docs/img/kubernetes-logo.png)
# Установка KUBERNETES кластера через KUBESPRAY

## Ansible server
#### Установка Ansible и необходимых пакетов на debian 12

```ShellSession
sudo apt update && sudo apt install python3-pip git ansible -y
```

####  Беспарольный доступ SSH по ключам с Ansible сервера до узлов
#### Гекнерация ключа
```ShellSession
cat .ssh/id_rsa.pub
```
#### распечатаем и скопируем
```ShellSession
cat .ssh/id_rsa.pub
```
## k8s node server (вставляем скопированный ключ)
```ShellSession
mkdir .ssh/  && nano .ssh/authorized_keys
```

## Ansible server
#### Настройка кластера в плэйбуках ansible (нужно только заменить IP и имена для ваших машин)
```ShellSession
nano inventory/mycluster/hosts.yaml 
```

####  Настройка кластера (если есть необходимость)
```ShellSession
nano inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml 
```

####  Установка библиотек для python (если есть необходимость, то решаем проблемы)
```ShellSession
pip install -r requirements.txt 
```

#### Начинаем установку (ЗАЙМЕТ 20-30 Минут)
```ShellSession
ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml
```

## k8s-master
####  Для того чтоб kubectl работал от пользователя без рута
```ShellSession
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
####  Для автокомпиляции
```ShellSession
sudo apt install  bash-completion
source <(kubectl completion bash)
```
####  Если комадну kubectl так же сократили алиасом k:
```ShellSession
source <(kubectl completion bash | sed s/kubectl/k/g)
```


