# Hub de Hardening e Resiliencia DoS HTTP/2 e Kernel Linux

Especificacao tecnica corporativa de engenharia defensiva e blueprints de infraestrutura para mitigar vetores de saturacao HTTP/2 Rapid Reset (CVE-2023-44487) e NoisyTCP, hardening de kernel Linux (sysctl) e verificacao CLI deterministica local.

## Escopo Tecnico do Repositorio

Este repositorio fornece blueprints de hardening testados em producao e utilitario CLI deterministico:

1. **sysctl-hardening.conf:** Parametros otimizados em `/proc/sys/net/ipv4` mitigando inundacoes SYN, ataques de timestamp NoisyTCP e esgotamento de sockets orfaos.
2. **nginx-http2-hardened.conf:** Configuracao de proxy reverso NGINX blindada impondo cotas estritas de concorrencia (`http2_max_concurrent_streams 64`) e limitacao de taxa em quadros RST_STREAM.
3. **nftables-dos-protection.conf:** Regras de filtragem de pacotes na Camada 4 descartando rajadas anomalas TCP SYN e RST antes da alocacao de sockets no kernel.
4. **docker-compose-hardened.yml:** Arquitetura de conteineres isolados aplicando limites rigidos de recursos (memoria, CPU, ulimits) em proxies de borda.
5. **http2_kernel_dos_checker.py:** Utilitario CLI local deterministico executado com privilegios de sistema no host para auditar diretamente parametros `/proc/sys` e verificar conformidade RFC.

> [!NOTE]
> A inspecao ativa do estado do kernel Linux e dos parametros de rede exige privilegios de sistema e deve ser executada diretamente no servidor via utilitario CLI em Python. Paginas web em sandbox de navegador nao possuem permissao para auditar parametros do kernel hospedeiro.

## Uso do Utilitario CLI

Execute o script de auditoria deterministica diretamente no host do servidor:

```bash
# 1. Baixar o script de auditoria
curl -sO https://raw.githubusercontent.com/TAVARDT-Portugal/http2-kernel-dos-resilience-checker-pt/main/http2_kernel_dos_checker.py

# 2. Inspecionar parametros locais do kernel Linux
sudo python3 http2_kernel_dos_checker.py -kernel

# 3. Exportar resultados em formato JSON
sudo python3 http2_kernel_dos_checker.py -kernel -json
```

## Governanca e Soberania

* **Organizacao:** TAVARDT-Portugal
* **Jurisdicao:** Portugal
* **Padroes de Referencia:** RFC 7540, RFC 9113, CVE-2023-44487, Diretrizes Zero Trust

Desenvolvido pela Divisao de Ciberseguranca e Infraestrutura da **TAVARDT Portugal**. Todos os direitos reservados.
