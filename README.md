# Desafio TDC — Agente Avaliador

Agente de IA que aplica uma pequena prova em formato de conversa sobre um tema de Engenharia de Software escolhido pelo aluno, faz pelo menos 3 perguntas (adaptando a próxima pergunta à resposta anterior), avalia as respostas e devolve uma nota (0–10) com feedback curto.

Feito para o Desafio TDC 2026 São Paulo — trilha Agentes de IA. Rodando em Cloudflare Workers, usando a API do Gemini para conduzir a conversa e avaliar.

## Como funciona

- `src/index.js` serve a página (frontend de chat embutido) em `GET /` e a rota `POST /api/chat`.
- O frontend guarda o histórico da conversa no navegador e manda o histórico inteiro a cada turno.
- O Worker monta o prompt de sistema (regras da prova) + histórico e chama a Gemini API.
- Quando o modelo decide encerrar a prova, ele responde em um formato fixo (`RESULTADO_FINAL / TEMA / NOTA / FEEDBACK`) que o frontend reconhece e mostra como cartão de resultado.

## Rodando localmente

1. Instale as dependências:

   ```bash
   npm install
   ```

2. Configure sua chave da Gemini API em `.dev.vars` (arquivo local, **não é commitado**, já está no `.gitignore`):

   ```
   GEMINI_API_KEY=sua-chave-aqui
   ```

3. Suba o servidor local:

   ```bash
   npm run dev
   ```

4. Abra `http://localhost:8787`.

## Deploy no Cloudflare

1. Login: `npx wrangler login`
2. Configure a chave como secret (não vai para o código nem para o repositório):

   ```bash
   npx wrangler secret put GEMINI_API_KEY
   ```

3. Deploy:

   ```bash
   npm run deploy
   ```

## Configuração

- `GEMINI_MODEL` (em `wrangler.jsonc`, seção `vars`): modelo do Gemini usado. Padrão: `gemini-3.5-flash-lite`. Se a chamada à API falhar com erro de modelo não encontrado, troque esse valor pelo id de modelo correto disponível na sua conta.
