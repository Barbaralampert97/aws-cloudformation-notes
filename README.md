# 🌐 AWS CloudFormation – Infraestrutura Automatizada

## 📖 Descrição do Projeto
Este repositório contém **anotações e insights** adquiridos durante a prática do projeto **"Implementando Infraestrutura Automatizada com AWS CloudFormation"** da **DIO** em parceria com o **Santander**, no programa **Code Girls 2025**.  

> O objetivo é consolidar conceitos de **Infraestrutura como Código (IaC)** utilizando o **AWS CloudFormation**, ferramenta que permite criar, configurar e gerenciar recursos da AWS de forma **automatizada, consistente e segura**.

---

## 🚀 O que é AWS CloudFormation?
O **AWS CloudFormation** é um serviço da AWS que permite **definir a infraestrutura em arquivos de template (JSON ou YAML)** e **provisionar recursos automaticamente**.  

- Os recursos são organizados em **stacks (pilhas)**, que facilitam a gestão e replicação de ambientes.  
- Cada stack agrupa recursos relacionados, permitindo criar, atualizar ou deletar tudo de uma vez.  
- Ideal para quem quer **automatizar, padronizar e controlar ambientes na AWS**.

📚 [O que é AWS CloudFormation](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/Welcome.html)

---
## 💻 O que é IaC (Infrastructure as Code)
**IaC** significa **Infraestrutura como Código**. É a prática de **gerenciar e provisionar infraestrutura usando arquivos de configuração**, em vez de fazer tudo manualmente.  

### Benefícios principais:
- 🤖 **Automatização:** Criação e configuração de recursos de forma automática.  
- 📦 **Reprodutibilidade:** Ambientes idênticos podem ser criados várias vezes (dev, teste, produção).  
- 📜 **Versionamento:** Infraestrutura rastreável e controlada via Git.  
- ✅ **Padronização:** Evita erros humanos e garante consistência entre ambientes.  

📚 [Documentação AWS sobre IaC](https://docs.aws.amazon.com/cloudformation/index.html)

---
## 🎯 Principais Benefícios do CloudFormation

### 1️⃣ Automatização 🤖
- Criação, configuração e gerenciamento de recursos **automatizados**.  
- Redução de **erros manuais** em tarefas repetitivas.

### 2️⃣ Consistência & Padronização 📦
- Templates podem ser **reutilizados** para gerar **cópias idênticas** da infraestrutura.  
- Facilita manutenção e replicação de ambientes (**desenvolvimento, teste, produção**).  
- Garante que todos os ambientes sigam o **mesmo padrão de configuração**.

### 3️⃣ Economia de Custos 💰
- Evita retrabalho e desperdício de recursos.  
- Permite **simular e validar** a infraestrutura antes da criação, reduzindo gastos desnecessários.  
- Reduz custos relacionados a erros humanos e configurações manuais.

### 4️⃣ Segurança 🔐
- Permite aplicar **permissões e políticas de segurança** diretamente nos templates.  
- Mantém ambientes **consistentes e confiáveis**, reduzindo riscos de falhas críticas.

📚 [Benefícios CloudFormation](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/Welcome.html#concepts-benefits)

---

## 🧩 Estrutura Básica de um Template
Um **template CloudFormation** descreve toda a infraestrutura que será criada e geralmente contém:  

- **AWSTemplateFormatVersion** → versão do template.  
- **Description** → explicação do que o template faz.  
- **Parameters** → valores que podem ser passados para customizar a stack.  
- **Resources** → os recursos a serem criados (ex.: EC2, S3, VPC).  
- **Outputs** → informações úteis após a criação da stack.  

📚 [Estrutura de Templates](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/template-anatomy.html)

### 📝 Sobre JSON vs YAML
- **YAML** → mais legível, menos verboso, ideal para leitura humana.  
- **JSON** → mais rígido, requer atenção com vírgulas e chaves, mas tem **suporte mais amplo em ferramentas AWS**.  
- Ambos são oficialmente suportados pelo CloudFormation.

📚 [JSON e YAML no CloudFormation](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/template-formats.html)

---

## 🔄 Fluxo Prático de Uso
1. Criar um **template YAML ou JSON**.  
2. Abrir o **Console da AWS → CloudFormation**.  
3. Clicar em **Create Stack**.  
4. Fazer **upload do template**.  
5. Definir **parâmetros, permissões e opções de execução**.  
6. Executar a **criação da stack**.  
7. Acompanhar o status até a conclusão.  

📚 [Criando Stacks](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html)

---

## 📊 Comparação: CloudFormation x Terraform

| Critério        | ☁️ CloudFormation                     | 🌍 Terraform                      |
|-----------------|-------------------------------------|----------------------------------|
| Integração      | 100% integrado à AWS                 | Multicloud (AWS, Azure, GCP etc.) |
| Linguagem       | JSON/YAML                            | HCL                               |
| Flexibilidade   | Focado na AWS, menos multicloud      | Alta flexibilidade, multicloud     |
| Gerenciamento   | Stacks                               | State Files                        |
| Custos          | Sem custo adicional (paga apenas pelos recursos) | Open-source, requer backend para estados remotos |
| Observação      | Totalmente integrado e confiável para AWS | Mais flexível, mas exige configuração adicional |

📚 [CloudFormation vs Terraform](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/cfn-vs-terraform.html)

---

## 🛠️ Exemplo Prático de Template (YAML)

```yaml
# 🌐 Template de exemplo: EC2 + Security Group

AWSTemplateFormatVersion: "2010-09-09"
Description: >
  Exemplo de criação de uma instância EC2 com Security Group usando CloudFormation.

Parameters:
  KeyName:
    Description: "Nome da chave SSH existente"
    Type: AWS::EC2::KeyPair::KeyName

  InstanceType:
    Description: "Tipo da instância EC2"
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - t2.small
      - t2.medium

Resources:
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: "Security Group de exemplo"
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0

  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      KeyName: !Ref KeyName
      ImageId: ami-0c02fb55956c7d316
      SecurityGroups:
        - !Ref MySecurityGroup

Outputs:
  InstanceId:
    Description: "ID da instância EC2 criada"
    Value: !Ref MyEC2Instance

  PublicIP:
    Description: "Endereço IP público da instância"
    Value: !GetAtt MyEC2Instance.PublicIp

```
✅ Conclusão

O AWS CloudFormation é essencial para quem deseja trabalhar com infraestrutura automatizada na AWS:

✔ Automatiza processos

✔ Mantém consistência e padronização

✔ Reduz custos e retrabalho

✔ Aumenta segurança e confiabilidade

✔ Facilita replicação de ambientes

✔ Integrado totalmente à AWS

Ideal para quem quer ambientes confiáveis, reproduzíveis e fáceis de manter.
