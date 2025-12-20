# Microsserviços — Gerenciamento de Usuários e Departamentos

Projeto **protótipo** composto por duas aplicações **Spring Boot independentes**, demonstrando comunicação entre microsserviços via REST.

## Arquitetura

- **department-service**  
  Responsável pelo gerenciamento de departamentos  
  Porta: **8080**

- **user-service**  
  Responsável pelo gerenciamento de usuários e consumo do department-service  
  Porta: **8081**

### Resumo do funcionamento
O **user-service** persiste usuários no banco `employee_db`.  
Ao retornar os dados completos de um usuário, ele consome o **department-service** via REST para obter as informações do departamento associado.

---

## Funcionalidades

- Gerenciamento de departamentos
- Gerenciamento de usuários
- Comunicação entre microsserviços usando **RestTemplate**
- Persistência de dados com **MySQL**
- Uso de **DTOs** para transferência de dados
- Arquitetura em camadas:
  - Controller
  - Service
  - Repository

---

## Tecnologias Utilizadas

- Java 17  
- Spring Boot 2.7.x  
- Spring Web  
- Spring Data JPA  
- MySQL 8.0  
- Maven  
- Lombok 1.18.30  

---

## Pré-requisitos

- Java 17
- Maven 3.8 ou superior
- MySQL Server 8.0
- IDE (Eclipse ou VS Code)
- XAMPP 3.3.0

---

## Configuração Rápida

### 1. Clonar o repositório

```bash
git clone https://github.com/SamirSoaresDeMelo/Microsservicos.git
cd Microsservicos
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

OBS: Lembre-se de iniciar o **MySQL** no **XAMPP Control Panel**.

### 1. Iniciar primeiro o department-service

```powershell 
cd department-service
mvn spring-boot:run
``` 

### 2. Iniciar o user-service em outro terminal

```powershell 
cd user-service
mvn spring-boot:run
``` 

---

## Endpoints da API

Instale no **Visual Studio Code** a extensão **Thunder Client** (menu lateral *Extensions*) para consumir APIs, ou utilize o **Postman** para realizar as requisições.

### Salvar um departamento

- **Method:** POST  
- **URL:** http://localhost:8080/api/departments  
- **Body (JSON):**

```json 
{
  "departmentName": "TI",
  "departmentAddress": "Tecnologia da Informação",
  "departmentCode": "TI-001"
}
``` 

---

### Obter departamento por ID

- **Method:** GET  
- **URL:** http://localhost:8080/api/departments/1

---

### Salvar usuário

- **Method:** POST  
- **URL:** http://localhost:8081/api/users  
- **Body (JSON):**

```json 
{
  "firstName": "Samir",
  "lastName": "Soares",
  "email": "samirsoaresdemelo@gmail.com",
  "departmentId": 1
}
``` 

---

### Obter usuário por ID

- **Method:** GET  
- **URL:** http://localhost:8081/api/users/1

---

## 📂 Estrutura do Projeto

```text 
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

---

## Fluxo de Comunicação

- Cliente faz requisição para o **User Service**
- User Service busca os dados do usuário no banco **employee_db**
- User Service chama o **Department Service** via REST para obter os dados do departamento
- Department Service retorna as informações do departamento
- User Service combina os dados e retorna a resposta completa ao cliente

---

## Referências

Projeto criado por meio do tutorial disponível no link:  
https://www.javaguides.net/2022/10/spring-boot-microservices-communication-using-resttemplate.html






