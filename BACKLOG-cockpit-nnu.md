# Backlog & Decisões — Cockpit de Portfólio NNU

Documento de referência para guardar e consultar quando precisar mexer. Cobre: histórico de versões, decisões travadas, modelo de dados, onde mudar cada coisa no código (`cockpit-urbanismo.html` / `index.html`), e backlog do que ainda não foi feito.

---

## 1. Histórico de versões

- **V1** — dashboard simples de carteira.
- **V2** — Stage-Gate até o lançamento; IPPE/IPPL; ritos por natureza do item; aguardando órgão × pendência interna; SLA por órgão semeado da EAP R22; escopo originação→lançamento; EAP como referência (não substituída).
- **V3** — linguagem executiva (saiu "nossa bola"); tipografia Aptos/Segoe (sem fonte externa, 100% offline); carteira renomeada (IGM, IG1A, IPGO); modais próprios no lugar de prompt/alert/confirm nativos; manual embutido; prioridade manual na carteira; zerar fase/frente; registrar atualização; migração de nomes antigos.
- **V4** — status da frente (ativa/monitorada/arquivada/obras) com KPIs contando ativas; "início de obra" → "Lançado · interface com Obras"; item especial cai na fase certa (faseId); dossiê editável; KPI "Pacotes sem exigência registrada"; observações viram histórico (append-only); botões de status com texto; impressão executiva; três níveis de acesso (view/edit/master) com senhas e presets por URL.
- **V4 + esteira** — aba "Produto & Lançamento" com IPPL em destaque; esteira comercial enriquecida (mix de produto, aprovação de produto, tabela de preços, material de vendas, estratégia/data de lançamento); chip de lançamento nos cards. Esteira corre em paralelo, **não** entra no avanço regulatório.
- **Deploy** — tela de senha na entrada (GATE); leitura de `dados.json` publicado; botão "Publicar (dados.json)"; pacote para GitHub Pages.

---

## 2. Decisões travadas (não reverter sem motivo)

- **Avanço = marcos ponderados (EVM-lite):** Σ(peso×%entregável)÷Σpeso. Crédito por passo: concluído 1, elaboração .5, aguardando órgão .6, exigência .4, não iniciado 0. Sem EVM completo (sem baseline PV/EV/AC).
- **Stage-Gate:** 14 gates reais do R22, escopo originação→lançamento (obra/TVO/entrega fora).
- **Ritos:** regulatório (Material técnico→Protocolo→Análise no órgão→Aprovação), estudo (Elaboração→Revisão→Aprovação interna), simples (1 passo).
- **Aguardando órgão (externa, com SLA) ≠ pendência interna (da equipe).** Passou do SLA = atraso externo.
- **Não aplicável** sai do denominador. **Bloqueador absoluto** trava o gate (IPPE "travado").
- **IPPE** (prontidão do próximo gate) é a régua primária; **IPPL** (lançamento) é a segunda régua, paralela, não soma ao avanço regulatório.
- **Operação da semana** não entra no cálculo. Indicadores = os 5 KPIs oficiais do RASCI REV13.
- **Uma tela só**, linguagem executiva, tipografia Aptos/Segoe sem fonte externa, **modais próprios** (nunca prompt/alert/confirm nativos — o sandbox bloqueia).
- **Senha = trava operacional, não segurança.** Acesso real exige backend.

---

## 3. Modelo de dados

```
frente     = { id, nome, codigo, tipo, estagio, status:'ativa|monitorada|obras|arquivada',
               prioridade, ultAt, por, statusNota, dossie, dossieData,
               riscos{dim:'g|y|r'}, metas{faseId:data}, semana[], fases[] }
fase       = { id, nome, gate, trilha:'reg'|'produto', itens[] }
entregavel = { id, nome, dono, peso, rito:'reg|estudo|simples', orgao, abs, opc,
               trilha, aplicavel, blq{motivo,desde}|null, historico[], passos[] }
passo      = { nome, dono, kind:'n'|'o', status:'nao|exec|aguarda|exig|ok', data, orgao, sla, desde, rodada }
acaoSemana = { id, titulo, resp, prazo, prio, status, alerta, vinc }
historico  = { data, autor, texto }
```

---

## 4. Onde mudar cada coisa (mapa do código)

Tudo em um arquivo (`index.html` / `cockpit-urbanismo.html`), no `<script>`.

| Quero mudar… | Onde | Como |
|---|---|---|
| **Senhas** de acesso | `const PWD={...}` | edite os valores (view/edit/master) |
| **Pedir senha ao abrir** (liga/desliga) | `const GATE=true` | `true` pede senha; `false` abre direto |
| **Arquivo de dados publicado** | `const DADOS_URL="dados.json"` | nome do JSON ao lado do index |
| **SLA por órgão** | `const SLA={...}` | dias por órgão |
| **Gates, fases, entregáveis, pesos, donos, ritos** | `const TEMPLATE=[...]` | é a espinha dorsal; cada item tem peso/rito/órgão/abs/opc |
| **Esteira de produto/lançamento** | fases `prod` e `lanc` dentro do TEMPLATE | itens comerciais e pesos |
| **Biblioteca de itens especiais** | `const ESPECIAIS=[...]` | cada um tem `faseId` (fase de destino) |
| **Carteira inicial (exemplo)** | `function seed()` | as 12 frentes e seus estados |
| **Crédito parcial por passo** | `const CR={...}` | pesos .5/.6/.4 etc |
| **Cor/tema** | blocos `[data-theme=...]` no `<style>` | variáveis CSS |
| **Textos do manual** | `function manualHTML()` | conteúdo do botão Manual |
| **Migração de dados antigos** | `function migrate()` | renome/normalização ao carregar |

**Versionar dados:** `const STORE="opus_pmo_v3"`. Se mudar a estrutura de forma incompatível, suba para `v4` e ajuste `migrate()`.

---

## 5. Permissões por nível

- **Visualização (123):** navega, abre frentes, exporta, imprime. Não altera.
- **Edição simples (456):** status, datas, pendências, observações (histórico), operação da semana, registrar atualização, dossiê.
- **Master (opus890):** tudo acima + nova frente, prioridade na carteira, status da frente, item especial, zerar fase/frente, importar, restaurar, **Publicar (dados.json)**.

Presets por URL: `?modo=view | edit | master` (pula a tela de senha).

---

## 6. Backlog — ainda não feito / ideias

- **Backend real (multiusuário, ao vivo, log de auditoria):** Google Sheets + Apps Script, Firebase ou Supabase. É o que transforma "snapshot publicado" em painel compartilhado com papéis garantidos.
- **Data de lançamento como meta/gate** com SLA próprio na esteira de produto.
- **IPPL como KPI na home** (ex.: "Frentes prontas para lançar").
- **Edição em massa / herança de status** para reduzir cliques (12 frentes, 1 pessoa).
- **Exibir arquivadas** sob um toggle na home (hoje saem da carteira e dos cálculos).
- **Acessibilidade** (role/tabindex/aria nos cards) e ajuste fino de impressão.
- **Calibração** de pesos de gate e SLAs com base no histórico real.
- **Integração** futura com EAP R22 / Looker Studio via export JSON.

---

## 7. Arquivos do projeto

- `cockpit-urbanismo.html` — fonte de trabalho (master).
- `cockpit-site/index.html` + `cockpit-site/dados.json` + `cockpit-site/README.md` — pacote para GitHub Pages.
- `cockpit-urbanismo-codigo.txt` — cópia do código em texto para revisão por outra LLM.
- `briefing-revisao-v4.md` — contexto e decisões para revisão.
- `manual-cockpit-nnu.md` — manual avulso.
- `BACKLOG-cockpit-nnu.md` — este documento.
