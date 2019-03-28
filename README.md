# Demo Liquibase com Spring Boot

## O que é Liquibase

O Liquibase é uma biblioteca código-fonte aberto de alterações no esquema do banco de dados.

Leituras importantes antes de seguir com a Demo:
* https://www.baeldung.com/liquibase-refactor-schema-of-java-app
* https://www.devmedia.com.br/liquibase-gerenciando-mudancas-no-banco-de-dados-de-projetos-java/37434

Documentação:
* http://www.liquibase.org/documentation/

## Sobre a Demo

O Liquibase permite 4 tipos de arquivos para criação de scriopts de banco de dados: `xml`, `yaml`, `json` e `sql`.

O foco desta demo é utilizar arquivos `.sql`.

## Executar a Demo na minha máquina

### Ambiente necessário:

* Maven 3.x.x
* Java 8 ou superior
* Docker

### Executando:

1. Clonar o projeto
2. Subir uma instância do banco de dados PostgresSQL por meio do comando:  
    `docker run --name postgres-db -p 5432:5432 \`  
    `-e POSTGRES_PASSWORD=1234 \`  
    `-e POSTGRES_USER=postgres-user \`  
    `-e POSTGRES_DB=postgres_db \`  
    `-d postgres:latest`  
3. Executar o comando `mvn clean install`
4. Executar o comando `mvn spring-boot:run` ou executar a classe `Application`.

### Testando a Demo:

Assim que executar a aplicação, conectar no banco de dados e verificar se foram criadas as seguintes tabelas:
* `databasechangelog`: tabela de versionamento dos scripts. (é criada automaticamente pelo liquibase).
* `databasechangeloglock`: tabela de lock da tabela de versionamento dos scripts (é criada automaticamente pelo liquibase).
* `test1`: tabela referente ao script adicionado para versionamento em: `/resources/db/changelog/scripts/v1_sql.sql`.
* `test2`: tabela referente ao script adicionado para versionamento em:  `/resources/db/changelog/scripts/v2_sql.sql`

### Passo a passo de desenvolvimento:

1. Criar um projeto Spring Boot e adicionar a dependência:

        <dependency>
			<groupId>org.liquibase</groupId>
			<artifactId>liquibase-core</artifactId>
			<version>3.6.2</version>
		</dependency>

2. Configurar em `/src/main/resources/application.yml` as propriedades do banco de dados.
3. Criar o diretório  `/db/changelog` dentro de `/resources`.
4. Criar o arquivo `db.changelog-master.yaml` dentro de `/resources/db/changelog/`.
5. Criar o diretório `scripts` dentro de `/resources/db/changelog/`.
5. Adicionar a seguinte configuração no arquivo `/resources/db/changelog/db.changelog-master.yaml`:

        databaseChangeLog:
        - includeAll:
            path: scripts/
            relativeToChangelogFile: true
6. Criar scripts de alteração de banco de dados dentro do diretório `/resources/db/changelog/scripts`.