# Guia Iniciante SQL

## Comandos internos e de administraçao

### DCL

## DCL - Linguagem de Controle de Dados

Controla os aspectos de autorização de dados e licenças de usuários para controlar quem tem acesso a visualização ou manipulação dos dados dentro do banco de dados. 

**Duas palavras-chaves da DCL:**

**A -GRANT -** - autoriza ao usuário executar ou setar operações.

**B - REVOLKE -** remove ou restringe a capacidade de um usuário de executar operações. 


**A - GRANT**

Concede privilégios ao usuários do banco de dados nos comandos Create, Alter, Drop, Insert, UpDate, Delete, Select e outros.

**Ex1.:** 

```
GRANT tipo_privilégio ON tipo_objeto TO usuario@host;    
```

​        permissões     banco.tabela       usuário 

**Para atribuir um privilégio em todos os bancos **

```
GRANT SELECT ON *.* TO joao@'%';

GRANT UPDATE ON *.* TO maria@localhost;

GRANT DELETE ON *.* TO joao@'132.160.25.89'; 
```

**B - REVOKE**Revoga privilégios concedidos a um usuário do banco de dados.

**Ex1.:** REVOKE tipo_privilégio ON tipo_objeto TO usuario@host;    
          permissões     banco.tabela       usuário 

**Para remover um privilégio em um os bancos**

```
REVOKE SELECT ON dbveiculo.* FROM joao@'%';
```

**Para remover um privilégio em todos os bancos**

```
REVOKE ALL PRIVILEGES ON dbveiculo.* FROM joao@'%'; 
```

-----

