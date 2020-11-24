[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<br />
<p align="center">
  <a href="http://ayty.org/index">
    <img src="http://ayty.org/_media/logogrande.png?w=300&tok=5eeef7" alt="Logo">
  </a>

  <h3 align="center">Exercício #01 - Crud </h3>

  <p align="center">
    Módulo do usuário
    <br />
    <a href="http://ayty.org/"><strong>Conheça o Ayty »</strong></a>
  </p>
</p>

## Sobre o exercício

O exercício consiste em criar um crud de usuarios para uma api rest, dessa forma você irá começar a se familiarizar com o framework e alguns conceitos de api rest. CRUD é o acrônimo da expressão do idioma Inglês, Create (Criação), Read (Consulta), Update (Atualização) e Delete (Destruição), sendo as quatro operações básicas utilizadas em bases de dados relacionais fornecidas aos utilizadores do sistema.

O nosso sistema começa com a criação de um Usuário. Um usuário pode ter mais de um tipo de conta vinculada a ele. De um Usuário (User), queremos saber seu Nome Completo, CPF, Número de Telefone, e-mail e Senha. CPFs e e-mails devem ser únicos no sistema. Sendo assim, seu sistema deve permitir apenas um cadastro com o mesmo CPF ou endereço de e-mail.

Os tipos de contas disponíveis serão Consumidor (Consumer) e Lojista (Seller). Todo Consumidor ou Lojista deve estar vinculado a um usuário existente. De um Lojista queremos saber a Razão Social, o Nome Fantasia, o CNPJ e seu Username, além do id de Usuário que será dono dessa conta. De um Consumidor, queremos saber apenas seu Username, além do id de Usuário que será dono dessa conta. Os usernames devem ser únicos dentro do sistema, mesmo entre contas de tipos diferentes. Devido a algumas limitações do sistema, cada Usuário pode ter apenas uma conta de cada tipo.

Seu sistema deve ser capaz de listar todos os usuários, além de conseguir trazer informações detalhadas de um usuário específico. Durante a listagem, deve ser possível filtrar os resultados por Nome ou Username. Para fins didáticos, sua busca deve considerar apenas resultados que comecem com a string especificada na busca. Como exemplo, GET /users?q=joao deve retornar apenas Usuários cujos Nomes ou Usernames comecem com a string joao. Não há a necessidade de lidar com acentos. Por fim seu sistema também deve poder atualizar o nome do usuário e também deletar todas suas informações do sistema.

_Para fins de estudo e facilitar no desen recomenda-se utilizar o H2 database, caso se interesse acesse o mvnrepository, busque por H2 database e por fim adicione a dependência maven no seu pom.xml. O H2 um sistema de gerenciamento de banco de dados relacional escrito em Java. Ele pode ser incorporado em aplicativos Java ou executado no modo cliente-servidor, possui várias ferramentas como uma interface de administração web com Auto Completion para SQL._

### Arquitetura

A seguinte arquitetura é sugerida: 

- main
  - Entidade
  - Repositório
  - Serviço
  - Controlador
  - Exceções
  - VO

## Exemplos de requisições

#### POST localhost:8080/api/users
```json
{
    "fullname":"Clebson Augusto Fonseca",
    "email":"clebson.augusto@dcx.upfb.br",
    "password":"123456",
    "cpf":"123456789",
}
```
#### Response - 200 OK
```json
{
    "status":"OK",
    "message":"User created",
    "payload":{
        "id": 1,
        "fullname":"Clebson Augusto Fonseca",
        "email":"clebson.augusto@dcx.upfb.br",
        "password":"123456",
        "cpf":"123456789"
    }
}
```
#### Response - 403 Already exists
```json
{
    "status":"NOT_FOUND",
    "message":"User already exists",
}
```
#
<br/>


#### POST localhost:8080/api/users/consumers
```json
{
    "user_id":1,
    "username":"clebson",
}
```
#### Response - 200 OK
```json
{
    "status":"OK",
    "message":"Consumer created",
    "payload":{
        "id": 1,
        "user_id": 1,
        "username": "clebson"
    }
}
```
#### Response - 403 Already exists
```json
{
    "status":"NOT_FOUND",
    "message":"Consumer already exists",
}
```
#
<br/>

#### POST localhost:8080/api/users/sellers
```json
{
    "user_id": 1,
    "cnpj": "123456789",
    "fantasy_name": "Hiper Loja do clebson",
    "social_name": "Loja do clebson LTDA",
    "username": "loja_do_clebson"
}
```
#### Response - 200 OK
```json
{
    "status":"OK",
    "message":"Consumer created",
    "payload":{
        "id": 1,
        "user_id": 1,
        "cnpj": "123456789",
        "fantasy_name": "Hiper Loja do clebson",
        "social_name": "Loja do clebson LTDA",
        "username": "loja_do_clebson"
    }
}
```
#### Response - 403 Already exists
```json
{
    "status":"NOT_FOUND",
    "message":"Seller already exists",
}
```
#
<br/>


#### GET localhost:8080/api/users/1
```json

```
#### Response - 200 OK
```json
{
    "status":"OK",
    "message":"User found",
    "payload":{
        "id": 1,
        "fullname":"Clebson Augusto Fonseca",
        "email":"clebson.augusto@dcx.upfb.br",
        "password":"123456",
        "cpf":"123456789",
        "accounts": {
            "consumer": {
                "id": 1,
                "user_id": 1,
                "username": "clebson"
            },
            "seller": {
                "cnpj": "123456789",
                "fantasy_name": "Hiper Loja do clebson",
                "id": 1,
                "social_name": "Loja do clebson LTDA",
                "user_id": 1,
                "username": "loja_do_clebson"
            }
        }
    }
}
```
#### Response - 404 NOT FOUND
```json
{
    "status":"NOT_FOUND",
    "message":"User not found",
}
```
#
<br/>

#### PUT localhost:8080/api/users/1
```json
{
    "fullname":"Clebson Fonseca",
}
```
#### Response - 200 OK
```json
{
    "status":"OK",
    "message":"User updated",
    "payload":{
        "id": 1,
        "fullname":"Clebson Fonseca",
        "email":"clebson.augusto@dcx.upfb.br",
        "password":"123456",
        "cpf":"123456789"


    }
}
```
#### Response - 404 NOT FOUND
```json
{
    "status":"NOT_FOUND",
    "message":"User not found",
}
```
#
<br/>

#### DELETE localhost:8080/api/users/1
```json

```
#### Response - 200 OK
```json
{
    "status":"OK",
    "message":"User deleted",
    "payload":{
        "username":"Clebsonf",
        "fullname":"Clebson Augusto Fonseca",
        "email":"clebson.augusto@dcx.upfb.br",
        "password":"123456",
        "cpf":"123456789",
        "accounts": {
            "consumer": {
                "id": 1,
                "user_id": 1,
                "username": "clebson"
            },
            "seller": {
                "cnpj": "123456789",
                "fantasy_name": "Hiper Loja do clebson",
                "id": 1,
                "social_name": "Loja do clebson LTDA",
                "user_id": 1,
                "username": "loja_do_clebson"
            }
        }
    }
}
```
#### Response - 404 NOT FOUND
```json
{
    "status":"NOT_FOUND",
    "message":"User not found",
}
```

## Referências:

* [SPRINGBOOT](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
* [H2 Database](http://www.h2database.com/html/tutorial.html)
* [Maven](https://maven.apache.org/guides/index.html)

## Começando:

Antes da instalação se pressupõe que você possui o java 8, o git e uma IDE de sua preferência já instalados, se não estiverem devem ser instaladas antes de continuar.

### Como realizar o exercício?

1. Faça fork do projeto.
2. Clone o projeto (`git clone https://github.com/{YOUR-USER-NAME}/SPRING-EXERCICIO-01.git`)
3. Abra na IDE de sua preferência
4. Faça a mágica :D
4. Abra um Pull Request

## Conteúdo bônus:

### Padrões de projetos para dados

* [VO](https://gist.github.com/clebsonf/e3d370179afae4d9ab37eb857f2da43e): é um pequeno objeto que representa uma entidade simples, cuja igualdade não é baseada em identidade: ou seja, dois objetos de valor são iguais quando têm o mesmo valor, não necessariamente sendo o mesmo objeto.
* [DTO](https://gist.github.com/clebsonf/b7a63b1ca2460e70e51d016161a22ed2): Data Transfer Object, o próprio nome já diz muito: um objeto simples usado para transferir dados de um local a outro na aplicação, sem lógica de negócios em seus objetos e comumente associado à transferência de dados entre uma camada de visão (view layer) e outra de persistência dos dados (model layer).


<!-- MARKDOWN LINKS & IMAGES -->
[contributors-shield]: https://img.shields.io/github/contributors/ayty-org/SPRING-exercicio-01.svg?style=flat-square
[contributors-url]: https://github.com/ayty-org/SPRING-EXERCICIO-01/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/ayty-org/SPRING-EXERCICIO-01.svg?style=flat-square
[forks-url]: https://github.com/ayty-org/SPRING-EXERCICIO-01/network/members
[stars-shield]: https://img.shields.io/github/stars/ayty-org/SPRING-EXERCICIO-01.svg?style=flat-square
[stars-url]: https://github.com/ayty-org/SPRING-EXERCICIO-01/stargazers
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=flat-square&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/groups/8994167/
