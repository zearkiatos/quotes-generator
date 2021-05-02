# quotes-generator
A simple PHP application to generate a random quote as HTML.

# For access to the virtual machine with SSH use this command

ssh -i [private_key] [user]@[your_public_server]

# Notes:

**Command Line Examples**
- $(aws ecr get-login --no-include-email --region us-west-2)
- aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin [resource_password]
**Note: You need to give permission to your user in the resource**

**Configuration kods k8s in AWS**
sudo apt update
sudo apt install -y awscli
sudo snap install kubectl --classic
curl -LO https://github.com/kubernetes/kops/releases/download/1.7.0/kops-linux-amd64
chmod +x kops-linux-amd64
mv ./kops-linux-amd64 /usr/local/bin/kops
Tienen que crear un usuario llamado kops en IAM.
Entren en IAM, hagan un nuevo usuario.
Configuren esto como acceso programatico.
Apunten el Access Key ID y el password.
Asígnenle el rol de AdministratorAccess (un rol preconfigurado en AWS IAM).
Salvar.
Regresen de la consola de AWS a tu consola / terminal, y continúen con lo siguiente:
aws config
aws iam create-group --group-name kops
aws iam attach-group-policy --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess --group-name kops
aws iam attach-group-policy --policy-arn arn:aws:iam::aws:policy/AmazonRoute53FullAccess --group-name kops
aws iam attach-group-policy --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --group-name kops
aws iam attach-group-policy --policy-arn arn:aws:iam::aws:policy/IAMFullAccess --group-name kops
aws iam attach-group-policy --policy-arn arn:aws:iam::aws:policy/AmazonVPCFullAccess --group-name kops
aws iam add-user-to-group --user-name kops --group-name kops
aws s3api create-bucket --bucket s3kopstudominiocom --region us-west-2
Antes de ejecutar el próximo comando, anexen lo siguiente a su archivo ~/.bashrc (al final):
export AWS_ACCESS_KEY_ID=tuaccesskey
export AWS_SECRET_ACCESS_KEY=tusecret
export KOPS_STATE_STORE=s3://s3kopstudominiocom
export KOPS_CLUSTER_NAME=kops-cluster-tudominio
Sálvenlo. Cierren sesión con “exit” y vuelvan a entrar. Ahora si, ejecuta:
kops create cluster --name=kops-cluster-tudominio --cloud=aws --zones=us-west-2a --state=s3kopstudominiocom
Esta operación puede tardar 20 minutos.
Cuando terminen, denle:
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
Con eso, se instalará el dashboard de k8s que vieron en el ejemplo.
Loguearse con user admin, y el password se obtiene con:
kops get secrets kube --type secret -oplaintext
Cuando se conecten, seleccionen anunciarse por token, y el token lo obtienen ejecutando lo siguiente:
kops get secrets admin --type secret -oplaintext
Con eso, ya podrán dar click en “Create” y poder poner su imagen del contenedor en ECR.
Cuando termine de hacer el deployment, encontrarán la url en la sección en el menú llamada “Services”.

**Other alernative**
aws configure
aws eks --region us-east-2 describe-cluster --name academy-cluster --query cluster.status
aws eks --region us-east-2 update-kubeconfig --name academy-cluster

**Run kubernetes dashboard**

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml

kubectl proxy
**Access into the next URL**
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/.