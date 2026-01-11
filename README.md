# Atividade – CI/CD com GitHub Actions e CloudFormation

## Descrição
Este projeto implementa um pipeline de CI/CD utilizando GitHub Actions para realizar o deploy automático de uma infraestrutura AWS definida com CloudFormation.

A infraestrutura inclui:
- EC2
- Application Load Balancer
- RDS MySQL
- Security Groups

---

## Estrutura do Repositório
aws-devops-ci-cd/
├── cloudformation/
│   └── template.yaml
├── .github/
│   └── workflows/
│       └── deploy.yml
└── README.md

## Funcionamento do Pipeline

O pipeline é acionado automaticamente quando ocorre:
- Push na branch `main`
- Pull Request para a branch `main`

Etapas:
1. Checkout do código
2. Autenticação na AWS
3. Validação do template CloudFormation
4. Deploy automático da stack na AWS

O estado da infraestrutura é gerenciado diretamente pelo AWS CloudFormation.

---

## Configuração de Secrets

É necessário configurar os seguintes secrets no GitHub:

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- KEY_PAIR_NAME
- DB_USERNAME
- DB_PASSWORD

---

## Como reproduzir o ambiente

1. Clone o repositório
2. Configure os secrets no GitHub
3. Faça um push na branch main
4. Acompanhe a execução em **Actions**
5. Verifique os recursos criados no AWS Console

---

## Limpeza dos recursos

Após a avaliação, remova a stack:

```bash
aws cloudformation delete-stack --stack-name aws-devops-ci-cd
