FROM python:3.9-slim-buster

LABEL Name="Python Flask Demo App" Version="1.4.2"
LABEL org.opencontainers.image.source="https://github.com/benc-uk/python-demoapp"

# Definir a variável de build com valor padrão "src"
ARG srcDir=src

# Definir o diretório de trabalho
WORKDIR /app

# Copiar o arquivo de requisitos e instalar as dependências
COPY $srcDir/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar o arquivo run.py e o diretório app para o diretório de trabalho
COPY $srcDir/run.py .
COPY $srcDir/app ./app

# Expor a porta 5000
EXPOSE 5000

# Comando de inicialização do Gunicorn para rodar o Flask
CMD ["gunicorn", "-b", "0.0.0.0:5000", "run:app"]
