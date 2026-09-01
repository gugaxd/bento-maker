# bento maker

Gerador de layouts bento: arraste imagens do computador, ajuste os blocos na malha
e exporte em PNG, JPG, SVG ou PDF. Todo o processamento acontece no navegador —
nenhuma imagem é enviada para servidor algum.

## Rodar local

```bash
npm install
npm run dev
```

## Publicar na Vercel

Sem configuração. Importe o repositório e a Vercel detecta o Vite sozinho:

- Framework: Vite
- Build: `npm run build`
- Saída: `dist`

Não há variáveis de ambiente, back end ou `vercel.json`.

## Estrutura

```
index.html        margem zerada do body (o app usa 100vh)
src/main.jsx      entrypoint React
src/App.jsx       a ferramenta inteira: lógica, tokens visuais e a fonte da marca
vite.config.js    plugin React
```

`App.jsx` é autocontido de propósito: o CSS dos tokens vive num `<style>` dentro do
componente e a fonte da marca (Host Grotesk SemiBold) está embutida em base64,
reduzida ao subconjunto de caracteres do nome. Não há requisição de rede em runtime,
exceto o chunk do jsPDF, carregado sob demanda ao clicar em PDF.

## Como funciona o layout

O empacotamento dos blocos é calculado em JavaScript, não pelo CSS Grid: uma matriz
de ocupação decide linha e coluna de cada imagem. Por isso a tela, o canvas do PNG,
o SVG e o PDF partem exatamente da mesma geometria — o que você vê é o que sai.

As imagens nunca são distorcidas: cada célula recorta (preencher) ou encaixa (caber)
mantendo a proporção original, com zoom e ponto de foco ajustáveis por bloco.

## Sistema visual

Segue os tokens do Grid Maker — dois temas, raio de 2px, monoespaçada para todo
número, ciano como acento de estrutura e magenta como acento de seleção.

Um desvio consciente: o sistema pede movimento zero, mas os blocos têm 190 ms de
transição ao acomodar no encaixe. Sem isso o redimensionamento salta de célula em
célula. `prefers-reduced-motion` desliga tudo.
