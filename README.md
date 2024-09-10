
# GITLAB CICD

Este repositório contém uma aplicação de exemplo em Python usando Flask, destinada a demonstrar a estrutura e fluxo de desenvolvimento usando Docker, Azure Web App e testes unitários. O projeto utiliza um **Makefile** para automatizar tarefas comuns como testes, criação de imagem Docker e deploy na nuvem.

## Clonando o Repositório

Para clonar o projeto, execute o seguinte comando no terminal:

```
git clone https://github.com/benc-uk/python-demoapp
```

### Estrutura do projeto clonado

```
.
├── src/                             # Application source code directory
│   ├── app/                         # Contains the Flask application and its modules
│   ├── requirements.txt             # List of Python dependencies
│   └── run.py                       # Main file to start the Flask server
├── build/                           # Dockerfile and scripts related to building the Docker image
├── deploy/                          # Configuration files for deploying to Azure
├── tests/                           # Directory containing API integration tests
└── Makefile                         # File that automates development and deployment tasks
```

### Testes unitários

Os testes unitários da aplicação estão localizados no diretório `src/app/tests`. Esses testes são responsáveis por validar o comportamento do código localmente, antes de qualquer deploy ou integração contínua.

![image](https://github.com/user-attachments/assets/3e37d839-b2ff-4164-96fe-9ec0f3c6cf2a)

![image](https://github.com/user-attachments/assets/eccbc530-286b-479d-a717-5ff591a63a80)

### makefile

Makefile é utilizado para a automação de tarefas de desenvolvimento e implantação de uma aplicação Python, através da definição de regras e comandos específicos, permitindo a execução de workflows predefinidos, como instalação de dependências, execução de testes e deploy

Para executar os testes unitários do projeto, o comando `make test` realizará as seguintes funções definidas no arquivo makefile:

1. **Criação do Ambiente Virtual**:
   - Criará um ambiente virtual Python no diretório `src/.venv`.

2. **Instalação das Dependências**:
   - Instalará as dependências listadas no arquivo `src/requirements.txt` dentro do ambiente virtual criado.

3. **Execução dos Testes**:
   - Executará os testes unitários do projeto usando `pytest`. Os testes devem estar localizados em `src/app/tests`.

![image](https://github.com/user-attachments/assets/600b840f-994f-4fe0-812b-3aa69e114262)


## CREATE CI/CD PIPELINE

### Pipeline será escrito em código:
 
Pipeline é um conjunto de etapas automatizadas que ajudam a construir, testar e implantar seu código. Em vez de executar essas etapas manualmente, você escreve um pipeline que descreve essas etapas como código.

### create gitlab-ci.yml file

Este é o nome padrão do arquivo de configuração do pipeline para o GitLab CI/CD. O GitLab CI/CD é uma ferramenta que permite a integração contínua e a entrega contínua de software. O arquivo .gitlab-ci.yml é onde você escreve as instruções sobre como o GitLab deve construir, testar e implantar seu código.

## Screenshot

![screen](https://user-images.githubusercontent.com/14982936/30533171-db17fccc-9c4f-11e7-8862-eb8c148fedea.png)
