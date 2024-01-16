# base da imagem Docker a ser utilizada
FROM python:3.9-alpine

# adicionar a camada que será o diretório padrão a ser utilizado
WORKDIR /ex7

# copiar os arquivos da pasta src (source) para diretório padrão do container
COPY ./app /ex7/app

# copiar o arquivo de instalação dos requisitos do ambiente
COPY requirements.txt /ex7

# rodar um comando para instalar os pacotes necessários 
RUN pip install -r requirements.txt
