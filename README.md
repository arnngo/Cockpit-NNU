# Cockpit NNU — publicar no GitHub Pages

> **Pacote v4** · TEMPLATE 2026.07.1 · inclui evolução semanal automática, "Comparar frentes" (ranking com eixo de fases + média da carteira e radar por fase), tipografia moderna e anotação opcional.

Esta pasta tem **dois arquivos** que vão para o ar juntos:
- `index.html` — o painel.
- `dados.json` — os dados que todos verão (o snapshot publicado).

> **O que esperar (importante):** o painel é estático. Quem abre o link **lê o `dados.json` publicado**. Não é tempo real: para atualizar o que os outros veem, você (master) gera um `dados.json` novo e o substitui no repositório (passo 5). As senhas são **trava operacional**, não segurança forte — quem entende de navegador consegue ver o código. Para nada disso valer, não publique dado sigiloso.

---

## Passo a passo (pelo site do GitHub, sem instalar nada)

**1. Criar conta / repositório**
- Entre em github.com e crie uma conta (se ainda não tem).
- Clique em **New repository**. Nome sugerido: `cockpit-nnu`. Deixe **Public**. Crie.

**2. Subir os arquivos**
- No repositório novo, clique **uploading an existing file** (ou Add file → Upload files).
- Arraste **`index.html`** e **`dados.json`** desta pasta.
- Clique **Commit changes**.

**3. Ligar o GitHub Pages**
- No repositório: **Settings** → menu lateral **Pages**.
- Em **Source**, escolha **Deploy from a branch**.
- Em **Branch**, selecione **main** e pasta **/ (root)**. Clique **Save**.
- Aguarde ~1 minuto. A página mostra a URL publicada, algo como:
  `https://SEU-USUARIO.github.io/cockpit-nnu/`

**4. Os dois acessos**
- **Visualização (com senha):** mande o link normal
  `https://SEU-USUARIO.github.io/cockpit-nnu/`
  Ao abrir, o painel **pede senha**. Senha de visualização: **`123`**.
- **Master (você):** use o link com modo direto
  `https://SEU-USUARIO.github.io/cockpit-nnu/?modo=master`
  Entra direto como master, sem a tela de senha. **Mantenha este link privado** — quem tiver ele entra como master.
- Senhas (definidas no código, bloco `PWD` do `index.html`): visualização **`123`**, edição simples **`456`**, master **`opus890`**. Troque-as antes de divulgar.

**5. Atualizar os dados que todos veem (publicar)**
1. Abra o painel como **master**, faça as alterações.
2. Clique em **Publicar (dados.json)** no topo — baixa um `dados.json` atualizado.
3. No GitHub, entre no repositório → clique no `dados.json` → ícone de lápis (ou Add file → Upload) → substitua pelo novo → **Commit**.
4. Em ~1 minuto o link reflete os novos dados para todos.

---

## Dúvidas rápidas
- **Abri o index.html no meu PC (duplo clique) e não carrega o dados.json.** Normal: navegador bloqueia leitura de arquivo local. No GitHub Pages funciona. Localmente ele cai nos dados de exemplo.
- **Um visualizador editou e agora vê dados diferentes.** Só edição/master alteram, e isso fica salvo no navegador *dele*. Em "Restaurar dados padrão" (master) ou limpando o cache do navegador, volta ao publicado.
- **Quero esconder a tela de senha (uso interno só meu).** No `index.html`, troque `const GATE=true;` por `const GATE=false;`.
- **Quero acesso de verdade (multiusuário, ao vivo, log de quem mudou).** Aí precisa de backend: Google Sheets + Apps Script, Firebase ou Supabase. É o próximo nível, fora do GitHub Pages puro.
