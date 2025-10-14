# 🏗 Stack com AWS CloudFormation
Este repositório contém o template do AWS CloudFormation para provisionar a infraestrutura.

## 📁 Estrutura do Repositório
Arquivo/Pasta	Descrição
cloudformation/	Contém os templates (modelos) do CloudFormation.
cloudformation/main.yaml	Template principal que define todos os recursos da stack.
scripts/	Scripts auxiliares, como para deploy via CLI (opcional).
README.md	Este arquivo de documentação.

## Exportar para as Planilhas
📋 Pré-requisitos
Para criar a stack, você precisará ter:

1. Uma conta ativa na AWS.

2. AWS CLI configurada localmente (ou acesso ao Console AWS).

3. As permissões necessárias para criar os recursos definidos no template (ex: iam:CreateRole, s3:CreateBucket, etc.).

## 🚀 Geração da Stack
O processo pode ser feito via Console AWS ou AWS CLI.

### 1. Via AWS Console
Acesse o serviço AWS CloudFormation no Console AWS e selecione a região de destino.

#### Clique em "Create stack" (Criar stack) e escolha "With new resources (standard)".

Em "Specify template", selecione "Upload a template file" e faça upload do arquivo cloudformation/main.yaml.

#### Clique em "Next".

Na página "Specify stack details":

Defina um Stack name (Nome da stack), ex: Projeto-[NomeDoSeuProjeto].

Preencha os Parameters (Parâmetros) solicitados no template (ex: nome do ambiente, prefixo de recursos).

Siga as próximas telas, revisando as configurações e as Capabilities (Capacidades), e clique em "Create stack".

Monitore a aba "Events" até que o status mude para CREATE_COMPLETE.

### 2. Via AWS CLI (Linha de Comando)
Execute o comando abaixo, substituindo os valores entre colchetes ([ ]):

| Bash | 
|--------------------------------------------------------------|
| aws cloudformation create-stack \
--stack-name [NOME_DA_STACK] \
--template-body file://cloudformation/main.yaml \
--parameters \
    ParameterKey=Ambiente,ParameterValue=[dev|prod] \
    ParameterKey=BucketPrefix,ParameterValue=[meuprojeto] \
--capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
--region [REGIAO_AWS] |
ℹ️ Observação sobre CAPABILITY_IAM: O parâmetro --capabilities é obrigatório se o seu template criar recursos do IAM (ex: Roles, Policies).

## 🛠 Visualização dos Recursos
Após a criação bem-sucedida, você pode encontrar as informações e endpoints importantes na aba "Outputs" (Saídas) da sua stack no Console CloudFormation.

### Exemplo de Output que você pode obter:

| Chave |	Descrição |	Valor (Exemplo) | 
|---|---|---|
| S3BucketName	| Nome do S3 Bucket criado |	meuprojeto-s3-backup-dev
| LambdaFunctionName |	Nome da Função Lambda |	processa-dados-dev

## Exportar para as Planilhas
🗑 Exclusão da Stack
Para excluir todos os recursos criados pela stack (e evitar cobranças), siga os passos:

Acesse o CloudFormation Console.

Selecione a stack pelo nome ([NOME_DA_STACK]).

Clique em "Delete" (Excluir).

Atenção: Se o template tiver políticas de exclusão (DeletionPolicy: Retain ou DeletionPolicy: Snapshot), alguns recursos podem não ser excluídos. Verifique o template antes.

Monitore os eventos até que o status mude para DELETE_COMPLETE.
