# Exemplo de Autenticação JWT com Spring Boot
Este projeto demonstra como implementar autenticação JWT em uma aplicação Spring Boot 3.4.5 com Java 21.

## Tecnologias Utilizadas
- Spring Boot 3.4.5
- Spring Security
- JSON Web Tokens (JWT)
- Spring Data JPA
- PostgreSQL
- Liquibase (para migrações de banco de dados)
- Maven

## Pré-requisitos
- Java 21
- Maven
- Docker (para o banco de dados PostgreSQL)

## Começando
1. Clone o repositório:
```
git clone https://github.com/bruno-npc/jwtbase
```

2. Inicie o banco de dados PostgreSQL com Docker Compose:
```
docker-compose up -d
```

3. Compile e execute a aplicação:
```
mvn spring-boot:run
```

A aplicação estará disponível em `http://localhost:8080`.
## Estrutura do Projeto
- `src/main/java/com/b/base/model`: Classes de entidade
- `src/main/java/com/b/base/repository`: Interfaces de repositório
- `src/main/java/com/b/base/security`: Classes relacionadas à segurança e JWT
- `src/main/java/com/b/base/controller`: Controladores REST
- `src/main/java/com/b/base/dto`: Objetos de Transferência de Dados
- `src/main/java/com/b/base/config`: Classes de configuração
- `src/main/resources/db/changelog`: Arquivos de changelog do Liquibase para migrações de banco de dados

## Endpoints da API

### Autenticação
- `POST /api/auth/signup`: Registrar um novo usuário
- `POST /api/auth/signin`: Autenticar um usuário e obter token JWT

### Endpoints de Teste
- `GET /api/test/all`: Acesso público
- `GET /api/test/user`: Requer papel de USUÁRIO
- `GET /api/test/admin`: Requer papel de ADMINISTRADOR

## Testando a Aplicação
Veja o [Guia do Postman](POSTMAN_GUIDE.md) para instruções detalhadas sobre como testar a aplicação.

## Configuração de Segurança
A configuração de segurança está definida em `SecurityConfig.java`. Ela inclui:

- Filtro de autenticação JWT
- Gerenciamento de sessão stateless
- Regras de autorização de endpoints
- Codificação de senha

## Implementação JWT
Os tokens JWT são gerados após a autenticação bem-sucedida e incluem:
- Nome de usuário no assunto
- Data de emissão
- Data de expiração (configurável em application.properties)
- Papéis (roles) de usuário nas autoridades

## Migrações de Banco de Dados

Este projeto utiliza Liquibase para migrações de esquema de banco de dados. O Liquibase garante que:

1. Todas as alterações no banco de dados são versionadas
2. O esquema do banco de dados é consistente em todos os ambientes
3. Tabelas necessárias são criadas automaticamente na inicialização da aplicação
4. Dados iniciais (como papéis) são inseridos automaticamente

Veja o [Guia do Liquibase](LIQUIBASE_README.md) para mais detalhes sobre como o Liquibase é implementado neste projeto.

## Configuração
Os principais parâmetros de configuração podem ser encontrados em `application.properties`:

```properties
# Configuração JWT
jwt.secret=sua-chave-secreta
jwt.expiration=86400000  # 24 horas em milissegundos

# Configuração Liquibase
spring.liquibase.change-log=classpath:/db/changelog/db.changelog-master.yaml
```

## Licença
Este projeto está licenciado sob a Licença MIT. 
