# GTM Health Dashboard 🩺

Um dashboard interativo desenvolvido em Python com Streamlit para realizar auditorias técnicas e monitorar a saúde de contêineres do Google Tag Manager (GTM). Esta ferramenta foi desenhada para analistas e engenheiros de dados que buscam garantir a performance de rastreamento sem comprometer a velocidade do site (Core Web Vitals).

---

⚠️ **Nota de Transparência:** Este repositório funciona como um **Estudo de Caso Técnico**. O código-fonte original é mantido em repositório privado por questões de segurança e proteção de dados proprietários. O objetivo aqui é compartilhar a metodologia, a arquitetura de dados e a stack tecnológica utilizada na solução.

---

## 🚀 Funcionalidades Principais

- **Métricas de Performance:** Cálculo de saúde baseado no peso do script (KB) e complexidade de execução.
- **Inventário Automatizado:** Extração completa de Tags, Triggers e Variáveis da versão Live (Produção).
- **Detecção de Débito Técnico:** Identificação automática de "Tags Órfãs" (sem acionadores vinculados).
- **Análise de Tendências:** Gráficos interativos para acompanhar o crescimento do contêiner ao longo do tempo.
- **Exportação Multi-formato:** Geração de relatórios cruzados em CSV e Excel (.xlsx).

## 🛠️ Stack Tecnológica

- **Linguagem:** Python 3.x
- **Interface:** Streamlit (Web App Framework)
- **Data Stack:** Pandas (Tratamento de dados) e Plotly (Visualizações interativas)
- **Integração:** Google Tag Manager API v2
- **Segurança:** OAuth 2.0 com protocolo PKCE

## 📋 Arquitetura e Autenticação

A aplicação utiliza o fluxo **OAuth 2.0 Web Server**, permitindo acesso centralizado a todas as contas GTM vinculadas ao e-mail do usuário de forma segura, sem armazenamento de credenciais locais.

### Fluxo de Configuração (Resumo)
1. **Google Cloud Console:** Ativação da GTM API e configuração da Tela de Consentimento.
2. **Credenciais:** Criação de ID do cliente OAuth tipo "Web Application".
3. **Gestão de Segredos:** Implementação via `secrets.toml` no Streamlit para proteção de variáveis de ambiente.

---
*Desenvolvido com foco em eficiência analítica e governança de dados.*