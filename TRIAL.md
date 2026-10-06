MongoDB (SGBD NoSQL de documentos)

Antes de começar (professor)
 Teste no seu próprio Codespace antes da aula: o Codespace padrão costuma ter Docker, mas
confirme com docker --version.
 A primeira vez que o comando docker run roda, ele baixa a imagem do Mongo (algumas centenas de
MB). Peça para todos executarem logo no início para o download acontecer enquanto você faz o
contexto.
 Se o Docker não funcionar no Codespace da turma, o plano B é o MongoDB Atlas (plano gratuito M0),
que exige cadastro de cada aluno e usa o mesmo mongosh ou o navegador.
 Quando o Codespace é desligado, o contêiner para. Para retomar: docker start mongo5.

Contexto
��️ Condução
Retome a aula 4: no NoSQL não existe tabela com colunas fixas. Perguntar à turma: &quot;Na aula 2, quando
guardamos JSON dentro de uma coluna, o que era estranho?&quot; (o JSON ficava preso dentro de uma
tabela).
Apresente a ideia: o MongoDB guarda o próprio JSON como dado principal. Compare no quadro:
 Banco relacional → banco → tabela → linha
 MongoDB → banco → coleção → documento
Analogia: uma coleção é uma gaveta de fichas; cada documento é uma ficha, e cada ficha pode ter
campos diferentes.
Peça que copiem a comparação no caderno/apresentação.

Etapa 0 – Subir o MongoDB
No terminal do Codespace:
docker run -d --name mongo5 -p 27017:27017 mongo:7
docker exec -it mongo5 mongosh

O prompt deve mudar para test&gt; . Isso significa que o mongosh conectou.
Etapa 1 – Criar o banco e inserir documentos
use escola5
db.alunos.insertOne({ nome: &quot;Ana&quot;, idade: 16, curso: &quot;ADS&quot; })
db.alunos.insertMany([
{ nome: &quot;Bruno&quot;, idade: 15, curso: &quot;ADS&quot;, notas: [7, 8, 9] },
{ nome: &quot;Carla&quot;, idade: 17, curso: &quot;ADS&quot;, endereco: { cidade: &quot;Maringá&quot;,
estado: &quot;PR&quot; } }
])
show collections
Não foi preciso criar a coleção antes; o MongoDB cria o campo _id (sozinho); Bruno tem um campo

notas (lista) e Carla um campo endereco (documento dentro de documento).

use escola5
 O que faz: Cria (se não existir) e muda o contexto para o banco de dados chamado
escola5.
 Diferencial: No MongoDB, você não precisa dar um comando CREATE DATABASE. O banco
só passa a existir de verdade no disco quando você insere o primeiro dado nele.
db.alunos.insertOne({ nome: &quot;Ana&quot;, idade: 16, curso: &quot;ADS&quot; })
 O que faz: Insere um único registro (chamado de documento) dentro de uma tabela
chamada alunos (chamada de coleção no MongoDB).
Etapa 2 – C
��️ Mostrar na tela
db.alunos.find() -----------------------&gt; find() = SELECT *;
db.alunos.find({ nome: &quot;Ana&quot; }) ---&gt; find({ filtro }) = WHERE;
db.alunos.find({ idade: { $gt: 15 } }) --&gt; $gt = maior que (&gt;);
db.alunos.find({}, { nome: 1, _id: 0 })
db.alunos.countDocuments()

Etapa 3 – Atualizar
��️ Mostrar na tela
db.alunos.updateOne({ nome: &quot;Ana&quot; }, { $set: { idade: 17 } })
db.alunos.find({ nome: &quot;Ana&quot; })
Relacionar com json_set da aula 3: $set também altera só o campo indicado.

Etapa 4 – Apagar
��️ Mostrar na tela
db.alunos.deleteOne({ nome: &quot;Bruno&quot; })
db.alunos.countDocuments()
O resultado deve ser 2.

Pergunta pra pensar
&quot;Na Etapa 1, Bruno tinha notas, Carla tinha endereco e Ana não tinha nenhum dos dois. O MongoDB
reclamou? O que aconteceria se fizéssemos isso em uma tabela do SQLite?&quot;

✅ Gabarito
O MongoDB não reclamou: cada documento pode ter campos diferentes (sem esquema fixo). Em uma
tabela relacional, todas as linhas têm as mesmas colunas; para guardar notas ou endereco só para
alguns alunos seria preciso criar colunas novas (ficando NULL nos demais) ou outra tabela.

Desafio final (10 min)
��️ Enunciado para os alunos
Dentro do banco escola5, crie uma coleção chamada jogos e faça:
 Insira 3 jogos com titulo, genero e nota. Um deles deve ter também um campo extra (por
exemplo, plataformas).
 Consulte só os jogos com nota maior ou igual a 9.
 Altere a nota de um dos jogos.
 Apague um jogo e mostre quantos sobraram.
✅ Gabarito
db.jogos.insertMany([
{ titulo: &quot;Minecraft&quot;, genero: &quot;Sandbox&quot;, nota: 9 },
{ titulo: &quot;Zelda&quot;, genero: &quot;Aventura&quot;, nota: 10, plataformas: [&quot;Switch&quot;] },
{ titulo: &quot;FIFA&quot;, genero: &quot;Esporte&quot;, nota: 7 }
])
db.jogos.find({ nota: { $gte: 9 } })

db.jogos.updateOne({ titulo: &quot;FIFA&quot; }, { $set: { nota: 8 } })
db.jogos.deleteOne({ titulo: &quot;Minecraft&quot; })
db.jogos.countDocuments()
Qualquer coleção com nomes de campos diferentes serve. O critério é usar insertMany, find com filtro,
updateOne com $set e deleteOne.

Saída esperada
 find({ nota: { $gte: 9 } }) mostra Minecraft e Zelda.
 Depois do updateOne, FIFA aparece com nota 8.
 Depois do deleteOne (Minecraft), countDocuments() retorna 2.

Fechamento e gancho para a próxima aula
Peça que digitem exit para sair do mongosh. Recapitule na lousa/tela: banco → coleção → documento, e as
quatro operações (insert, find, update, delete).
Gancho: &quot;Instalamos o Mongo dentro do nosso Codespace. E se o banco de dados ficasse em um servidor
da Amazon, sem instalar nada? Na próxima aula veremos os bancos de dados na AWS (RDS e DynamoDB).&quot;