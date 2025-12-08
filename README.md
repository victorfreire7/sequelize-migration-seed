# Sequelize Migration

Este repositório tem como objetivo o estudo e melhor entendimento de migrations e seed, utilizando a ferramenta [Sequelie-Cli](https://sequelize.org/docs/v6/other-topics/migrations/) para melhor desenvolvimento. Para melhores informações, leia a documentação sobre migrations do software sequelize. 

É importante lembrar que, neste repositório 'migrations' está inserida no contexto de atualização de um banco de dados entre envolvidos em um determinado projeto.

---
# O que é migration?

"Migration é a definição que se dá ao gerenciamento de mudanças incrementais e reversíveis em esquemas (estrutura) de banco de dados. Isso permite que seja possível ter um controle "das versões" do banco de dados.

As migrations são executadas sempre que for necessário atualizar a estrutura do banco ou reverter as alterações para uma migration antiga.

Não necessariamente cada migration é uma atualização no banco de dados, a forma mais comum é uma atualização fazer uso de várias migrations.

É algo muito usado no desenvolvimento de software ágil, onde geralmente o desenvolvimento da aplicação é feito em conjunto com um banco de dados que está em construção. Assim, a estrutura da base de dados vai sendo alterada em conjunto com o desenvolvimento." 

\- [Jéf Bueno (Usuário da stack overflow)](https://pt.stackoverflow.com/users/18246/j%c3%a9f-bueno) 

---

## Iniciando projeto:

``` terminal
    npm install mysql2 sequelize
```

``` terminal
    npm install --save-dev sequelize-cli
```

``` terminal
    npx sequelize-cli init
```

## Configuração
Configure o seu arquivo 'config/config.json' de acordo com seus respectivos valores:

``` json
{
  "development": {
    "username": "root",
    "password": null,
    "database": "database_development",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "database_test",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "database_production",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```

## Criando schema do banco:
caso voce nao tenha um banco de dados criado ainda, após a configuraçao do JSON dele, você pode rodar o seguinte comando para cria-lo automaticamente:

``` terminal
    npx sequelize-cli db:create
```

## Criando Model e Migration:

``` terminal
    npx sequelize-cli model:generate --name User --attributes firstName:string,lastName:string,email:string
```

## Rodando migration para o banco:
``` terminal
    npx sequelize-cli db:migrate
```

## Desfazendo migração:
```
    npx sequelize-cli db:migrate:undo
```
Ou, para desfazer uma especifica:
``` terminal
    npx sequelize-cli db:migrate:undo:all --to XXXXXXXXXXXXXX-create-posts.js
```
---
# O que é seed?

Seed, como o próprio nome ja diz, serve para preencher uma tabela com dados padronizados.

Você pode utilizar seed para gerenciar todas as migrações de dados. Os arquivos seed são alterações nos dados que podem ser usadas para preencher tabelas do banco de dados com dados de amostra ou de teste.

O seguinte código, cria o formato de uma seed no sequelize em uma nova pasta 'seeders'.

```terminal
    npx sequelize-cli seed:generate --name demo-user
```

Com o novo arquivo criado, podemos alterar as informações.

```javaScript
    'use strict';

/** @type {import('sequelize-cli').Migration} */
    module.exports = {
    async up (queryInterface, Sequelize) {
        return queryInterface.bulkInsert('Users', [
        {
            firstName: 'Victor',
            lastName: 'Hugo',
            email: 'example@example.com',
            createdAt: new Date(),
            updatedAt: new Date(),
        },
        ]);
    },

    async down (queryInterface, Sequelize) {
        await queryInterface.bulkDelete('Users', null, {});
    }
    };
```

## Desfazendo Seed:
``` terminal
    npx sequelize-cli db:seed:undo
```
