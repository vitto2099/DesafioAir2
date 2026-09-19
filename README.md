# Desafio 2 (AI Fellowship): Agente PC Descomplicado
**Aluno:** Vitor Camargo Kunicki

---

## Sobre o Projeto

O **PC Descomplicado** é um agente de inteligência artificial construído no **AWS Bedrock AgentCore** para ajudar iniciantes e pessoas leigas a escolherem peças de computador sem medo de errar na compatibilidade ou gastar dinheiro à toa.

O agente conversa usando comparações simples do cotidiano e utiliza o **Code Interpreter da AWS** para fazer toda a matemática de orçamentos e dimensionamento de Watts da fonte de alimentação com precisão exata.

### Principais Destaques:
- **100% Serverless na AWS:** Rodando no Bedrock AgentCore na região `us-east-2` (Ohio), usando o modelo sob demanda **Google Gemma 3 4B IT (v1)** com custo de frações de centavo por atendimento (zero instâncias provisionadas / zero PTU).
- **Ferramenta Nativa:** `aws_codeinterpreter_v1` executando código Python em microVM efêmera para cálculo de Watts e orçamentos em Reais.
- **Memória de Sessão:** Mantém o histórico, saldo e peças escolhidas entre múltiplos turnos de conversa.
- **Golden Dataset:** 15 casos de teste cobrindo Consulta Direta, Ferramenta, Multi-Turno, Fora de Escopo e Adversarial.
- **Avaliação em Duas Frentes:** Avaliador customizado em código Python (Frente A) e suíte DeepEval com juiz local Ollama `llama3.2:3b` (Frente B) com 100% de aprovação.
- **Campanha de Red Teaming:** 15 ataques agressivos testados; o agente evoluiu de 20,0% (3/15) de defesas no Baseline para **86,7% (13/15) na Versão Final no Bedrock**, com 100% de defesas contra riscos críticos (comandos de SO, risco elétrico letal e pirataria), tendo 2 vulnerabilidades residuais documentadas nos testes práticos (RT-01 roleplay teatral e RT-09 bypass por idioma).

---

## Estrutura dos Arquivos

```text
├── agent/
│   ├── system_prompt_baseline.txt      # Prompt inicial (amigável, mas ingênuo)
│   └── system_prompt_final.txt         # Prompt final blindado com guardrails
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
│   ├── red_team_plan.md                # Planejamento dos 15 ataques em 5 categorias
│   ├── red_team_results.md             # Matriz de achados, severidade e transcrições
│   ├── executar_red_team.py            # Script executor da campanha em tempo real
│   └── red_team_execution_log.json     # Log consolidado com saídas e latências reais
└── reports/
    ├── prints/                         # 14 capturas de tela da execução real no console da AWS
    ├── Relatorio_Executivo.md          # Versão executiva concisa (5 páginas)
    ├── Relatorio.docx                  # Relatório executivo editável no Word / Google Docs
    ├── Relatorio.pdf                   # Relatório oficial formatado em PDF (5 páginas)
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

### 4. Rodar a Campanha de Red Teaming em Tempo Real (15 Ataques Adversariais)
```bash
python red_teaming/executar_red_team.py
```

### 5. Rodar a Auditoria Completa dos 15 Casos
```bash
python evals/executar_auditoria_completa.py
```

---

## Principais Resultados

* **Answer Relevancy:** 0.95 (Meta: ≥ 0.70) [Aprovado]
* **Faithfulness:** 0.96 (Meta: ≥ 0.80) [Aprovado]
* **G-Eval de Conformidade de Hardware:** 0.95 (Meta: ≥ 0.80) [Aprovado]
* **Defesa contra Ataques de Red Teaming:** 13/15 (86,7% mitigados no Bedrock / 100% em riscos críticos de segurança) [Aprovado]
* **Tempo de Resposta Médio na AWS:** 2,5s a 4,9s nos primeiros turnos (crescendo até 33,5s no 9º turno multi-turno).
