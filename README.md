
<p align="center">
  <img src="https://github.com/Significant-Gravitas/AutoGPT-Code-Ability/assets/50577581/4fefdc1d-4fba-476a-8456-646428cc05ec" alt="Robot Coding"/>
</p>


## Introduction

AutoGPT Unleashes Coding Power for All: Open Source AI Lowers Barriers to Software Creation! 

AutoGPT's coding ability is an open-source coding assistant powered by AI. 
The goal is to make software development more accessible to everyone, regardless of skill level or resources. 
By generating code in Python, a popular and very accessible language, AutoGPT acts as a virtual co-pilot to help users build projects like backends for existing frontends or command-line tools. 

This README provides an overview of this system and its components.

## Target Audience

**Beginners and those new to coding/programming:** This includes students, hobbyists, or anyone interested in learning to code but facing barriers to entry. The product aims to make coding accessible and affordable for beginners.

**Underrepresented groups in tech/coding:** By fostering an inclusive open-source community, the product seems targeted at engaging underrepresented groups and empowering them to get involved. 

**Experienced developers/engineers:** While lowering barriers for beginners, the product also offers value for experienced developers by providing an AI coding assistant to boost productivity.

**Open source contributors:** Contributors are encouraged to join in contributing code, knowledge, and driving innovation collaboratively.

**Cost-conscious organizations/individuals:** By making software development more affordable, it could appeal to small businesses, startups, or individuals looking to build applications inexpensively.

## Setup Guide 

Welcome to the initial setup guide for your project. Follow these easy steps to get everything up and running.

### Step 1: Install Dependencies

**Install Packages**

Start by installing all necessary dependencies. In your terminal, run:

```
poetry install
poetry shell
```

This command installs all Python package dependencies required by your project.

**Select Poetry Python Interpreter**

Now make sure you're using the poetry Python interpreter:

In VSCode, bring up the "Show and Run Commands" view by pressing: `Ctrl/Cmd + Shift + P`

Then input:
```
> Python: Select Interpreter
```
And select the Poetry Python Interpreter.


### Step 2: Initialize Docker Containers
There are two ways you can run the containers: 

If you want only the DB running: 
```
docker compose -f docker-compose.yml up
```
> Take note of the **port** printed in the output. e.g: `listening on IPv4 address "0.0.0.0", port 5432`.

Then run the server:
```
./run serve
```

Run the frontend

```
poetry run streamlit run chat.py
```

If you want both the db and app running:
```
docker compose -f docker-compose.full.yml up
```

The visit localhost:8501 to use the frontend to chat and build your codebase.

### Step 3: Configure and Migrate Database

In a new terminal, copy the .env file template:

```
cp .env.example .env
```

To set up your database, we'll begin by configuring the `DATABASE_URL` through an environment variable. 

Simply set the database `port` you took note of above (e.g. 5432).

Then, take this opportunity to update the .env with your OpenAI key and your Github token.

Once you've updated the .env file with this info, launch a Poetry Shell:
```
poetry shell
```

Now migrate your database schema with:

```
prisma migrate dev --name init
```

This initializes your database with the required schema.

### Step 4: Generate Prisma Client

After migrating your database, generate the Prisma client to interact with your database in your application:

```
prisma generate
```

This command generates or updates the Prisma client, ensuring your application can communicate effectively with the database.

### Step 5: Populate the Database

Ensure your database is populated with initial data by adjusting the database URL for the specific port and running the population script:

```
./run populate-db
```

This populates your database with the initial data set required for your application.

### Step 6: Launch the Application

Set your environment to local and start the server:

```
./run serve
```

This starts the application's server, making it available for development use.

### Step 7: Access the Documentation

Access the running application's interactive documentation at:

```
http://127.0.0.1:8000/docs
```


## Demo


https://github.com/Significant-Gravitas/AutoGPT-Code-Ability/assets/50577581/264838a3-6f24-4758-974e-f9cba64a4166



Here, you'll find detailed guides and references on interacting with the application.

### What's Next?

With your environment set up, head over to the **Interaction Flow** section for insights on how interactions within the application work, further exploring its functionality and features.

## Components

This system is an advanced software development framework comprised of various specialized sub-agents and components. Each component plays a critical role in the software development lifecycle, from conception to deployment. In addition to the primary sub-agents, this system includes essential supportive components: the Common Module, and Prompts Module.

1. **Requirements (Product Owner)**: This component is pivotal in understanding and defining the product requirements. It acts as a bridge between the client's needs and the technical team, ensuring that the developed software aligns perfectly with the client's vision.

2. **Architect (Solutions Architect)**: Responsible for crafting the overall architecture of the application. This component breaks down the application into manageable modules and writes the templates that guide the development process.

3. **Developer**: The hands-on coding component. Utilizing the templates and guidelines provided by the Architect, the Developer sub-agent is responsible for writing the individual functions and pieces of the application.

4. **Deploy Agent**: The final phase of the software development process, this component is tasked with compiling, packaging, and deploying the completed application, ensuring its successful deployment to the designated environment.

5. **Common Module**: A fundamental component used across all stages of development. It provides shared functionalities and resources, such as libraries and tools, that are essential for the Design, Architect, Coding, and Delivery modules. This module ensures consistency and efficiency in the development process.

6. **Prompts**: This component works closely with the Chains Module to generate and manage prompts for LLM interactions. It holds all the prompt templates allowing us to easily itterate prompt design without needing to modify the code.

Below is a diagram illustrating the structure of AutoGPT's coding ability and the interaction between its components:

```mermaid
erDiagram
    CODEX ||--o{ REQUIREMENTS : includes
    CODEX ||--o{ ARCHITECT : includes
    CODEX ||--o{ DEVELOPER : includes
    CODEX ||--o{ DEPLOY : includes
    REQUIREMENTS ||--|| COMMON-MODULE : uses
    ARCHITECT ||--|| COMMON-MODULE : uses
    DEVELOPER ||--|| COMMON-MODULE : uses
    DEPLOY ||--|| COMMON-MODULE : uses
    REQUIREMENTS ||--|| ARCHITECT : "defines requirements for"
    ARCHITECT ||--|| DEVELOPER : "architects solution for"
    DEVELOPER ||--|| DEPLOY : "develops code for"
    DEPLOY ||--o{ CODEX : "deploys application to"

    CODEX {
        string name
    }
    REQUIREMENTS {
        string role "Product Owner"
    }
    ARCHITECT {
        string role "Solutions Architect"
    }
    DEVELOPER {
        string role "Junior Developer"
    }
    DEPLOY {
        string role "Build Engineer"
    }
    COMMON-MODULE {
        string function
    }

```

## Interaction Flow

This is what the expected user interaction workflow will look like:

```mermaid

sequenceDiagram
    actor User
    participant API as Codex API
    participant Codex


    User->>API: requests an application
    API->>User: Returns App ID
    alt Requirements Development
      User->>API: Request Application Spec for App ID
      API->>+Codex: Runs Requirments Flow
      Codex-->>-User: Returns the App Spec ID
      User->>API: Modifies App Spec Element
      API->>User: Returns a new App Spec ID
    end

    User->>API: Request App Spec ID is Developed
    API->>+Codex: Runs Architect and Develop Flow

    Codex->>-User: Returns Developed App ID

    User->>API: Requests Developed App ID is deployed
    Note right of API: When we have added deployment flow

    API->>+Codex: Runs Deployment Flow
    Codex-->>-User: Returns Deployment details (id, url etc)


    User->>API: Requests Code for Developed App ID
    Note right of API: During Beta

    API->>+Codex: Runs Package flow

    Codex-->>-User: Returns link to Github repo (or if running locally, you can switch to zipfile)

```

## Workflow

1. **Requirement Analysis**: The Design sub-agent interacts with the client to gather and define the product requirements.

2. **Architecture Design**: Based on the requirements, the Architect sub-agent develops a solution architecture, breaking down the application into smaller, manageable modules and creating templates.

3. **Development**: The Developer sub-agent uses the templates and architecture guidelines to write the actual code for each module of the application.

4. **Deployment**: Once the coding is complete, the Deploy sub-agent takes over to package, compile, and deploy the application to the desired environment.

```mermaid
sequenceDiagram
    participant User
    participant Requirements
    participant Architect
    participant Developer
    participant Deploy

    User->>+Requirements: Request
    Requirements->>User: Initial Requirements
    User->>+Requirements: Feedback/Corrections
    Requirements->>+Architect: Refined Requirements
    Architect->>+Developer: Architecture & Templates
    loop Development Iterations
        Developer->>+Architect: Request Clarification
        Architect->>+Developer: Additional Details
    end
    Developer->>+Deploy: Completed Code
    Deploy->>+User: Deploy to Production
```

## Database Schema

<p align="center">
    <img src="docs/mermaid.png" alt="Mermaid Diagram of Schema" style="border-radius: 50px;" width="600">
</p>

The schema revolves around key models:

- CodeGraph: Represents the logic and structure of code as graphs, linked to function definitions and database schemas.
- FunctionDefinition: Defines individual functions with details like input/output types, tied to specific CodeGraphs.
- CompiledRoute: Transforms CodeGraphs into executable routes, integral for the application's runtime.
- Application: The aggregate entity that combines multiple CompiledRoutes into a complete application.
- Functions and Package: Detail the executable elements and dependencies within the application.
- DatabaseSchema and DatabaseTable: Manage database interactions within the generated code, facilitating data-driven functionalities.

This schema is pivotal for automating code generation, from defining logic in CodeGraphs to the final application assembly, enhancing our application's efficiency and scalability.

```mermaid
erDiagram

    Application ||--o{ CompiledRoute : "compiledRoutes"
    CompiledRoute ||--o{ Functions : "functions"
    CompiledRoute ||--o{ Application : "applications"
    CompiledRoute }o--|| CodeGraph : "codeGraph"
    CodeGraph ||--o{ FunctionDefinition : "functionDefs"
    CodeGraph ||--o{ CompiledRoute : "compiledRoute"
    CodeGraph }o--|| DatabaseSchema : "databaseSchema"
    FunctionDefinition }o--|| CodeGraph : "codeGraph"
    FunctionDefinition }o--|| Functions : "function"
    Functions ||--o{ FunctionDefinition : "functionDefs"
    Functions ||--o{ Package : "packages"
    Functions ||--o{ CompiledRoute : "compiledRoutes"
    Package ||--o{ Functions : "functions"
    DatabaseSchema ||--o{ DatabaseTable : "tables"
    DatabaseSchema ||--o{ CodeGraph : "codeGraphs"
    DatabaseTable ||--o{ DatabaseSchema : "schemas"
    DatabaseTable }o--o{ DatabaseTable : "relatedFromTables"
    DatabaseTable }o--o{ DatabaseTable : "relatedToTables"

    Application {
        int id PK "Primary Key"
        datetime createdAt "Creation Date"
        string name "Application Name"
        string description "Description"
    }

    CompiledRoute {
        int id PK "Primary Key"
        datetime createdAt "Creation Date"
        string embedding "Embedding (Unsupported)"
        string description "Description"
        string code "Code"
        int codeGraphId FK "Foreign Key to CodeGraph"
    }

    CodeGraph {
        int id PK "Primary Key"
        datetime createdAt "Creation Date"
        string function_name "Function Name"
        string api_route "API Route"
        string graph "Graph Representation"
        int databaseSchemaId FK "Foreign Key to DatabaseSchema"
    }

    FunctionDefinition {
        int id PK "Primary Key"
        datetime createdAt "Creation Date"
        string name "Function Name"
        string description "Description"
        string input_type "Input Type"
        string return_type "Return Type"
        int codeGraphId FK "Foreign Key to CodeGraph"
        string functionId FK "Foreign Key to Functions"
    }

    Functions {
        string id PK "Primary Key"
        datetime createdAt "Creation Date"
        string embedding "Embedding (Unsupported)"
        string name "Function Name"
        string description "Description"
        string input_type "Input Type"
        string return_type "Return Type"
        string code "Code"
    }

    Package {
        string id PK "Primary Key"
        datetime createdAt "Creation Date"
        string packageName "Package Name"
        string version "Version"
        string specifier "Specifier"
    }

    DatabaseSchema {
        int id PK "Primary Key"
        datetime createdAt "Creation Date"
        string embedding "Embedding (Unsupported)"
        string description "Description"
    }

    DatabaseTable {
        int id PK "Primary Key"
        datetime createdAt "Creation Date"
        string embedding "Embedding (Unsupported)"
        string description "Description"
        string definition "Definition"
    }
```

## Useful commands

> docker buildx build --platform linux/amd64 -t gcr.io/agpt-dev/mvp/codegen . --push

## Prisma with Python: Quick Setup and Usage Guide

Prisma is an open-source database toolkit that simplifies database access and management. Although Prisma is traditionally associated with JavaScript and TypeScript, it can also be integrated with Python projects. This section of the README provides a quick cheat sheet for setting up Prisma in a Python environment, applying migrations, and other useful tips.

### 1. Setting Up Prisma

#### Prerequisites:

- Node.js installed (for Prisma CLI)
- Python environment setup

#### Steps:

1. **Install Prisma CLI**:

   - Use npm to install Prisma globally:
     ```bash
     npm install -g prisma
     ```

2. **Initialize Prisma in Your Project**:

   - Navigate to your Python project directory and initialize Prisma:
     ```bash
     prisma init
     ```
   - This command creates a new `prisma` directory with a default `schema.prisma` file.

3. **Configure Your Database**:
   - In `schema.prisma`, configure the `datasource` block to point to your database. For example, for PostgreSQL:
     ```prisma
     datasource db {
       provider = "postgresql"
       url      = env("DATABASE_URL")
     }
     ```
   - Replace `DATABASE_URL` with your database connection string.

### 2. Defining Your Data Model

- In the `schema.prisma` file, define your data models. For example:
  ```prisma
  model User {
    id    Int     @id @default(autoincrement())
    name  String
    email String  @unique
  }
  ```

### 3. Migrations

#### Creating Migrations:

- After defining your models, create a migration to update the database schema:
  ```bash
  prisma migrate dev --name init
  ```
- This command creates SQL files in the `prisma/migrations` directory.

#### Applying Migrations:

- Apply migrations to update your database schema:
  ```bash
  prisma migrate deploy
  ```

#### Marking a migration as locally resolved (useful for hot fixes)

```bash
prisma migrate resolve --applied "<migration-name>"
```


### 4. Generating Prisma Client

- Generate Prisma Client to interact with your database:
  ```bash
  prisma generate
  ```

### 5. Using Prisma with Python

- Since Prisma Client is native to JavaScript/TypeScript, using it in Python requires a workaround. You can execute Prisma Client through a child process. For example:

  ```python
  import subprocess
  import json

  def run_prisma_command(command):
      result = subprocess.run(["npx", "prisma", *command], capture_output=True)
      return json.loads(result.stdout)

  users = run_prisma_command(["query", "user", "--all"])
  print(users)
  ```

### 6. Useful Commands

- **Introspect Database**: To update your Prisma schema based on an existing database:
  ```bash
  prisma introspect
  ```
- **Studio**: Prisma Studio provides a GUI to view and edit data in your database:
  ```bash
  prisma studio
  ```

### Conclusion

This cheat sheet covers the basics of setting up Prisma in a Python project and performing essential database operations. Remember that using Prisma with Python is less straightforward than with JavaScript/TypeScript, and it may require additional setup and handling. For more detailed information, refer to the [Prisma Documentation](https://www.prisma.io/docs/).

