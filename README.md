# Bee Languages · Placement Test

Teste de nivelamento de inglês (A1 a C1) com formulário, teste interativo, relatório e envio dos resultados para um banco de dados seguro.

Tecnologias: **GitHub** (código) + **Cloudflare Pages** (site e função de envio) + **Cloudflare D1** (banco de dados). Tudo tem plano gratuito suficiente para começar.

---

## 1. O que tem nesta pasta

```
placement-test/
├── index.html                  página principal
├── styles.css                  identidade visual Bee Languages + impressão
├── app.js                      formulário, teste, relatório e envio
├── data/
│   ├── questions.js            gramática, vocabulário e leitura (edite aqui)
│   └── skills.js               listening, writing e speaking (edite aqui)
├── functions/
│   └── api/
│       ├── submit.js           recebe o teste, valida e grava no D1
│       └── admin/
│           └── submissions.js  consulta protegida por token (só a professora)
├── schema.sql                  estrutura do banco D1 (rodar uma vez)
├── wrangler.toml               configuração do Cloudflare
├── .gitignore                  impede subir segredos ao GitHub
└── README.md                   este arquivo
```

**Importante:** nenhuma resposta de aluno, senha ou token fica no código. Os dados vão direto para o banco D1.

---

## 2. Limites do que foi feito (leia antes de publicar)

- **Gramática, vocabulário, leitura e listening** são corrigidos automaticamente pelo navegador.
- **Writing e speaking** não são avaliados por inteligência artificial. Recebem apenas uma checagem simples (tamanho e conectores). A Teacher Anne precisa revisar.
- **Áudio (listening)** usa a voz do próprio dispositivo (síntese de voz). Não é gravação humana, e a voz muda entre celulares e computadores. Cada áudio pode ser ouvido **no máximo 3 vezes**.
- **Speaking por voz** depende do navegador ter reconhecimento de voz (Chrome e Edge costumam ter). A pronúncia **não é avaliada**. Sem o microfone, o aluno digita a resposta.
- **Nível estimado** é uma estimativa pedagógica, não uma certificação. Só um teste real com alunos pode confirmar se as questões estão bem calibradas.
- **Não testei o site num navegador nem o banco D1 real dentro deste ambiente.** A lógica de validação do servidor foi testada com um banco simulado. Na primeira publicação, faça o teste do item 8 deste guia.

---

## 3. Antes de começar: pontos jurídicos

- O formulário coleta nome, e-mail, telefone, faixa etária, objetivo e respostas. Isso são **dados pessoais** pela LGPD.
- Para **menores de 18 anos**, a LGPD (art. 14) exige cuidado reforçado e consentimento específico de pelo menos um dos pais ou responsável legal. O formulário exige os dados e a autorização do responsável. **Confirme o texto final com um advogado** antes de divulgar o link para crianças e adolescentes.
- Defina com um advogado o prazo de retenção (sugestão: 12 meses após o último contato) e o procedimento de exclusão.

---

## 4. Publicar no GitHub

1. Entre em **github.com** e clique em **New repository**.
2. Nome: `bee-placement-test`. Escolha **Private** (privado), para o código não ficar público. Clique em **Create repository**.
3. Na tela do repositório, clique em **uploading an existing file**.
4. Arraste **todos os arquivos e pastas** de `placement-test` (inclusive `functions`, `data` e `assets`).
5. Clique em **Commit changes**.

> Confira no GitHub que a pasta `functions/api` e a pasta `data` aparecem. Se faltar alguma, a página fica em branco ou a função não existe.

---

## 5. Criar o banco de dados D1

1. No painel da Cloudflare, vá em **Storage & Databases → D1 SQL Database** (ou **Workers & Pages → D1**).
2. Clique em **Create database**. Nome: `bee-placement`. Região: a mais próxima disponível (América do Sul, se aparecer).
3. Abra o banco criado e vá na aba **Console**.
4. Abra o arquivo `schema.sql`, copie **todo** o conteúdo, cole no console e clique em **Execute**.
5. Não precisa copiar nenhum ID. A ligação entre o site e o banco é feita no painel (passo 6.1).

---

## 6. Publicar no Cloudflare Pages

1. No painel da Cloudflare, vá em **Workers & Pages → Create → Pages → Connect to Git**.
2. Autorize o GitHub e escolha o repositório `bee-placement-test`.
3. Configurações de build:
   - **Framework preset:** None
   - **Build command:** deixe **vazio**
   - **Build output directory:** `/` (ou deixe em branco)
4. Clique em **Save and Deploy**. Em alguns minutos o site fica pronto em um endereço `.pages.dev`.

### 6.1 Conectar o banco D1 ao site

1. No projeto Pages, vá em **Settings → Bindings** (ou **Functions → D1 database bindings**).
2. Clique em **Add binding** → tipo **D1 database**.
3. **Variable name:** `DB` (exatamente assim, em maiúsculas).
4. **D1 database:** escolha `bee-placement`.
5. Salve e faça um novo deploy (**Deployments → Retry deployment** ou um novo commit no GitHub).

> Sem esse binding, o formulário mostra a mensagem "Storage is not configured" e nada é gravado.

### 6.2 Criar o token da professora

O token protege a consulta dos resultados. Ele é uma senha longa, **nunca vai para o GitHub**.

1. Gere uma senha longa (pelo menos 24 caracteres). Pode usar um gerenciador de senhas.
2. No projeto Pages, vá em **Settings → Variables and Secrets**.
3. Clique em **Add**:
   - **Type:** Secret (encrypted)
   - **Name:** `ADMIN_TOKEN`
   - **Value:** a senha longa que você gerou
4. Salve e faça um novo deploy.

Guarde o token em um gerenciador de senhas. Se ele vazar, troque o valor no painel.

### 6.3 Domínio próprio: teste.beelanguages.com.br

Vamos usar o subdomínio **teste.beelanguages.com.br**. Ele não mexe no site principal nem no e-mail (Zoho), porque é um registro separado.

O Cloudflare Pages entrega o site em `bee-placement-test.pages.dev`. Você precisa apenas apontar o subdomínio para esse endereço.

**Passo 1: descubra onde está o DNS do domínio.**
- Se o DNS está no **Cloudflare** (o domínio aparece na lista de *Websites* do Cloudflare): use o caso A.
- Se o DNS está no **Registro.br**: use o caso B.

**Caso A: DNS no Cloudflare**
1. No projeto Pages, vá em **Custom domains → Set up a custom domain**.
2. Digite `teste.beelanguages.com.br` e clique em **Continue**. O Cloudflare cria o registro sozinho.
3. Espere o status ficar **Active** (normalmente alguns minutos).

**Caso B: DNS no Registro.br (sem trocar os servidores de nome)**
1. No projeto Pages, vá em **Custom domains → Set up a custom domain**, digite `teste.beelanguages.com.br` e clique em **Continue**. Ele vai mostrar um alvo para o registro CNAME.
2. No painel do Registro.br, abra o domínio **beelanguages.com.br** → **Editar zona DNS** → **Adicionar registro**.
3. Crie o registro:

| Tipo  | Nome   | Valor / Destino                  | TTL  |
|-------|--------|----------------------------------|------|
| CNAME | teste  | bee-placement-test.pages.dev     | Padrão |

4. Não apague nem altere os registros MX e TXT do Zoho. Eles são de outro tipo e não conflitam com o CNAME do subdomínio.
5. Volte ao Cloudflare Pages e clique em **Check DNS records** até o status ficar **Active**. A propagação pode levar de 10 minutos a algumas horas.

> Não troque os servidores de nome (nameservers) do domínio para o Cloudflare sem antes recriar no Cloudflare todos os registros MX e TXT do Zoho. Se trocar sem isso, o e-mail da Bee Languages para de funcionar.

**Quando estiver ativo**, o site fica em **https://teste.beelanguages.com.br**. Use esse link com os alunos.

---

## 7. Como a professora consulta os resultados

Sempre use o token, e nunca cole o token em lugares públicos.

**Listar os 50 envios mais recentes** (no navegador, escreva a URL e use uma extensão ou ferramenta que permita enviar o cabeçalho `Authorization`. Ou use o terminal):

```
curl -H "Authorization: Bearer SEU_TOKEN" https://SEU-SITE.pages.dev/api/admin/submissions
```

**Ver um envio completo** (com as respostas), usando o ID que aparece na lista:

```
curl -H "Authorization: Bearer SEU_TOKEN" "https://SEU-SITE.pages.dev/api/admin/submissions?id=ID_DO_ENVIO"
```

**Consultar direto no banco** (mais simples): no painel, abra o D1 `bee-placement` → **Console** e rode, por exemplo:

```sql
SELECT created_at, full_name, email, phone, age_range, estimated_level, status
FROM submissions
WHERE deleted_at IS NULL
ORDER BY created_at DESC;
```

**Marcar um aluno como revisado e registrar o nível final da Anne:**

```sql
UPDATE submissions
SET status = 'reviewed', teacher_level = 'B1', teacher_notes = 'Speaking ok; work on listening'
WHERE id = 'ID_DO_ENVIO';
```

**Excluir um aluno a pedido** (direito de exclusão da LGPD):

```sql
DELETE FROM submissions WHERE email = 'aluno@exemplo.com';
```

---

## 8. Teste obrigatório antes de divulgar

Faça estes testes no site publicado, em um celular e em um computador:

1. **Campos obrigatórios:** tente continuar sem nome, com e-mail inválido, sem telefone, sem faixa etária e sem marcar o consentimento. Cada caso deve mostrar a mensagem certa.
2. **Faixas etárias:** teste as seis opções, especialmente **40 anos ou mais**. Para menores de 18, o bloco do responsável deve aparecer.
3. **Navegação:** avance e volte nas perguntas. As respostas devem continuar salvas.
4. **Áudio:** toque o áudio três vezes. Na quarta vez o botão deve ficar desativado.
5. **Speaking:** tente o microfone (se o navegador permitir) e também digite uma resposta.
6. **Envio:** faça um teste completo. Ao final deve aparecer "Sent to Bee Languages" com um código de referência.
7. **Banco:** rode no console do D1: `SELECT * FROM submissions ORDER BY created_at DESC LIMIT 1;`. O envio de teste deve estar lá.
8. **Relatório:** teste **Copy report** e **Save as PDF** (o navegador abre a janela de impressão; escolha "Salvar como PDF").
9. **Falha:** desligue a internet no meio do teste e envie. A mensagem deve dizer que não foi possível enviar, sem fingir sucesso.

Quando os testes passarem, apague os registros de teste:

```sql
DELETE FROM submissions WHERE email = 'seu-email-de-teste@exemplo.com';
```

---

## 9. Atualizar o projeto

1. Edite os arquivos no computador (questões em `data/`, textos em `app.js`).
2. No GitHub, abra o arquivo, clique no lápis (**Edit**), cole a mudança e clique em **Commit changes**.
3. O Cloudflare Pages publica sozinho em cerca de 1 minuto.

Para mudar as questões, altere só `data/questions.js` (gramática, vocabulário e leitura) ou `data/skills.js` (listening, writing e speaking). Cada item tem:

- `level`: A1, A2, B1, B2 ou C1
- `prompt`: o enunciado
- `options`: as alternativas
- `answer`: o número da resposta certa, começando em 0

Depois de qualquer mudança, refaça o teste 8 para não quebrar o site.

---

## 10. Compartilhar com os alunos

Envie o link do site (o `.pages.dev` ou o seu domínio próprio). Sugestão de mensagem:

> Olá! Antes da nossa primeira aula, faça o teste de nível da Bee Languages. Leva cerca de 50 minutos, pode ser feito no celular ou no computador, e o resultado ajuda a Anne a montar seu plano de estudos. Link: [seu link]

Para alunos menores de 18 anos, peça que o responsável preencha o formulário junto.

---

## 11. Próximas melhorias recomendadas

- **Proteção contra robôs:** adicionar Cloudflare Turnstile no formulário (gratuito). Hoje a proteção é só o campo oculto, o tempo mínimo e o limite por e-mail.
- **Aviso automático para a Anne:** receber um e-mail ou WhatsApp a cada novo envio (Cloudflare Email Routing ou Zapier).
- **Correção de writing e speaking com IA:** exige integração com um modelo de IA paga, com consentimento específico do aluno para envio do texto ou áudio.
- **Português completo das instruções:** o botão de idioma ainda não existe nesta versão.
- **Calibração:** aplicar o teste a 15 a 20 alunos reais e ajustar as questões e o critério de 70%.
