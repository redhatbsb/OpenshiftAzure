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

`export AZURE_SUBSCRIPTION_ID=af23a97c-43f1-4cd7b-974bb-26d3f`

`export AZURE_CLIENT_ID=95142c89-ccd4-4f4xb-8464-0c76c`

`export AZURE_SECRET=87aba571-1982-44946-8bq88-7ce98`

`export AZURE_TENANT=80bc85s16-d421e-4711-a03ec-2764fbb`

