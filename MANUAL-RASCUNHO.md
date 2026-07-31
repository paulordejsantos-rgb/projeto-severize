# Manual de Operação — Bistrô Elegante (rascunho)

> Este documento existe para que alguém sem o Paulo consiga manter, atualizar e resolver problemas neste projeto com autonomia real — não é só um checklist técnico, é o raciocínio por trás de cada decisão.
>
> **Este é um projeto de demonstração/portfólio da agência A Casa & Designer**, não um restaurante real operando pedidos de clientes de verdade — o site linka de volta para `acasaedesigner.com` e usa o mesmo Supabase/WhatsApp compartilhado dos outros projetos-demo desta pasta.
>
> **⚠️ O nome da pasta/repositório ("Severize") não tem nenhuma relação com o nome de marca exibido no site ("Bistrô Elegante").** Isso é mais confuso aqui do que em qualquer outro projeto-irmão — veja seção 1.
>
> **⚠️ Diferença estrutural:** assim como o Projeto-Davar, este é **HTML/CSS/JS puro, sem framework Next.js/React e sem build** — diferente da Pizzaria e do Hamburgueria.

## 1. Visão geral do projeto

- **Nome do restaurante / marca:** "Bistrô Elegante — Cardápio Digital" (`<title>` do `index.html`). **Esse nome foi corrigido recentemente** — o commit mais recente do repositório é literalmente `84d7f08 Corrige nome do restaurante para Bistrô Elegante`, o que sugere que em algum momento o texto estava errado/desatualizado em produção. Vale checar se todos os textos do site (não só o título) refletem esse nome corrigido.
- **Pasta local:** `C:\MEUS PROJETOS\02-Demos-Cardapio\Projeto-Severize`
- **Nome interno (package.json) e do repositório:** `projeto-severize` — "Severize" não aparece em nenhum texto visível do site; é puramente um codinome de projeto do Paulo, sem relação com a marca "Bistrô Elegante".
- **Repositório GitHub:** `https://github.com/paulordejsantos-rgb/projeto-severize.git`, branch `main`.
- **Projeto Vercel vinculado:** `projeto-severize` (projectId `prj_AGPPKsgorIJzcbMweM8FhrgdXSvY`, org `team_Xax7L5SLxypHBqEEv8YwFDAH` — mesma organização dos demais projetos-demo).
- **URL pública em produção:** `https://projeto-severize.vercel.app`

**Por quê / critério geral:** mesma lógica dos outros projetos-demo (custo zero, sem infraestrutura própria), implementado aqui em HTML/JS puro como o Projeto-Davar — provavelmente os dois (Davar e Severize) foram construídos na mesma fase de exploração de uma versão "sem framework" do produto, enquanto Pizzaria/Hamburgueria representam a linha "com Next.js".

---

## 2. Como publicar uma alteração

**Como (confirmado com o Paulo — mesmo fluxo do Projeto-Davar):**
1. Editar `index.html` ou `config-cardapio.js` localmente.
2. Publicar manualmente via CLI da Vercel:
   ```
   $env:NODE_OPTIONS="--use-system-ca"
   npx vercel --prod
   ```
3. **Não é deploy automático por git push.** Depois de publicar, também commite e dê push (`git add`, `git commit`, `git push origin main`) para manter o repositório sincronizado — a Vercel não faz isso sozinha aqui.

**Por quê:** o arquivo `.claude/settings.local.json` deste próprio projeto tem o comando exato registrado (`npx vercel --prod` com `NODE_OPTIONS=--use-system-ca`), então isso não é inferência — é o comando real já usado para publicar este projeto especificamente.

⚠️ **Achado relevante no histórico do Git:** há uma sequência de commits (`Testa deploy automático após conectar repositório na Vercel`, `Testa deploy automático via GitHub`, e principalmente `Corrige Output Directory da Vercel para servir a raiz do projeto`) que mostra que **o deploy automático via Git já foi tentado neste projeto e teve um bug real**: a Vercel esperava encontrar o site numa pasta de build (padrão de frameworks como Next.js) e não na raiz do projeto, então o site publicado ficava errado/vazio até alguém corrigir o campo **Output Directory** para `.` (raiz) — isso está refletido em `vercel.json` (`"outputDirectory": "."`).

**Se algo sair diferente do esperado:**
- Se decidir tentar reativar o deploy automático via Git novamente, **confirme que `outputDirectory: "."` continua configurado** tanto no `vercel.json` quanto no painel do projeto na Vercel — removê-lo ou recriar o projeto do zero na Vercel provavelmente reintroduz o mesmo bug de "site vazio/errado" que já aconteceu antes.
- Se o site não atualizar após publicar manualmente: confira o painel da Vercel (Deployments) antes de qualquer outra suposição.

---

## 3. Estrutura de arquivos e o que cada parte faz

| Caminho | O que é |
|---|---|
| `index.html` (1039 linhas) | O site inteiro — HTML, CSS e JS num arquivo só, mesmo padrão do Projeto-Davar. |
| `config-cardapio.js` | Controle de disponibilidade — **aqui a chave é um número de ID** (`1` a `15`), com comentário indicando o prato correspondente. Isso é **diferente do Projeto-Davar, que usa o nome do item como chave** — não assuma que os dois projetos seguem o mesmo padrão só porque são ambos HTML puro. |
| `vercel.json` | Define cache agressivo para `.webp`/`.js`, `index.html` sem cache, e **`outputDirectory: "."`** — este último campo é o que resolve o bug de deploy descrito na seção 2; não remova. |
| `public/brasao.png` | O favicon/logo fica dentro de `public/`, diferente do Projeto-Davar (onde `brasao.png` fica solto na raiz) — mais uma pequena inconsistência entre os dois projetos-irmãos em HTML puro. |
| `*.png` / `*.webp` na raiz | Fotos dos pratos, originais e otimizadas — este projeto tem fotos individuais de pratos principais (moqueca, picanha, costela, feijoada, frango caipira, camarão, caldos), diferente do Davar (que só tem bebidas/sobremesas por ser um sistema a quilo). |
| `gcm-diagnose.log` | Arquivo de diagnóstico do Git Credential Manager, não commitado — é lixo de depuração de uma sessão de terminal, seguro de apagar, não faz parte do projeto. **Ainda presente** — não foi incluído na limpeza de 2026-07-31 (só `compress-images.js` e o PDF de relatório foram removidos). |
| `.claude/settings.local.json` | Não é parte do site — é configuração local do Claude Code para este projeto, mas **contém o registro real do comando de deploy usado** (ver seção 2), o que a torna uma fonte de informação útil, não só metadado. |

> **Limpeza feita em 2026-07-31:** `compress-images.js` (gitignorado, script de otimização usando `sharp` como devDependency) e `RELATORIO SEVERIZA 23.05.2026.pdf` (relatório de negócio untracked) foram removidos da raiz. O `gcm-diagnose.log` continua lá (não foi pedido para remover). O `package.json` ainda lista `sharp` em `devDependencies`, agora sem script que a use.

**Por quê essa estrutura:** mesma resposta do Projeto-Davar — sem framework, sem processo de build, tudo solto na raiz.

**Se algo sair diferente do esperado / critério:**
- Para mudar um prato (nome, preço, descrição, foto): edite `index.html` diretamente.
- Para marcar como esgotado: edite `config-cardapio.js` usando o **número do ID**, não o nome — confira o comentário ao lado de cada linha para saber qual ID corresponde a qual prato.

---

## 4. Dependências externas

### Supabase
- Mesma URL e chave anônima compartilhada: `https://chwxepwdsyspcdkalaic.supabase.co`, hardcoded em `index.html` (linhas 872-873).
- Tabelas `avaliacoes` e `sugestao_dia`, filtradas por `restaurante=eq.Bistrô Elegante` (`Bistr%C3%B4%20Elegante` codificado na URL).
- Mesmos riscos e cuidados do banco compartilhado descritos no manual da Pizzaria — nunca altere schema sem considerar os projetos-irmãos.

### Outras integrações
- **WhatsApp:** mesmo número `5521990980965` de todos os projetos-demo.
- **VLibras:** mesma integração pública.
- **Sem `.env`.**

---

## 5. Como rodar localmente para testar

Mesma situação do Projeto-Davar: não há `npm run dev`. Abrir `index.html` direto no navegador funciona para inspecionar o layout, mas para testar chamadas ao Supabase de forma confiável, sirva a pasta com um servidor HTTP local:
```
npx serve .
```

⚠️ Assim como no Projeto-Davar, **não há documentação oficial de como testar localmente** — esta é uma recomendação técnica razoável, não um processo confirmado pelo Paulo.

---

## 6. Status do PWA

**Não implementado.** Diferente do Projeto-Davar (que tem `manifest.json` + `sw.js` funcionando), este projeto **não tem nenhum dos dois arquivos**. Não assuma que, por serem tecnicamente parecidos (ambos HTML puro), os dois têm os mesmos recursos — não têm.

**Se isso for esperado:** seria possível copiar o padrão `manifest.json`/`sw.js` do Projeto-Davar como ponto de partida, mas exigiria reescrever completamente a lista de assets cacheados (`ASSETS` em `sw.js`) e os textos de `manifest.json` para refletir "Bistrô Elegante" em vez de "Peso & Sabor".

---

## 7. Customizações exclusivas deste projeto — NÃO copiar para outros

- Cardápio de pratos (moqueca de peixe, picanha na brasa, costela no bafo, feijoada completa, frango caipira, camarão alho e óleo, caldo de mocotó, caldo verde, bolinho de mandioca) e bebidas/sobremesas — tudo dentro de `index.html`.
- O texto `restaurante: 'Bistrô Elegante'` usado no Supabase.
- `outputDirectory: "."` em `vercel.json` — isso é uma correção de bug específica deste projeto (ver seção 2); pode ser relevante replicar em qualquer outro projeto HTML-puro novo, mas não é algo "de marca", é técnico e potencialmente reutilizável.

O que é comum à família: Supabase e WhatsApp compartilhados, VLibras, alto contraste, narração por voz, link para "A Casa & Designer".

---

## 8. Pegadinhas e comportamentos incomuns

1. **O nome de marca ("Bistrô Elegante") não tem relação nenhuma com o nome do projeto/repositório ("Severize")** — ao procurar ou conversar sobre este projeto, tenha isso em mente; é o caso mais confuso entre todos os projetos-demo revisados até agora.
2. **`outputDirectory: "."` em `vercel.json` já corrigiu um bug real de deploy** (site publicando vazio/errado por a Vercel procurar uma pasta de build que não existe) — não mexa nesse campo sem entender por que ele está lá.
3. **`config-cardapio.js` usa ID numérico como chave, não nome de item** — o oposto do Projeto-Davar. Não copie um arquivo de disponibilidade de um projeto para o outro sem adaptar o formato.
4. **Sem PWA** — ao contrário do Projeto-Davar, não há cache offline nem Service Worker aqui, então não há o risco de "cache do PWA escondendo mudanças" que existe lá.
5. **Deploy manual via `npx vercel --prod`** com a mesma variável de ambiente `NODE_OPTIONS=--use-system-ca` necessária no Projeto-Davar.
6. `brasao.png` fica em `public/`, não na raiz (diferente do Davar) — se for reaproveitar um trecho de código ou CSS de um projeto no outro, confira o caminho da imagem antes de copiar.
7. `gcm-diagnose.log` continua avulso e não commitado na raiz — não faz parte do site, seguro de ignorar ou remover, mas não confunda com arquivo de configuração ativo.
8. Mesmo Supabase e WhatsApp compartilhados — mesmos cuidados já descritos nos outros manuais.

---

## 9. Como decidir o que não está documentado aqui

- **Não assuma que o Projeto-Davar e este projeto seguem o mesmo padrão só porque são os dois em HTML puro** — já há pelo menos três diferenças concretas entre eles (chave de disponibilidade por nome vs. ID, presença ou não de PWA, localização do `brasao.png`). Verifique sempre o código deste projeto especificamente antes de aplicar um padrão aprendido no outro.
- **O histórico de commits deste projeto mostra tentativa e erro real com deploy** (a correção do Output Directory) — isso é um sinal de que vale a pena ler o `git log` antes de mexer em configuração de deploy/Vercel aqui, porque já houve um problema documentado nesse exato ponto.
- **Antes de "corrigir" a discrepância entre o nome do projeto (Severize) e o nome de marca (Bistrô Elegante), pergunte ao Paulo** — pode ser proposital (codinome interno) e não um erro a ser corrigido.
- Mesmo critério dos outros manuais: prefira a solução mais simples, nunca mexa no Supabase compartilhado sem considerar os projetos-irmãos, e trate isso como material de portfólio, não operação real.
