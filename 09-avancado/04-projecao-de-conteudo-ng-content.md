# Lição 9.4: Projeção de Conteúdo: `<ng-content>`

!Moldura

Até agora, o conteúdo de um componente (o seu HTML) está "fechado" dentro do seu próprio ficheiro `.html`. Mas e se quiséssemos criar um componente de "layout", como um cartão, um painel ou uma janela modal, e quiséssemos que o utilizador desse componente pudesse colocar *qualquer conteúdo* lá dentro?

Para isto, usamos a **Projeção de Conteúdo** com a tag especial do Angular `<ng-content>`.

**Analogia:** Pensa numa **moldura de uma fotografia**. A moldura (`Componente`) é sempre a mesma: tem a sua estrutura, a sua cor, o seu vidro. Mas a fotografia que pões lá dentro (`Conteúdo Projetado`) pode ser qualquer uma. A `<ng-content>` é o espaço vazio na moldura onde a tua foto vai aparecer.

## `ng-content` Simples: Uma Única Ranhura

Vamos criar um componente `CardComponent` genérico que pode ser usado para mostrar qualquer tipo de conteúdo.

1.  Gera o componente: `ng g c card`
2.  No template do `CardComponent`, usamos `<ng-content>`.

**`src/app/card/card.component.html`**
```html
<div class="card-wrapper">
  <div class="card-header">
    <h3>Cartão Genérico</h3>
  </div>
  <div class="card-body">
    <!-- Qualquer conteúdo que for passado entre as tags <app-card>
         será "projetado" ou renderizado aqui. -->
    <ng-content></ng-content>
  </div>
</div>
```

Agora, no nosso `AppComponent` (ou noutro componente qualquer), podemos usar o `CardComponent` e passar-lhe conteúdo personalizado.

**`src/app/app.component.html`**
```html
<!-- Exemplo 1: Usar o cartão para mostrar um texto simples -->
<app-card>
  <p>Este é um texto simples que foi projetado para dentro do cartão.</p>
</app-card>

<!-- Exemplo 2: Usar o cartão para mostrar o nosso SneakerCardComponent! -->
<app-card>
  <app-sneaker-card [sneaker]="meuSneaker"></app-sneaker-card>
</app-card>
```

O `CardComponent` não sabe nem precisa de saber o que está a renderizar. Ele apenas fornece a "moldura".

## `ng-content` com `select`: Múltiplas Ranhuras

E se a nossa moldura tiver espaços específicos para coisas diferentes? Por exemplo, um espaço para o título e outro para o corpo do cartão. Podemos usar o atributo `select` no `<ng-content>` para criar múltiplas "ranhuras".

**`src/app/card/card.component.html` (Atualizado)**
```html
<div class="card-wrapper">
  <div class="card-header">
    <!-- Projeta aqui qualquer elemento que tenha o atributo 'card-title' -->
    <ng-content select="[card-title]"></ng-content>
  </div>
  <div class="card-body">
    <!-- Projeta aqui todo o resto que não foi selecionado -->
    <ng-content></ng-content>
  </div>
</div>
```

Agora, ao usarmos o componente, podemos "endereçar" o conteúdo para a ranhura correta.

**`src/app/app.component.html` (Atualizado)**
```html
<app-card>
  <!-- Este h2 tem o atributo 'card-title', por isso vai para a primeira ranhura -->
  <h2 card-title>Título Personalizado</h2>

  <!-- Este parágrafo não tem seletor, por isso vai para a ranhura por defeito -->
  <p>Este é o corpo do cartão, que aparece na segunda ranhura.</p>
</app-card>
```

O `select` pode ser usado com nomes de tags (`select="h2"`), classes CSS (`select=".minha-classe"`) ou atributos (`select="[meu-atributo]`), tornando a projeção de conteúdo uma ferramenta extremamente poderosa e flexível.

---

**Módulo 9 Concluído!** 🏆

Dominaste as técnicas avançadas de comunicação e composição de componentes. Agora sabes controlar o ciclo de vida, criar um fluxo de dados robusto com `@Input` e `@Output`, e construir componentes de layout flexíveis com `<ng-content>`.

Estás mais do que pronto para o projeto do módulo: **Refatorar o `SneakerCard`**.