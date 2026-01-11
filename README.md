# Atividade – CI/CD com GitHub Actions e CloudFormation

## Descrição
Este projeto demonstra a implementação de uma infraestrutura na AWS utilizando CloudFormation (Infraestrutura como Código) integrada a um pipeline CI/CD com GitHub Actions. O objetivo é automatizar o provisionamento de recursos de forma segura, reprodutível e alinhada às boas práticas de DevOps.

A infraestrutura provisionada é composta por:
- Uma instância EC2 (Free Tier)
- Um Application Load Balancer (ALB)
- Um banco de dados RDS MySQL em subnets privadas
Todo o processo de deploy é realizado automaticamente a partir de eventos de push ou pull request na branch principal do repositório.
---

## Estrutura do Repositório
.
├── .github/
│   └── workflows/
│       └── deploy.yml
├── cloudformation/
│   └── template.yaml
└── README.md

- `template.yaml`: define toda a infraestrutura AWS utilizando CloudFormation
- `deploy.yml`: workflow do GitHub Actions responsável pelo deploy automático
- `README.md`: documentação do projeto

## Estrutura do Repositório
A arquitetura segue boas práticas da AWS:
- EC2 em subnet pública
- Application Load Balancer distribuído em duas subnets públicas em zonas de disponibilidade diferentes
- Banco de dados RDS em subnets privadas
- Controle de acesso por Security Groups
- AMI do Amazon Linux 2 obtida dinamicamente via SSM Parameter Store

## Funcionamento do Pipeline

O pipeline é acionado automaticamente quando ocorre:
- Push na branch `main`
- Pull Request para a branch `main`

Etapas:
1. Checkout do código
2. Autenticação na AWS
3. Validação do template CloudFormation
4. Deploy automático da stack na AWS

A execução do pipeline garante que toda a infraestrutura seja criada sem intervenção manual, reforçando o conceito de infraestrutura como código.

---

## Configuração de Secrets

As credenciais e variáveis sensíveis são armazenadas de forma segura utilizando GitHub Secrets.

| Nome do Secret          | Descrição                                         |
| ----------------------- | ------------------------------------------------- |
| `AWS_ACCESS_KEY_ID`     | Access Key do usuário IAM                         |
| `AWS_SECRET_ACCESS_KEY` | Secret Key do usuário IAM                         |
| `KEY_PAIR_NAME`         | Nome do Key Pair existente na AWS                 |
| `DB_USERNAME`           | Usuário master do banco RDS                       |
| `DB_PASSWORD`           | Senha do banco RDS                                |
| `VPC_ID`                | ID da VPC                                         |
| `PUBLIC_SUBNET_1`       | Subnet pública (AZ 1)                             |
| `PUBLIC_SUBNET_2`       | Subnet pública (AZ 2)                             |
| `PRIVATE_SUBNETS`       | Lista de subnets privadas (separadas por vírgula) |

---

## Como reproduzir o ambiente

1. Criar um repositório no GitHub e clonar localmente
2. Adicionar os arquivos do projeto conforme a estrutura apresentada
3. Configurar os secrets no GitHub
4. Realizar um commit e push para a branch main
5. Acompanhar a execução do pipeline na aba Actions
6. Após a execução, verificar a stack criada no AWS CloudFormation

---

## Limpeza dos recursos
Após a execução bem-sucedida do pipeline:
- A stack aparece como CREATE_COMPLETE no CloudFormation
- O DNS do Application Load Balancer fica disponível
- O acesso ao DNS retorna a página hospedada na instância EC2

---

## Limpeza dos recursos

Para evitar custos desnecessários, recomenda-se remover a stack após os testes:

```bash
aws cloudformation delete-stack --stack-name aws-devops-ci-cd ```


