
# Microsserviços — Gerenciamento de Usuários e Departamentos

Projeto protótipo com duas aplicações Spring Boot independentes:
- `department-service` — gerencia departamentos (porta 8080)
- `user-service` — gerencia usuários e consulta departamentos via REST (porta 8081)

Resumo: o `user-service` persiste usuários no banco `employee_db` e, ao retornar dados completos, consome o `department-service` para incluir informações do departamento.

## Funcionalidades
- Gerenciamento de departamentos
- Comunicação entre serviços via RestTemplate
- Persistência com MySQL
- DTOs para transferência entre camadas
- Estrutura em camadas (Controller → Service → Repository)

## Tecnologias
- Java 17
- Spring Boot 2.7.x
- Spring Web, Spring Data JPA
- MySQL
- Maven 
- Lombok 1.18.30

## Pré-requisitos
- Java 17
- Maven 3.8+
- MySQL Server 8.0
- IDE Eclipse e VS Code
- XAMPP 3.3.0

## Configuração rápida

1. Clonar repositório
```bash
git clone https://github.com/seu-usuario/seu-repo-nome.git
cd seu-repo-nome
```

2. Criar bancos MySQL
```sql
CREATE DATABASE employee_db;
CREATE DATABASE department_db;
```

3. Ajustar propriedades
- department-service/src/main/resources/application.properties
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/department_db
spring.datasource.username=root
spring.datasource.password=sua-senha
spring.jpa.hibernate.ddl-auto=update
server.port=8080
```

- user-service/src/main/resources/application.properties
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/employee_db
spring.datasource.username=root
spring.datasource.password=sua-senha
spring.jpa.hibernate.ddl-auto=update
server.port=8081
```

4. Build (Windows)
```powershell
# department-service
cd department-service
mvn clean install

# user-service
cd ../user-service
mvn clean install
```

## Executar (Windows)
OBS: Lembre-se de iniciar o MySQL no XAMPP Control Panel  
1. Iniciar primeiro o department-service:
```powershell
cd department-service
mvn spring-boot:run
```
2. Iniciar o user-service em outro terminal:
```powershell
cd user-service
mvn spring-boot:run
```

## Endpoints da API
Instale no seu visual studio code a extensão Thunder Cliente no menu lateral esquerdo (Extensions) para consumir APIs ou use o Postman e faça requests para testar a aplicação com as seguintes configurações para:

### Salvar um departamento:
Method: POST  
URL: http://localhost:8080/api/departments  
Body (JSON):  
{  
  "departmentName": "TI",  
  "departmentAddress": "Tecnologia da Informação",  
  "departmentCode": "TI-001"  
}

### Obter departamento por ID:
Method: GET  
URL: http://localhost:8080/api/departments/1

### Salvar usuário:
Method: POST  
URL: http://localhost:8081/api/users  
Body (JSON):  
{  
  "firstName": "Samir",  
  "lastName": "Soares",  
  "email": "samirsoaresdemelo@gmail.com",  
  "departmentId": 1  
}

### Obter usuário:
Method: GET  
URL: http://localhost:8081/api/users/1
  
## 📂 Estrutura do Projeto

```bash
├── department-service/
│   └── src/main/java/net/javaguides/departmentservice/
│       ├── controller/        # Controladores REST
│       ├── entity/            # Entidades JPA
│       ├── repository/        # Repositórios Spring Data
│       └── service/           # Lógica de negócio
│           └── impl/
│
└── user-service/
    └── src/main/java/net/javaguides/userservice/
        ├── controller/        # Controladores REST
        ├── dto/               # Objetos de Transferência de Dados
        ├── entity/            # Entidades JPA
        ├── repository/        # Repositórios Spring Data
        └── service/           # Lógica de negócio e integração
            └── impl/
```
Fluxo de Comunicação

- Cliente faz requisição para o User Service
- User Service busca dados do usuário no banco employee_db
- User Service chama o Department Service via REST para obter dados do departamento
- Department Service retorna informações do departamento
- User Service combina os dados e retorna a resposta completa ao cliente

--- 
Projeto criado por meio do tutorial disponível no link: https://www.javaguides.net/2022/10/spring-boot-microservices-communication-using-resttemplate.html






