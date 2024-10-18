
## Visão Geral do Projeto
Esse Porjeto e originario do projeto do Carlos silva cujo os videos estão nesse projeto 

Modificações do projeto original

1- instalações via script
2- fluxo com telegram

Neste projeto, os alertas gerados pela ferramenta de segurança SIEM são enviados para uma plataforma de gerenciamento de incidentes (DFIR-IRIS), onde uma IA faz uma análise inicial.

Com o tempo, playbooks e runbooks alinhados ao modelo de IA podem ajudar muito na tomada de decisões em cenários específicos, dependendo de como a IA foi treinada.

### Funcionalidades Úteis para um SOC

- **Tomada de decisão** baseada em playbooks e runbooks.
- **Classificação do nível de ameaça** considerando vários fatores, como usuário, IP e correlação com ferramentas de CTI.

Com um ambiente orquestrado e inteligente, você pode focar sua criatividade e tempo em tarefas mais estratégicas, aumentando a eficiência e a eficácia das operações de segurança.


### Arquitetura do Projeto
![Workflow](https://github.com/karellenn/analise-ia-logs/blob/main/IMG/diagrama.png)

Com um ambiente orquestrado e inteligente, você pode focar sua criatividade e tempo em tarefas mais estratégicas, aumentando a eficiência e a eficácia das operações de segurança.

### Requisitos
- 4x Máquinas virtuais para rodar o ambiente
- Acesso à internet para atualização de pacotes e comunicação com plataformas externas
  
Criado scripts para automação do processo de instalação das ferramentas 
### [Parte I: Instalação](https://github.com/karellenn/analise-ia-logs/tree/main/PARTE%201%20-%20INSTALACAO)

Nesta seção, detalharemos os passos necessários para instalar todas as ferramentas e configurar o ambiente.

### [Parte II: Construção do workflow](https://github.com/karellenn/analise-ia-logs/tree/main/PARTE%202%20-%20CONFIGURA%C3%87%C3%83O)
Aqui, construiremos a solução integrada, conectando o SIEM, a IA e a plataforma de gerenciamento de incidentes. Também discutiremos como personalizar e expandir as funcionalidades para atender a diferentes necessidades.

### [Parte III: Construção do workflow WAZUH](https://github.com/karellenn/analise-ia-logs/tree/main/PARTE%203%20-%20WAZUH)

---

Este projeto é ideal para equipes de segurança que desejam implementar uma solução poderosa de SOC utilizando apenas ferramentas de código aberto, combinando o poder da inteligência artificial com práticas avançadas de gerenciamento de incidentes.
