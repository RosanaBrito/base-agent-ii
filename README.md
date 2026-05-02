# Base Agent II - Agente de Triagem Médica com LangGraph

Este projeto foi desenvolvido a partir do workshop **Imersão Tech – Agentes de IA com Python**, promovido pela [WomakersCode](https://github.com/womakerscode).

## Visão geral

Este projeto implementa um agente de triagem médica capaz de:
- interpretar sintomas informados pelo paciente;
- consultar um protocolo de triagem externo;
- decidir entre agendar consulta ou chamar emergência;
- manter contexto de conversa com memória;
- operar com ferramentas controladas por regras de segurança.

## Objetivo

O objetivo foi transformar um notebook do Google Colab em um projeto local organizado no VS Code, executado pelo terminal Ubuntu e preparado para versionamento no GitHub.

## Arquitetura do agente

O agente foi construído com LangGraph, organizando o fluxo em nós e arestas condicionais.

### Fluxo implementado

1. O usuário envia uma mensagem com sintomas.
2. O agente injeta o `system_prompt` com regras e persona.
3. O LLM decide se precisa chamar uma tool.
4. O nó de tools executa a ferramenta solicitada.
5. O fluxo retorna ao agente para avaliar o resultado.
6. O agente produz a resposta final ao paciente.

## Tecnologias utilizadas

| Tecnologia | Papel no projeto |
|---|---|
| Python | Linguagem principal |
| LangGraph | Orquestração do fluxo do agente |
| LangChain | Integração com tools e mensagens |
| Gemini API | Modelo usado como cérebro do agente |
| Tavily | Ferramenta preparada para expansão |
| python-dotenv | Leitura de variáveis de ambiente |
| VS Code | Desenvolvimento local |
| Git/GitHub | Versionamento e publicação |

## Persona do agente

A persona foi definida no `system_prompt` como um assistente de triagem do Hospital ABCD, com tom proativo, resolutivo e profissional.

### Responsabilidades da persona
- classificar o caso clínico;
- seguir o protocolo;
- não mudar de papel;
- manter linguagem profissional.

## Tools implementadas

### 1. `consultar_protocolo_triagem()`
Lê o protocolo salvo em arquivo `.txt` e retorna as regras de triagem.

### 2. `agendar_consulta(especialidade, urgencia, data_preferencial)`
Executa o agendamento de consulta para casos não críticos.

### 3. `chamar_emergencia(tipo_emergencia, endereco)`
Aciona a emergência local para casos classificados como vermelhos.

## Grounding com arquivo local

O projeto usa uma forma simples de grounding: o agente consulta um arquivo externo chamado `protocolo_triagem.txt` antes de agir.

## Guardrails aplicados

O agente possui guardrails definidos diretamente no `system_prompt`.

### Regras de segurança
- não dar conselhos médicos;
- não fornecer diagnóstico;
- não recomendar remédios;
- não responder fora do escopo de triagem;
- bloquear tentativas de prompt injection ou pedidos sensíveis.

### Guardrails operacionais
- consultar o protocolo antes de agir;
- só chamar emergência em casos graves;
- só agendar consulta em casos permitidos pelo protocolo.

## Estrutura do projeto

```bash
base-agent-ii/
├── .env.example
├── .gitignore
├── README.md
├── protocolo_triagem.txt
├── requirements.txt
├── notebooks/
└── src/
    └── base_agent_ii.py
```

## Como executar localmente

### 1. Criar e ativar ambiente virtual

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Instalar dependências

```bash
pip install -r requirements.txt
```

### 3. Criar arquivo `.env`

```env
GOOGLE_API_KEY=sua_chave_aqui
GEMINI_API_KEY=sua_chave_aqui
TAVILY_API_KEY=sua_chave_aqui
```

### 4. Executar o agente

```bash
python src/base_agent_ii.py
```

## Exemplo de execução

Entrada:
```text
Estou com febre alta há dois dias, muita tontura e vômito. Moro na Rua das Flores, 123.
```

Saída esperada:
```text
A emergência foi acionada e uma ambulância está a caminho do endereço Rua das Flores, 123. Por favor, aguarde o atendimento no local.
```

## Aprendizados do projeto

- migração de notebook do Google Colab para projeto local em Python;
- uso de `.env` para gerenciamento de chaves;
- organização de um agente com LangGraph;
- uso de tools para ação estruturada;
- separação entre persona, regras e execução;
- aplicação de guardrails e grounding com documento externo.

## Melhorias futuras

- adicionar interface web com Streamlit ou FastAPI;
- substituir arquivo `.txt` por base vetorial;
- incluir avaliação automática do comportamento do agente;
- registrar logs das decisões;
- integrar com banco de dados e agenda real.
