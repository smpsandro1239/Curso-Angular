# Lição 2.3: Composição: Encaixar Componentes

![Lego Encaixando](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExbWw5a2N5c3JqY254b250b294c251b294c251b294c251b294/l41lHw24p23v5TzSE/giphy.gif)

Uma aplicação raramente é feita de um único componente. O poder do Angular (e de outros frameworks modernos) vem da **composição**: a capacidade de construir componentes complexos a partir de componentes mais simples.

**Analogia:** Não constróis um carro de Lego com um único bloco. Tu pegas na peça do chassis, encaixas as peças das rodas, depois a do volante, os bancos, etc. Cada peça é um componente, e ao juntá-las, compões uma estrutura maior.

## Relação Pai-Filho

No exercício do Módulo 1, fizeste exatamente isto!

1.  Geraste um `HeaderComponent` com o CLI.
2.  Colocaste a tag `<app-header></app-header>` dentro do `app.component.html`.

Ao fazeres isto, estabeleceste uma relação **pai-filho**:
-   O `AppComponent` tornou-se o componente **pai**.
-   O `HeaderComponent` tornou-se um componente **filho**.

O template do `AppComponent` é o "anfitrião" que "convida" o `HeaderComponent` para ser renderizado dentro dele. Podes ter múltiplos filhos e até netos, criando uma árvore de componentes que forma a tua aplicação inteira.

```html
<!-- src/app/app.component.html (Pai) -->

<!-- O filho <app-header> é renderizado aqui -->
<app-header></app-header>

<main>
  <h1>{{ title }}</h1>
  <p>Conteúdo principal da aplicação.</p>
  
  <!-- Podemos ter outro componente filho aqui -->
  <app-footer></app-footer>
</main>
```

## O `app.module.ts`: O "Índice" da Nossa Caixa de Lego

Como é que o `AppComponent` sabe que `<app-header>` existe e é válido?

Quando usaste o comando `ng generate component header`, o Angular CLI fez duas coisas:
1.  Criou os 4 ficheiros do `HeaderComponent`.
2.  Atualizou automaticamente o ficheiro `src/app/app.module.ts`.

O `app.module.ts` funciona como o "índice" ou o "manual de peças" do nosso kit de Lego. Ele diz ao Angular quais as peças (componentes, serviços, etc.) que pertencem a este módulo da aplicação.

**`src/app/app.module.ts`**
```typescript
// ... imports ...
import { AppComponent } from './app.component';
import { HeaderComponent } from './header/header.component'; // CLI importou-o

@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent // CLI declarou-o aqui
  ],
  // ... outros metadados ...
})
export class AppModule { }
```

A secção `declarations` é um array que lista todos os componentes que fazem parte deste módulo. Como tanto o `AppComponent` como o `HeaderComponent` estão declarados no mesmo módulo, eles "conhecem-se" e podem ser usados um dentro do outro.

---

**Módulo 2 Concluído!** 🏆

Agora já entendes o bloco de construção mais fundamental do Angular. Sabes o que é um componente, como é composto pelos seus ficheiros `.ts`, `.html` e `.scss`, e como os podes encaixar para criar uma UI.

No próximo módulo, vamos aprender a fazer estes componentes comunicar, passando dados do pai para o filho e reagindo a eventos do utilizador. Bem-vindo ao mundo do **Data Binding**!