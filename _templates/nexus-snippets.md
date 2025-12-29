# NEXUS - Snippets Reutilizaveis (Vanilla JS)

## 1. ESTRUTURA BASICA HTML5

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Seu App</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="#">Logo</a>
        </div>
    </nav>

    <main class="container my-5">
        <h1>Bem-vindo</h1>
        <p>Conteudo aqui</p>
    </main>

    <footer class="bg-dark text-white text-center py-3">
        <p>&copy; 2025 Seu App</p>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script src="script.js"></script>
</body>
</html>
```

## 2. MODAL REUTILIZAVEL

```html
<div class="modal fade" id="exampleModal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title">Titulo</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                Conteudo do modal
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Fechar</button>
                <button type="button" class="btn btn-primary">Salvar</button>
            </div>
        </div>
    </div>
</div>

<script>
const modal = new bootstrap.Modal(document.getElementById('exampleModal'));
modal.show(); // Abrir
modal.hide(); // Fechar
</script>
```

## 3. FORMULARIO COM VALIDACAO

```html
<form id="meuForm" novalidate>
    <div class="mb-3">
        <label for="email" class="form-label">Email</label>
        <input type="email" class="form-control" id="email" required>
        <div class="invalid-feedback">Email invalido</div>
    </div>
    <button type="submit" class="btn btn-primary">Enviar</button>
</form>

<script>
const form = document.getElementById('meuForm');
form.addEventListener('submit', (e) => {
    e.preventDefault();
    if (form.checkValidity() === false) {
        e.stopPropagation();
    }
    form.classList.add('was-validated');
    // Processar formulario
});
</script>
```

## 4. CARD COM IMAGEM

```html
<div class="card">
    <img src="https://via.placeholder.com/300" class="card-img-top" alt="...">
    <div class="card-body">
        <h5 class="card-title">Titulo do Card</h5>
        <p class="card-text">Descricao breve</p>
        <a href="#" class="btn btn-primary">Saiba Mais</a>
    </div>
</div>
```

## 5. LISTA DINAMICA COM JAVASCRIPT

```html
<ul id="minhaLista" class="list-group"></ul>

<script>
const items = ['Item 1', 'Item 2', 'Item 3'];
const lista = document.getElementById('minhaLista');

items.forEach(item => {
    const li = document.createElement('li');
    li.className = 'list-group-item';
    li.textContent = item;
    lista.appendChild(li);
});
</script>
```

## 6. REQUISICAO FETCH COM LOADING

```html
<button id="btnCarregar" class="btn btn-primary">Carregar Dados</button>
<div id="loading" class="d-none">
    <div class="spinner-border" role="status">
        <span class="visually-hidden">Carregando...</span>
    </div>
</div>
<div id="resultado"></div>

<script>
const btn = document.getElementById('btnCarregar');
const loading = document.getElementById('loading');
const resultado = document.getElementById('resultado');

btn.addEventListener('click', async () => {
    loading.classList.remove('d-none');
    try {
        const res = await fetch('https://jsonplaceholder.typicode.com/todos/1');
        const data = await res.json();
        resultado.innerHTML = `<pre>${JSON.stringify(data, null, 2)}</pre>`;
    } catch (err) {
        resultado.innerHTML = `<div class="alert alert-danger">Erro: ${err.message}</div>`;
    } finally {
        loading.classList.add('d-none');
    }
});
</script>
```

## 7. LOCALSTORAGE - SALVAR DADOS

```javascript
// Salvar
localStorage.setItem('usuario', JSON.stringify({nome: 'João', email: 'joao@test.com'}));

// Recuperar
const usuario = JSON.parse(localStorage.getItem('usuario'));

// Deletar
localStorage.removeItem('usuario');

// Limpar tudo
localStorage.clear();
```

## 8. TABS/ABAS FUNCIONAIS

```html
<ul class="nav nav-tabs" role="tablist">
    <li class="nav-item" role="presentation">
        <button class="nav-link active" data-bs-toggle="tab" data-bs-target="#tab1">Tab 1</button>
    </li>
    <li class="nav-item" role="presentation">
        <button class="nav-link" data-bs-toggle="tab" data-bs-target="#tab2">Tab 2</button>
    </li>
</ul>
<div class="tab-content">
    <div class="tab-pane fade show active" id="tab1">Conteudo Tab 1</div>
    <div class="tab-pane fade" id="tab2">Conteudo Tab 2</div>
</div>
```

## 9. PAGINACAO

```html
<nav>
    <ul class="pagination">
        <li class="page-item"><a class="page-link" href="#">Anterior</a></li>
        <li class="page-item active"><a class="page-link" href="#">1</a></li>
        <li class="page-item"><a class="page-link" href="#">2</a></li>
        <li class="page-item"><a class="page-link" href="#">Proximo</a></li>
    </ul>
</nav>
```

## 10. CHECKLIST DE OTIMIZACAO

- [ ] Usar CSS classes do Bootstrap (nao inline styles)
- [ ] Adicionar aria-labels para acessibilidade
- [ ] Testar em Chrome, Firefox, Safari, Edge
- [ ] Mobile first: testar em DevTools (375px, 768px, 1440px)
- [ ] Minificar CSS/JS antes de deploy
- [ ] Adicionar favicon
- [ ] Comprimir imagens
- [ ] Validar HTML5 com https://validator.w3.org/
- [ ] Lighthouse score > 90
- [ ] GitHub Pages configurado
