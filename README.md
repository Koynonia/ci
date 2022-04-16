# Projeto Docker SQL Server (v. 1.0.0)
Este projeto tem como base o post ["Dicas — Docker: SQL Server base de dados para validação de testes"](https://medium.com/@leandrobianch/dicas-docker-imagem-sqlserver-com-cria%C3%A7%C3%A3o-de-banco-de-dados-cria%C3%A7%C3%A3o-de-usu%C3%A1rio-e-carga-inicial-87609d4ecc0) de autoria de Leandro Bianch no Medium.

O projeto visa criar e disponibilizar uma base de testes do SQL Server via Docker, e está utilizando como ferramenta de fluxo de trabalho o Git Flow.

Este cenário pode ser contemplado quando há ausência de um micro [ORM ](https://www.devmedia.com.br/orm-object-relational-mapper/19056) [(Dapper)](https://dapper-tutorial.net/dapper) ou macro ORM ([EF](https://docs.microsoft.com/pt-br/ef/) [NHiberante](https://nhibernate.info/)) quando não consegue aplicar a técnica de [migração)](https://docs.microsoft.com/pt-br/ef/core/managing-schemas/migrations/?tabs=dotnet-core-cli) ou a base de dados é mantida por um time de DBA por exemplo.

## O cenário
Disponibilizar um ambiente sqlserver para o time de testes e desenvolvimento para validação:

## Os benefícios a serem alcançados
- Necessidade que a base de dados esteja limpa a cada validação.
- Autonomia de limpeza de dados para validação (não depender de terceiros, independencias entre o time de engenharia, testes e desenvolvimento).
- Feedback de evolução da base de times distintos.
- Realização de testes para cada nova versão do SQLServer.
- Melhoria na qualidade das entregas.

___
## Estrutura do Projeto

| Indíce | Módulo | Descrição                                                                        |
| :----: | :----- | :------------------------------------------------------------------------------- |
|   01   | infra  | Scripts de configuração para baixar, configurar e executar o container do Docker |
|   02   | script | Scripts docker-compose e sql                                                     |
|   03   | logs   | Arquivos de logs da execução dos scripts do módulo Infra                         |


## 01 - Infra
Diretório que contém o scripts ‘.shs’ que serão executado para a nossa infraestrutura do *SQL*.
### 1.1 - log.sh
Script que contém a função para escrita de log output e no arquivo texto do host do *Docker*.
### 1.2 - config.sh
Script que contém todas as configurações de variaveis de ambiente.
### 1.3 - execute-test.sh
Script responsavel por iniciar o container, validar se o container já está pronto para execução dos scripts de criação do banco, usuário da aplicação, tabela e carga inicial no sql server e finaliza o container após 90 segundos.
- Um detalhe importante: a instrução de *healthcheck* configurada no **docker-compose-sql-server-qa.yaml** nos permite interagir com propriedade ‘*.State.Health.Status*’, para que possamos garantir quando o container estever pronto para a inicialização da base de dados de testes.
### 1.4 - sqlserver.sh
Script que contém todos do códigos do sqlcmd para inicialização de base de dados para testes.
## 02 - Script
Diretório que contém o script sql necessário que será executado para quando o container subir o serviço do sqlserver, executar no banco de dados.
- docker-compose-sql-server-qa.yaml

Detalhe importante: a instrução de healthcheck do container nos ajuda na inspeção se o container em está pronto para execução, evitando problemas quando você for publicar no seu pipeline.
## 03 - Logs
Diretório que contém os logs gerados na execução dos scripts e interação com o banco de dados.

Estes diretório e logs são criados dinamicamente ao executar o script *execute-test.sh*
___
## Executando o ambiente
Acesse o diretório ci e digite no terminal (Bash):

```sh
$ ./infra/execute-test.sh
```