<p align="center">
  <a href="https://ag.tavardt.com/pt/">
    <img src="https://ag.tavardt.com/img/tavardt-banner.png" alt="TAVARDT" width="1024" />
  </a>
</p>

# HTTP/2 & Linux Kernel DoS Resilience Checker

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-5b3cf5.svg)](https://tavardt-portugal.github.io/http2-kernel-dos-resilience-checker-pt/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![RFC 7540](https://img.shields.io/badge/RFC-7540-blue.svg)](https://datatracker.ietf.org/doc/html/rfc7540)
[![RFC 9113](https://img.shields.io/badge/RFC-9113-blue.svg)](https://datatracker.ietf.org/doc/html/rfc9113)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

***

Auditor automatizado de parâmetros de rede do kernel Linux, negociação de protocolo HTTP/2 (ALPN) e resiliência contra ataques de Negação de Serviço, desenvolvido pela **TAVARDT**, em estrito alinhamento com as diretrizes do CNCS, a Diretiva NIS2 e o RGPD da União Europeia.

Serviços web modernos enfrentam ameaças contínuas de negação de serviço que exploram camadas fundamentais de rede, incluindo NoisyTCP e HTTP/2 Rapid Reset. Sem calibração disciplinada de parâmetros do kernel e validação de protocolos, servidores de origem sofrem exaustão de sockets e esgotamento de threads computacionais sob cargas volumétricas assimétricas.

***

## Capacidades Centrais e Arquitetura Defensiva

* **Auditoria Determinística de Parâmetros de Kernel**: Inspeciona variáveis em `/proc/sys/net/` contra especificações formais para conter NoisyTCP, saturação de conexões e SYN Flood.
* **Verificação Remota do Protocolo HTTP/2**: Avalia a negociação ALPN sobre TLS em conexões de rede para certificar o suporte estrito à multiplexação binária `h2`.
* **Identificação de Vazamento de Metadados**: Detecta a exposição indevida de banners de software e frameworks nos cabeçalhos `Server` e `X-Powered-By`.
* **Zero Dependências Externas**: Construído exclusivamente com a biblioteca padrão do Python (`socket`, `ssl`, `json`, `os`, `sys`), garantindo execução ultraveloz sem pacotes adicionais.
* **Automação para Pipelines Contínuas**: Emite relatórios estruturados em formato JSON através da opção `-json` para auditoria integrada em rotinas de CI/CD.

***

## Vetores de Risco e Impacto Operacional

Manter servidores web expostos à internet com configurações padrão do kernel Linux fragiliza a infraestrutura contra ataques volumétricos. A ferramenta audita quatro vetores de risco essenciais:

* **NoisyTCP e Envenenamento de Sequência**: Exploração de timestamps TCP para provocar reordenação artificial de pacotes e saturar filas de processamento de rede.
* **Saturação por SYN Flood e Esgotamento de Sockets**: Ocupação completa das filas de conexões pendentes por falta de SYN cookies e limites reduzidos de backlog.
* **Degradação de Protocolo e Conexões Inseguras**: Falha na seleção de `h2` via ALPN, forçando o tráfego a retroceder para transmissões HTTP/1.1 menos eficientes.
* **Reconhecimento Automatizado via Banners**: Exposição pública de versões de servidores web em cabeçalhos HTTP, facilitando ataques dirigidos a vulnerabilidades conhecidas.

***

## Instalação e Execução CLI

Execute auditorias locais no kernel Linux ou avaliações remotas de protocolo utilizando comandos Python padronizados:

```bash
# Clonar o repositório oficial
git clone https://github.com/TAVARDT-Portugal/http2-kernel-dos-resilience-checker-pt.git

# Acessar o diretório de trabalho
cd http2-kernel-dos-resilience-checker-pt

# Auditar parâmetros locais de rede do kernel Linux
python http2_kernel_dos_checker.py -kernel

# Auditar endpoint remoto quanto a suporte HTTP/2 e banners
python http2_kernel_dos_checker.py -host example.com -port 443

# Emitir relatório estruturado em JSON para pipelines
python http2_kernel_dos_checker.py -host example.com -json
```

***

## Perguntas Técnicas Frequentes

### Como a calibração de parâmetros sysctl do kernel Linux mitiga vetores de ataque NoisyTCP?

A desativação de timestamps TCP impede a previsão maliciosa de sequências numéricas e a reordenação artificial de pacotes que sobrecarregam a pilha de rede. Simultaneamente, a ativação de SYN cookies e o aumento das filas de backlog garantem a continuidade operacional durante ataques volumétricos de conexão.

### Por que a validação da negociação ALPN é indispensável para a segurança do protocolo HTTP/2?

O mecanismo ALPN assegura que cliente e servidor estabeleçam a multiplexação de fluxos binários de modo seguro sobre TLS sem degradar para protocolos não criptografados. Confirmar a seleção de h2 atesta que controles modernos de fluxo e limites de janelas foram corretamente acordados durante o aperto de mão inicial.

### De que forma a exibição de banners Server e X-Powered-By eleva a vulnerabilidade a ataques de negação de serviço?

A divulgação de versões exatas de software possibilita que atacantes explorem falhas específicas de implementação e vulnerabilidades conhecidas de processamento no servidor. Ocultar cabeçalhos informativos priva ferramentas automáticas de reconhecimento dos dados necessários para orquestrar abusos direcionados de memória ou protocolo.

### Qual é a diferença operacional entre executar a auditoria local de kernel e a checagem remota de rede?

A auditoria local inspeciona parâmetros sysctl diretamente no sistema de arquivos proc do Linux para validar a postura defensiva do sistema operacional. Por outro lado, a auditoria remota avalia o handshake TLS, a compatibilidade ALPN e o vazamento de banners a partir da rede externa sem exigir credenciais privilegiadas de host.

### O auditor de resiliência pode ser integrado em esteiras automatizadas de integração contínua (CI/CD)?

O utilitário CLI gera saídas estruturadas através da opção -json, permitindo que esteiras de automação analisem programaticamente os índices de conformidade do kernel e os parâmetros TLS. Fluxos de DevOps podem consumir esse relatório para interromper a promoção de imagens e servidores que não atendam aos requisitos mínimos de segurança.

***

## Links Úteis

* **Portal Corporativo:** https://ag.tavardt.com/pt/
* **Suíte de Diagnósticos:** https://tools.tavardt.com/pt/
* **Contacto Técnico:** https://ag.tavardt.com/pt/contacto/

***

### Licença
Distribuído sob a Licença MIT. Consulte [LICENSE](LICENSE) para obter detalhes completos.

***

© 2026 TAVARDT. Todos os direitos reservados.
