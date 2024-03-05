# AWS

## Topicos 
- [AWS](#aws)
  - [Topicos](#topicos)
  - [Notifcação via e-mail quando um objeto é carregado em um bucket no S3](#notifcação-via-e-mail-quando-um-objeto-é-carregado-em-um-bucket-no-s3)
  - [Criando o bucket e usuario sem permissão de delete no AWS CL](#criando-o-bucket-e-usuario-sem-permissão-de-delete-no-aws-cl)

## Notifcação via e-mail quando um objeto é carregado em um bucket no S3

Primeiro é a criação de um tópico 

    aws sns create-topic --name 'nome do topico'

Depois edite a politica de acesso para que o S3 tenha acesso ao SNS

    aws sns set-topic-attributes --topic-arn 'ARN do tópico' --attribute-name Policy --attribute-value file://'nome do arquivo .json'

Utilizei a seguinte política

    {
    "Version": "2012-10-17",
    "Id": "example-ID",
    "Statement": [
      {
        "Sid": "Example SNS topic policy",
        "Effect": "Allow",
        "Principal": {
          "Service": "s3.amazonaws.com"
        },
        "Action": "SNS:Publish",
        "Resource": "arn:aws:sns:us-east-1:429968804647:S3-Object",
        "Condition": {
          "StringEquals": {
            "aws:SourceAccount": "429968804647"
          },
          "ArnLike": {
            "aws:SourceArn": "arn:aws:s3:*:*:*"
          }
        }
      }
    ]
  }
    
Criando a Assinatura

    aws sns subscribe --topic-arn 'ARN do topico criado' --protocol email --notification-endpoint 'e-mail para notificação'

## Criando o bucket e usuario sem permissão de delete no AWS CL

Primeiro criando a política de acesso

    aws iam create-policy --policy-name 'nome da política' --policy-document file://'caminho do arquivo .json'

Criando o usuario 

    aws iam create-user --user-name 'nome de usuario'

Caso precise de login via console

    aws iam create-login-profile --user-name 'nome de usuario' --password 'senha' --password-reset-required    

Criando as chaves de acesso

    aws iam create-access-key --user-name 'nome de usuario'


Anexando a política

Primeiro para pegar o ARN da política
 
    aws iam list-policies --scope Local 
Com o ARN copiado 
 
    aws iam attach-user-policy --user-name 'usuario criado' --policy-arn 'arn da política'

Criando o bucket
  
    aws s3api create-bucket --bucket 'nome do bucket' --region us-east-1


