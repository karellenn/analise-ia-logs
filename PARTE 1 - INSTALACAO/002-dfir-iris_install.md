# Instalação DFIR-IRIS SCRIPT


1. **Crie o arquivo do script**
    nano instalador_dfir_iris.sh


2. **Copie e Cole**
**************************************************************************************************************************************************************************************************
    #!/bin/bash

    # Variáveis de cores para saída no terminal
    GREEN='\033[0;32m'
    NC='\033[0m' # Sem cor

    echo -e "${GREEN}Iniciando a instalação do DFIR-IRIS...${NC}"

    # 1. Atualizando o sistema
    echo -e "${GREEN}Atualizando pacotes do sistema...${NC}"
    sudo apt update && sudo apt upgrade -y

    # 2. Verificando e instalando o Git, se não estiver instalado
    if ! command -v git &> /dev/null
    then
        echo -e "${GREEN}Git não encontrado. Instalando Git...${NC}"
        sudo apt install git -y
    else
        echo -e "${GREEN}Git já está instalado.${NC}"
    fi

    # 3. Clonando o repositório DFIR-IRIS para o diretório /opt
    echo -e "${GREEN}Clonando o repositório DFIR-IRIS...${NC}"
    cd /opt
    sudo git clone https://github.com/dfir-iris/iris-web.git
    cd iris-web

    # 4. Verificando e instalando o Docker, se não estiver instalado
    if ! command -v docker &> /dev/null
    then
        echo -e "${GREEN}Docker não encontrado. Instalando Docker...${NC}"
        curl -fsSL https://get.docker.com | sh
    else
        echo -e "${GREEN}Docker já está instalado.${NC}"
    fi

    # 5. Verificando e instalando o Docker Compose, se não estiver instalado
    if ! command -v docker-compose &> /dev/null
    then
        echo -e "${GREEN}Docker Compose não encontrado. Instalando Docker Compose...${NC}"
        sudo apt install docker-compose -y
    else
        echo -e "${GREEN}Docker Compose já está instalado.${NC}"
    fi

    # 6. Fazendo checkout para a versão 2.4.11 do DFIR-IRIS
    echo -e "${GREEN}Alternando para a versão 2.4.11 do DFIR-IRIS...${NC}"
    sudo git checkout v2.4.11

    # 7. Copiando o arquivo .env.model para .env
    echo -e "${GREEN}Criando o arquivo de configuração .env...${NC}"
    sudo cp .env.model .env

    # 8. Construindo e inicializando os contêineres Docker
    echo -e "${GREEN}Construindo e iniciando os contêineres Docker...${NC}"
    sudo docker compose build
    sudo docker compose up -d
    echo -e "${GREEN}Senha de acesso iris ...${NC}"
    sudo docker compose logs app | grep "admin"

    # 9. Mensagem final de sucesso
    echo -e "${GREEN}Instalação concluída! Você pode acessar o DFIR-IRIS na URL: https://ip-da-vm:443 (ou conforme a configuração no arquivo .env).${NC}"

**************************************************************************************************************************************************************************************************

3. **Torne o script executável**
    chmod +x instalador_dfir_iris.sh

4. **Execute o script**
    ./instalador_dfir_iris.sh

### 5. Acessando o DFIR-IRIS
Após a inicialização, você pode acessar a interface web do DFIR-IRIS no seu navegador. A URL padrão geralmente é https://vm02:443, mas verifique a configuração no arquivo .env se necessário.

Considerações Finais
Certifique-se de que todos os pré-requisitos estão atendidos e que as portas necessárias estão abertas em seu firewall. O DFIR-IRIS deve agora estar funcionando e pronto para ser integrado com outras ferramentas e sistemas de segurança.

Para suporte adicional ou para contribuir com o projeto, consulte a documentação oficial e participe da comunidade no GitHub.
https://github.com/dfir-iris
