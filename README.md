# 📱 Gerenciador de Assinaturas de Apps (Clean Architecture)

Este projeto é uma API REST desenvolvida em **Java** com **Spring Boot** para gerenciar uma loja de aplicativos, focando no ciclo de vida de assinaturas, pagamentos e catálogo de apps. O sistema foi arquitetado seguindo os princípios da **Clean Architecture**, garantindo desacoplamento entre regras de negócio e infraestrutura.

## 🎯 Objetivo

Oferecer um backend robusto para gerenciar:
* **Aplicativos** (Cadastro e gestão de custos mensais).
* **Clientes** (Dados pessoais e histórico).
* **Assinaturas** (Vigência, status e vínculo com apps).
* **Pagamentos** (Registro, validação de valores e estornos).

## 🏗️ Arquitetura

O projeto adota a **Clean Architecture** para isolar o núcleo da aplicação de frameworks e banco de dados.

### Camadas do Projeto
1.  **Domain (Domínio):** Contém as entidades corporativas (`User`, `Client`, `Applicative`, `Signature`, `Payment`) e Enums. É o "coração" do sistema, sem dependências externas.
2.  **Application (Aplicação):** Contém os **Use Cases** (Casos de Uso). Aqui reside a lógica da aplicação (ex: validar se um pagamento cobre o custo do app).
3.  **Infrastructure (Infraestrutura):** Implementação técnica. Contém os Repositórios (Spring Data JPA), Entidades de Banco de Dados e Controladores REST.

## 🚀 Funcionalidades (Casos de Uso)

O sistema implementa regras de negócio específicas através de classes de Caso de Uso dedicadas:

### 💰 Gestão de Pagamentos
**Use Case:** `RegisterPaymentUC`
* Registra pagamentos associados a uma assinatura.
* **Regra de Negócio:** Verifica automaticamente se o valor pago é suficiente para cobrir o custo mensal do aplicativo.
    * *Sucesso:* Retorna status `OK`.
    * *Falha:* Retorna status `INCORRECT_VALUE` se o valor for insuficiente.

### 📝 Gestão de Assinaturas
**Use Case:** `ValidSignatureUC`, `GetSignaturesForClientUC`, `GetSignaturesForAppUC`
* **Validação:** Verifica se uma assinatura existe e se está ativa/válida no período atual.
* **Consultas:**
    * Listar todas as assinaturas de um **Cliente**.
    * Listar todas as assinaturas de um **Aplicativo**.
    * *Automação:* Atualiza o status das assinaturas automaticamente a cada consulta.

### 📱 Gestão de Aplicativos
**Use Case:** `UpdateCostUC`
* Permite a atualização do custo mensal de um aplicativo.
* Valida regras de consistência (ex: custo não pode ser negativo).

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Java 17+
* **Framework:** Spring Boot 3
* **Persistência:** Spring Data JPA / Hibernate
* **Banco de Dados:** H2 (Dev) / MySQL (Prod)
* **Arquitetura:** Clean Architecture

## 📂 Estrutura de Pastas e Pacotes

A organização do projeto reflete a aplicação estrita da Clean Architecture, separando responsabilidades em pacotes distintos para garantir a inversão de dependência.

```bash
src/main/java/tf/fds/app
├── application
│   ├── requestDTO          # Objetos de entrada (Input Data) - Isolam o domínio da API externa
│   ├── responseDTO         # Objetos de saída (Output Data) - Formatam a resposta para o cliente
│   └── useCases            # Orquestração das regras de negócio (Casos de Uso)
│
├── domain
│   ├── entities            # Entidades puras do negócio (Sem anotações de framework)
│   ├── Enums               # Constantes de domínio (ex: StatusPagamento)
│   ├── repositories        # Interfaces (Contratos) que a Infra deve implementar
│   └── services            # Interfaces de serviços de domínio
│
└── infra
    ├── controllers         # Pontos de entrada REST (Spring Controllers)
    └── repositories        # Implementação técnica da persistência
        ├── adapter         # Adaptadores para converter Entidades JPA em Entidades de Domínio
        ├── entities        # Entidades ORM (JPA/Hibernate) mapeadas para o banco
        ├── implemRepositories # Implementação concreta dos repositórios do domínio
        └── InterfJPA       # Interfaces que estendem JpaRepository (Spring Data)
```


## 🗄️ Modelo de Dados (Entidades)

O sistema utiliza um modelo relacional robusto:

* **Signature**: Conecta `Client` e `Applicative`.
* **Payment**: É vinculado a uma `Signature`.
* **User**: Gerencia o acesso administrativo ao sistema.

## ▶️ Como Executar

### Pré-requisitos
* Java JDK 17 ou superior.
* Maven.

### Passos

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/Daniel-C-Martins/App_Store_Subscription_Manager.git
   ```
2. **Execute a aplicação:**
   ```bash
   ./mvnw spring-boot:run
   ```
3. **Acesse a API:**
   * A aplicação iniciará na porta `8080` (padrão).
   * Use o Postman ou Insomnia para testar os endpoints.

---

## 👥 Autores

* **Bruno Neves, Daniel Martins, Gabriel de Cezaro, Igor Ponticelli** - *Desenvolvimento e Arquitetura*

*Desenvolvido como parte da disciplina de Fundamentos de Desenvolvimento de Software (FDS).*

   
