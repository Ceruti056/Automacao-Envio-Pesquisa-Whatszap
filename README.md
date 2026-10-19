# 📊 Pesquisa de Satisfação - Pacientes

<div align="center">

![n8n](https://img.shields.io/badge/n8n-Workflow-FF6D5A?style=flat-square&logo=n8n&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containers-2496ED?style=flat-square&logo=docker&logoColor=white)
![Coolify](https://img.shields.io/badge/Coolify-VPS-000000?style=flat-square&logo=coolify&logoColor=white)
![Evolution API](https://img.shields.io/badge/Evolution--API-WhatsApp-25D366?style=flat-square&logo=whatsapp&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)

_Sistema de automação para busca, filtragem, distribuição temporal e envio de pesquisas de satisfação pós-consulta via WhatsApp._

</div>

---

## 📖 Sobre o Projeto

O **Pesquisa de Satisfação - Pacientes** é uma solução automatizada criada para otimizar a coleta de feedbacks de atendimento clínico e hospitalar. O sistema consome dados de consultas finalizadas (ex.: via AmigoApp API), realiza a normalização e sanitização dos dados dos pacientes, sorteia horários inteligentes para envio de mensagens com intervalos humanizados e dispara os links de formulário via **Evolution API** (WhatsApp).

O projeto conta ainda com um subfluxo receptor de eventos (Webhooks), garantindo a auditoria do status de entrega e leitura das mensagens (`SERVER_ACK`, `DELIVERY_ACK`, `READ`, `ERROR`), prevenindo reenvios duplicados e permitindo retentativas automatizadas.

---

## ✨ Principais Funcionalidades

- 🔍 **Busca Automática de Consultas**: Integração com APIs médicas para capturar atendimentos com status _Finalizado_ / _Concluído_.
- 🛡️ **Anti-Reenvio & Modo Teste**: Controle interno para evitar múltiplos disparos para o mesmo atendimento e suporte a execução em ambiente de testes com números controlados.
- 🎲 **Distribuição Temporal Humanizada**: Sorteio randômico de horários (janelas personalizáveis, ex.: 09h às 18h) e intervalos de segurança entre envios para evitar bloqueios no WhatsApp.
- 💬 **Variabilidade de Mensagens**: Sorteio dinâmico entre múltiplos modelos de texto (sem repetição consecutiva), aumentando a engajamento do paciente.
- 📥 **Receptor de Status em Tempo Real**: Captura eventos via Webhook da Evolution API (`MESSAGES_UPDATE`) para validar se a mensagem foi efetivamente entregue e lida.
- 🔄 **Loop de Verificação e Retentativas**: Validação de confirmação de envio e rotinas configuráveis de _retry_ em caso de falhas temporárias.

---

## 🛠️ Tecnologias Utilizadas

- **[n8n](https://n8n.io/)**: Orquestrador de automação e integração de fluxos.
- **[Evolution API](https://github.com/EvolutionAPI/evolution-api)**: API para integração e envio de mensagens pelo WhatsApp.
- **[Docker](https://www.docker.com/)**: Conteinerização de toda a infraestrutura (n8n e Evolution API).
- **[Coolify](https://coolify.io/) / VPS**: Plataforma auto-hospedada para deploy e gerenciamento dos contêineres Docker em servidor virtual privado.

---

## 🚀 Pré-requisitos e Instalação

### Pré-requisitos

- Servidor **VPS** com Docker e **Coolify** instalados.
- Instância ativa do **n8n**.
- Instância conectada da **Evolution API** com QR Code pareado.
- Token de acesso à API de agendamentos/consultas (ex.: AmigoApp).

### Instalação dos Workflows no n8n

1. Clone o repositório em sua máquina:
   ```bash
   git clone [https://github.com/seu-usuario/pesquisa-satisfacao-n8n.git](https://github.com/seu-usuario/pesquisa-satisfacao-n8n.git)
   cd pesquisa-satisfacao-n8n
   ```

---

## 💡 Como Usar

### Configuração do Fluxo Principal

Acesse o nó **Config** no fluxo principal do n8n e ajuste os parâmetros conforme a sua necessidade:

```json
{
  "evolutionUrl": "[https://seu-evolution.dominio.com](https://seu-evolution.dominio.com)",
  "instanceName": "SUA_INSTANCIA",
  "evolutionApiKey": "SUA_API_KEY",
  "linkFormulario": "[https://forms.gle/exemplo](https://forms.gle/exemplo)",
  "horaInicio": 9,
  "horaFim": 18,
  "diasMax": 2,
  "intervaloMinSeg": 120,
  "intervaloMaxSeg": 420,
  "modoTeste": false,
  "numeroTeste": "5527999999999"
}
```

### Execução

- **Agendada**: O trigger `Agendamento diário` executa automaticamente a rotina todos os dias às 08:00 (ou no horário parametrizado).
- **Manual**: Clique em **Execute Workflow** no nó `Execução manual` para homologar o funcionamento em tempo real.
