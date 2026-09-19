# Desafio 2 — AI Fellowship (Air Company)

> **Contexto:** No Mês 1 você construiu e avaliou um chatbot local. Agora o nível sobe: você vai construir um agente de verdade no **AgentCore**, avaliá-lo em duas frentes (**AgentCore Evaluations** e **DeepEval**) e, o ponto central, tentar quebrá-lo com uma campanha estruturada de **red teaming**. A ideia é aplicar tudo o que você viu nos dois meses em uma única campanha de qualidade completa.

---

## Escopo e Etapas do Desafio

### 1. Construção do Agente
Suba um agente no **AgentCore Harness**. Escolha um domínio (você pode evoluir o chatbot do mês 1 ou partir de um novo). O agente precisa ter:
- **Instruções claras:** papel, tom e regras ou limites de comportamento.
- **Pelo menos uma ferramenta real:** Code Interpreter, Browser ou uma base de conhecimento via RAG.
- **Uma conversa multi-turno:** que use o contexto da sessão.
- **Definições iniciais:** escopo, riscos (o que seria uma falha grave desse agente) e os thresholds que você vai adotar.
- **Modelo Juiz:** Configure o modelo juiz que vai usar no DeepEval e nos avaliadores customizados do AgentCore (prefira o modelo mais forte disponível, pois juízes fracos geram scores instáveis).

---

### 2. Sessão Exploratória
Explore o agente por **60 a 90 minutos** com um charter simples, anotando comportamentos suspeitos:
- Respostas inventadas;
- Promessas indevidas;
- Falhas de recusa;
- Uso incorreto da ferramenta;
- Vazamento de contexto entre turnos.  
*Essas descobertas devem orientar tanto o seu dataset quanto a sua campanha de red teaming.*

---

### 3. Golden Dataset
Monte um dataset com **no mínimo 15 casos de teste**, cobrindo as 5 categorias obrigatórias:
1. **Consulta direta:** produto, informação, dado factual.
2. **Tarefa com ferramenta:** caso que só passa se o agente acionar a ferramenta corretamente.
3. **Multi-turno:** caso que depende do contexto acumulado ao longo de vários turnos.
4. **Fora de escopo:** perguntas que o agente deve recusar educadamente.
5. **Adversarial:** tentativas de induzir alucinação ou promessa indevida.  
*Cada caso deve conter: input (ou a sequência de turnos), critério esperado e o contexto de referência quando aplicável.*

---

### 4. Avaliação em Duas Frentes
Avalie o mesmo agente com os dois ecossistemas:

- **Frente A, AgentCore Evaluations:**  
  Rode uma avaliação sobre as interações do agente com **pelo menos 2 avaliadores integrados** e **pelo menos 1 avaliador customizado** (ou baseado em código) que verifique uma regra específica do seu agente.

- **Frente B, DeepEval:**  
  Implemente as três métricas mínimas, executando via pytest (`deepeval test run` ou `pytest`):
  - **Answer Relevancy ≥ 0,7:** a resposta responde de fato à pergunta.
  - **Faithfulness ≥ 0,8:** a resposta é fiel ao contexto, sem informação inventada.
  - **G-Eval de conformidade ≥ 0,8:** uma regra do seu domínio (os critérios acompanham os materiais).

*Ao final, compare as duas frentes: o que cada uma pegou, seus pontos fortes e seus limites.*

---

### 5. Campanha de Red Teaming
*Esta é a parte central do desafio.* Rode uma campanha estruturada de red teaming contra o seu agente, usando as técnicas e ferramentas vistas no curso. Cubra no **mínimo 4 categorias de ataque**, por exemplo:
- Prompt injection (direto e via ferramenta ou conteúdo consultado);
- Jailbreak ou bypass das regras do agente;
- Vazamento de informação (system prompt, dados de outra sessão);
- Indução de conteúdo perigoso ou promessa indevida;
- Uso indevido da ferramenta.

*Documente no mínimo 15 tentativas, cada uma com: objetivo do ataque, técnica usada, resultado (o agente resistiu ou falhou) e severidade. Consolide em uma tabela de achados (vulnerabilidade, severidade, evidência).*

---

### 6. Análise e Correção
- Analise as falhas das duas avaliações e da campanha de red teaming.
- Melhore o agente onde fizer sentido: instruções ou prompt, guardrails e restrições da ferramenta.
- Reexecute as avaliações e reteste as vulnerabilidades que você encontrou.
- Compare baseline × versão final nas duas frentes (métricas e red teaming).

---

### 7. Relatório Final (4 a 6 páginas)
Documente:
1. **Planejamento:** escopo, riscos, thresholds;
2. **O agente:** arquitetura (modelo, ferramenta, memória);
3. **Dataset e técnicas de design;**
4. **Resultados da avaliação em duas frentes;**
5. **Campanha de red teaming e achados;**
6. **Análise baseline × final;**
7. **Conclusão com uma avaliação de risco:** você colocaria esse agente em produção? Por quê?

---

## Entregáveis

1. **Repositório (ou pasta):** com o dataset, a suíte DeepEval, a configuração ou código dos avaliadores do AgentCore, o log da campanha de red teaming e as instruções de execução.
2. **Relatório final (4 a 6 páginas):** em formato Markdown ou PDF consolidado.
3. **Apresentação de no máximo 6 minutos:** no demo day de encerramento.

---

## Critérios de Avaliação (100 pts)

| Item | Descrição | Pontuação |
| :--- | :--- | :---: |
| **Agente no AgentCore + Golden Dataset** | Cobertura das 5 categorias e uso das técnicas de design | **20 pts** |
| **Avaliação em Duas Frentes** | AgentCore Evaluations + DeepEval implementados e executando | **25 pts** |
| **Campanha de Red Teaming** | Mínimo 4 categorias cobertas, documentação e classificação de severidade (≥ 15 tentativas) | **30 pts** |
| **Análise e Correção** | Comparativo baseline × final nas duas frentes (métricas e red teaming) | **15 pts** |
| **Relatório e Demo** | Relatório técnico completo de 4 a 6 páginas e roteiro de demo de até 6 min | **10 pts** |
| **TOTAL** | | **100 pts** |

> **Mínimo para aprovação:** Agente rodando no AgentCore + pelo menos uma frente de avaliação funcionando + campanha de red teaming documentada com ≥ 15 tentativas + relatório entregue.
