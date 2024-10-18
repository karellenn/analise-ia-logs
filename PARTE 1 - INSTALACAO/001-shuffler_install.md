
# Instalação do Shuffle atraves de script

1. **Criando arquivo**
    nano instalador_shuffle.sh

2. **Copie e cole o codigo**
******************************************************************************************************************************************************************************************   
    #!/bin/bash

    # Atualizar o sistema
    echo "Atualizando o sistema..."
    sudo apt update && sudo apt upgrade -y

    # Verificar e instalar o Git, se não estiver instalado
    if ! [ -x "$(command -v git)" ]; then
    echo "Git não está instalado. Instalando o Git..."
    sudo apt install git -y
    else
    echo "Git já está instalado."
    fi

    # Instalar o Docker
    echo "Instalando o Docker..."
    curl -fsSL https://get.docker.com | sh

    # Navegar até o diretório /opt e clonar o repositório do Shuffle
    echo "Clonando o repositório do Shuffle..."
    cd /opt
    sudo git clone https://github.com/Shuffle/Shuffle
    cd Shuffle

    # Criar o diretório para o banco de dados e ajustar permissões
    echo "Criando diretório shuffle-database e ajustando permissões..."
    sudo mkdir shuffle-database
    sudo chown -R 1000:1000 shuffle-database

    # Iniciar os serviços Docker com o Docker Compose
    echo "Iniciando o Shuffle com Docker Compose..."
    sudo docker compose up -d

    # Verificar o status dos contêineres
    echo "Verificando o status dos serviços..."
    sudo docker compose ps

    # Instrução para acessar a interface web
    echo "Instalação concluída! Acesse o Shuffle em: http://SEU_IP:3443"
**********************************************************************************************************************************************************************************

3. **Torne o script executável**
    chmod +x instalador_shuffle.sh

4. **Execute o script**
    ./instalardor_shuffle.sh

5.  **Acessando o Shuffle**
    
    Acesse a interface web do Shuffle através do navegador, usando o endereço e a porta configurados no arquivo `docker-compose.yml` (geralmente `http://vm01:3443`, onde `PORTA` é a porta configurada).
    
6.  **Configuração Adicional**
    
    Dependendo da configuração específica do seu ambiente, pode ser necessário realizar ajustes adicionais no arquivo de configuração do Shuffle ou nas configurações do Docker Compose. Consulte a documentação oficial do Shuffle para detalhes sobre configuração e ajustes adiciona