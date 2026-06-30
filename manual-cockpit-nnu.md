# Manual — Cockpit de Portfólio NNU

Painel executivo da carteira do Urbanismo (NNU). Acompanha cada frente **da originação ao lançamento comercial**. Resume a EAP R22 em marcos executivos; **não substitui** a EAP nem o cronograma do projeto.

## 1. Conceitos

- **Gate** — portão de decisão entre fases (14 no total, até o Lançamento). A trilha no topo da frente mostra o gate concluído, o atual e os seguintes.
- **Avanço** — percentual por marcos ponderados: cada entregável tem um peso; o total é a soma dos pesos concluídos dividida pela soma dos pesos aplicáveis.
- **IPPE — prontidão para a próxima etapa** — o quanto falta para destravar o próximo gate. É diferente do avanço geral: uma frente pode ter avanço alto e IPPE baixo por faltar um único item crítico.
- **IPPL — prontidão para lançamento** — esteira de produto/comercial. Aparece quando há itens de produto em andamento.
- **Aguardando órgão (pendência externa)** — já protocolamos; a análise está no órgão. Conta dias contra um **SLA** por órgão; ao ultrapassá-lo, vira atraso externo.
- **Pendência interna** — responsabilidade da equipe: falta decisão, documento, contratação, pagamento de taxa ou minuta. É distinta de aguardar órgão.
- **Bloqueador absoluto** — marco que trava o gate. Enquanto pendente, o IPPE aparece como "travado".
- **Não aplicável** — item que não existe naquela frente (ex.: barragem). Sai do cálculo sem zerar o percentual; fica registrado que foi avaliado e descartado.
- **Rito** — passos internos de cada entregável. Regulatório: Material técnico → Protocolo → Análise no órgão → Aprovação. Estudo: Elaboração → Revisão → Aprovação interna. Simples: um passo.

## 2. Como usar

- **Abrir uma frente** — clique no card da carteira. Use as abas Resumo, Entregáveis, Operação da semana e Riscos.
- **Atualizar um entregável** — na aba Entregáveis, clique no item para abrir os passos e marque o estado (não iniciado / em elaboração / aguardando / exigência / concluído). O avanço recalcula sozinho.
- **Registrar pendência externa** — marque o passo "Análise no órgão" como aguardando, informe o órgão e a data de protocolo; o SLA já vem semeado do R22 e pode ser ajustado.
- **Registrar pendência interna** — no rodapé do entregável, "Registrar pendência interna" e descreva o motivo.
- **Marcar não aplicável** — no rodapé do entregável; ele sai do cálculo.
- **Item especial** — botão "+ Item especial" na aba Entregáveis (barragem, DNPM/ANM, linha de transmissão, servidão, faixa de domínio, etc.).
- **Operação da semana** — tarefas e alertas manuais com prazo e prioridade. **Não alteram** o percentual técnico; servem para registrar prioridades e cobranças.
- **Backup** — "Exportar" gera o arquivo JSON do estado; "Importar" recarrega. É o backup e a futura ponte para Google Sheets / Looker Studio.

## 3. Modo de edição e senha

O painel abre em **somente leitura**. Para alterar dados, clique em "Acessar" e informe a senha. Há três níveis:

- **Visualização** (senha `123`) — só lê.
- **Edição** (senha `URB`) — edita os itens. Ao entrar, o sistema pergunta **"Quem está atualizando?"** e você escolhe o responsável (DPRO - EDU, LEG, DTU, OBR, CONS...). Cada alteração fica registrada com esse nome no campo "atualizado por". O nome escolhido aparece no botão de acesso ("Edição (LEG)…").
- **Master** (senha `opus890`, a sua / NNU) — edita tudo e ainda gerencia listas: botão **"Editores"** (adiciona/remove quem aparece na seleção de "Quem está atualizando?") e botão **"Departamentos"**.

> **Atenção:** a senha é uma **trava operacional** — evita alterações acidentais e indica quem editou. **Não é segurança de dados.** Como o painel roda no navegador, quem tiver conhecimento técnico contorna (DevTools, localStorage). Para controle de acesso real — multiusuário, registro garantido de quem alterou o quê — a evolução é migrar para Google Sheets/Apps Script, Firebase ou Supabase. As senhas ficam no código, na constante `PWD` (atuais: `123` / `URB` / `opus890`).

## 4. Governança

- O painel resume a carteira; a fonte técnica continua sendo a **EAP R22**, a **Matriz RASCI REV13** e o **Mapa de Governança REV4**.
- Os indicadores seguem os 5 KPIs oficiais do RASCI REV13: status atualizado no mês, pendências críticas vencidas, pacotes completos na 1ª submissão, retrabalhos técnicos e dossiês atualizados.
- O escopo vai da originação ao lançamento. Após o lançamento, o acompanhamento migra para a rotina de Obras/Entrega; aqui permanece apenas como referência.
