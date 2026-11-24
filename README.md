# Hub de Hardening e Resiliência DoS HTTP/2 e Kernel Linux

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-5b3cf5.svg)](https://tavardt-portugal.github.io/http2-kernel-dos-resilience-checker-pt/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![RFC 7540](https://img.shields.io/badge/RFC-7540-blue.svg)](https://datatracker.ietf.org/doc/html/rfc7540)
[![RFC 9113](https://img.shields.io/badge/RFC-9113-blue.svg)](https://datatracker.ietf.org/doc/html/rfc9113)

**Hub de Arquitetura Defensiva e Mitigação contra DoS em HTTP/2 (Rapid Reset CVE-2023-44487) e Kernel Linux**

Desenvolvido e mantido pela divisão de engenharia de segurança em infraestrutura da **TAVARDT Portugal**.

***

## Portais Oficiais

* **Portal Institucional**: [Portal Institucional](https://ag.tavardt.com/pt/)
* **Central de Ferramentas**: [Central de Ferramentas](https://tools.tavardt.com/pt/)
* **Canal de Atendimento e Suporte Técnico**: [https://ag.tavardt.com/pt/contacto/](https://ag.tavardt.com/pt/contacto/)

***

## Demonstração Online e Hub de Arquitetura Defensiva

Você pode acessar a especificação técnica corporativa, os testes de diagnóstico arquitetural e os blueprints de mitigação diretamente pelo navegador:

* **[Aplicação Web Oficial no GitHub Pages](https://tavardt-portugal.github.io/http2-kernel-dos-resilience-checker-pt/)**
* **Central de Ferramentas TAVARDT**

> [!NOTE]
> **Escopo de Execução Técnica:** Este hub web fornece especificações de arquitetura defensiva e blueprints de configuração. Auditorias ativas de queda de kernel e resiliência TCP exigem privilégios de sistema (root) e execução diretamente no próprio servidor hospedeiro via o utilitário CLI oficial disponibilizado neste repositório (`http2_kernel_dos_checker.py`). Páginas web em sandbox de navegador não possuem permissão para auditar parâmetros internos do kernel.

***

## Blueprints Técnicos de Hardening

1. **sysctl-hardening.conf:** Parâmetros otimizados em `/proc/sys/net/ipv4` mitigando inundações SYN, ataques de timestamp NoisyTCP e esgotamento de conexões órfãs.
2. **nginx-http2-hardened.conf:** Configuração de proxy reverso NGINX blindada impondo cotas estritas de concorrência (`http2_max_concurrent_streams 64`) e limitação de taxa em quadros RST_STREAM.
3. **nftables-dos-protection.conf:** Regras de filtragem de pacotes na Camada 4 descartando rajadas anômalas TCP SYN e RST antes da alocação de sockets no kernel.
4. **docker-compose-hardened.yml:** Arquitetura de contêineres isolados aplicando limites rígidos de recursos (memória, CPU, ulimits) em proxies de borda.
5. **http2_kernel_dos_checker.py:** Utilitário CLI local determinístico executado com privilégios de sistema no host para auditar diretamente parâmetros `/proc/sys` e verificar conformidade RFC.

***

## Uso do Utilitário CLI Determinístico

Para executar a inspeção determinística diretamente no host do servidor Linux:

```bash
# 1. Baixar o script de auditoria do repositório oficial
curl -sO https://raw.githubusercontent.com/TAVARDT-Portugal/http2-kernel-dos-resilience-checker-pt/main/http2_kernel_dos_checker.py

# 2. Inspecionar parâmetros locais do kernel Linux
sudo python3 http2_kernel_dos_checker.py -kernel

# 3. Exportar diagnóstico em formato estruturado JSON
sudo python3 http2_kernel_dos_checker.py -kernel -json
```

***

© 2026 TAVARDT. Todos os direitos reservados.
