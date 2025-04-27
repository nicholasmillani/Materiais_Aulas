# Aula Prática de Banco de Dados com Supabase

## 💡 Tema
Operações básicas em SQL no Supabase — Criação, manipulação de tabelas e relacionamentos

---

## 🔍 Objetivo

Ao final da aula, você será capaz de:
- Criar tabelas no Supabase
- Inserir, consultar, atualizar e deletar dados
- Alterar a estrutura de uma tabela
- Criar relacionamentos entre tabelas
- Compreender a base para modelagem de dados simples e normalização
- Criar sua própria base de dados com 10 colegas da turma
- Praticar Github.

---

## 📚 Ambiente

- Plataforma: [Supabase](https://supabase.com/)
- Local: SQL Editor do projeto Supabase (schema `public`)

---

# Roteiro da Aula

## 1. CREATE TABLE — Criação de Tabelas

### Tabela de Alunos
```sql
CREATE TABLE alunos (
  id SERIAL PRIMARY KEY,
  nome TEXT NOT NULL,
  turma TEXT NOT NULL,
  curso TEXT NOT NULL,
  data_nascimento DATE
);
```

**Explicação:**
- Tabela principal que armazena informações dos alunos.

### Tabela de Cursos
```sql
CREATE TABLE cursos (
  id SERIAL PRIMARY KEY,
  nome TEXT NOT NULL,
  duracao_anos INT
);
```

**Explicação:**
- Tabela que lista os cursos disponíveis na instituição.

### Tabela de Matrículas
```sql
CREATE TABLE matriculas (
  id SERIAL PRIMARY KEY,
  aluno_id INT REFERENCES alunos(id) ON DELETE CASCADE,
  curso_id INT REFERENCES cursos(id) ON DELETE CASCADE,
  data_matricula DATE DEFAULT CURRENT_DATE
);
```

**Explicação:**
- Tabela de relacionamento entre `alunos` e `cursos`, onde um aluno se matricula em um curso.
- `REFERENCES` cria chaves estrangeiras que garantem integridade dos dados.
- `ON DELETE CASCADE` apaga as matrículas se o aluno ou curso for removido.

---

## 2. INSERT INTO — Inserir Dados

### Inserir Alunos
```sql
INSERT INTO alunos (nome, turma, curso, data_nascimento)
VALUES ('Ana Lima', '1A', 'Engenharia Civil', '2002-05-10'),
       ('Bruno Souza', '1B', 'Administração', '2003-08-15');
```

### Inserir Cursos
```sql
INSERT INTO cursos (nome, duracao_anos)
VALUES ('Engenharia Civil', 5),
       ('Administração', 4);
```

### Inserir Matrículas
```sql
INSERT INTO matriculas (aluno_id, curso_id)
VALUES (1, 1),
       (2, 2);
```

**Explicação:**
- Primeiro criamos os alunos e os cursos.
- Depois associamos os alunos aos seus respectivos cursos via `matriculas`.

---

## 3. SELECT — Consultar Dados

### Consultar todos os alunos
```sql
SELECT * FROM alunos;
```

### Consultar todos os cursos
```sql
SELECT * FROM cursos;
```

### Consultar matrículas com informações completas
```sql
SELECT a.nome AS aluno, c.nome AS curso, m.data_matricula
FROM matriculas m
JOIN alunos a ON m.aluno_id = a.id
JOIN cursos c ON m.curso_id = c.id;
```

**Explicação:**
- `JOIN` conecta as tabelas para exibir dados relacionados.

---

## 4. UPDATE — Atualizar Dados

Atualizar curso de um aluno:
```sql
UPDATE alunos
SET curso = 'Engenharia de Produção'
WHERE nome = 'Ana Lima';
```

Atualizar nome de curso:
```sql
UPDATE cursos
SET nome = 'Administração de Empresas'
WHERE nome = 'Administração';
```

**Explicação:**
- `UPDATE` altera dados em uma tabela específica.

---

## 5. DELETE — Remover Dados

Deletar uma matrícula:
```sql
DELETE FROM matriculas
WHERE id = 1;
```

Deletar um aluno (e suas matrículas serão apagadas automaticamente):
```sql
DELETE FROM alunos
WHERE nome = 'Ana Lima';
```

**Explicação:**
- `DELETE` remove registros.
- Devido ao `ON DELETE CASCADE`, a exclusão em `alunos` também afeta `matriculas`.

---

## 6. ALTER TABLE — Alterar Tabelas

Adicionar telefone ao aluno:
```sql
ALTER TABLE alunos
ADD COLUMN telefone TEXT;
```

Inserir número de telefone para o aluno Bruno Souza:
```sql
UPDATE alunos
SET telefone = '(11) 98765-4321'
WHERE nome = 'Bruno Souza';
```

**Explicação:**
- `ALTER TABLE` adiciona uma nova coluna `telefone` para armazenar o número de telefone dos alunos.
- Depois, `UPDATE` é usado para preencher essa nova informação para alunos específicos.

---

## 7. DROP TABLE — Excluir Tabelas

Deletar uma tabela inteira (cuidado!):
```sql
DROP TABLE matriculas;
```

**Explicação:**
- `DROP TABLE` remove a tabela e todos os seus dados.

---

# 🌟 Desafio Final dessa aula: Criar uma Base de Dados Completa

**Desafio:**
- Criar suas próprias tabelas `alunos`, `cursos` e `matriculas`. Onde terá os nomes de alunos presentes na sala de aula.
- Inserir no mínimo:
  - 10 alunos
  - 3 cursos ou mais e preferir
  - Cada aluno deve estar matriculado em um curso.
- Criar consultas utilizando `JOIN` para listar alunos e seus cursos.

## Exemplo de Consulta de Relacionamento

```sql
SELECT a.nome AS aluno, c.nome AS curso, m.data_matricula
FROM matriculas m
JOIN alunos a ON m.aluno_id = a.id
JOIN cursos c ON m.curso_id = c.id;
```

# 📊 Resumo de Comandos

| Comando | Objetivo | Exemplo |
|:---|:---|:---|
| CREATE TABLE | Criar tabela | `CREATE TABLE alunos (...)` |
| INSERT INTO | Inserir dados | `INSERT INTO cursos (...) VALUES (...)` |
| SELECT | Consultar dados | `SELECT * FROM alunos` |
| SELECT JOIN | Consultar tabelas relacionadas | `SELECT ... JOIN ...` |
| UPDATE | Atualizar dados | `UPDATE alunos SET ... WHERE ...` |
| DELETE | Deletar dados | `DELETE FROM alunos WHERE ...` |
| ALTER TABLE | Modificar estrutura | `ALTER TABLE alunos ADD COLUMN ...` |
| DROP TABLE | Excluir tabela | `DROP TABLE matriculas` |

---
---
**Objetivo a ser atingindo ao final:**
- Primero contato com Supbase.
- Inserção correta dos dados.
- No final crie um repositorio com todos os comandos e valores usados, depois inseria o link dio seu repositório na planilha que for disponibilizada no drive turma, junto com os materiais do dia.

# 🚀 Boa aula! #MãoNaMassa

---
