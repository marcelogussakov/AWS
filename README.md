# AWS

## Topicos 
1. [Notifcação via e-mail quando um objeto é carregado em um bucket no S3](#-notifcação-via-e-mail-quando-um-objeto-é-carregado-em-um-bucket-no-s3)


## Notifcação via e-mail quando um objeto é carregado em um bucket no S3

Primeiro é a criação de um tópico e uma assinatura no serviço SNS

Imagens 

No tópico criado vá até a parte de política de acesso e substitua o que está la pela seguinte política:

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
            "Action": [
                "SNS:Publish"
            ],
            "Resource": "SNS-topic-ARN",
            "Condition": {
                "ArnLike": {
                    "aws:SourceArn": "arn:aws:s3:*:*:bucket-name"
                },
                "StringEquals": {
                    "aws:SourceAccount": "bucket-owner-account-id"
                }
            }
        }
    ]
}                  