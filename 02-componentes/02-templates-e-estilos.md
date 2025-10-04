# Lição 2.2: Templates e Estilos: Os Ficheiros `.html` e `.scss`

!Cara e Maquilhagem

Já vimos que o ficheiro `.ts` é o cérebro do nosso componente. Agora, vamos ver as outras duas partes que lhe dão uma identidade visual: o template HTML e a folha de estilos SCSS.

**Analogia:** Se o ficheiro `.ts` é o Bilhete de Identidade, o ficheiro `.html` é a **fotografia** (a cara) e o ficheiro `.scss` é a **maquilhagem** que se aplica a essa fotografia.

## 1. O Template (`.html`) – A Cara do Componente

O ficheiro `app.component.html` é, na sua essência, um ficheiro HTML normal. Podes escrever qualquer tag HTML válida que conheças. A magia do Angular é que ele adiciona "superpoderes" a este HTML.

### Interpolação (`{{ }}`): Mostrar Dados Dinâmicos

Como vimos no exercício anterior, a interpolação é a forma mais simples de *data binding* (ligação de dados). É uma ligação **unidirecional** (*one-way*): os dados fluem da classe TypeScript (`.ts`) para o template HTML (`.html`).

Vamos adicionar mais uma propriedade à nossa classe para ver isto em ação.

**`src/app/app.component.ts`**
```typescript
export class AppComponent {
  title = 'A Minha Primeira App Angular';
  autor = 'Sandro Pereira';
  ano = 2024;
}
```

Agora, podemos usar todas estas propriedades no nosso template.

**`src/app/app.component.html`**
```html
<h1>{{ title }}</h1>
<p>Criado por {{ autor }} no ano de {{ ano }}.</p>
```

Podes colocar qualquer expressão JavaScript simples dentro das chavetas: `{{ 1 + 1 }}`, `{{ autor.toUpperCase() }}`, etc.

## 2. Os Estilos (`.scss`) – A Maquilhagem Privada

O ficheiro `app.component.scss` é onde escrevemos os estilos para o nosso componente. A característica mais poderosa aqui é o **encapsulamento de estilos** (*style encapsulation*).

**Isto significa que os estilos que defines aqui aplicam-se APENAS a este componente e não afetam mais nenhuma parte da tua aplicação.**

**Analogia:** A maquilhagem que pões num ator não salta magicamente para o ator que está ao lado dele no palco. Cada um tem o seu próprio estilo.

Vamos experimentar.

**`src/app/app.component.scss`**
```scss
/* Este estilo só se aplica aos <p> dentro do app.component.html */
p {
  font-style: italic;
  color: #555;
  text-align: center;
}
```

Se tiveres um parágrafo `<p>` dentro do teu `header.component.html`, vais reparar que ele **não** fica com este estilo itálico. Isto é incrivelmente útil para evitar conflitos de CSS em aplicações grandes.

## Templates e Estilos *Inline* (Em Linha)

Para componentes muito pequenos, em vez de usares ficheiros separados (`templateUrl` e `styleUrls`), podes escrever o HTML e o CSS diretamente no ficheiro `.ts` usando as propriedades `template` e `styles`.

```typescript
@Component({
  selector: 'app-mini-botao',
  template: `<button class="mini-btn">Clica-me</button>`,
  styles: [`.mini-btn { padding: 5px; }`]
})
export class MiniBotaoComponent { }
```

Embora seja possível, a prática recomendada para a maioria dos casos é usar ficheiros separados, pois mantém o código mais organizado e fácil de ler.

Na próxima lição, vamos ver como podemos "encaixar" componentes uns nos outros e construir uma hierarquia visual.