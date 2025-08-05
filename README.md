# Linux cheatsheet

This is a command cheatsheet which can provide IT professionals with necessery know how, in order to ease their day-to-day tasks.

## List of content
 * [Openssl Commands](#openssl-commands)
 * [Curl Commands](#curl-commands)
 * [SNMPv3 Commands](#snmpv3-commands)
 * [Screen Commands](#screen-commands)
 * [Android Debug Bridge (adb) Commands](#adb-commands)
 * [Youtube downloader (yt-dlp) Commands](#yt-dlp-commands)
 * [Docker Commands](#docker-commands)
 * [Kubernetes Commands](#kubernetes-commands)
 * [Helm Commands](#helm-commands)

### Openssl Commands
Get all certificates for a given site
```bash
echo | openssl s_client -showcerts -connect www.example.com:443 \
| awk '/BEGIN CERT/,/END CERT/ {print $0}' > chain.pem
```

Get all certificates for a given site using SNI
```bash
echo | openssl s_client -showcerts -servername www.example.com -connect www.example.com:443 \
| awk '/BEGIN CERT/,/END CERT/ {print $0}' > chain.pem
```

Read CSR
```bash
openssl req -noout -text -in certificate.csr
```

Read SSL certificate
```bash
openssl x509 -noout -text -in certificate.cer
```

Extract certificate chain from PFX
```bash
openssl pkcs12 -in certificate.pfx -nodes -nokeys -out ca-bundle.pem
```

Extract private key from PFX
```bash
openssl pkcs12 -in certificate.pfx -nodes -nocerts -out ca-bundle.key
```

Remove private key passphrase
```bash
openssl rsa -in private_w_pf.key -out private.key
```

Create CSR with SAN's
```bash
openssl req -new -sha256 -nodes \
-newkey rsa:2048 -keyout example.key \
-subj "/C=MK/L=example/OU=IT/O=example/CN=www.example.com" \
-addext "subjectAltName=DNS:www.example.com,DNS:example.com" \
-out example.csr
```

### Curl Commands

Test HTTPS with certificate chain file
```bash
curl -vI --cacert ca-bundle.pem "https://www.example.com"
```

Check certificate validity for given site
```bash
curl -kvI https://example.com 2>&1 | awk '/Server certificate/,/issuer/ { print }'
```

### SNMPv3 Commands

Create SNMPv3 User
```bash
net-snmp-create-v3-user -ro \
-A AuthPass -a SHA-256 \
-X EncryptPass -x AES256 snmpuser
```

### Screen Commands

| Command | Description |
|-------------|---------|
|```screen -S <screen_name>```|Create new screen session|
|```screen -x <screen_name>```|Join screen session |
|```screen -d```|Detach from screen|
|```screen -r <screen_name>```|Re-attach to screen |

Keybord Shortcuts

| Description | Shortcut |
|-------------|----------|
|Detach from screen|```Ctrl + a + Ctrl + d```|   
|Split vertically  |``` Ctrl + a + \|``` |
|Split horizontally|```Ctrl + a + S```|
|Change window|```Ctrl + a + Tab```|
|Start new screen session in window|```Ctrl + a + Ctrl + c```|
|Exit all screen windows|```Ctrl + a + \```|

### ADB Commands

| Command | Description |
|-------------|---------|
|```adb devices```|Search for devices|
|```adb shell pm list packages```|List all packages on the phone|
|```adb shell pm uninstall --user 0 <package>```|Remove Android package from phone|
|```adb exec-out screencap -p > ./picture.png```|Take phone screenshot|

### YT-DLP Commands

Download song from youtube to mp3 format
```bash
yt-dlp -x --audio-format 'mp3' <url>
```

### Docker Commands

| Description | Command |
|-------------|---------|
|List all containers|```docker ps -a```|
|List images|```docker images```|
|List volumes|```docker volume ls```|
|List networks|```docker network ls```|
|Check container logs|```docker logs <container_name>```|
|Execute commands inside containers|```docker exec -it <container_name> <command>```|
|Inspect images|```docker image inspect <image_name>```|
|Inspect network|```docker network inspect <network_name>```|
|Inspect container|```docker container inspect <container_name>```|
|Inspect volume|```docker volume inspect <volume_name>```|
|Start multiple containers with docker compose|```docker compose up -d```|
|Start multiple containers with docker compose </br> and  force images rebuild|```docker compose up -d --build```|
|Stop multiple containers with docker compose|```docker compose down```|
|Remove all unused images|```docker image prune -a```|

**Note:** Install docker from the [official page](https://docs.docker.com/engine/install/)

### Kubernetes Commands

| Command | Description |
|-------------|---------|
|```kubectl config get-contexts```|See which kubernetes cluster you are connected to.|
|```kubectl config set-context <context>```|Set which kubernets cluster to run commands on.|
|```kubectl get nodes -o wide```|See all cluster nodes|
|```kubectl get all```|See all cluster resources|
|```kubectl get all -n <namespace>```|See all resources inside the `<namespace>`|
|```kubectl get <resources> -n <namespace>```|See given ```<resources>``` in the ```<namespace>```|
|```kubectl get all -n <namespace>```|See all cluster resources in given ```<namespace>```|
|`kubectl describe <type> <name> -n <namespace>`|Provide details for `<type>` with `<name>` in  `<namespace>`|
|```kubectl edit <resource> <name>```|Edit config for exiting resources|
|`kubectl delete <podname> -n <namespace>`|Delete pod with name `<podname>` in `<namespace>`|
|`kubectl cordone <node>`|Prevent pods to be deployed on `<node>`|
|`kubectl uncrodone <node>`|Enable pod scheduling on `<node>`|
|`kubectl drain <node>`|Drain `<node>` from resources and disable pod scheduling|

### Helm Commands

| Command |Description|
|-------------|---------|
|`helm repo update`|Update local repositories|
|`helm repo add <name> <repositry_url>`|Add repository|
|`helm repo list`|List local repositories|
|`helm search repo -l <repository_name>/<chart_name>`|Check chart version for given app|
|`helm delete <release> -n <namespace>`| Delete `<release>` under `<namespace>`|
|`helm pull <repo>/<chart> --version <version> --untar`|Download the app chart with `<version>` for inspection.|

### Azure Command Line Interface
| Command |Description|
|-------------|---------|
|`az login`| Login to Azure |
|`az account list -o table`| List subscriptions and active subscription. |
|`az account set --subscription <id>`| Set subscription with `<id>` to be active. |