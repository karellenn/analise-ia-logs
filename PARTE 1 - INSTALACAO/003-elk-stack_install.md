
# Instalação do elastic-stack Docker Script
 
1. **Crie o arquivo do script**
  nano instalador_elk_stack.sh

2. **Copie e Cole**

**************************************************************************************************************************************************************************************************
```
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
  
  # Verificar e instalar o Docker, se não estiver instalado
  if ! [ -x "$(command -v docker)" ]; then
    echo "Docker não está instalado. Instalando o Docker..."
    curl -fsSL https://get.docker.com | sh
  else
    echo "Docker já está instalado."
  fi
  
  # Criar diretório para o Elastic Stack
  echo "Criando diretório /opt/elk-stack..."
  sudo mkdir -p /opt/elk-stack
  cd /opt/elk-stack
  
  # Criar o arquivo docker-compose.yml
  echo "Criando arquivo docker-compose.yml..."
  cat <<EOL | sudo tee docker-compose.yml
  version: '3'
  services:
    setup:
      image: docker.elastic.co/elasticsearch/elasticsearch:8.12.1
      ports:
        - 9200:9200
      environment:
        - ELASTIC_PASSWORD=admin
        - KIBANA_PASSWORD=admin
      container_name: setup
      command:
        - bash
        - -c
        - |
          echo "Waiting for Elasticsearch availability";
          until curl -s http://elasticsearch:9200 | grep -q "missing authentication credentials"; do sleep 30; done;
          echo "Setting kibana_system password";
          until curl -s -X POST -u "elastic:admin" -H "Content-Type: application/json" http://elasticsearch:9200/_security/user/kibana_system/_password -d "{\"password\":\"admin\"}" | grep -q "^{}"; do sleep 10; done;
          echo "All done!";
  
    elasticsearch:
      image: docker.elastic.co/elasticsearch/elasticsearch:8.12.1
      container_name: elasticsearch
      environment:
        - discovery.type=single-node
        - cluster.name=elasticsearch
        - bootstrap.memory_lock=true
        - ES_JAVA_OPTS=-Xms1g -Xmx1g
        - ELASTIC_PASSWORD=admin
        - xpack.security.http.ssl.enabled=false
  
    kibana:
      image: docker.elastic.co/kibana/kibana:8.12.1
      container_name: kibana
      ports:
        - 5601:5601
      environment:
        - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
        - ELASTICSEARCH_USERNAME=kibana_system
        - ELASTICSEARCH_PASSWORD=admin
        - TELEMETRY_ENABLED=false
  EOL
  
  # Criar o arquivo .env com as senhas definidas
  echo "Criando o arquivo .env..."
  cat <<EOL | sudo tee .env
  ELASTIC_PASSWORD=admin
  KIBANA_PASSWORD=admin
  EOL
  
  # Subir os containers com o Docker Compose
  echo "Subindo os contêineres com Docker Compose..."
  sudo docker compose up -d
  
  # Mostrar os contêineres em execução
  echo "Verificando o status dos contêineres..."
  sudo docker ps
  
  # Instruções adicionais para configurar as chaves de criptografia no Kibana
  echo "Para ajustar as chaves de criptografia no Kibana, siga os passos abaixo:"
  echo "1. Liste os contêineres para identificar o ID do Kibana: sudo docker ps"
  echo "2. Gere as chaves de criptografia executando o comando abaixo (substitua [ID_KIBANA] pelo ID do contêiner Kibana):"
  echo "   sudo docker exec -it [ID_KIBANA] ./bin/kibana-encryption-keys generate"
  echo "3. Configure as novas chaves no arquivo kibana.yml:"
  echo "   sudo docker exec -it [ID_KIBANA] /bin/bash -c \"echo -e '\\nxpack.encryptedSavedObjects.encryptionKey: ecaafedbd4daf82cf92f90d0f7b4fca3\\nxpack.reporting.encryptionKey:  
  c2e97cfc7218bb0ff79c63a0be0495de\\nxpack.security.encryptionKey: 7e5528384de8ae29cfaf76bb5b928cf2\\n' >> config/kibana.yml\""
```

**************************************************************************************************************************************************************************************************
Centos os

#!/bin/bash

# Atualizar o sistema
echo "Atualizando o sistema..."
sudo yum update -y

# Verificar e instalar o Git, se não estiver instalado
if ! [ -x "$(command -v git)" ]; then
  echo "Git não está instalado. Instalando o Git..."
  sudo yum install git -y
else
  echo "Git já está instalado."
fi

# Verificar e instalar o Docker, se não estiver instalado
if ! [ -x "$(command -v docker)" ]; then
  echo "Docker não está instalado. Instalando o Docker..."
  curl -fsSL https://get.docker.com | sh
  sudo systemctl start docker
  sudo systemctl enable docker
else
  echo "Docker já está instalado."
fi

# Verificar e instalar o Docker Compose, se não estiver instalado
if ! [ -x "$(command -v docker-compose)" ]; then
  echo "Docker Compose não está instalado. Instalando o Docker Compose..."
  sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  sudo chmod +x /usr/local/bin/docker-compose
else
  echo "Docker Compose já está instalado."
fi

# Criar diretório para o Elastic Stack
echo "Criando diretório /opt/elk-stack..."
sudo mkdir -p /opt/elk-stack
cd /opt/elk-stack

# Criar o arquivo docker-compose.yml
echo "Criando arquivo docker-compose.yml..."
cat <<EOL | sudo tee docker-compose.yml
version: '3'
services:
  setup:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.12.1
    ports:
      - 9200:9200
    environment:
      - ELASTIC_PASSWORD=admin
      - KIBANA_PASSWORD=admin
    container_name: setup
    command:
      - bash
      - -c
      - |
        echo "Waiting for Elasticsearch availability";
        until curl -s http://elasticsearch:9200 | grep -q "missing authentication credentials"; do sleep 30; done;
        echo "Setting kibana_system password";
        until curl -s -X POST -u "elastic:admin" -H "Content-Type: application/json" http://elasticsearch:9200/_security/user/kibana_system/_password -d "{\"password\":\"admin\"}" | grep -q "^{}"; do sleep 10; done;
        echo "All done!";

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.12.1
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - cluster.name=elasticsearch
      - bootstrap.memory_lock=true
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
      - ELASTIC_PASSWORD=admin
      - xpack.security.http.ssl.enabled=false

  kibana:
    image: docker.elastic.co/kibana/kibana:8.12.1
    container_name: kibana
    ports:
      - 5601:5601
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
      - ELASTICSEARCH_USERNAME=kibana_system
      - ELASTICSEARCH_PASSWORD=admin
      - TELEMETRY_ENABLED=false
EOL

# Criar o arquivo .env com as senhas definidas
echo "Criando o arquivo .env..."
cat <<EOL | sudo tee .env
ELASTIC_PASSWORD=admin
KIBANA_PASSWORD=admin
EOL

# Subir os containers com o Docker Compose
echo "Subindo os contêineres com Docker Compose..."
sudo docker-compose up -d

# Mostrar os contêineres em execução
echo "Verificando o status dos contêineres..."
sudo docker ps

# Instruções adicionais para configurar as chaves de criptografia no Kibana
echo "Para ajustar as chaves de criptografia no Kibana, siga os passos abaixo:"
echo "1. Liste os contêineres para identificar o ID do Kibana: sudo docker ps"
echo "2. Gere as chaves de criptografia executando o comando abaixo (substitua [ID_KIBANA] pelo ID do contêiner Kibana):"
echo "   sudo docker exec -it [ID_KIBANA] ./bin/kibana-encryption-keys generate"
echo "3. Configure as novas chaves no arquivo kibana.yml:"
echo "   sudo docker exec -it [ID_KIBANA] /bin/bash -c \"echo -e '\\nxpack.encryptedSavedObjects.encryptionKey: ecaafedbd4daf82cf92f90d0f7b4fca3\\nxpack.reporting.encryptionKey: c2e97cfc7218bb0ff79c63a0be0495de\\nxpack.security.encryptionKey: 7e5528384de8ae29cfaf76bb5b928cf2\\n' >> config/kibana.yml\""


***********************************************************************************************************************



```
3. **Torne o script executável:**
```
  chmod +x instalador_elk_stack.sh
```
5. **Execute o script:**
  ./instalador_elk_stack.sh

```
Gere o arquivo de chaves de criptografia:
```
docker exec -it [ID_KIBANA] ./bin/kibana-encryption-keys generate
```
Configure as novas chaves no arquivo de configuração kibana.yml:
```
docker exec -it [ID_KIBANA] /bin/bash -c "echo -e '\nxpack.encryptedSavedObjects.encryptionKey: ecaafedbd4daf82cf92f90d0f7b4fca3\nxpack.reporting.encryptionKey: c2e97cfc7218bb0ff79c63a0be0495de\nxpack.security.encryptionKey: 7e5528384de8ae29cfaf76bb5b928cf2\n' >> config/kibana.yml"
```
**Substitua [ID_KIBANA] pelo ID real do contêiner do Kibana obtido no passo anterior**
