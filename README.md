# Gerenciamento-de-Seguran-a-e-Identidade-na-AWS-
Este repositório apresenta uma aplicação prática para explorar os principais serviços de **Segurança e Identidade** da AWS, incluindo **IAM**, **KMS** e **CloudTrail**. O objetivo é fornecer uma solução funcional que possa ser utilizada por empresas para gerenciar acessos, criptografia e auditoria de forma eficiente.
---

## **📋 Pré-requisitos**

1. **Conta AWS**: Crie ou utilize uma conta existente.
2. **AWS CLI Configurado**: Configure as credenciais utilizando o comando:
   ```bash
   aws configure
   ```
3. **Python 3.8+**: Instale o Python para executar scripts locais.

---

## **1️⃣ IAM: Controle de Acesso**

### **Objetivo**
Criar um sistema de gerenciamento de acesso com usuários, grupos e políticas customizadas.

### **Passos no Console AWS**

1. Acesse o serviço **IAM**.
2. **Crie um Grupo**:
   - Nome do grupo: `AdminGroup`.
   - Permissões: Anexe a política `AdministratorAccess`.
3. **Crie um Usuário**:
   - Nome do usuário: `AdminUser`.
   - Tipo de acesso: Habilite "Programmatic access" e "AWS Management Console access".
   - Adicione o usuário ao grupo `AdminGroup`.
4. Salve as credenciais geradas (Access Key e Secret Key).

### **Script para Testar o Acesso**

Crie um script em Python para listar os usuários utilizando as credenciais do IAM:
```python
import boto3

def listar_usuarios():
    iam = boto3.client('iam')
    response = iam.list_users()
    for user in response['Users']:
        print(f"Usuário: {user['UserName']}")

listar_usuarios()
```

---

## **2️⃣ KMS: Criptografia**

### **Objetivo**
Criar e gerenciar chaves de criptografia para proteger dados sensíveis.

### **Passos no Console AWS**

1. Acesse o serviço **KMS**.
2. **Crie uma Chave Gerenciada**:
   - Tipo: "Symmetric".
   - Alias: `MyEncryptionKey`.
   - Defina permissões específicas para usuários ou roles.
3. Anote o ARN da chave criada.

### **Script para Criptografia e Descriptografia**

Utilize a chave criada para criptografar e descriptografar dados:
```python
import boto3

def criptografar_texto(chave_arn, texto):
    kms = boto3.client('kms')
    response = kms.encrypt(
        KeyId=chave_arn,
        Plaintext=texto.encode('utf-8')
    )
    return response['CiphertextBlob']

def descriptografar_texto(ciphertext):
    kms = boto3.client('kms')
    response = kms.decrypt(
        CiphertextBlob=ciphertext
    )
    return response['Plaintext'].decode('utf-8')

# Exemplo de uso
arn_chave = "ARN_DA_SUA_CHAVE"
ciphertext = criptografar_texto(arn_chave, "Texto secreto")
print("Texto Criptografado:", ciphertext)
print("Texto Original:", descriptografar_texto(ciphertext))
```

---

## **3️⃣ CloudTrail: Auditoria e Monitoramento**

### **Objetivo**
Configurar o CloudTrail para rastrear atividades e armazenar logs de auditoria.

### **Passos no Console AWS**

1. Acesse o serviço **CloudTrail**.
2. Clique em **Create Trail** e configure:
   - Nome: `MyTrail`.
   - S3 Bucket: Crie ou selecione um bucket para armazenar os logs.
   - Habilite a opção de logs multi-regiões.
3. Salve as configurações.

### **Visualizando Logs**

1. Acesse o bucket S3 configurado para verificar os logs.
2. Utilize o seguinte script para listar os logs:
```python
import boto3

def listar_logs(bucket_name):
    s3 = boto3.client('s3')
    response = s3.list_objects_v2(Bucket=bucket_name, Prefix='AWSLogs/')
    for obj in response.get('Contents', []):
        print(f"Log encontrado: {obj['Key']}")

listar_logs("NOME_DO_BUCKET")
```

---

## **🌟 Estrutura do Projeto**

```
SegurancaAWS/
│
├── iam_test.py                # Script para testar o IAM
├── kms_encrypt.py             # Script para criptografar/descriptografar com KMS
├── cloudtrail_logs.py         # Script para listar logs do CloudTrail
├── requirements.txt           # Dependências do Python
├── README.md                  # Documentação do projeto
└── .gitignore                 # Arquivos a serem ignorados pelo Git
```

---

## **📜 Como Contribuir**

1. Faça um fork do repositório.
2. Crie uma branch para sua contribuição:
   ```bash
   git checkout -b minha-contribuicao
   ```
3. Envie suas melhorias e crie um Pull Request.

---

## **📎 Links Úteis**

- [Documentação AWS IAM](https://docs.aws.amazon.com/iam/)
- [Documentação AWS KMS](https://docs.aws.amazon.com/kms/)
- [Documentação AWS CloudTrail](https://docs.aws.amazon.com/cloudtrail/)

---

Desenvolvido com 💡 e AWS!
