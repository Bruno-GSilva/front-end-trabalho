
# Documentação do Banco de Dados - Sistema de Agendamento de Clínica

## Tabelas Principais

### 1. Pacientes

Tabela que armazena os dados dos pacientes.

| Coluna    | Tipo         | Descrição                      |
|-----------|--------------|-------------------------------|
| id        | INT (PK)     | Identificador único            |
| nome      | VARCHAR(255) | Nome do paciente               |
| data_nasc | DATE         | Data de nascimento (opcional) |
| telefone  | VARCHAR(20)  | Telefone de contato (opcional)|

### 2. Agendamentos

Tabela que armazena os agendamentos feitos pelos pacientes.

| Coluna      | Tipo         | Descrição                              |
|-------------|--------------|---------------------------------------|
| id          | INT (PK)     | Identificador único                   |
| paciente_id | INT (FK)     | Referência ao paciente (Pacientes)    |
| data        | DATE         | Data do agendamento                   |
| hora        | TIME         | Hora do agendamento                   |

## Principais comandos SQL

### Criar tabelas

```sql
CREATE TABLE pacientes (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nome VARCHAR(255) NOT NULL,
  data_nasc DATE,
  telefone VARCHAR(20)
);

CREATE TABLE agendamentos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  paciente_id INT NOT NULL,
  data DATE NOT NULL,
  hora TIME NOT NULL,
  FOREIGN KEY (paciente_id) REFERENCES pacientes(id) ON DELETE CASCADE
);
```

### Inserir dados

```sql
INSERT INTO pacientes (nome, data_nasc, telefone)
VALUES ('João Silva', '1980-05-15', '11999999999');

INSERT INTO agendamentos (paciente_id, data, hora)
VALUES (1, '2025-06-10', '14:30:00');
```

### Consultar dados

Listar todos os pacientes:

```sql
SELECT * FROM pacientes;
```

Listar agendamentos com o nome do paciente:

```sql
SELECT a.id, p.nome, a.data, a.hora
FROM agendamentos a
JOIN pacientes p ON a.paciente_id = p.id
ORDER BY a.data, a.hora;
```

### Atualizar dados

```sql
UPDATE pacientes
SET telefone = '11988887777'
WHERE id = 1;
```

### Excluir dados

Excluir um agendamento pelo id:

```sql
DELETE FROM agendamentos
WHERE id = 10;
```

Excluir um paciente (exclui também os agendamentos relacionados por causa do ON DELETE CASCADE):

```sql
DELETE FROM pacientes
WHERE id = 1;
```

## Observações

- A relação entre pacientes e agendamentos é de um para muitos (um paciente pode ter vários agendamentos).
- Use ON DELETE CASCADE para garantir que ao excluir um paciente, todos os agendamentos dele sejam excluídos automaticamente.
