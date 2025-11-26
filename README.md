👨‍💻 Projeto desenvolvido para a disciplina de Programação Orientada a Objetos da faculdade:

##  Autores

**Pedro Philipe da Costa Aguiar, Júlia Rodrigues, João Paulo Barbosa de Oliveira, João Augusto Alves Gonçalves, João gabriel spineli da Silva**  
Desenvolvedores e estudantes de programação.  

# 📢 Sistema de Denúncias Anônimas

Este é um sistema simples de registro e acompanhamento de **denúncias anônimas** usando **Python** e **SQLite**. O projeto permite registrar denúncias, visualizar todas e alterar o status de cada uma delas.

---

## 🛠 Tecnologias utilizadas

- Python 3
- SQLite (banco de dados local)
- Programação orientada a objetos
- Classes abstratas (com `abc.ABC`)

---

## 📁 Estrutura do Projeto

```
sistema_denuncias/
│
├── denuncias.db          # Banco de dados SQLite (gerado automaticamente)
├── sistema_denuncias.py  # Arquivo principal com toda a lógica do sistema
└── README.md             # (Este arquivo) Documentação do projeto
```
## 📋 Funcionalidades

### Menu principal:
```
1. Registrar Denúncia
2. Exibir Denúncias
3. Alterar Status
4. Sair
```

---

## 🧠 Como o sistema funciona

### Banco de Dados

- O sistema usa o SQLite para armazenar os dados localmente no arquivo `denuncias.db`.
- A tabela `denuncias` é criada automaticamente se não existir.

### Classes

#### 🔹 `Banco`

Responsável por gerenciar a conexão com o banco de dados.

#### 🔹 `EntidadeBanco` (classe abstrata)

Define o que toda entidade que usa banco de dados deve implementar:

- `criar_tabela()`
- `registrar()`
- `exibir_todos()`
- `alterar()`

#### 🔹 `Denuncia`

Herdeira de `EntidadeBanco`, representa uma denúncia.

- `criar_tabela()` – Cria a tabela de denúncias.
- `registrar(titulo, descricao)` – Insere uma nova denúncia no banco.
- `exibir_todos()` – Exibe todas as denúncias.
- `alterar(id, novo_status)` – Atualiza o status da denúncia.

#### 🔹 `SistemaDenuncias`

Interface principal para o usuário. Gerencia o menu e as opções do sistema.

---

## 🖼 Exemplo de Uso

```bash
Bem-vindo ao Sistema de Denúncias Anônimas

Menu:
1. Registrar Denúncia
2. Exibir Denúncias
3. Alterar Status
4. Sair
```

### Após registrar uma denúncia:
```bash
Denúncia registrada com sucesso!
```

### Ao exibir denúncias:
```bash
ID: 1
Título: Buraco na rua
Descrição: Grande buraco na rua principal
Status: Pendente
Data de Criação: 2025-06-04 10:35:22
----------------------------------------
```

---

## 📝 Observações

- Os status permitidos são: `Pendente`, `Resolvido` e `Em andamento`.
- O sistema não usa interface gráfica, é todo via terminal.
- Simples e ideal para fins educacionais.

---

## 📌 Possíveis melhorias futuras

- Adicionar autenticação para administrador.
- Adicionar campo de localização da denúncia.
- Exportar denúncias em CSV.
- Criar interface web com Flask ou Django.

---



