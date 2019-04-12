# Ansible Playbooks para infra Cluster Openshift na Azure

Instruçoes basicas para Construcao do Cluster... Comandos servem para Centos e (possivelmente) RHEL.

Quick how to Azure:

- Install azure-cli:

`sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc`

`sudo sh -c 'echo -e "[azure-cli]\nname=Azure CLI\nbaseurl=https://packages.microsoft.com/yumrepos/azure-cli\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'`


- Instala pacotes (Centos/Rhel)

`sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release`

`sudo yum install -y python-pip python-wheel`

`sudo pip install ansible[azure]`

- Cria uma ServicePrincipal

`az login`

`az ad sp create-for-rbac --name ServicePrincipalName`

Anotar resultado ex:
Changing "ServicePrincipalName" to a valid URI of "http://ServicePrincipalName2", which is the required format used for service principal names
Retrying role assignment creation: 1/36
{
  "appId": "95142c89-ccd4-4f4b-8464-0c028c76c",
  "displayName": "ServicePrincipalName",
  "name": "http://ServicePrincipalName",
  "password": "87aba571-1982-4946-8b88-731287ce8",
  "tenant": "80bc8516-d41e-4711-a0ec-227764bb"
}

Criar diretorio de credenciais:
mkdir ~/.azure
vi ~/.azure/credentials
[default]
subscription_id=<your-subscription_id>
client_id=<security-principal-appid>
secret=<security-principal-password>
tenant=<security-principal-tenant>

Onde: 
subscription_id é o subscriptionid que voce obtem em 
client_id é o que voce obtem ao criar o principal: appid
secret é o que voce obtem ao criar o principal: password
tenant é o que voce obtem ao criar o principal: tenant


oooouuuu.... exporta... ex:

`export AZURE_SUBSCRIPTION_ID=af23a97c-43f1-4c7b-97bb-26d3f`

`export AZURE_CLIENT_ID=95142c89-ccd4-4f4b-8464-0c76c`

`export AZURE_SECRET=87aba571-1982-4946-8b88-7ce8`

`export AZURE_TENANT=80bc8516-d41e-4711-a0ec-2764bb`


Ex: Sintaxe config.yaml  
\# ==========================  
\# Azure Specifics  
\# ==========================  
resource_group_name: Openshift  
location_resource_group: westus  
storage_account_name: stropenshift  
virtual_network_name: openshiftvn  
private_virtual_network_name: pvtopenshiftvn  
cluster_subnet_name: clustersubnet  
private_cluster_subnet_name: pvtclustersubnet  
admin_username_vm: ansible  
admin_password_vm: Redhat4ever!  
\# ==========================  
\# Subscription data  
\# ==========================  
user: email@subscriptionredhat  
password:   
poolid:   
\# ==========================  
\# Sisu Openshift Cluster  
\# ==========================  
domain: openshift.4players.com.br  
cluster_hostname: master.{{ domain }}  
cluster_public_hostname: master.{{ domain }}  
nfs_server: bastion.{{ domain }}  
subdomain: apps.{{ domain }}  
master_machine_type: Standard_D4s_v3  
infra_machine_type: Standard_D4s_v3  
worker_machine_type: Standard_D2s_v3  
number_of_masters: 3  
number_of_infras: 2  
number_of_workers: 2  

Ex: Sintaxe config.yml (para centos apoio)  
\# ========================  
\# Configuracoes essenciais  
\# =======================  
provider: aws  

\# ========================  
\# Configuracoes AWS  
\# ========================  
aws_access_key: <aws access key>  
aws_secret_key: <aws secret key>  
aws_vpc_network: vpc-87fb8d4a  
aws_subnet_id: subnet-569s2dd0d  
aws_security_group: sg-a9sfee3d9  
aws_key_name: aws  
usuario_ssh_aws: centos  
chave_ssh: /home/seuusuario/aws/aws.pem  
image: ami-cb5803a7  

Informacoes Importantes para Azure como Servico:  
- Usar Google Chrome ao solicitar (Firefox possui bugs)
- Gerar o Vault de senha com os comandos abaixo:  
`az keyvault create -n KeyOpenshiftNew -g Openshift -l 'West US' --enabled-for-template-deployment true`  
`az keyvault secret set --vault-name KeyOpenshiftNew -n OpenshiftSecret --file ~/.ssh/id_rsa`
