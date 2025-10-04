# Lição 3.2: Event Binding (Da UI para a Lógica)

!Seta para a Lógica

O *Event Binding* permite-nos ouvir e reagir a eventos do utilizador no nosso template. A informação flui agora da UI para a classe do componente.

**Analogia:** Pensa no botão de "like" do Instagram. Quando **tu** tocas no botão (um evento na UI), a aplicação reage: o coração fica vermelho e o contador de likes aumenta (a lógica é executada).

A sintaxe do *Event Binding* usa parênteses `()` à volta do nome do evento HTML (como `click`, `mouseover`, `keyup`, etc.).

` (evento)="funcaoAExecutar()" `

## Exemplo: Um Contador de Cliques

Vamos criar um botão que, cada vez que é clicado, incrementa um contador na nossa classe.

**`meu-componente.component.ts`**
```typescript
export class MeuComponenteComponent {
  numeroDeCliques = 0;

  // Criamos um método (uma função dentro de uma classe) para lidar com o evento.
  incrementarCliques() {
    this.numeroDeCliques++; // 'this' refere-se à própria classe
    console.log(`O botão foi clicado ${this.numeroDeCliques} vezes.`);
  }
}
```

**`meu-componente.component.html`**
```html
<!-- Quando o evento 'click' acontece no botão, o método 'incrementarCliques()' é chamado. -->
<button (click)="incrementarCliques()">Clica em Mim!</button>

<p>Número de cliques: {{ numeroDeCliques }}</p>
```

Agora, cada vez que clicas no botão:
1.  O evento `click` é disparado.
2.  O Angular chama o método `incrementarCliques()`.
3.  O método atualiza a propriedade `numeroDeCliques`.
4.  Como a propriedade mudou, a interpolação `{{ numeroDeCliques }}` na UI é automaticamente atualizada pelo Angular.

## O Objeto `$event`

Por vezes, precisamos de informação sobre o próprio evento. Por exemplo, qual a tecla que foi pressionada? Ou qual o valor de um campo de input?

O Angular dá-nos acesso a um objeto especial, `$event`, que podemos passar para o nosso método.

### Exemplo: Ler o Valor de um Input

**`meu-componente.component.ts`**
```typescript
export class MeuComponenteComponent {
  textoAtual = '';

  // O método agora aceita um argumento do tipo 'any' (qualquer tipo)
  onInput(event: any) {
    // O valor do input está em event.target.value
    this.textoAtual = event.target.value;
  }
}
```

**`meu-componente.component.html`**
```html
<!-- O evento 'input' é disparado cada vez que o valor do campo muda. -->
<input type="text" (input)="onInput($event)" placeholder="Escreve aqui...">

<p>Texto em tempo real: {{ textoAtual }}</p>
```

Agora, à medida que escreves no campo de input, o parágrafo por baixo atualiza-se em tempo real!

Este padrão é tão comum que o Angular nos dá um atalho ainda mais poderoso para o fazer, que vamos ver na próxima lição: o **Two-Way Data Binding**.