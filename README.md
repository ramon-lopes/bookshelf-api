# 📚 BookShelf API

API REST para gerenciamento de biblioteca pessoal com sistema de empréstimos e avaliações de livros.

## 🎯 Sobre o projeto

O BookShelf API é um projeto backend desenvolvido em Java com Spring Boot. O objetivo é ir além de um simples CRUD, aplicando regras de negócio reais como controle de estado de empréstimos, restrições por usuário e avaliações condicionais.

## 🚀 Tecnologias

- **Java 21**
- **Spring Boot 3.2**
- **Spring Data JPA** + Hibernate
- **Spring Security** + JWT
- **PostgreSQL**
- **Maven**

## 📐 Modelo de Domínio

```
Book ──────────< Loan >────────── User
  │                                  │
  └──────────< Review >──────────────┘
```

### Entidades

| Entidade | Responsabilidade |
|----------|-----------------|
| `Book` | Catálogo com status (AVAILABLE / BORROWED / RESERVED) |
| `User` | Usuários com roles (ADMIN / MEMBER) |
| `Loan` | Empréstimos com prazo e status |
| `Review` | Avaliações de 1 a 5 estrelas |

### Regras de negócio

- Um livro só pode ser emprestado se estiver `AVAILABLE`
- Um usuário não pode ter mais de **3 empréstimos ativos** simultaneamente
- Avaliações só são permitidas após a devolução do livro
- Um usuário pode avaliar o mesmo livro apenas **uma vez**
- Prazo padrão de empréstimo: **14 dias**

## 🗂️ Estrutura do projeto

```
src/
└── main/
    └── java/com/bookshelf/api/
        ├── model/          # Entidades de domínio
        ├── repository/     # Interfaces JPA (fase 3)
        ├── service/        # Regras de negócio (fase 3)
        ├── controller/     # Endpoints REST (fase 4)
        ├── dto/            # Request e Response DTOs (fase 4)
        ├── exception/      # Tratamento global de erros (fase 4)
        └── config/         # Segurança e configurações (fase 5)
```

## 🗺️ Roadmap

### ✅ Fase 1 — Modelagem (atual)
- [x] Classes de domínio (`Book`, `User`, `Loan`, `Review`)
- [x] Enums de status (`BookStatus`, `UserRole`, `LoanStatus`)
- [x] Regras básicas no modelo

### 🔲 Fase 2 — Spring Boot
- [ ] Subir a aplicação com Spring Boot
- [ ] Estrutura de pacotes por camada
- [ ] Endpoint de health check

### 🔲 Fase 3 — Persistência (JPA)
- [ ] Mapeamento com `@Entity`, `@ManyToOne`, `@OneToMany`
- [ ] Repositórios com Spring Data JPA
- [ ] Migrations com Flyway

### 🔲 Fase 4 — API REST
- [ ] CRUD de livros e usuários
- [ ] Endpoint de empréstimo (`POST /loans`)
- [ ] Endpoint de devolução (`PATCH /loans/{id}/return`)
- [ ] Tratamento de erros com `@ControllerAdvice`
- [ ] Validação com Bean Validation

### 🔲 Fase 5 — Segurança
- [ ] Autenticação com Spring Security + JWT
- [ ] Roles: ADMIN gerencia catálogo, MEMBER empresta
- [ ] Proteção de rotas por role

## ⚙️ Como executar

### Pré-requisitos

- Java 21+
- Maven 3.9+
- PostgreSQL 15+ (fase 3 em diante)

### Rodando localmente

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/bookshelf-api.git
cd bookshelf-api

# Configure o banco (fase 3)
# Crie um banco PostgreSQL chamado 'bookshelf'
# e ajuste as credenciais em application.properties

# Rode a aplicação
./mvnw spring-boot:run
```

## 📋 Endpoints (planejados)

```
GET    /books              Lista todos os livros
POST   /books              Cadastra um livro (ADMIN)
GET    /books/{id}         Busca livro por ID
PUT    /books/{id}         Atualiza livro (ADMIN)
DELETE /books/{id}         Remove livro (ADMIN)

POST   /loans              Realiza empréstimo
PATCH  /loans/{id}/return  Devolve livro
GET    /loans/user/{id}    Histórico do usuário

POST   /reviews            Avalia um livro
GET    /reviews/book/{id}  Avaliações de um livro

POST   /auth/register      Cadastro de usuário
POST   /auth/login         Login (retorna JWT)
```

## 📄 Licença

MIT License — sinta-se livre para usar e adaptar.
