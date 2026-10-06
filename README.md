# Banco-MongoDB

Monte o Nosso Banco de Lanches!

Cada aluno será responsável por inserir o seu próprio nome e o seu lanche favorito no banco
de dados da turma usando o terminal do GitHub Codespaces.

Regra do Desafio: Nenhum lanche pode ser repetido! Combine com seus colegas antes de
digitar para garantir que cada um escolha um lanche diferente.

Como o MongoDB cria Bancos de Dados e Coleções?
Diferente dos bancos tradicionais (SQL), no MongoDB você não precisa criar um banco de
dados ou uma tabela (coleção) antes de usar.

Criação do Banco: Quando você digita use nome_do_banco, o MongoDB apenas se
prepara para entrar nele. Se o banco não existir, ele não é criado imediatamente. O MongoDB
espera você inserir o primeiro dado para criá-lo de forma automática na memória.

Criação da Coleção: O mesmo acontece com as coleções (tabelas). No momento em que
você roda o comando de inserir um documento (db.nome_da_colecao.insertOne()), o
MongoDB percebe que essa coleção não existe e a cria instantaneamente junto com o seu
dado.

Passo 1: Entrar no terminal do MongoDB
No terminal do seu GitHub Codespaces, digite o comando abaixo e aperte Enter:
bash
mongosh

Passo 2: Direcionar para o novo banco de dados
Digite o comando abaixo. Lembre-se: o banco banco_da_turma ainda não existe fisicamente,
você está apenas dizendo ao MongoDB que quer criá-lo e usá-lo a partir de agora:

javascript
use banco_da_turma

Passo 3: Criar a coleção e Inserir o seu dado (A hora de criar!)
Agora é a sua vez. Modifique o comando abaixo colocando o seu nome e o seu lanche
favorito.
Assim que você apertar Enter, o MongoDB vai criar automaticamente a coleção chamada
alunos e salvar o seu registro dentro do banco criado no passo anterior:

javascript
db.alunos.insertOne({
	nome: "Seu nome",
	lanche_favorito: "Seu lanche favorito"
})

Passo 4: Ver o resultado final de toda a turma
Depois que todos os colegas inserirem seus dados, rode o comando abaixo para ver a lista
completa e conferir como o banco de dados foi estruturado:
alunos e salvar o seu registro dentro do banco criado no passo anterior:

```javascript
db.alunos.insertOne({
	nome: "Seu nome",
	lanche_favorito: "Seu lanche favorito"
})
```

db.alunos.find().pretty()

Sem uso de IAs ...
