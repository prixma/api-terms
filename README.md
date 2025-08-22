# Termos de Uso da API

Este repositório publica uma página estática que **carrega e renderiza** o arquivo `terms.md` (Markdown) no navegador.

> Dica: Para editar os termos, altere **apenas** `terms.md`. O `index.html` buscará esse arquivo e renderizará automaticamente via [Marked.js](https://marked.js.org/) (MIT).

## Estrutura
```
.
├─ index.html    # página que busca e renderiza terms.md
├─ terms.md      # conteúdo principal em Markdown
├─ styles.css    # estilos
└─ README.md
```

## Desenvolvimento local
Basta abrir `index.html` no navegador. Se o navegador bloquear `fetch` de arquivo local, rode um servidor simples:

```bash
# Python 3
python -m http.server 8080
# depois acesse: http://localhost:8080
```

## Licença
Este template é distribuído sob licença MIT.
