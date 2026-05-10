# Documentação Técnica: GTM Health Dashboard

## 1. Visão Geral
O **GTM Health Dashboard** resolve o problema da "caixa-preta" em implementações complexas de Web Analytics. Em ambientes com centenas de tags, a falta de visibilidade técnica pode levar à degradação da performance do site e perda de integridade dos dados. Esta aplicação automatiza a auditoria, transformando requisições de API brutas em insights acionáveis.

---

## 2. Engenharia de Analytics e Lógica de Negócio

### Auditoria da Versão Live
O sistema consome o endpoint `accounts.containers.versions.live`, garantindo que o analista visualize exatamente o que o usuário final está carregando no navegador no momento da consulta.

### Lógica de Performance (Heurística)
Para estimar o impacto técnico, a aplicação aplica pesos aos elementos do contêiner:
- **Tamanho do Script (KB):** Calculado via serialização JSON da versão completa. O sistema dispara alertas críticos se $size > 200KB$.
- **Complexidade de Execução (ms):** Calculada através da fórmula heurística:
  $$Complexity = (Tags \times 1.5) + (Variables \times 0.5)$$
  *Nota: Atribuí peso maior às Tags devido à execução de scripts externos (Third-party scripts), enquanto Variáveis representam apenas consultas de memória.*

---

## 3. Arquitetura de Software e Segurança



### Fluxo de Autenticação Segura
A aplicação implementa o protocolo **PKCE (Proof Key for Code Exchange)**. Essa camada extra de segurança é vital para aplicações Python que rodam em nuvem, pois impede ataques de interceptação de código de autorização através de um verificador de código gerado dinamicamente em cada sessão do Streamlit.

### Stack de Bibliotecas
* **Streamlit:** Gestão de estado de sessão (session_state) e interface reativa.
* **Google API Python Client:** Abstração da camada de transporte para requisições REST à API do Google.
* **Pandas:** Motor de transformação para converter dicionários JSON aninhados em DataFrames relacionais.
* **Plotly Express:** Renderização de visualizações baseadas em JSON para garantir interatividade (hover/zoom) superior a bibliotecas estáticas.
* **XlsxWriter:** Engine de serialização para garantir compatibilidade total na exportação de planilhas complexas com múltiplas abas.

---

### 4. Configuração da Infraestrutura (GCP)

Para garantir a viabilidade e segurança da aplicação, foi estruturado um ambiente no **Google Cloud Console** seguindo as diretrizes de governança de dados:

* **Habilitação de APIs:** Ativação da *Google Tag Manager API v2* para permitir o consumo programático dos metadados dos contêineres.
* **Configuração de Consentimento:** Estruturação da tela de OAuth focada no escopo `tagmanager.readonly`. Isso garante o **Princípio do Privilégio Mínimo**, assegurando que a aplicação tenha acesso apenas à leitura das configurações, sem permissões de escrita ou alteração nos ambientes dos clientes.
* **Gestão de Credenciais:** Configuração de IDs de cliente OAuth específicos para ambiente Web (Streamlit Cloud), com gerenciamento rigoroso de **URIs de redirecionamento** para prevenir vulnerabilidades de segurança e garantir que o token retorne apenas para o domínio oficial da aplicação.

---

## 5. Governança e Segurança da Informação

A aplicação foi desenhada sob o princípio de **Zero-Trust Storage**:
1.  **Sem Hardcoding:** Nenhuma ID de cliente ou conta é fixada no código.
2.  **Stateless:** O token de acesso reside apenas na memória volátil da sessão do usuário.
3.  **Secrets Management:** Uso de criptografia em repouso para as chaves do cliente através do Streamlit Secrets Cloud.
4.  **Git Governance:** Uso rigoroso de `.gitignore` para prevenir o vazamento acidental de arquivos de configuração e credenciais de teste.