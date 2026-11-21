# Construção de API REST com Node.js e MySQL

**Disciplina:** Programação Web III (PW3)  
**Aluno:** Miguel Batista da Silva
**Turma:** PW3  

---
## Arquitetura MVC Adotada

A refatoração separou as responsabilidades do sistema em camadas desacopladas, eliminando consultas diretas ao banco no arquivo de inicialização:

- **`config/db.js`**: Gerencia a criação do pool de conexões com o MySQL/MariaDB utilizando a biblioteca `mysql2/promise` e carrega as variáveis de ambiente com `dotenv`.
- **`models/usuarioModel.js`**: Camada de dados responsável exclusivamente por executar instruções SQL (`SELECT`, `INSERT`, `UPDATE`, `DELETE`) com queries parametrizadas (placeholders `?`) contra SQL Injection.
- **`controllers/usuarioController.js`**: Camada de controle que recebe as requisições (`req`), valida os dados de entrada, aciona a `usuarioModel` e devolve a resposta HTTP adequada com status correspondente (`res`).
- **`routes/usuarioRoutes.js`**: Define os endpoints REST através do `express.Router()` e mapeia cada verbo HTTP ao método do respectivo controller.
- **`server.js`**: Ponto de entrada da aplicação, responsável apenas pela configuração de middlewares (`cors`, `express.json`), registro das rotas e inicialização do servidor HTTP.
- **Camada View**: Como se trata de uma API REST backend, a resposta estruturada em JSON retornada pelo Express cumpre o papel da View.

---

## Estrutura de Pastas

```text
├── src/
│   ├── config/
│   │   └── db.js                # Pool de conexão com MySQL
│   ├── models/
│   │   └── usuarioModel.js      # Consultas SQL isoladas
│   ├── controllers/
│   │   └── usuarioController.js # Lógica de requisição e resposta
│   ├── routes/
│   │   └── usuarioRoutes.js     # Definição dos endpoints REST
│   └── server.js                # Configuração de middlewares e subida do servidor
├── .env.example                 # Modelo de variáveis de ambiente sem senhas reais
├── .gitignore                   # Bloqueio de node_modules/ e .env
├── package.json                 # Dependências e scripts
├── schema.sql                   # Script de criação do banco e tabela
└── README.md                    # Documentação do projeto
```

---
## Como Rodar o Projeto Localmente

### 1. Clonar o repositório
```bash
git clone https://github.com/SEU_USUARIO/miguel-batista-API-node-com-sql.git
cd miguel-batista-API-node-com-sql
```

### 2. Instalar dependências
```bash
npm install
```

### 3. Configurar variáveis de ambiente
Copie o arquivo `.env.example` para `.env` e ajuste as credenciais do seu banco de dados se necessário:
```bash
cp .env.example .env
```

### 4. Criar o Banco de Dados e a Tabela
Execute o script `schema.sql` no seu MySQL ou MariaDB:
```bash
mysql -u root -p < schema.sql
```
Ou abra o arquivo `schema.sql` e execute as instruções SQL no seu cliente de banco de dados (DBeaver, phpMyAdmin, MySQL Workbench ou terminal).

### 5. Iniciar o Servidor
Modo desenvolvimento com recarregamento automático:
```bash
npm run watch
```
O servidor estará rodando em: `http://localhost:3002`

---

## Rotas da API

| Método | Rota | Descrição |
|---|---|---|
| `POST` | `/usuarios` | Cadastra um novo usuário (`nome` e `email` no body) |
| `GET` | `/usuarios` | Lista todos os usuários cadastrados |
| `PUT` | `/usuarios/:id` | Atualiza dados do usuário informado no parâmetro de rota |
| `DELETE` | `/usuarios/:id` | Remove o usuário pelo ID |
