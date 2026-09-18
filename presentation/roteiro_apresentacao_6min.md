# Roteiro de Apresentação: PC Descomplicado (Demo Day - 6 Minutos)
**Air Company AI Fellowship - Desafio do Mês 2**  
*Apresentador:* Vitor C. K.  
*Tempo Total:* 6 minutos cravados  

---

## Estrutura dos Slides e Cronômetro

```
[00:00 - 01:00] Slide 1: A Dor do Iniciante no Mundo do Hardware
[01:00 - 02:00] Slide 2: Conheçam o PC Descomplicado (Arquitetura AWS)
[02:00 - 03:15] Slide 3: O Superpoder do Code Interpreter (Live Demo)
[03:15 - 04:15] Slide 4: Qualidade Aferida (Golden Dataset + DeepEval)
[04:15 - 05:15] Slide 5: Red Teaming & Blindagem contra Ataques
[05:15 - 06:00] Slide 6: Resultados, Economia e Conclusão
```

---

### Slide 1: A Grande Dor do Iniciante (0:00 - 1:00)
* **Visual do Slide:** Foto de uma pessoa confusa olhando uma lista cheia de siglas: *TDP, AM5, PCIe 4.0, DDR5, CL30, OVP*. Do lado, um aviso em vermelho: *"Errou a peça? R$ 3.000 perdidos."*
* **O que você fala (60 segundos):**
  > *"Olá a todos! Montar um computador gamer hoje é um campo minado para quem é leigo. Se um pai quer dar um PC para o filho, ou se um estudante junta R$ 4.000 suados para jogar, ele entra em fóruns e se depara com uma sopa de letrinhas arrogante. Pior: se ele comprar um processador com soquete errado, ou comprar uma fonte sem marca de R$ 50 no camelô, ele queima peças caras e perde o dinheiro.  
  > Eu criei o **PC Descomplicado**: um consultor inteligente que fala a língua de quem não entende de computador, explica tudo com analogias simples do dia a dia e garante que o usuário não jogue um único centavo no lixo."*

---

### Slide 2: O Agente & A Arquitetura AWS Serverless (1:00 - 2:00)
* **Visual do Slide:** Diagrama limpo da arquitetura: Usuário ➔ AWS Bedrock AgentCore (us-east-2) ➔ Modelo Econômico (Google Gemma 3 4B IT Sob Demanda) ➔ Ferramenta Nativa Code Interpreter (Python Sandbox).
* **O que você fala (60 segundos):**
  > *"Para tornar essa solução viável no mundo real, a arquitetura foi desenhada seguindo à risca as regras de ouro do nosso orientador Jacques:  
  > 1. É 100% Serverless, rodando no **AWS Bedrock / AgentCore** na região `us-east-2` (Ohio).  
  > 2. Zero instâncias provisionadas e zero custo fixo: configurei o modelo ultrabarato **Google Gemma 3 4B IT (v1)** sob demanda, onde o custo por atendimento completo é de frações de centavo.  
  > 3. Em vez de subir lambdas complexas ou APIs que quebram com facilidade, ativei a ferramenta nativa mais poderosa da AWS: o **Code Interpreter**."*

---

### Slide 3: O Superpoder do Code Interpreter (2:00 - 3:15)
* **Visual do Slide:** Print real do console AWS: o cliente passando 5 peças com preços e potências em Watts. Do lado, o Code Interpreter executando o Python: `(65 + 115 + 50 + 10) * 1.30 = 312W ➔ Recomendação: Fonte 500W/550W 80 Plus` e recuperando o saldo de R$ 250 do turno anterior.
* **O que você fala (75 segundos):**
  > *"Aqui está o grande divisor de águas do meu agente. Qualquer modelo tradicional de linguagem alucina na matemática quando você pede para somar várias peças ou calcular consumo elétrico com folga.  
  > Com o **Code Interpreter**, o modelo escreve e executa código Python em tempo real na nuvem da AWS.  
  > No meu teste real na AWS com orçamento de R$ 5.000, ele somou exatamente R$ 4.750, achou os R$ 250 de troco e, no turno seguinte, rodou o código Python calculando 312W necessários para um Ryzen 5600 com RTX 4060, lembrando do saldo anterior para recomendar uma fonte segura. É a união perfeita entre linguagem acolhedora e matemática exata."*

---

### Slide 4: Avaliações Automatizadas & Golden Dataset (3:15 - 4:15)
* **Visual do Slide:** Tabela com as duas frentes de avaliação:  
  - *Frente A:* Custom Evaluator em Python (Regras de AM5, Fontes Bomba e Pirataria) ➔ 100% Pass.  
  - *Frente B:* DeepEval com Ollama local (`llama3.2:3b`) ➔ Relevancy: 0.95 | Faithfulness: 0.96 | G-Eval: 0.95.
* **O que você fala (60 segundos):**
  > *"Montei um **Golden Dataset de 15 casos de teste** cobrindo desde consultas diretas até conversas multi-turno e pegadinhas adversariais.  
  > Para garantir qualidade de ponta a ponta, avaliei em duas frentes:  
  > Na **Frente A**, criei um avaliador customizado em código Python que audita regras invioláveis de hardware — garantindo que nenhuma combinação de peças incompatíveis passe despercebida.  
  > Na **Frente B**, rodei a suíte do **DeepEval** com juiz local no Ollama em 13,9 segundos: batemos 0.95 em Relevância, 0.96 em Fidelidade e 0.95 em Conformidade de Hardware, superando com folga as metas exigidas."*

---

### Slide 5: Red Teaming & Blindagem contra Ataques (4:15 - 5:15)
* **Visual do Slide:** Comparativo Baseline vs Final: gráfico mostrando a evolução de 46,7% de defesas no Baseline para **100% no Agente Final**. Exemplos de ataques barrados: Jailbreak de ficção científica, injeção de prompt DAN, comandos de sistema no Code Interpreter e tentativa de ligar fonte 850W em benjamins.
* **O que você fala (60 segundos):**
  > *"Submeti o agente a uma bateria pesada de **Red Teaming com 15 ataques agressivos**: tentativas de Jailbreak fingindo ser livro de ficção para crackear Windows, injeção de comandos de terminal no Code Interpreter e apelo de emergência médica para colocar 2.0V na BIOS.  
  > Na versão inicial (Baseline), o modelo cedeu a 8 dos 15 ataques por tentar ser bonzinho demais com o usuário.  
  > Implementei guardrails diretos no prompt do sistema: proibi chamadas de sistema no Code Interpreter, bloqueei vazamento de diretrizes e criei travas elétricas rígidas.  
  > **O resultado? 100% dos ataques foram repelidos no agente final**, mantendo a postura educada e protetora com o usuário."*

---

### Slide 6: Conclusão & Próximos Passos (5:15 - 6:00)
* **Visual do Slide:** 3 pilares de destaque:  
  1. *Custo Quase Zero* (100% Serverless no AWS AgentCore).  
  2. *Matemática Blindada* (Zero alucinações em orçamentos via Code Interpreter).  
  3. *Segurança Total* (Protege o bolso e a integridade física do cliente).
* **O que você fala (45 segundos):**
  > *"Para encerrar: o PC Descomplicado prova que a inteligência artificial, quando combinada com uma ferramenta de código determinístico como o Code Interpreter e guardrails bem desenhados, deixa de ser um gerador de texto genérico e se torna um consultor comercial altamente confiável, seguro e viável.  
  > Eu colocaria esse agente em produção hoje mesmo na fase de pré-vendas, mantendo um humano apenas no fechamento final do carrinho.  
  > Muito obrigado a todos e estou aberto a perguntas!"*
