# Backlog & Decisões — Cockpit de Portfólio NNU

Documento de referência para guardar e consultar quando precisar mexer. Cobre: histórico de versões, decisões travadas, modelo de dados, onde mudar cada coisa no código (`cockpit-urbanismo.html` / `index.html`) e backlog do que ainda não foi feito.

---

## 1. Histórico de versões

- **V1** — dashboard simples de carteira.
- **V2** — Stage-Gate até o lançamento; IPPE/IPPL; ritos por natureza do item; aguardando órgão × pendência interna; SLA por órgão semeado da EAP R22; escopo originação→lançamento; EAP como referência (não substituída).
- **V3** — linguagem executiva (saiu "nossa bola"); tipografia Aptos/Segoe (sem fonte externa, 100% offline); carteira renomeada (IGM, IG1A, IPGO); modais próprios no lugar de prompt/alert/confirm nativos; manual embutido; prioridade manual na carteira; zerar fase/frente; registrar atualização; migração de nomes antigos.
- **V4** — status da frente (ativa/monitorada/arquivada/obras); item especial cai na fase certa (faseId); dossiê editável; observações viram histórico (append-only); botões de status com texto; impressão executiva; três níveis de acesso (view/edit/master) com senhas e presets por URL.
- **V4 + esteira** — aba "Produto & Lançamento" com IPPL em destaque; esteira comercial enriquecida. Esteira corre em paralelo, **não** entra no avanço regulatório.
- **Deploy** — tela de senha na entrada (GATE); leitura de `dados.json` publicado; botão "Publicar (dados.json)"; pacote para GitHub Pages.
- **Lote 1** — home enxuta: removidos KPI "Gates em risco" e painel "Indicadores oficiais NNU"; "Prioridades da semana" virou **lista manual** (frentes numeradas pela prioridade). Esteira de Produto: removido "Definição de produto"; separados Paisagismo/Obras civis e Imagens/Cenas/Planta humanizada. **Excluir frente** (master, confirmação) e destaque no botão "‹ Carteira".
- **Lote 2** — reestruturação do TEMPLATE para **14 fases regulatórias** (+2 de produto): split **SAA/SES/RDE conceituais**; macro **Área Institucional** (pedir→receber→destinação→termo→garantia) após o "de acordo"; **Licença de Instalação** reestruturada (PRAD, recomposição florística, supressões de travessias/EEE/CR, estimativa de valor de obra, item LI com passos próprios, **Condicionantes** com texto livre); macro **Aprovar projetos complementares** (TER/PAV/GAL/SAA/SES/RDE/ASV); **Decreto** reestruturado (GERAAP, Caução, Termo de Compromisso, Minuta Decreto, Decreto). Capacidade de base: **passos/ritos customizados por entregável** (`steps`) e **campo de texto livre** (`livre`). Exemplo marca fases por **id** (não índice). `STORE` → `v4`.
- **Lote 3** — módulo **Projetos Especiais** abaixo da carteira: **Contrapartidas (Município)** com 9 passos genéricos (criar/renomear/remover; add/remover passos; % de avanço) e **Diversos** (acompanhamento sem %, status "onde estamos" + notas). Backup/`dados.json` viraram pacote único `{frentes, esp}` (export/import/publicar/boot).
- **Lote 4** — nova **matriz de Riscos** por dimensão: **Probabilidade × Impacto 3×3** com **nível calculado** (Baixo/Médio/Alto/Crítico), **Tendência** (↑→↓), **Ação de mitigação** que cria/vincula tarefa na Operação da semana, e **"Aceitável p/ avançar o gate?"** amarrado ao IPPE/saúde (Não = trava/criticidade). Sem pós-mitigação.
- **Lote 5** — **% manual por entregável** (override do cálculo, 0–100, em branco = automático); **renomear entregável**; **pendência interna com responsável (pessoa)** visível em card/Resumo/etiqueta; **tema escuro em verde** (Opus). Confirmado que o **item especial** já tem CRUD completo (criar do zero / fase / posição / reordenar / renomear / remover). Adicionados (e depois ajustados): **mapa de calor de riscos** e **sugestão automática de nível** a partir das pendências, e **migração que preserva dados** (carimbo `_tv` + `rebuildFrente`). **Mapa de calor retirado da home** a pedido — a avaliação de risco fica só dentro de cada frente (aba Riscos).

---

## 2. Decisões travadas (não reverter sem motivo)

- **Avanço = marcos ponderados (EVM-lite):** Σ(peso×%entregável)÷Σpeso. Crédito por passo: concluído 1, elaboração .5, aguardando órgão .6, exigência .4, não iniciado 0. Sem EVM completo.
- **Stage-Gate:** 14 fases/gates regulatórios (originação→lançamento; obra/TVO/entrega fora) + 2 de produto. Ordem reg: orig, viab, fund, dir, conc, ante, amb, prot, anal, **inst (Área Institucional)**, li, **aprov (complementares)**, dec, reg.
- **Ritos:** regulatório (Material técnico→Protocolo→Análise no órgão→Aprovação), estudo (Elaboração→Revisão→Aprovação interna), simples (1 passo) **e passos customizados** por entregável (`steps`).
- **Aguardando órgão (externa, com SLA) ≠ pendência interna (da equipe, com responsável).** Passou do SLA = atraso externo.
- **Não aplicável** sai do denominador. **Bloqueador absoluto** trava o gate (IPPE "travado"). **Risco "não aceitável p/ gate"** também trava/criticiza.
- **IPPE** (prontidão do próximo gate) é a régua primária; **IPPL** (lançamento) é a segunda régua, paralela, não soma ao avanço regulatório.
- **Operação da semana** não entra no cálculo.
- **Uma tela só**, linguagem executiva, tipografia Aptos/Segoe sem fonte externa, **modais próprios** (nunca prompt/alert/confirm nativos — o sandbox bloqueia).
- **Senha = trava operacional, não segurança.** Acesso real exige backend.
- **Risco = Prob × Impacto 3×3**, nível calculado, sem pós-mitigação; mitigação puxa da Operação da semana.

---

## 3. Modelo de dados

```
frente     = { id, nome, codigo, tipo, estagio, status:'ativa|monitorada|obras|arquivada',
               prioridade, ultAt, por, statusNota, dossie, dossieData,
               riscos{dim:{prob,impacto,tend,acaoId,aceitavel}}, metas{faseId:data},
               semana[], fases[], _tv }
fase       = { id, nome, gate, trilha:'reg'|'produto', itens[] }
entregavel = { id, nome, dono, peso, rito:'reg|estudo|simples', orgao, abs, opc,
               livre, texto, pctManual(null=auto), esp(itens especiais),
               trilha, aplicavel, blq{motivo,resp,desde}|null, historico[], passos[] }
passo      = { nome, dono, kind:'n'|'o', status:'nao|exec|aguarda|exig|ok', data, orgao, sla, desde, rodada }
acaoSemana = { id, titulo, resp, prazo, prio, status, alerta, vinc }
historico  = { data, autor, texto }

esp (Projetos Especiais, fora das frentes, chave própria):
  { contrapartidas:[{id, nome, passos:[{nome,status:'nao|exec|ok'}]}],
    diversos:[{id, nome, status, notas:[{data,autor,texto}]}] }
snapshot   = { frentes:[...], esp:{...} }   // export / import / dados.json / publicar
```

Níveis de risco (matriz Prob×Impacto): alta×{baixo→Médio, médio→Alto, alto→Crítico}; média×{baixo→Baixo, médio→Médio, alto→Alto}; baixa×{baixo→Baixo, médio→Baixo, alto→Médio}.

---

## 4. Onde mudar cada coisa (mapa do código)

Tudo em um arquivo (`index.html` / `cockpit-urbanismo.html`), no `<script>`.

| Quero mudar… | Onde | Como |
|---|---|---|
| **Senhas** de acesso | `const PWD={...}` | edite view/edit/master |
| **Pedir senha ao abrir** | `const GATE=true` | `true` pede senha; `false` abre direto |
| **Arquivo de dados publicado** | `const DADOS_URL="dados.json"` | nome do JSON ao lado do index |
| **SLA por órgão** | `const SLA={...}` | dias por órgão |
| **Gates, fases, entregáveis, pesos, donos, ritos** | `const TEMPLATE=[...]` | espinha dorsal; cada item tem peso/rito/órgão/abs/opc/`steps`/`livre` |
| **Conjuntos de passos reutilizáveis** | `RITO_REGI / RITO_COMPL / RITO_CAUCAO / RITO_PEAE` | listas de passos customizados |
| **Esteira de produto/lançamento** | fases `prod` e `lanc` no TEMPLATE | itens comerciais e pesos |
| **Biblioteca de itens especiais** | `const ESPECIAIS=[...]` | cada um com `faseId` |
| **Carteira inicial (exemplo)** | `function seed()` | as 12 frentes (`through` por id da fase) |
| **Projetos Especiais (exemplo)** | `CONTRA_PASSOS / CONTRA_SEED / DIV_SEED` + `seedEsp()` | contrapartidas, passos e diversos |
| **Matriz de risco (níveis)** | `const NIVEL={...}` + `NIVLBL/NIVCOL` | regra Prob×Impacto e cores |
| **Sugestão automática de risco** | `DIM_ORGAOS` + `sugereRisco()` | mapeia órgão→dimensão |
| **Crédito parcial por passo** | `const CR={...}` | pesos .5/.6/.4 etc |
| **Cor/tema (claro navy / escuro verde)** | blocos `[data-theme=...]` no `<style>` | variáveis CSS |
| **Textos do manual** | `function manualHTML()` | conteúdo do botão Manual |
| **Migração que preserva dados** | `rebuildFrente()` + `copyItemState()` + `migrate()` | reconstrói a frente contra o TEMPLATE preservando o preenchido |

**Versionar dados:** `const STORE="opus_pmo_v4"` (localStorage das frentes), `ESP_STORE="opus_pmo_esp_v1"` (Projetos Especiais). **Mudou a estrutura do TEMPLATE? Basta subir `const TEMPLATE_VERSION` (carimbo `_tv`)** — ao abrir, `rebuildFrente()` migra os dados salvos sem precisar "Restaurar". Só suba `STORE` se a incompatibilidade for além do que o rebuild cobre.

---

## 5. Permissões por nível

- **Visualização (123):** navega, abre frentes, exporta, imprime. Não altera.
- **Edição simples (456):** status, datas, pendências (com responsável), histórico, operação da semana, registrar atualização, dossiê, % manual, renomear entregável, riscos (prob/impacto/tendência/aceitável/mitigação/sugestão), Projetos Especiais (status de passos, notas).
- **Master (opus890):** tudo acima + nova frente, prioridade na carteira, status/excluir frente, item especial (criar/posição/reordenar/remover), criar/renomear/remover contrapartidas e diversos (e passos), zerar fase/frente, importar, restaurar, **Publicar (dados.json)**.

Presets por URL: `?modo=view | edit | master` (pula a tela de senha).

---

## 6. Backlog — ainda não feito / ideias

- **Backend real (multiusuário, ao vivo, log de auditoria):** Google Sheets + Apps Script, Firebase ou Supabase. É o que transforma "snapshot publicado" em painel compartilhado com papéis garantidos.
- **Mapa de calor de riscos** — implementado em `renderHeatmap()`, **retirado da home** a pedido. Pode voltar dentro de uma visão dedicada se quiser, sem poluir a tela principal.
- **Data de lançamento como meta/gate** com SLA próprio na esteira de produto; **IPPL como KPI na home**.
- **Edição em massa / herança de status** para reduzir cliques (12 frentes, 1 pessoa).
- **Exibir arquivadas** sob um toggle na home.
- **Sugestão de risco mais fina** (calibrar mapeamento órgão→dimensão e limiares) e **posição arbitrária** do item especial (hoje início/fim + ↑/↓).
- **Acessibilidade** (role/tabindex/aria) e ajuste fino de impressão.
- **Calibração** de pesos de gate e SLAs com base no histórico real; integração futura com EAP R22 / Looker Studio via export JSON.

---

## 7. Arquivos do projeto

- `cockpit-urbanismo.html` — fonte de trabalho (master).
- `cockpit-site/index.html` + `cockpit-site/dados.json` + `cockpit-site/README.md` — pacote para GitHub Pages.
- `cockpit-urbanismo-codigo.txt` — cópia do código em texto para revisão por outra LLM.
- `briefing-revisao-v4.md` — contexto e decisões para revisão.
- `manual-cockpit-nnu.md` — manual avulso.
- `BACKLOG-cockpit-nnu.md` — este documento.
