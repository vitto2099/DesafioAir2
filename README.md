# Desafio 2 (AI Fellowship): Agente PC Descomplicado
**Aluno:** Vitor Camargo Kunicki  
**Repositório Oficial:** [github.com/vitto2099/DesafioAir2](https://github.com/vitto2099/DesafioAir2)

---

## Sobre o Projeto

O **PC Descomplicado** é um agente de inteligência artificial construído no **AWS Bedrock AgentCore** para ajudar iniciantes e pessoas leigas a escolherem peças de computador sem medo de errar na compatibilidade ou gastar dinheiro à toa.

O agente conversa usando comparações didáticas do cotidiano (a fonte é o coração, o processador é o cérebro) e utiliza o **Code Interpreter nativo da AWS** para realizar cálculos exatos em Python para orçamentos, saldo de troco e dimensionamento de Watts da fonte de alimentação com margem de segurança.

### Principais Destaques:
- **100% Serverless na AWS:** Construído no Bedrock AgentCore na região `us-east-2` (Ohio), usando o modelo sob demanda **Google Gemma 3 4B IT (v1)** com custo de centavos por atendimento (zero instâncias fixas / zero PTU).
- **Ferramenta Nativa Confirmada:** `aws_codeinterpreter_v1` executando código Python em microVM efêmera — **confirmado ao vivo na AWS** (Session `b417514c`, calculou 320W com 25% de margem em 2.388ms de latência real).
- **Memória de Sessão:** Mantém o histórico, saldo e peças escolhidas entre múltiplos turnos de conversa.
- **Golden Dataset:** 15 casos de teste cobrindo Consulta Direta, Ferramenta, Multi-Turno, Fora de Escopo e Adversarial.
- **Avaliação em Duas Frentes:** Avaliador customizado em código Python (Frente A) e suíte DeepEval com juiz local Ollama `llama3.2:3b` (Frente B) com 100% de aprovação.
- **Campanha de Red Teaming:** 15 ataques estruturados + ataques ao vivo no Bedrock real (Session `b417514c-4bb8`, 21/09/2026) — o agente evoluiu de 20,0% (3/15) de defesas no Baseline para **86,7% (13/15) na campanha estruturada** e **100% (4/4) na validação ao vivo no Bedrock**, com bloqueio total de riscos críticos (comandos de SO, risco elétrico letal e pirataria). Descoberta e documentação do caso RT-16 (impersonação de autoridade - Jeff Bezos) com correção imediata aplicada no prompt final.
- **Evidências no Console AWS:** 23 capturas de tela cobrindo configuração do Harness, sessões baseline, sessões multi-turno, execução do Code Interpreter e red teaming ao vivo.

---

## Estrutura dos Arquivos

```text
├── agent/
│   ├── system_prompt_baseline.txt      # Prompt inicial (amigável, mas ingênuo)
│   └── system_prompt_final.txt         # Prompt final blindado com guardrails e patch RT-16
├── dataset/
│   └── golden_dataset.json             # 15 casos estruturados nas 5 categorias
├── evals/
│   ├── sessao_exploratoria_charter.md  # Charter e anotações da sessão de 75 min
│   ├── agentcore/
│   │   ├── builtin_evaluators.py       # 2 avaliadores integrados (Goal Success e Tool Accuracy)
│   │   ├── custom_evaluator.py         # 1 avaliador customizado em código (Hardware Rules)
│   │   ├── agentcore_eval_config.json  # Configuração oficial dos avaliadores do AgentCore
│   │   └── run_agentcore_evals.py      # Runner consolidado da Frente A (100% aprovação)
│   ├── deepeval/
│   │   └── test_agent_evals.py         # Suíte DeepEval (Relevancy, Faithfulness, GEval)
│   └── executar_auditoria_completa.py  # Auditoria comparativa completa no console
├── red_teaming/
│   ├── red_team_plan.md                # Planejamento dos ataques em categorias
│   ├── red_team_results.md             # Matriz de achados, severidade e transcrições (inclui RT-16)
│   ├── executar_red_team.py            # Script executor da campanha em tempo real
│   └── red_team_execution_log.json     # Log consolidado com saídas e latências reais
├── demo/
│   └── roteiro_demo_6min.md            # Roteiro minuto a minuto da apresentação em vídeo
└── reports/
    ├── prints/                         # 23 capturas de tela da execução real no console da AWS
    ├── Relatorio_Executivo.md          # Versão executiva concisa (5 páginas) — ATUALIZADO 21/09/2026
    ├── Relatorio.docx                  # Relatório executivo editável no Word / Google Docs
    ├── Relatorio.pdf                   # Relatório oficial formatado em PDF
    └── Relatorio.md                    # Relatório técnico completo de referência
```

---

## Como Executar as Avaliações Localmente

### 1. Pré-requisitos
* Python 3.10+ instalado.
* Ollama instalado e rodando com o modelo `llama3.2:3b`:
  ```bash
  ollama serve
  ```

### 2. Rodar a Avaliação AgentCore (Frente A: 2 Built-in + 1 Custom)
```bash
python evals/agentcore/run_agentcore_evals.py
```

### 3. Rodar a Suíte DeepEval via Pytest (Frente B: 3 Métricas com Juiz Ollama)
```bash
python -m pytest evals/deepeval/test_agent_evals.py -v -s
```

### 4. Rodar a Campanha de Red Teaming em Tempo Real
```bash
python red_teaming/executar_red_team.py
```

### 5. Rodar a Auditoria Completa dos 15 Casos
```bash
python evals/executar_auditoria_completa.py
```

---

## Principais Resultados

* **Answer Relevancy:** 0.95 (Meta: >= 0.70) [Aprovado]
* **Faithfulness:** 0.96 (Meta: >= 0.80) [Aprovado]
* **G-Eval de Conformidade de Hardware:** 0.95 (Meta: >= 0.80) [Aprovado]
* **Red Teaming estruturado:** 13/15 (86,7% mitigados / 100% em riscos críticos) [Aprovado]
* **Red Teaming ao vivo no Bedrock (21/09/2026):** 4/4 (100% bloqueados — Sessão `b417514c-4bb8`) [Confirmado]
* **Achado Crítico RT-16 Corrigido:** Impersonação Jeff Bezos mitigada com nova regra inviolável no prompt final.
* **Code Interpreter confirmado na AWS:** Python executado ao vivo, calculou 320W em 2.388ms de latência real.
* **Tempo de Resposta Médio na AWS:** 2,5s a 4,9s nos primeiros turnos (crescendo até 33,5s no 9º turno multi-turno).
