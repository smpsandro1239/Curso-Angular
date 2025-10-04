# Lição 6.2: `<router-outlet>` e `routerLink`: O Palco e os Bilhetes

!Palco e Bilhetes

O nosso mapa de rotas está pronto, mas a aplicação ainda não mostra nada quando mudamos o URL. Isto acontece porque falta dizermos ao Angular *onde* renderizar os componentes das rotas e *como* navegar entre elas.

**Analogia:** Pensa num festival de música com vários palcos.
-   **`<router-outlet>`:** É o **palco principal**. A banda que está a tocar nesse palco (`Componente`) muda consoante a hora do dia (`URL`), mas o palco em si está sempre no mesmo sítio.
-   **`routerLink`:** São os **bilhetes ou as pulseiras** que te dão acesso a cada área/palco. Clicar num `routerLink` é como mostrar a tua pulseira para mudar de uma zona para outra.

## 1. `<router-outlet>`: O Palco

A diretiva `<router-outlet>` é um marcador de posição. Colocamo-la no template de um componente (normalmente no `app.component.html`) e ela diz ao Angular: "Quando o URL for `/tarefas`, é aqui dentro que deves renderizar o `TasksComponent`. Quando for `/sobre`, renderiza aqui o `AboutComponent`."

Vamos refatorar o nosso `app.component.html` para ser a "concha" da nossa aplicação, contendo os elementos que são comuns a todas as páginas (como o header e o footer) e o `<router-outlet>` para o conteúdo que muda.

**`src/app/app.component.html` (Atualizado)**
```html
<!-- O Header fica fixo no topo de todas as páginas -->
<app-header></app-header>

<main>
  <!-- O Angular vai renderizar o componente da rota ativa aqui dentro -->
  <router-outlet></router-outlet>
</main>

<!-- O Footer fica fixo no fundo de todas as páginas -->
<app-footer></app-footer>
```

Agora, se fores a `http://localhost:4200/tarefas`, o `TasksComponent` será renderizado dentro da tag `<main>`. Se fores a `/sobre`, o `AboutComponent` aparecerá no mesmo sítio.

## 2. `routerLink`: Os Bilhetes

Para criar links de navegação, **não usamos `href`**. Um link `<a href="/tarefas">` causaria um recarregamento completo da página, o que destrói o propósito de uma SPA.

Em vez disso, usamos a diretiva `routerLink`.

Vamos adicionar uma barra de navegação ao nosso `HeaderComponent`.

**`src/app/header/header.component.html` (Atualizado)**
```html
<header>
  <nav>
    <ul>
      <li>
        <a routerLink="/inicio">Início</a>
      </li>
      <li>
        <a routerLink="/tarefas">Tarefas</a>
      </li>
      <li>
        <a routerLink="/sobre">Sobre</a>
      </li>
    </ul>
  </nav>
</header>
```

Agora, quando clicares nestes links, o Angular vai intercetar o clique, atualizar o URL na barra de endereços e renderizar o componente correspondente no `<router-outlet>`, tudo sem recarregar a página.

## `routerLinkActive`: Destacar o Bilhete Ativo

Queremos dar um feedback visual ao utilizador, mostrando qual é a página ativa. Para isso, usamos a diretiva `routerLinkActive`. Ela aplica uma classe CSS ao elemento quando o seu `routerLink` corresponde ao URL atual.

**`src/app/header/header.component.scss`**
```scss
/* Adiciona este estilo para o link ativo */
.link-ativo {
  font-weight: bold;
  text-decoration: underline;
  color: #0d6efd;
}
```

**`src/app/header/header.component.html` (Final)**
```html
<header>
  <nav>
    <ul>
      <li><a routerLink="/inicio" routerLinkActive="link-ativo">Início</a></li>
      <li><a routerLink="/tarefas" routerLinkActive="link-ativo">Tarefas</a></li>
      <li><a routerLink="/sobre" routerLinkActive="link-ativo">Sobre</a></li>
    </ul>
  </nav>
</header>
```

Agora, o link da página em que estás terá a classe `link-ativo` aplicada, dando um feedback visual imediato ao utilizador.

Na próxima lição, vamos ver como podemos criar rotas mais dinâmicas, passando informação diretamente no URL, como o ID de um utilizador (`/utilizadores/123`).