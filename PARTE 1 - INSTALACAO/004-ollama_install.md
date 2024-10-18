
# Instalação ollama script

1. **Crie um arquivo**
    nano instalador_ollama_open_webui.sh


2. **Copie e Cole o conteúdo do script**
**************************************************************************************************************************************************************************************************
    #!/bin/bash

    # Variáveis de cores para saída no terminal
    GREEN='\033[0;32m'
    NC='\033[0m' # Sem cor

    echo -e "${GREEN}Iniciando a instalação do Ollama e Open WebUI...${NC}"

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

    # 3. Instalando Ollama
    echo -e "${GREEN}Instalando o Ollama...${NC}"
    curl -fsSL https://ollama.com/install.sh | sh

    # 4. Ajustando o serviço Ollama para escutar em todos os IPs
    echo -e "${GREEN}Ajustando o serviço Ollama para aceitar conexões de qualquer IP...${NC}"
    sudo sed -i '/\[Service\]/a Environment="OLLAMA_HOST=0.0.0.0"' /etc/systemd/system/ollama.service

    # 5. Recarregando e reiniciando o serviço Ollama
    echo -e "${GREEN}Recarregando e reiniciando o serviço Ollama...${NC}"
    sudo systemctl daemon-reload
    sudo systemctl restart ollama

    # 6. Verificando e instalando o Docker, se não estiver instalado
    if ! command -v docker &> /dev/null
    then
        echo -e "${GREEN}Docker não encontrado. Instalando Docker...${NC}"
        curl -fsSL https://get.docker.com | sh
    else
        echo -e "${GREEN}Docker já está instalado.${NC}"
    fi

    # 7. Instalando e iniciando o Open WebUI com Docker
    echo -e "${GREEN}Instalando e iniciando o Open WebUI...${NC}"
    sudo docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main

    # 8. Instalando o modelo com Ollama
    echo -e "${GREEN}Instalando o modelo ALIENTELLIGENCE/cyberaisecurityv2 no Ollama...${NC}"
    ollama run ALIENTELLIGENCE/cyberaisecurityv2

    # 9. Mensagem final de sucesso
    echo -e "${GREEN}Instalação concluída! Ollama está configurado e o Open WebUI está rodando na porta 3000.${NC}"
**************************************************************************************************************************************************************************************************
3. **Torne o script executável**
    chmod +x instalador_ollama_open_webui.sh
4. **Execute o script**
    ./instalador_ollama_open_webui.sh