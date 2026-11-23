# Auditor de Resiliência HTTP/2 & Queda de Kernel Linux - TAVARDT Portugal

> Auditoria de Resiliência contra NoisyTCP, Rapid Reset (CVE-2023-44487) e Hardening de Servidores Web

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Active-brightgreen)](https://tavardt-portugal.github.io/http2-kernel-dos-resilience-checker-pt/)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Security: CIS Linux](https://img.shields.io/badge/Security-CIS%20Linux%20%7C%20RFC%209113-red.svg)](https://www.cisecurity.org/)

## Visão Geral da Arquitetura e Resiliência HTTP/2 & Linux Kernel

A estabilidade e resiliência de servidores corporativos de alta demanda dependem diretamente da harmonia entre a pilha de transporte TCP do kernel Linux e os motores de multiplexação HTTP/2.

Duas classes críticas de vetores de ataque afetam infraestruturas desprotegidas:
1. **Falha NoisyTCP no Kernel Linux:** A manipulação de sequências TCP fora de ordem com flags anômalas induz o subsistema de rede do kernel a consumir buffers de memória de forma desordenada, resultando no travamento de instâncias Nginx, Apache HTTP Server e portas SSH sem necessidade de autenticação prévia.
2. **Abuso de Rapid Reset em HTTP/2 (CVE-2023-44487):** O envio em rajada de requisições intercaladas por cancelamentos imediatos de streams (`RST_STREAM`) exaure os descritores de arquivos e capacidade de processamento do servidor de aplicação.

Este repositório fornece tanto uma aplicação web interativa executada 100% no navegador quanto um utilitário CLI em Python de biblioteca padrão para auditar parâmetros locais de kernel e servidores remotos, gerando blueprints de configuração defensiva para `sysctl`, `nginx` e `apache`.

## Aplicação Web Interativa

Acesse a ferramenta online e audite seu servidor em tempo real:

* [https://tavardt-portugal.github.io/http2-kernel-dos-resilience-checker-pt/](https://tavardt-portugal.github.io/http2-kernel-dos-resilience-checker-pt/)

A aplicação permite simular 5 etapas críticas de segurança, calcula o índice de resiliência (0 a 100), gera diagnósticos arquiteturais detalhados e fornece código defensivo pronto para implantação.

## Utilização do Scanner CLI (Python)

O repositório inclui o script autônomo `http2_kernel_dos_checker.py`, desenvolvido exclusivamente com a biblioteca padrão do Python (sem dependências externas):

```bash
# Auditar parametros locais do kernel Linux contra NoisyTCP
python3 http2_kernel_dos_checker.py --kernel

# Auditar servidor remoto (negociacao ALPN h2, TLS e vazamento de versao)
python3 http2_kernel_dos_checker.py --host seu-servidor.com --port 443

# Executar auditoria em modo JSON para esteiras de CI/CD
python3 http2_kernel_dos_checker.py --host seu-servidor.com --json
```

## Controles Arquiteturais e Mitigações Defensivas

A ferramenta gera configurações recomendadas para os principais componentes de infraestrutura:

* **Kernel Linux (/etc/sysctl.conf):** Parametrização de `net.ipv4.tcp_syncookies = 1`, desativação de `net.ipv4.tcp_timestamps = 0` para conter explorações NoisyTCP, calibração de buffers de socket (`tcp_rmem`, `tcp_wmem`) e `somaxconn = 4096`.
* **Nginx (nginx.conf):** Supressão de banners (`server_tokens off`), limite de streams concorrentes (`http2_max_concurrent_streams 100`) e zonas de contenção de taxa (`limit_req_zone`) para mitigar abusos de `RST_STREAM`.
* **Apache HTTP Server (httpd.conf):** Ocultação de assinaturas (`ServerTokens Prod`, `ServerSignature Off`), restrição de multiplexação via `mod_http2` (`H2MaxSessionStreams 100`) e controle de timeouts contra ataques lentos com `mod_reqtimeout`.
* **Perímetro de Borda:** Terminação TLS gerenciada com regras WAF e isolamento de origem contra tráfego direto.

## Portais Oficiais

* **Portal Corporativo TAVARDT Portugal:** [https://ag.tavardt.com/pt/](https://ag.tavardt.com/pt/)
* **Serviços de Cibersegurança:** [https://ag.tavardt.com/pt/ciberseguranca/](https://ag.tavardt.com/pt/ciberseguranca/)
* **Hub de Ferramentas Defensivas:** [https://tools.tavardt.com/pt/](https://tools.tavardt.com/pt/)
* **Organização GitHub:** [https://github.com/TAVARDT-Portugal](https://github.com/TAVARDT-Portugal)

## Licença

Distribuído sob a licença MIT. Consulte o arquivo `LICENSE` para mais informações.
