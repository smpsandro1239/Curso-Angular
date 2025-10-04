# Lição 2.1: Anatomia de um Componente: O Ficheiro `.ts`

!ID Card

Vamos abrir o capô do nosso `AppComponent` e ver as peças que o compõem, focando-nos no ficheiro `app.component.ts`. Este ficheiro é o "cérebro" do nosso componente.

**Analogia:** O ficheiro `.ts` é o **Bilhete de Identidade** do nosso componente. Contém toda a informação essencial sobre ele: quem é, como se chama e onde mora.

Quando abres o `src/app/app.component.ts`, vês duas partes principais: o **Decorator `@Component`** e a **Classe do Componente**.

```typescript
import { Component } from '@angular/core';

// 1. O Decorator @Component (os metadados)
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})

// 2. A Classe do Componente (a lógica)
export class AppComponent {
  title = 'minha-primeira-app';
}
```

## 1. O Decorator `@Component`

Um *Decorator* em TypeScript é uma função especial que começa com `@` e que serve para adicionar "metadados" (informação extra) a uma classe. O `@Component` diz ao Angular: "Esta não é uma classe qualquer, é um Componente Angular!".

Ele recebe um objeto de configuração com três propriedades essenciais:

-   **`selector: 'app-root'`**
    -   **O que é?** É o "nome de guerra" do nosso componente. É a tag HTML personalizada que vamos usar para o colocar noutros templates.
    -   **Onde é usado?** No `index.html`, vês `<app-root></app-root>`. É aqui que o Angular vai renderizar este componente.

-   **`templateUrl: './app.component.html'`**
    -   **O que é?** É o caminho para o ficheiro HTML que contém a estrutura visual do componente. É a "cara" do nosso componente.

-   **`styleUrls: ['./app.component.scss']`**
    -   **O que é?** É um array com os caminhos para os ficheiros de estilo que se aplicam **apenas** a este componente. Os estilos são "encapsulados", o que significa que o CSS que escreveres aqui não vai "vazar" e afetar o resto da aplicação. É como ter um camarim privado para cada ator.

## 2. A Classe do Componente

```typescript
export class AppComponent {
  title = 'minha-primeira-app';
}
```

Esta é uma classe TypeScript normal. É aqui que vive toda a **lógica** e os **dados** do nosso componente.

-   **`export`**: Esta palavra-chave torna a classe `AppComponent` disponível para ser usada noutras partes da nossa aplicação (como no `app.module.ts`).
-   **`class AppComponent`**: O nome da classe, por convenção, é o nome do ficheiro em `PascalCase` seguido de `Component`.
-   **Propriedades (como `title`)**: As variáveis declaradas aqui (chamadas de propriedades da classe) guardam o "estado" do nosso componente. São os dados que queremos mostrar ou manipular.

Qualquer propriedade que definas aqui, como a `title`, fica automaticamente disponível para ser usada no template HTML associado (`app.component.html`), como vimos no exercício com a sintaxe `{{ title }}`.

---

Na próxima lição, vamos olhar com mais detalhe para os outros dois ficheiros que compõem o nosso componente: o template (`.html`) e os estilos (`.scss`), e ver como podemos começar a dar-lhes mais vida.