# Lição 7.1: Template-Driven Forms: A Abordagem Simples

!Formulário de Papel

Os **Template-Driven Forms** (Formulários Guiados pelo Template) são a forma mais rápida de começar a trabalhar com formulários em Angular. A maior parte da lógica, incluindo a validação, é definida diretamente no template HTML.

**Analogia:** É como preencher um **formulário de papel**. As regras (ex: "este campo é obrigatório") estão escritas no próprio papel. É ótimo para formulários simples como uma caixa de pesquisa ou um formulário de login.

## O Papel do `ngModel`

Já conhecemos o `[(ngModel)]` do módulo de *Data Binding*. Nos Template-Driven Forms, ele ganha superpoderes. Quando usado dentro de uma tag `<form>`, o Angular automaticamente cria um objeto na memória que representa o estado do formulário.

Para que isto funcione, o `FormsModule` tem de estar importado no nosso módulo, como já fizemos anteriormente.

## Passo 1: Criar o Componente e o Template

Vamos criar um formulário de login simples.

1.  Gera um novo componente: `ng g c login-form`
2.  No `login-form.component.html`, vamos criar o nosso formulário.

**`src/app/login-form/login-form.component.html`**
```html
<form #loginForm="ngForm" (ngSubmit)="onSubmit(loginForm)">
  <div class="form-group">
    <label for="email">Email</label>
    <input 
      type="email" 
      id="email" 
      name="email" 
      [(ngModel)]="modelo.email"
      required>
  </div>

  <div class="form-group">
    <label for="password">Password</label>
    <input 
      type="password" 
      id="password" 
      name="password" 
      [(ngModel)]="modelo.password"
      required>
  </div>

  <button type="submit">Login</button>
</form>
```

**O que está a acontecer aqui?**
-   `#loginForm="ngForm"`: Estamos a criar uma variável de referência de template (`#loginForm`) e a exportar a diretiva `ngForm` para ela. Agora, `loginForm` é um objeto que contém todo o estado do nosso formulário (valores, se é válido, etc.).
-   `(ngSubmit)="onSubmit(loginForm)"`: Em vez de usarmos o evento `(submit)` normal, usamos o `(ngSubmit)` do Angular. Ele garante que o formulário só é submetido se for válido e previne o recarregamento da página. Passamos o nosso objeto `loginForm` para o método.
-   `name="email"`: Para que o `ngModel` funcione dentro de um `<form>`, cada input **precisa** de ter um atributo `name`.
-   `required`: Este é um atributo de validação HTML normal. O Angular reconhece-o e usa-o para determinar se o formulário é válido.

## Passo 2: A Lógica no Componente

O nosso componente é bastante simples. Ele só precisa de um objeto para guardar os dados do formulário e de um método para lidar com a submissão.

**`src/app/login-form/login-form.component.ts`**
```typescript
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-login-form',
  templateUrl: './login-form.component.html',
  styleUrls: ['./login-form.component.scss']
})
export class LoginFormComponent {
  // Objeto para guardar os valores do formulário
  modelo = {
    email: '',
    password: ''
  };

  onSubmit(form: NgForm) {
    console.log('Formulário submetido!');
    console.log('Valores:', form.value); // O objeto com os valores do formulário
    console.log('Válido?', form.valid); // true ou false
  }
}
```

Os Template-Driven Forms são ótimos para começar, mas para formulários mais complexos, com validações dinâmicas ou campos que dependem uns dos outros, eles podem tornar-se limitados. Na próxima lição, vamos ver a abordagem mais poderosa e escalável: os **Reactive Forms**.