# Backlog & Decisões — Cockpit de Portfólio NNU

Documento de referência para guardar e consultar quando precisar mexer. Cobre: histórico de versões, decisões travadas, modelo de dados, pontuação atual, onde mudar cada coisa no código (`cockpit-urbanismo.html` / `index.html`) e backlog do que ainda não foi feito.

---

## 1. Histórico de versões

- **V1–V4 + esteira** — base do Stage-Gate até o lançamento; IPPE/IPPL; ritos por natureza do item; aguardando órgão × pendência interna; SLA por órgão; linguagem executiva; tipografia Aptos/Segoe (offline); modais próprios; três níveis de acesso (view/edit/master) com senhas e presets por URL; aba "Produto & Lançamento" (IPPL paralelo).
- **Deploy** — tela de senha (GATE); leitura de `dados.json` publicado; botão "Publicar (dados.json)"; pacote para GitHub Pages.
- **Lote 1** — home enxuta (sem KPI "Gates em risco" e sem painel "Indicadores oficiais NNU"); "Prioridades da semana" virou lista manual (frentes numeradas); Produto: removido "Definição de produto", separados Paisagismo/Obras civis e Imagens/Cenas/Planta; excluir frente (master); destaque no botão "‹ Carteira".
- **Lote 2** — TEMPLATE reestruturado para **14 fases regulatórias** (+2 de produto): split SAA/SES/RDE conceituais; macro Área Institucional; Licença de Instalação reestruturada (com passos próprios e campo de texto livre nas Condicionantes); macro Aprovar projetos complementares; Decreto reestruturado. Base: **passos/ritos customizados por entregável** (`steps`) e **campo livre** (`livre`). Marcação de fases por **id** (não índice).
- **Lote 3** — módulo **Projetos Especiais** (Contrapartidas com 9 passos genéricos + Diversos de acompanhamento). Backup/`dados.json` viraram pacote único `{frentes, esp}`.
- **Lote 4** — **matriz de Riscos** 3×3 (Prob × Impacto → nível calculado, tendência, ação ligada à Operação da semana, "aceitável p/ gate" amarrado ao IPPE/saúde). Sem pós-mitigação.
- **Lote 5** — **% manual por entregável e por fase** (override do cálculo); **renomear entregável**; **pendência com responsável (pessoa)** visível em card/Resumo; **tema escuro verde** (Opus); item especial com CRUD completo confirmado; **migração que preserva dados** (carimbo `_tv` + `rebuildFrente`); mapa de calor de riscos e sugestão automática de nível (mapa de calor depois retirado da home — a avaliação fica só dentro da frente, aba Riscos).
- **Correções pós-lotes** — bug da **data** (ano "0026" → normaliza para 20xx); **% editável no cabeçalho da fase**; **fase de peso 0 não trava o gate** (passa direto).
- **Recalibração de pesos** — total dos pesos regulatórios reescalado e calibrado por fase (ver seção 4). **Anteprojeto → "Projeto Executivo"**; **OOAU** renomeado para "OOAU — Outorga Onerosa de Alteração de Uso" (peso 8, opcional).
- **Reestruturação Ambiental/LI** — Laudos separados em três (vegetação abaixo do RAS, engenharia, geologia); **IPHAN** virou grupo (FCA, TR, PAIPA, RAIPA); "Licença Prévia (LP) + ASV" → "Licença Prévia (LP)"; **ASV** movido para a última linha da Licença de Instalação; **biblioteca de itens especiais** reativada no "+ Item especial" (puxar item pronto ou criar do zero).
- **Evolução automática semanal** — cada frente guarda `snapshots` semanais (rótulo "Sem DD/MM" pela segunda-feira). O registro é **automático**: a semana corrente é regravada com o avanço calculado a cada edição (e ao abrir o painel); ao virar a semana, a anterior congela. Painel "Evolução do projeto" no Resumo com sparkline + variação (+pts · +%). Link "registrar período manual…" só para preencher histórico; registros manuais não são sobrescritos pelo automático.
- **Tipografia moderna (offline)** — pilha de sistema (Segoe UI Variable no Windows 11 / San Francisco no Mac), tamanho/entrelinha/espaçamento calibrados. Sem fonte externa.
- **"Registrar atualização" → "Anotação no projeto"** — edições já salvam e atualizam o "atualizado há…" sozinhas; o botão virou opcional, só para nota narrada no histórico.
- **Comparar frentes** — botão na barra de topo (leitura, todos os níveis) abrindo dois gráficos em **SVG na mão (offline)**: (1) **Ranking de avanço** — barra por frente, ordenada, colorida pela saúde; eixo inferior com as **14 fases numeradas** posicionadas pelo peso acumulado (fases iniciais espremidas à esquerda) + **linha vertical da média da carteira**; clicar no nome abre a frente; legenda numerada das fases embaixo. (2) **Comparativo por fase (radar)** — teia de **7 eixos** (Originação, Diretrizes & Outorgas, Ambiental & Complementares, Análise & "de acordo", Licença de Instalação, Aprovar projetos complementares, Decreto), cada eixo = % de conclusão da fase, comparando 2 a 4 frentes escolhidas nos chips.
- **Departamento responsável** — a lista de departamentos (NNU, DPRO - EDU, DPRO - H, LEG, CONS, DTU, OBRA, MKT, COM, JUR, PROJ EXT, FIN, CTB; salva em `opus_pmo_depts_v1`) fica **só no "Registrar pendência interna"**, onde se escolhe o departamento responsável (+ pessoa, opcional), mostrado na tag da pendência e no painel "Pendências internas". Os seletores por entregável/passo foram **retirados das linhas** (ficavam extensas) — o passo volta a mostrar a sigla do responsável como rótulo simples. Master gerencia a lista pelo botão **"Departamentos"** na barra de topo, que **adiciona e remove** (não só acrescenta). Siglas antigas fora da lista são preservadas.
- **Análise Seplanh em 3 linhas** — CHEADV, Cartografia, GERAAP (antes uma linha só), mantendo o absoluto "Seplanh de acordo".
- **Matrícula / SPE / Contrato de parceria movidos para a Originação** (deixaram a Base Fundiária). O peso foi junto: Originação passou a 9 e Base Fundiária a 4, total 195 mantido.
- **Risco "Aprovação"** — sexta dimensão da matriz de riscos (além de Ambiental, Fundiário, Jurídico, Infraestrutura, Comercial).
- **SLA repensado (data prevista + reprogramar + log)** — informa-se Protocolado em + SLA esperado (dias); o sistema mostra a **data prevista** (protocolo + SLA) e "faltam Xd / vencido há Xd". Ao vencer, botão **Reprogramar** empurra a previsão e registra no histórico ("Reprogramado (órgão): previsto DD/MM → DD/MM — motivo"), com contador "reprog. Nx". Início (protocolo) e conclusão são logados automaticamente. Correção do **bug do ano** (datas com ano de 2 dígitos, ex.: "0026", normalizadas para 20xx no cálculo e na exibição, inclusive em dados já salvos). Migração agora preserva sla/previsto/reprog/rodada dos passos.

- **Preenchimento rápido + responsáveis dos ritos (TEMPLATE 2026.07.4)** — na linha do entregável, os botões 25/50/75/100/auto deram lugar a **um único botão "Concluído"** (marca 100%; clicar de novo volta ao automático pelos passos). No cabeçalho da fase, quando ela está travada manualmente, aparece a tag **"manual X% · trava a fase"** (modo edição), para não esconder que o override anula os itens. Nos passos de rito, **Elaboração do material técnico** segue o dono do entregável e **Protocolo** segue LEG, mas **Aprovação, Revisão e Aprovação interna** ficam **sem responsável** (normalização por nome do passo, vale para todos os ritos). **Imagens, Cenas, Planta humanizada e Maquete** passaram de MKT para **DPRO - EDU** (Book/ficha e Material de vendas seguem MKT).

- **Identidade do editor + nº do processo (TEMPLATE 2026.07.5)** — a senha de **edição** passou a ser `URB` (a de master segue `opus890`, a de leitura `123`). Ao entrar como editor, o sistema pergunta **"Quem está atualizando?"** e a pessoa escolhe o responsável (lista padrão: DPRO - EDU, LEG, DTU, OBR, CONS) — esse nome carimba o "atualizado por" de cada alteração e aparece no botão de acesso. O **master** gerencia essa lista pelo botão **"Editores"** (adiciona/remove), salvo em `opus_pmo_editores_v1`. E cada entregável ganhou campo **"Nº do processo / protocolo no órgão"** (livre, em qualquer item — ex.: AVT, AVTO, LP), editável na visão expandida e exibido como tag "proc. …" na linha.

- **Nº do processo no topo, datas 2024–2035 e master edita/remove comentários** — o campo **"Nº do processo / protocolo no órgão"** foi para o **topo do item expandido** (quadro destacado), em qualquer entregável (AVT, AVTO, LP...). Todos os campos de **data** passaram a ter faixa **2024–2035** (`min`/`max`), para o calendário abrir em 2026 e não cair em 2002. O **master** ganhou **editar** e **remover** em cada comentário do histórico — tanto nos **entregáveis** quanto nas **anotações dos Projetos Especiais (Diversos)**. Comentários novos passam a registrar quem escreveu (DPRO-EDU, LEG, etc., conforme o editor logado).

---

## 2. Decisões travadas (não reverter sem motivo)

- **Avanço = marcos ponderados (EVM-lite):** Σ(peso×%entregável)÷Σpeso. Crédito por passo: concluído 1, elaboração .5, aguardando órgão .6, exigência .4, não iniciado 0. Sem EVM completo.
- **Stage-Gate:** 14 fases/gates regulatórios (originação→registro) + 2 de produto (paralelo). Ordem reg: orig, viab, fund, dir, conc, **Projeto Executivo**, amb, prot, anal, inst, li, aprov, dec, reg.
- **Pontuação calibrada por fase** — total regulatório ≈ **195 pontos** (ver seção 4). Mudou por decisão de negócio; itens absolutos sempre são o item mais pesado da fase.
- **Ritos:** regulatório, estudo, simples **e passos customizados** (`steps`) por entregável.
- **Aguardando órgão (externa, com SLA) ≠ pendência interna (da equipe, com responsável).** Passou do SLA = atraso externo.
- **Não aplicável** sai do denominador. **Bloqueador absoluto** trava o gate. **Risco "não aceitável p/ gate"** trava/criticiza. **Fase de peso 0** conta como concluída (gate passa direto).
- **IPPE** (prontidão do próximo gate) é a régua primária; **IPPL** (lançamento) é a segunda régua, paralela, não soma ao avanço regulatório.
- **Uma tela só**, linguagem executiva, sem fonte externa, **modais próprios** (nunca prompt/alert/confirm nativos).
- **Senha = trava operacional, não segurança.** Acesso real exige backend.

---

## 3. Modelo de dados

```
frente     = { id, nome, codigo, tipo, estagio, status:'ativa|monitorada|obras|arquivada',
               prioridade, ultAt, por, statusNota, dossie, dossieData,
               riscos{dim:{prob,impacto,tend,acaoId,aceitavel}}, metas{faseId:data},
               semana[], snapshots[], fases[], _tv }
snapshot   = { key('YYYY-MM-DD' da segunda), rotulo('Sem DD/MM'), pts, total, pct, data, auto|manual }
fase       = { id, nome, gate, trilha:'reg'|'produto', pctManual(null=auto), itens[] }
entregavel = { id, nome, dono, peso, rito:'reg|estudo|simples', orgao, abs, opc,
               livre, texto, pctManual(null=auto), esp, steps(custom),
               trilha, aplicavel, blq{motivo,resp,desde}|null, historico[], passos[] }
passo      = { nome, dono(departamento), kind:'n'|'o', status:'nao|exec|aguarda|exig|ok', data, orgao, sla(dias esperados), desde(protocolo), previsto(data; só se reprogramado), reprog(nº), rodada }
riscos     = { dim:{prob,impacto,tend,acaoId,aceitavel} } — dimensões: Ambiental, Fundiário, Jurídico, Infraestrutura, Comercial, Aprovação
departamentos = lista editável (master cria) em localStorage `opus_pmo_depts_v1`
esp        = { contrapartidas:[{id,nome,passos:[{nome,status}]}],
               diversos:[{id,nome,status,notas:[{data,autor,texto}]}] }
snapshot   = { frentes:[...], esp:{...} }   // export / import / dados.json / publicar
```

Níveis de risco (Prob×Impacto): alta×{baixo→Médio, médio→Alto, alto→Crítico}; média×{baixo→Baixo, médio→Médio, alto→Alto}; baixa×{baixo→Baixo, médio→Baixo, alto→Médio}.

---

## 4. Pontuação atual (pesos por fase — base ≈ 195)

| Fase | Peso | % do projeto |
|---|---|---|
| Originação | 9 | 4,6% |
| Viabilidade | 5 | 2,6% |
| Base Fundiária | 4 | 2,1% |
| Diretrizes & Outorgas | 16 | 8,2% |
| Conceito Urbanístico | 16 | 8,2% |
| Projeto Executivo | 10 | 5,1% |
| Ambiental & Complementares | 16 | 8,2% |
| Protocolo Seplanh | 8 | 4,1% |
| Análise & "de acordo" | 20 | 10,3% |
| Área Institucional | 16 | 8,2% |
| Licença de Instalação | 21 | 10,8% |
| Aprovar projetos complementares | 18 | 9,2% |
| Decreto | 18 | 9,2% |
| Registro | 18 | 9,2% |

Dentro de cada fase o item **absoluto** é o mais pesado (ex.: "de acordo" 12/20; CRI 14/18; Decreto de aprovação 9/18; LI 10/21; LP 4/16). Itens **opcionais** (OOAU, GOINFRA, IPHAN, transferência/remembramento) só pesam onde a frente os tem; nas demais, marque "não aplicável" para tirá-los do denominador. A esteira de Produto/Lançamento (≈45 pontos) **não** entra nesse total nem no avanço regulatório.

---

## 5. Onde mudar cada coisa (mapa do código)

| Quero mudar… | Onde | Como |
|---|---|---|
| **Senhas** | `const PWD={...}` | view/edit/master |
| **Pedir senha ao abrir** | `const GATE=true` | `false` abre direto |
| **Arquivo de dados publicado** | `const DADOS_URL="dados.json"` | nome do JSON ao lado do index |
| **SLA por órgão** | `const SLA={...}` | dias por órgão |
| **Gates, fases, entregáveis, PESOS, donos, ritos** | `const TEMPLATE=[...]` | é a espinha dorsal; cada item tem peso/rito/órgão/abs/opc/`steps`/`livre` |
| **Passos reutilizáveis** | `RITO_REGI / RITO_COMPL / RITO_CAUCAO / RITO_PEAE` | listas de passos |
| **Biblioteca de itens especiais** | `const ESPECIAIS=[...]` | aparece no atalho "Puxar item pronto"; cada um com órgão/peso/faseId |
| **Carteira inicial (exemplo)** | `function seed()` | as 12 frentes (`through` por id da fase) |
| **Comparar frentes (gráficos)** | `cmpBars` (ranking + eixo de fases + média), `cmpRadar` + `RADAR_FASES` (radar por fase), `renderCompare`/`openCompare`, `phaseCum`, paleta `cmpColors` | tudo SVG na mão; cores via `style="fill:var(--x)"` |
| **Evolução semanal (snapshots)** | `autoSnap`/`autoSnapAll` (captura automática), `weekKey/weekLabel`, `evolucaoHTML` (sparkline), `snapFoto` (manual) | semana = segunda-feira; `autoSnap` roda no `touch()` e no boot |
| **Projetos Especiais (exemplo)** | `CONTRA_PASSOS / CONTRA_SEED / DIV_SEED` + `seedEsp()` | |
| **Matriz de risco (níveis)** | `const NIVEL={...}` + `NIVLBL/NIVCOL` | regra Prob×Impacto e cores |
| **Dimensões de risco** | `const RISK_DIMS=[...]` (+ `DIM_ORGAOS`) | hoje 6: Ambiental/Fundiário/Jurídico/Infraestrutura/Comercial/Aprovação |
| **Departamentos** | `DEPT_DEFAULT` + `loadDepts/saveDepts` + `deptOpts/onDeptChange/addDept` | lista editável; master cria; persiste em `opus_pmo_depts_v1` |
| **SLA / data prevista / reprogramar** | `previstoDate()`/`addDays()`, `reprogramar()`, `aguardando()`, `setSt()` (log início/fim via `logHist`) | data prevista = protocolo + SLA; reprogramar empurra e loga; ano<100 corrigido em `dias()`/`brdate()` |
| **Sugestão automática de risco** | `DIM_ORGAOS` + `sugereRisco()` | mapeia órgão→dimensão |
| **Crédito parcial por passo** | `const CR={...}` | .5/.6/.4 etc |
| **Tema (claro navy / escuro verde)** | blocos `[data-theme=...]` no `<style>` | variáveis CSS |
| **Migração que PRESERVA dados** | `rebuildFrente()` + `copyItemState()` + `migrate()` | reconstrói contra o TEMPLATE preservando o preenchido |

**Mudou a estrutura/pesos do TEMPLATE?** Suba `const TEMPLATE_VERSION` (carimbo `_tv`). Ao abrir, `rebuildFrente()` migra os dados salvos preservando status/% manual/pendências/itens especiais — **sem precisar "Restaurar"**. `STORE="opus_pmo_v4"` só muda em incompatibilidade que o rebuild não cubra.

---

## 6. Permissões por nível

- **Visualização (123):** navega, exporta, imprime. Não altera.
- **Edição simples (456):** status, datas, pendências (com responsável), histórico, operação da semana, registrar atualização, dossiê, % manual (entregável e fase), renomear entregável, riscos, Projetos Especiais.
- **Master (opus890):** tudo acima + nova frente, prioridade, status/excluir frente, item especial (biblioteca ou do zero, fase/posição/reordenar/remover), criar/renomear/remover contrapartidas e diversos, zerar fase/frente, importar, restaurar, **Publicar (dados.json)**.

Presets por URL: `?modo=view | edit | master`.

---

## 7. Backlog — ainda não feito / ideias

- **Refinamentos de UX (inspirados no Planear, ainda não feitos):** selo "Salvo · agora" no cabeçalho (feedback de persistência sempre visível); **feed de atividade por frente** consolidando os históricos num só lugar; **estados vazios honestos** ("não preenchido" ≠ "0%"). A tipografia desse conjunto já foi feita.
- **Backend real (multiusuário, ao vivo, log de auditoria):** Google Sheets + Apps Script, Firebase ou Supabase. É o que transforma "snapshot publicado" em painel compartilhado com papéis garantidos.
- **Mapa de calor de riscos** — existe em `renderHeatmap()`, retirado da home a pedido; pode voltar numa visão dedicada.
- **Data de lançamento como meta/gate** com SLA próprio; **IPPL como KPI na home**.
- **Edição em massa / herança de status** para reduzir cliques.
- **Exibir arquivadas** sob um toggle na home.
- **Sugestão de risco mais fina** (calibrar órgão→dimensão e limiares); **posição arbitrária** do item especial.
- **Acessibilidade** (role/tabindex/aria) e ajuste fino de impressão.
- **Calibração contínua** de pesos e SLAs com base no histórico real.

---

## 8. Arquivos do projeto

- `cockpit-urbanismo.html` — fonte de trabalho (master).
- `cockpit-site/index.html` + `cockpit-site/dados.json` + `cockpit-site/README.md` (+ este backlog) — pacote para GitHub.
- `cockpit-urbanismo-codigo.txt` — cópia do código em texto para revisão por outra LLM.
- `manual-cockpit-nnu.md` — manual avulso.
- `BACKLOG-cockpit-nnu.md` — este documento.
