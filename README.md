# Elite Barber — página de links (mobile first)

Página única, estática e sem build. Reúne o **agendamento online**, a
**avaliação no Google**, o cardápio de serviços, os horários e o endereço da
Elite Barber, em Divinópolis.

## O que tem

- Foto real da barbearia no topo, com o logo aplicado como um selo.
- Dois botões em formato de ticket: **Agendar horário** (ClickBarber) e
  **Avaliar no Google**.
- Nota **5,0 · 11 avaliações** no Google, com link direto para avaliar.
- Status **aberto/fechado** calculado em tempo real, no fuso de São Paulo.
- Depoimento real de cliente.
- Abas **Serviços** (com preços), **Horários** e **Onde fica**.
- Endereço com **Abrir no Maps** e **Copiar**, além de WhatsApp e telefone.
- Fontes Bebas Neue e Inter hospedadas localmente (sem CDN).
- Layout testado em 320, 360 e 390 px, com versão para desktop.

Os dados de endereço, telefone, horários, nota, avaliações e preços foram
conferidos no Google e no ClickBarber em **novembro de 2026**. Se algo mudar,
é só editar os pontos abaixo.

## Ver localmente

Abra o `index.html` no navegador. Tudo funciona, inclusive as fontes.

Para o botão **Copiar** funcionar em qualquer navegador, o ideal é abrir por um
servidor local (o `file://` bloqueia a área de transferência em alguns casos):

```bash
python -m http.server 8000
```

Depois acesse <http://localhost:8000>.

## Trocar os links

No fim do `index.html`, dentro do `<script>`, procure o bloco `LINKS`:

```js
const LINKS = {
  booking: "https://clickbarber.com.br/EliteBarber",
  review:  "https://www.google.com/search?...",
  whatsapp: "https://wa.me/5537991109906?text=...",
  maps: "https://www.google.com/maps/search/?api=1&query=..."
};
```

- **booking**: link do ClickBarber da barbearia.
- **review**: o link de avaliação do Google (o mesmo que você enviou).
- **whatsapp**: `https://wa.me/55DDDNUMERO?text=MENSAGEM`.
- **maps**: busca do Google Maps com o endereço.

Os botões e o link da nota usam esses valores automaticamente.

## Dados para manter atualizados

| O quê | Onde |
| --- | --- |
| Endereço | parágrafo `#addressText` e os dois links do Maps |
| Telefone / WhatsApp | links `tel:` e `wa.me` no HTML |
| Nota e número de avaliações | `data/reviews.json` (fallback no HTML) |
| Depoimento | bloco `.quote` |
| Horários | constantes `HOURS` e `GROUPS` no `<script>` |
| Serviços e preços | lista `.service-list` no HTML |
| Foto do topo | `assets/shop.jpg` |
| Logo | `assets/logo.png` |
| Cores e fontes | variáveis no `:root`, no início do CSS |
| Dados do Google (SEO) | JSON-LD no fim do arquivo |

### Horários

`HOURS` é a fonte do status aberto/fechado (0 = domingo … 6 = sábado).
`GROUPS` é só o agrupamento visual da aba Horários. Se mudar os horários,
atualize os dois.

```js
const HOURS = [
  { day: "Domingo", short: "dom", ranges: [] },
  { day: "Segunda-feira", short: "seg", ranges: [["09:00", "19:30"]] },
  // ...
];
```

### Nota e quantidade de avaliações

A nota e a contagem ficam em `data/reviews.json`:

```json
{ "rating": 5.0, "count": 11, "updatedAt": "2026-11-10" }
```

A página lê esse arquivo ao carregar e atualiza a nota do topo, a contagem, o
ticket de avaliação e o rodapé. Se o arquivo não carregar, os valores do HTML
continuam valendo.

Para atualizar, edite o JSON e faça commit/push. O `rating` usa ponto decimal
(5.0); a página mostra com vírgula. A atualização automática via Places API
ainda não está ligada; quando for, ela vai escrever nesse mesmo arquivo.

### Foto e logo

A foto do topo veio da capa do ClickBarber da própria barbearia. Para trocar,
substitua `assets/shop.jpg` por outra foto na mesma proporção (aprox. 4:3).
Se quiser remover a foto, apague a `<figure class="photo">` e ajuste a margem
negativa do logo (`.logo { margin-top: -42px }`).

O logo é o arquivo enviado, recortado para ficar mais legível em tamanho
pequeno. Para usar o arquivo original, substitua `assets/logo.png`.

A imagem de compartilhamento é `assets/share-elite-barber.jpg`, gerada a partir
da foto do barbeiro em 1200 × 630.

## Publicar

Site publicado em <https://raphaeldaysc.github.io/elite-barber/>.

- **Netlify Drop**: arraste a pasta inteira em <https://app.netlify.com/drop>.
- **Vercel**: importe o repositório ou rode `vercel` na pasta.
- **GitHub Pages**: o repositório já está com o Pages ativo.

Mantenha a pasta `assets/` junto do `index.html`. O `og:image` já aponta para
`assets/share-elite-barber.jpg` na URL pública do Pages, então o preview do
WhatsApp usa essa imagem. Se mudar de domínio, atualize as URLs absolutas no
`<head>`.

## Estrutura

```
index.html              página completa (HTML + CSS + JS)
data/reviews.json       nota e quantidade de avaliações
assets/logo.png         logo da Elite Barber (512 px)
assets/shop.jpg         foto do topo
assets/share-elite-barber.jpg  imagem de compartilhamento (WhatsApp/OG)
assets/fonts/           Bebas Neue e Inter em woff2
preview/                imagens de referência (não usadas pela página)
```

A pasta `preview/` é só para conferência visual — pode apagar antes de publicar.

## Detalhes técnicos

- Sem dependências, sem build, sem cookies.
- Fontes auto-hospedadas: o visual não quebra se a CDN falhar.
- Abas acessíveis (`role="tablist"`, setas ← → Home End, foco visível) e com
  altura animada.
- Respeita `prefers-reduced-motion`.
- Meta tags Open Graph, favicon e JSON-LD de `HairSalon` com endereço,
  telefone, horários e nota.
