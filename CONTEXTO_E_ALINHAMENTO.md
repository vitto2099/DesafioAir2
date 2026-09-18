# Histórico, Contexto e Alinhamento do Projeto
**Desafio do Mês 2 - Air Company AI Fellowship**  
*Data do Alinhamento:* 16 de Setembro de 2026  
*Status:* Preparado e pronto para execução no Sábado  

---

## 1. O Ponto de Partida e o Objetivo

O desafio consiste em construir e avaliar um agente inteligente completo de ponta a ponta na AWS, dividido em 7 requisitos principais:
1. **Construção do Agente no AgentCore:** Instruções claras, papel, limites, ferramenta real e conversa multi-turno com memória de sessão.
2. **Sessão Exploratória (60 a 90 min):** Identificação de alucinações, recusas indevidas e falhas de ferramenta.
3. **Golden Dataset:** Mínimo de 15 casos cobrindo 5 categorias (consulta direta, tarefa com ferramenta, multi-turno, fora de escopo e adversarial).
4. **Avaliação em Duas Frentes:**
   - *Frente A (AgentCore Evals):* 2 avaliadores nativos + 1 avaliador customizado em código.
   - *Frente B (DeepEval via Pytest):* Answer Relevancy (≥ 0.7), Faithfulness (≥ 0.8) e G-Eval (≥ 0.8).
5. **Campanha de Red Teaming:** 15 tentativas estruturadas em 4+ categorias de ataque com tabela de achados e severidade.
6. **Análise e Correção:** Blindagem do agente, reexecução das avaliações e comparativo *Baseline × Final*.
7. **Relatório Final (4 a 6 páginas) + Apresentação (6 min no Demo Day).**

---

## 2. As Diretrizes Inegociáveis do Orientador (Jacques)

Todas as orientações transmitidas pelo Jacques foram registradas e servirão de guia absoluto para a execução:
* **Modelos Econômicos:** Usar apenas modelos de baixo custo (por token). O Claude padrão do AgentCore deve ser trocado por opções baratas como **Amazon Nova Lite / Nova Micro**, **Llama 3.1 8B** ou modelos da família Google/Gemma.
* **100% Serverless:** **NÃO** utilizar máquinas provisionadas (Provisioned Throughput / PTU). Se algum dia for necessário, deve ser levado previamente para reunião com o Jacques.
* **Região AWS:** Operar preferencialmente em `us-east-2` (Ohio) ou `us-east-1` (N. Virginia). O ambiente atual está configurado em **us-east-2**.
* **Segurança Financeira:** Jamais cadastrar dados pessoais de cartão de crédito na conta de estudos.
* **Ferramenta Nativa:** Em vez de configurar APIs externas ou navegadores pesados, usar o **Code Interpreter** nativo do AgentCore.
* **Memória de Sessão:** Validar a manutenção de contexto entre turnos dentro da mesma sessão (`Session ID`).
* **Higiene e Exclusão de Recursos:** Garantir que nenhum recurso pago ou microVM fique ativo sem necessidade após o uso.
* **Suporte:** Jacques tem acesso às contas de alunos caso surja algum bloqueio inesperado.

---

## 3. A Escolha do Domínio: "PC Descomplicado"

Durante nossa conversa, avaliamos três caminhos:
1. *E-commerce tradicional de eletrônicos:* Funcional, mas genérico.
2. *Pokémon (Calculadora de Dano e Batalhas Competitivas):* Excelente pelo domínio prévio do Vitor com projetos como `pokedex` e `pokeapi`.
3. **"PC Descomplicado" (A Opção Vencedora):**
   - **Conceito:** Um consultor amigável e didático focado em ajudar **pessoas iniciantes e leigas** a montar o PC Gamer ideal sem cair na "sopa de letrinhas" técnicas (TDP, PCIe, AM5, DDR5, gargalo) e sem medo de queimar dinheiro.
   - **Por que é a escolha perfeita?**
     - O **Code Interpreter** brilha ao fazer os cálculos chatos para o iniciante: soma de preços de peças, cálculo de consumo elétrico (TDP) da fonte com margem de segurança e cálculo de troco/orçamento.
     - As regras de negócio são nítidas: proteger o usuário contra fontes perigosas sem marca ("fontes bomba") e contra peças fisicamente incompatíveis (ex: processador AM5 em placa-mãe AM4).
     - O case para o Demo Day é comercialmente valioso e fácil de explicar em 6 minutos.

---

## 4. O Roteiro de Execução de Sábado (Estimativa: 2h a 3h)

| Bloco | Tempo Estimado | O que faremos |
| :--- | :--- | :--- |
| **Etapa 1** | ~30 min | Abrir o **AgentCore** no console da AWS (`us-east-2`), colar o prompt baseline do `agent/system_prompt_baseline.txt`, selecionar o modelo econômico (Nova Lite/Micro ou Llama) e ativar o **Code Interpreter**. |
| **Etapa 2** | ~30 min | Realizar os primeiros testes no playground simulando um cliente leigo (sessão exploratória) e validar os 15 casos do `dataset/golden_dataset.json`. |
| **Etapa 3** | ~30 min | Executar o avaliador customizado em código (`custom_evaluator.py`) e disparar a suíte do DeepEval (`test_agent_evals.py`) com as 3 métricas mínimas. |
| **Etapa 4** | ~30 min | Rodar a campanha de **Red Teaming com os 15 ataques** documentados no `red_team_plan.md` (tentativas de injeção, indução de fontes bomba e quebra de regras). |
| **Etapa 5** | ~30 min | Aplicar a versão corrigida do prompt (Final com Guardrails), retestar o Baseline vs Final e gerar o **Relatório Final (4 a 6 páginas)** e os slides da apresentação. |

---

## 5. Índice de Arquivos Já Criados e Salvos

Todos os arquivos estão persistidos no disco local em `Desafio Air 2/`:
* 📄 [`README.md`](README.md) - Visão geral do projeto e checklist rápido.
* 🤖 [`agent/system_prompt_baseline.txt`](agent/system_prompt_baseline.txt) - Prompt inicial do assistente para colar na AWS.
* 📊 [`dataset/golden_dataset.json`](dataset/golden_dataset.json) - 15 casos de teste nas 5 categorias obrigatórias.
* 🛡️ [`red_teaming/red_team_plan.md`](red_teaming/red_team_plan.md) - Matriz completa dos 15 ataques estruturados com critérios de severidade.
* 🧪 [`evals/agentcore/custom_evaluator.py`](evals/agentcore/custom_evaluator.py) - Código Python do avaliador customizado de regras de hardware.
* 📈 [`evals/deepeval/test_agent_evals.py`](evals/deepeval/test_agent_evals.py) - Script com Relevancy, Faithfulness e G-Eval.

---
*Pronto para retomar no sábado a qualquer momento!*
