# Guia Passo a Passo para Autenticação JWT Spring Boot com Postman
Este guia ajudará você a testar a implementação de autenticação JWT usando o Postman.

## Pré-requisitos
1. Certifique-se de ter o Postman instalado.
2. A aplicação Spring Boot deve estar em execução.
3. O banco de dados PostgreSQL deve estar rodando com as credenciais corretas, conforme especificado no arquivo `application.properties`.

## Passo 1: Registrar um Novo Usuário
1. Abra o Postman.
2. Crie uma nova requisição POST com a URL: `http://localhost:8080/api/auth/signup`.
3. Vá para a aba Headers e adicione:
   - Chave: `Content-Type`
   - Valor: `application/json`
4. Vá para a aba Body, selecione "raw" e escolha "JSON" no dropdown.
5. Insira o seguinte payload JSON:

```json
{
    "username": "usuarioteste",
    "email": "usuario@exemplo.com",
    "password": "senha123",
    "roles": ["user"]
}
```

6. Clique em "Send". Você deve receber uma resposta com "Usuário registrado com sucesso!".

## Passo 2: Registrar um Usuário Administrador
1. Crie outra requisição POST com a URL: `http://localhost:8080/api/auth/signup`.
2. Use os mesmos cabeçalhos do passo anterior.
3. Insira o seguinte JSON no corpo:

```json
{
    "username": "admin",
    "email": "admin@exemplo.com",
    "password": "admin123",
    "roles": ["admin"]
}
```

4. Clique em "Send". Você deve receber uma resposta com "Usuário registrado com sucesso!".

## Passo 3: Login e Obtenção do Token JWT
1. Crie uma nova requisição POST com a URL: `http://localhost:8080/api/auth/signin`.
2. Vá para a aba Headers e adicione:
   - Chave: `Content-Type`
   - Valor: `application/json`
3. Insira o seguinte payload JSON:

```json
{
    "username": "usuarioteste",
    "password": "senha123"
}
```

4. Clique em "Send". Você deve receber uma resposta como esta:

```json
{
    "token": "eyJhbGciOiJIUzI1NiJ9...",
    "type": "Bearer",
    "id": 1,
    "username": "usuarioteste",
    "email": "usuario@exemplo.com",
    "roles": ["ROLE_USER"]
}
```

5. Copie o valor do token (sem aspas) da resposta.

## Passo 4: Testar Endpoint Público
1. Crie uma nova requisição GET com a URL: `http://localhost:8080/api/test/all`.
2. Clique em "Send". Você deve receber uma resposta: "Public Content."
3. Este endpoint é público e não requer autenticação.

## Passo 5: Testar Endpoint Protegido de Usuário
1. Crie uma nova requisição GET com a URL: `http://localhost:8080/api/test/user`.
2. Vá para a aba Headers e adicione:
   - Chave: `Authorization`
   - Valor: `Bearer eyJhbGciOiJIUzI1NiJ9...` (use o token do Passo 3)
3. Clique em "Send". Você deve receber uma resposta: "User Content."
4. Este endpoint é protegido e requer um papel de usuário.

## Passo 6: Testar Endpoint Protegido de Administrador
1. Primeiro, faça login como usuário administrador:
   - Crie uma requisição POST com URL: `http://localhost:8080/api/auth/signin`
   - Use as credenciais do administrador:
   ```json
   {
       "username": "admin",
       "password": "admin123"
   }
   ```
   - Copie o token de administrador da resposta.

2. Crie uma nova requisição GET com a URL: `http://localhost:8080/api/test/admin`.
3. Vá para a aba Headers e adicione:
   - Chave: `Authorization`
   - Valor: `Bearer eyJhbGciOiJIUzI1NiJ9...` (use o token de administrador)
4. Clique em "Send". Você deve receber uma resposta: "Admin Board."
5. Este endpoint é protegido e requer um papel de administrador.

## Passo 7: Testar Usuário Tentando Acessar Endpoint de Administrador
1. Crie uma nova requisição GET com a URL: `http://localhost:8080/api/test/admin`.
2. Vá para a aba Headers e adicione:
   - Chave: `Authorization`
   - Valor: `Bearer eyJhbGciOiJIUzI1NiJ9...` (use o token de usuário do Passo 3)
3. Clique em "Send". Você deve receber um erro 403 Forbidden.
4. Isso mostra que um usuário comum não pode acessar endpoints de administrador.

## Solução de Problemas
- Se você receber um erro 401 Unauthorized, certifique-se de que seu token é válido e está formatado corretamente no cabeçalho Authorization.
- Se seu token expirou, você precisará fazer login novamente para obter um novo token.
- Verifique se seu banco de dados está em execução e acessível com as credenciais em application.properties.
- Certifique-se de que a aplicação foi iniciada corretamente e está rodando na porta 8080.

## Estrutura do Token JWT
O token JWT tem três partes separadas por pontos:
1. Cabeçalho: Contém o algoritmo e tipo de token
2. Payload: Contém claims como id de usuário, papéis e expiração
3. Assinatura: Verifica a integridade do token

Você pode decodificar seu token JWT em https://jwt.io/ para ver seu conteúdo (mas nunca cole tokens de produção lá). 