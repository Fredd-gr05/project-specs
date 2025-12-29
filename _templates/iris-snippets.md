# IRIS - Snippets Vue.js 3 (via CDN)

## 1. ESTRUTURA BASICA COM VUE

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue App</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <div id="app" class="container my-5">
        <h1>{{ titulo }}</h1>
        <p>{{ mensagem }}</p>
    </div>

    <script src="https://unpkg.com/vue@3"></script>
    <script>
        const { createApp } = Vue;

        createApp({
            data() {
                return {
                    titulo: 'Bem-vindo ao Vue!',
                    mensagem: 'Seu MVP reativo'
                };
            }
        }).mount('#app');
    </script>
</body>
</html>
```

## 2. V-MODEL - TWO-WAY DATA BINDING

```html
<div id="app">
    <input v-model="nome" placeholder="Digite seu nome" class="form-control">
    <p>{{ nome ? `Ola, ${nome}!` : 'Digite seu nome acima' }}</p>
</div>

<script src="https://unpkg.com/vue@3"></script>
<script>
    const { createApp } = Vue;

    createApp({
        data() {
            return { nome: '' };
        }
    }).mount('#app');
</script>
```

## 3. COMPUTED PROPERTIES - CALCULO REATIVO

```html
<div id="app">
    <input v-model.number="quantidade" type="number" class="form-control">
    <input v-model.number="preco" type="number" class="form-control">
    <p class="mt-3">Total: R$ {{ total }}</p>
</div>

<script src="https://unpkg.com/vue@3"></script>
<script>
    const { createApp } = Vue;

    createApp({
        data() {
            return { quantidade: 1, preco: 100 };
        },
        computed: {
            total() {
                return this.quantidade * this.preco;
            }
        }
    }).mount('#app');
</script>
```

## 4. V-IF E V-SHOW - CONDICIONAL

```html
<div id="app">
    <button @click="mostrar = !mostrar" class="btn btn-primary">Toggle</button>
    
    <!-- v-if remove do DOM -->
    <div v-if="mostrar" class="alert alert-success mt-3">Visivel com v-if</div>
    
    <!-- v-show apenas esconde (display: none) -->
    <div v-show="mostrar" class="alert alert-info mt-3">Visivel com v-show</div>
</div>

<script src="https://unpkg.com/vue@3"></script>
<script>
    const { createApp } = Vue;

    createApp({
        data() {
            return { mostrar: false };
        }
    }).mount('#app');
</script>
```

## 5. V-FOR - LISTA DINAMICA

```html
<div id="app">
    <ul class="list-group">
        <li v-for="item in items" :key="item.id" class="list-group-item d-flex justify-content-between">
            {{ item.nome }}
            <button @click="deletar(item.id)" class="btn btn-sm btn-danger">X</button>
        </li>
    </ul>
    <input v-model="novoItem" @keyup.enter="adicionar" class="form-control mt-3" placeholder="Novo item">
</div>

<script src="https://unpkg.com/vue@3"></script>
<script>
    const { createApp } = Vue;

    createApp({
        data() {
            return {
                items: [{id: 1, nome: 'Item 1'}, {id: 2, nome: 'Item 2'}],
                novoItem: ''
            };
        },
        methods: {
            adicionar() {
                if (this.novoItem.trim()) {
                    this.items.push({id: Date.now(), nome: this.novoItem});
                    this.novoItem = '';
                }
            },
            deletar(id) {
                this.items = this.items.filter(item => item.id !== id);
            }
        }
    }).mount('#app');
</script>
```

## 6. FORMULARIO REATIVO

```html
<div id="app" class="col-6">
    <form @submit.prevent="enviar" class="needs-validation">
        <div class="mb-3">
            <label class="form-label">Email</label>
            <input v-model="form.email" type="email" class="form-control" required>
        </div>
        <div class="mb-3">
            <label class="form-label">Mensagem</label>
            <textarea v-model="form.mensagem" class="form-control" required></textarea>
        </div>
        <button type="submit" class="btn btn-primary">Enviar</button>
    </form>
    <p v-if="enviado" class="alert alert-success mt-3">Formulario enviado!</p>
</div>

<script src="https://unpkg.com/vue@3"></script>
<script>
    const { createApp } = Vue;

    createApp({
        data() {
            return {
                form: { email: '', mensagem: '' },
                enviado: false
            };
        },
        methods: {
            enviar() {
                this.enviado = true;
                setTimeout(() => this.enviado = false, 3000);
            }
        }
    }).mount('#app');
</script>
```

## 7. WATCHERS - OBSERVAR MUDANCAS

```javascript
data() {
    return { busca: '' };
},
watch: {
    busca(novoValor) {
        console.log('Buscando por:', novoValor);
        // Fazer requisicao API aqui
    }
}
```

## 8. FETCH + LOADING STATE

```html
<div id="app">
    <button @click="carregarDados" :disabled="carregando" class="btn btn-primary">
        {{ carregando ? 'Carregando...' : 'Carregar' }}
    </button>
    <div v-if="erro" class="alert alert-danger mt-3">{{ erro }}</div>
    <div v-if="dados" class="card mt-3">
        <div class="card-body">
            <h5>{{ dados.title }}</h5>
            <p>{{ dados.body }}</p>
        </div>
    </div>
</div>

<script src="https://unpkg.com/vue@3"></script>
<script>
    const { createApp } = Vue;

    createApp({
        data() {
            return { dados: null, carregando: false, erro: '' };
        },
        methods: {
            async carregarDados() {
                this.carregando = true;
                try {
                    const res = await fetch('https://jsonplaceholder.typicode.com/posts/1');
                    this.dados = await res.json();
                } catch (e) {
                    this.erro = e.message;
                } finally {
                    this.carregando = false;
                }
            }
        }
    }).mount('#app');
</script>
```

## 9. CLASSES DINAMICAS

```html
<div id="app">
    <div :class="{active: ativo, 'text-danger': erro}" class="card p-3">
        {{ ativo ? 'Ativo' : 'Inativo' }}
    </div>
    <button @click="ativo = !ativo" class="btn btn-primary mt-2">Toggle</button>
</div>

<style>
    .active { background: lightgreen; }
    .text-danger { color: red; }
</style>

<script src="https://unpkg.com/vue@3"></script>
<script>
    const { createApp } = Vue;
    createApp({
        data() { return { ativo: false, erro: false }; }
    }).mount('#app');
</script>
```

## 10. LOCALSTORAGE COM WATCHER

```javascript
data() {
    return { contador: localStorage.getItem('contador') || 0 };
},
watch: {
    contador(novoValor) {
        localStorage.setItem('contador', novoValor);
    }
}
```

## 11. TEMPLATE COM INTERPOLACAO

```html
{{ mensagem }}              <!-- Texto simples -->
{{ numero * 2 }}            <!-- Expressoes -->
{{ condicao ? 'Sim' : 'Nao' }}  <!-- Ternario -->
{{ array.join(', ') }}      <!-- Metodos -->
```

## 12. CHECKLIST OPTIMIZACAO VUE

- [ ] Usar Options API (simples) ou Composition API (avancada)?
- [ ] Data properties reativas configuradas
- [ ] Computed properties para valores derivados
- [ ] Methods para acoes
- [ ] Watchers para efeitos colaterais
- [ ] Form validation client-side
- [ ] Error handling em fetch/async
- [ ] Loading states durante operacoes
- [ ] LocalStorage para persistencia
- [ ] Testes em Chrome/Firefox/Safari
