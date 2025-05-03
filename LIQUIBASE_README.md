# Guia de Implementação do Liquibase
Este guia explica como o Liquibase é implementado na aplicação de Autenticação JWT com Spring Boot.

## Introdução
Liquibase é uma biblioteca independente de banco de dados para rastrear, gerenciar e aplicar mudanças no esquema do banco de dados. Neste projeto, usamos o Liquibase para:

1. Criar tabelas de banco de dados automaticamente
2. Inicializar dados necessários
3. Controlar versões das alterações no banco de dados
4. Garantir esquema de banco de dados consistente em todos os ambientes

## Configuração
A configuração do Liquibase está definida no arquivo `application.properties`:

```
spring.liquibase.change-log=classpath:/db/changelog/db.changelog-master.yaml
spring.liquibase.enabled=true
spring.jpa.hibernate.ddl-auto=none
```

Observe que `spring.jpa.hibernate.ddl-auto=none` está configurado para impedir que o Hibernate crie ou atualize o esquema automaticamente, deixando toda a gestão de esquema para o Liquibase.

## Estrutura de Changelogs
Nossos arquivos de changelog do Liquibase estão estruturados da seguinte forma:

- **Changelog Principal**: `src/main/resources/db/changelog/db.changelog-master.yaml`
  Este arquivo inclui todos os outros changelogs.

- **Arquivos de Mudança**: `src/main/resources/db/changelog/changes/`
  Diretório contendo arquivos individuais de changelog.

  - `01-create-users-and-roles.yaml`: Cria as tabelas iniciais (users, roles, user_roles) e insere papéis padrão.

## Adicionando Novas Mudanças
Para adicionar novas alterações no banco de dados:

1. Crie um novo arquivo de changelog em `src/main/resources/db/changelog/changes/` com um prefixo numérico sequencial (ex: `02-add-new-table.yaml`).
2. Defina suas alterações usando o formato YAML do Liquibase.
3. Inclua seu novo arquivo de changelog no changelog principal: `db.changelog-master.yaml`.

Exemplo de adição de uma nova tabela:
```
databaseChangeLog:
  - changeSet:
      id: 5
      author: seu_nome
      changes:
        - createTable:
            tableName: nova_tabela
            columns:
              - column:
                  name: id
                  type: BIGINT
                  autoIncrement: true
                  constraints:
                    primaryKey: true
                    nullable: false
              - column:
                  name: nome
                  type: VARCHAR(100)
```

## Benefícios do Uso do Liquibase
1. **Controle de Versão do Banco de Dados**: Acompanhe todas as alterações no esquema do banco de dados ao longo do tempo.
2. **Consistência de Ambiente**: Garanta estruturas de banco de dados consistentes entre ambientes de desenvolvimento, teste e produção.
3. **Automação**: Aplique automaticamente alterações no banco de dados quando a aplicação iniciar.
4. **Capacidade de Rollback**: Possibilidade de reverter alterações se necessário.

## Notas Importantes
- Nunca modifique changelogs existentes após terem sido commitados no controle de versão. Em vez disso, crie novos changelogs para alterações adicionais.
- Sempre teste os changelogs em um ambiente de desenvolvimento antes de aplicá-los em produção.
- Lembre-se de incluir novos arquivos de changelog no arquivo de changelog principal.

## Solução de Problemas
Se você encontrar problemas com o Liquibase:
1. Verifique os logs da aplicação para mensagens de erro específicas.
2. Verifique se seus arquivos de changelog estão formatados corretamente em YAML.
3. Certifique-se de que seu usuário de banco de dados tenha privilégios suficientes para criar e modificar tabelas.