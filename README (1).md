# Importa a biblioteca sqlite3, que permite usar um banco de dados leve e local
import sqlite3

# Importa a classe datetime para conseguir trabalhar com data e hora atuais
from datetime import datetime

# Importa ferramentas que permitem criar classes abstratas
from abc import ABC, abstractmethod




# Essa classe representa o banco de dados do sistema
class Banco:
    def _init_(self, nome_banco='denuncias.db'):
        # Aqui a gente define o nome do arquivo onde os dados serão armazenados
        self.nome_banco = nome_banco

    def conectar(self):
        # Sempre que for preciso acessar o banco, usamos esse método
        return sqlite3.connect(self.nome_banco)




# Modelo base que define o que qualquer entidade do sistema precisa ter para interagir com o banco
class EntidadeBanco(ABC):
    def _init_(self, banco: Banco):
        # Toda entidade precisa saber qual banco está usando
        self.banco = banco

    @abstractmethod
    def criar_tabela(self):
        # Toda entidade deve saber criar sua própria tabela
        pass

    @abstractmethod
    def registrar(self, *args, **kwargs):
        # Toda entidade deve saber como registrar dados no banco
        pass

    @abstractmethod
    def exibir_todos(self):
        # Toda entidade deve conseguir exibir todos os seus dados
        pass

    @abstractmethod
    def alterar(self, *args, **kwargs):
        # Toda entidade deve poder alterar seus dados
        pass




# Essa é a entidade que representa uma denúncia dentro do sistema
class Denuncia(EntidadeBanco):
    def criar_tabela(self):
        # Conecta ao banco
        conn = self.banco.conectar()
        cursor = conn.cursor()
        # Cria a tabela de denúncias, se ainda não existir
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS denuncias (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                titulo TEXT NOT NULL,
                descricao TEXT NOT NULL,
                status TEXT NOT NULL DEFAULT 'Pendente',
                data_criacao TEXT NOT NULL
            )
        ''')
        # Salva as alterações e fecha a conexão
        conn.commit()
        conn.close()

    def registrar(self, titulo, descricao):
        # Pega a data e hora atual formatada como string
        data_criacao = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        # Conecta ao banco
        conn = self.banco.conectar()
        cursor = conn.cursor()
        # Insere a nova denúncia com título, descrição e data
        cursor.execute('''
            INSERT INTO denuncias (titulo, descricao, data_criacao)
            VALUES (?, ?, ?)
        ''', (titulo, descricao, data_criacao))
        conn.commit()
        conn.close()
        print("Denúncia registrada com sucesso!")

    def exibir_todos(self):
        # Conecta ao banco
        conn = self.banco.conectar()
        cursor = conn.cursor()
        # Busca todas as denúncias registradas
        cursor.execute('SELECT * FROM denuncias')
        denuncias = cursor.fetchall()
        conn.close()

        # Se houver denúncias, imprime cada uma delas
        if denuncias:
            for d in denuncias:
                print(f"ID: {d[0]}\nTítulo: {d[1]}\nDescrição: {d[2]}\nStatus: {d[3]}\nData de Criação: {d[4]}\n{'-'*40}")
        else:
            print("Não há denúncias registradas.")

    def alterar(self, id_denuncia, novo_status):
        # Garante que o novo status seja um valor válido
        if novo_status not in ['Pendente', 'Resolvido', 'Em andamento']:
            print("Status inválido. Use: Pendente, Resolvido ou Em andamento.")
            return

        conn = self.banco.conectar()
        cursor = conn.cursor()

        # Verifica se a denúncia com o ID informado realmente existe
        cursor.execute('SELECT id FROM denuncias WHERE id = ?', (id_denuncia,))
        if cursor.fetchone() is None:
            print("Denúncia não encontrada.")
            conn.close()
            return

        # Atualiza o status da denúncia
        cursor.execute('''
            UPDATE denuncias
            SET status = ?
            WHERE id = ?
        ''', (novo_status, id_denuncia))
        conn.commit()
        conn.close()
        print(f"Status da denúncia {id_denuncia} atualizado para: {novo_status}")




# Essa é a classe que representa o sistema de denúncias como um todo
class SistemaDenuncias:
    def _init_(self):
        # Cria a conexão com o banco e a instância da classe Denuncia
        self.banco = Banco()
        self.denuncia = Denuncia(self.banco)
        # Garante que a tabela será criada ao iniciar o sistema
        self.denuncia.criar_tabela()

    def menu(self):
        print("\nBem-vindo ao Sistema de Denúncias Anônimas")
        for _ in iter(int, 1):  # loop infinito usando for
            print("\nMenu:")
            print("1. Registrar Denúncia")
            print("2. Exibir Denúncias")
            print("3. Alterar Status")
            print("4. Sair")

            opcao = input("Escolha uma opção: ")

            if opcao == '1':
                titulo = input("Título: ")
                descricao = input("Descrição: ")
                self.denuncia.registrar(titulo, descricao)

            else:
                if opcao == '2':
                    self.denuncia.exibir_todos()

                else:
                    if opcao == '3':
                        try:
                            id_denuncia = int(input("ID da denúncia: "))
                            novo_status = input("Novo status (Pendente, Resolvido, Em andamento): ")
                            self.denuncia.alterar(id_denuncia, novo_status)
                        except ValueError:
                            print("ID inválido. Certifique-se de digitar um número.")

                    else:
                        if opcao == '4':
                            print("Encerrando o sistema, muito obrigado ;)!")
                            break
                        else:
                            print("Opção inválida. Por favor, escolha uma das opções do menu.")


                            # Este bloco garante que o sistema só será iniciado se o arquivo for executado diretamente
if _name_ == "_main_":
    app = SistemaDenuncias()
    app.menu()
