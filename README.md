# CRUD 

Este projeto é uma aplicação simples de CRUD desenvolvida em Python, utilizando SQLite como banco de dados e Tkinter para a interface gráfica.

## 📌 Funcionalidades

- Adicionar contatos com nome, e-mail, telefone, endereço e CPF
- Listar todos os contatos cadastrados
- Atualizar informações de um contato existente
- Excluir contatos da lista

## 🛠️ Tecnologias Utilizadas

- Python 3
- SQLite3 (banco de dados local)
- Tkinter (interface gráfica)

## 📂 Estrutura do Código

### 1. Conexão com o Banco de Dados

A função `conectar()` estabelece a conexão com o banco de dados `banco.db`.

```python
 def conectar():
     return sqlite3.connect("banco.db")
```

A função `criar_tabela()` cria a tabela `contatos` caso ela não exista:

```python
 def criar_tabela():
     conexao = conectar()
     cursor = conexao.cursor()
     cursor.execute('''
         CREATE TABLE IF NOT EXISTS contatos (
             id INTEGER PRIMARY KEY AUTOINCREMENT,
             nome TEXT NOT NULL,
             email TEXT NOT NULL,
             telefone TEXT NOT NULL,
             endereco TEXT NOT NULL,
             cpf TEXT NOT NULL
         )
     ''')
     conexao.commit()
     conexao.close()
```

### 2. CRUD (Create, Read, Update, Delete)

#### Criar Contato

A função `adicionar_contato()` recebe os dados do usuário e insere-os no banco de dados.

```python
 def adicionar_contato():
     # Obtendo os dados
     nome = entry_nome.get().strip()
     email = entry_email.get().strip()
     telefone = entry_telefone.get().strip()
     endereco = entry_endereco.get().strip()
     cpf = entry_cpf.get().strip()
     
     # Validação e inserção
     if nome and email and telefone and endereco and cpf:
         conexao = conectar()
         cursor = conexao.cursor()
         cursor.execute("INSERT INTO contatos (nome, email, telefone, endereco, cpf) VALUES (?, ?, ?, ?, ?)",
                        (nome, email, telefone, endereco, cpf))
         conexao.commit()
         conexao.close()
         listar_contatos()
         messagebox.showinfo("Sucesso", "Contato adicionado com sucesso!")
     else:
         messagebox.showwarning("Atenção", "Preencha todos os campos!")
```

#### Listar Contatos

A função `listar_contatos()` busca e exibe os contatos cadastrados na interface gráfica.

```python
 def listar_contatos():
     for item in tree.get_children():
         tree.delete(item)
     conexao = conectar()
     cursor = conexao.cursor()
     cursor.execute("SELECT * FROM contatos")
     contatos = cursor.fetchall()
     conexao.close()
     for contato in contatos:
         tree.insert("", tk.END, values=contato)
```

#### Excluir Contato

A função `excluir_contato()` remove um contato selecionado.

```python
 def excluirdef conectar():
     return sqlite3.connect("banco.db")_contato():
     try:
         selected_item = tree.selection()[0]
         contato_id = tree.item(selected_item)['values'][0]
         conexao = conectar()
         cursor = conexao.cursor()
         cursor.execute("DELETE FROM contatos WHERE id = ?", (contato_id,))
         conexao.commit()
         conexao.close()
         tree.delete(selected_item)
     except IndexError:
         messagebox.showwarning("Atenção", "Selecione um contato para excluir!")
```

#### Atualizar Contato

A função `atualizar_contato()` permite editar um contato existente.

```python
 def atualizar_contato():
     try:
         selected_item = tree.selection()[0]
         contato_id = tree.item(selected_item)['values'][0]
         conexao = conectar()
         cursor = conexao.cursor()
         
         cursor.execute("SELECT nome, email, telefone, endereco, cpf FROM contatos WHERE id = ?", (contato_id,))
         contato_atual = cursor.fetchone()
         
         novo_nome = entry_nome.get().strip() or contato_atual[0]
         novo_email = entry_email.get().strip() or contato_atual[1]
         novo_telefone = entry_telefone.get().strip() or contato_atual[2]
         novo_endereco = entry_endereco.get().strip() or contato_atual[3]
         novo_cpf = entry_cpf.get().strip() or contato_atual[4]
         
         cursor.execute("UPDATE contatos SET nome = ?, email = ?, telefone = ?, endereco = ?, cpf = ? WHERE id = ?",
                        (novo_nome, novo_email, novo_telefone, novo_endereco, novo_cpf, contato_id))
         conexao.commit()
         conexao.close()
         listar_contatos()
         messagebox.showinfo("Sucesso", "Contato atualizado com sucesso!")
     except IndexError:
         messagebox.showwarning("Atenção", "Selecione um contato para atualizar!")
```

## 🎨 Interface Gráfica (Tkinter)

A interface é composta por:

- Campos de entrada para cada dado do contato
- Botões para adicionar, atualizar e excluir contatos
- Uma `Treeview` para exibir os contatos cadastrados

```python
 root = tk.Tk()
 root.title("Agenda de Contatos")
 root.geometry("600x450")
 root.configure(bg="#f0f0f0")
```

## ▶️ Como Executar o Projeto

1. Certifique-se de ter o Python instalado.
2. Instale a biblioteca Tkinter (caso necessário).
3. Execute o script:

```sh
python nome_do_arquivo.py
```

4. A interface será aberta e você poderá gerenciar seus contatos.

## 📜 Licença

Este projeto é de código aberto e pode ser utilizado livremente.

---

📌 **Desenvolvido com Python e Tkinter** 🚀

