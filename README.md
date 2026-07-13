# 🛡️ Wazuh SIEM Lab: Simulação de Ransomware e Engenharia de Detecção

Este repositório contém um laboratório prático de **Blue Team** focado na simulação de adversários e engenharia de detecção utilizando o **Wazuh SIEM** em um ambiente Linux. O objetivo é identificar comportamentos suspeitos comuns de Ransomware e táticas de reconhecimento (Discovery) do framework MITRE ATT&CK.

---

## 🚀 Estrutura do Lab

O projeto foi dividido em duas fases principais:
1. **Red Team Simulation:** Execução de um script em Bash que emula o comportamento de um ataque dentro de um diretório isolado.
2. **Blue Team Detection:** Coleta de logs pelo agente do Wazuh, análise dos eventos e criação de uma regra de detecção customizada de alta severidade.

---

## 🔴 1. Simulação da Ameaça (Red Team)

O script `simulador_soc.sh` executa as seguintes ações simuladas para gerar telemetria:
* **Criação massiva de arquivos:** Gera 40 arquivos de texto simulando alta atividade de escrita em disco (I/O)[cite: 2].
* **Criptografia Simulada:** Renomeia em massa os arquivos criados para a extensão `.locked` (padrão comportamental de Ransomwares)[cite: 2].
* **Acesso Restrito:** Tenta ler hashes de senhas no arquivo `/etc/shadow`[cite: 2].
* **Reconhecimento de Rede:** Mapeia portas e conexões ativas utilizando `ss -tulpn`[cite: 2].
* **Coleta de Identidade:** Executa rapidamente comandos de descoberta de privilégios (`id`, `whoami`, `uname -a`)[cite: 2].

### Código do Script Utilizado:
```bash
#!/bin/bash
# SCRIPT DE SIMULAÇÃO DE COMPORTAMENTO SUSPEITO
DIRETORIO_TESTE="$HOME/lab_simulacao_ransomware"
mkdir -p "$DIRETORIO_TESTE"

# Criação massiva de arquivos
for i in {1..40}; do
    echo "CONTEÚDO CONFIDENCIAL CRÍTICO" > "$DIRETORIO_TESTE/relatorio_financeiro_$i.txt"
    sleep 0.02
done

# Renomeação em sequência (.locked)
for arquivo in "$DIRETORIO_TESTE"/*.txt; do
    if [ -f "$arquivo" ]; then
        mv "$arquivo" "${arquivo}.locked"
        sleep 0.02
    fi
done

# Uso de comandos administrativos restritos e descoberta
head /etc/shadow 2>/dev/null
ss -tulpn 2>/dev/null
id > /dev/null
whoami > /dev/null
uname -a > /dev/null


<img width="1456" height="1107" alt="Captura de Tela 2026-06-26 às 21 02 37" src="https://github.com/user-attachments/assets/16f80ede-dcaf-40c3-8097-2c64729e5c80" />
<img width="1710" height="1107" alt="Captura de Tela 2026-06-26 às 20 54 47" src="https://github.com/user-attachments/assets/6ef0691f-b179-4b03-bc72-2ba99e505f8c" />
