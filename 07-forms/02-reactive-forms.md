# Lição 7.2: Reactive Forms: Controlo Total e Escalabilidade

!App de Pizzaria

Enquanto os Template-Driven Forms são ótimos para casos simples, os **Reactive Forms** oferecem uma abordagem mais robusta, escalável e testável. A lógica e o estado do formulário são definidos e geridos de forma explícita na classe do componente.

**Analogia:** É como usar a **app da pizzaria**. A lógica está toda no teu telemóvel (a classe do componente). Tu constróis o teu pedido (o `FormGroup`) no código, e a UI (o template) apenas se "liga" a esse modelo de dados. Tens controlo total e podes fazer coisas complexas, como adicionar ou remover ingredientes dinamicamente.

## Os Blocos de Construção dos Reactive Forms

-   **`FormControl`**: Representa um controlo individual de formulário (um `<input>`, um `<select>`).
-   **`FormGroup`**: Agrupa uma coleção de `FormControl`s. O nosso formulário inteiro será um `FormGroup`.
-   **`FormBuilder`**: Um serviço "ajudante" que nos dá uma sintaxe mais limpa para criar `FormGroup`s e `FormControl`s.

## Passo 1: Importar o `ReactiveFormsModule`

Tal como o `FormsModule`, os Reactive Forms vivem no seu próprio módulo.

**`src/app/app.module.ts`**
```typescript
import { ReactiveFormsModule } from '@angular/forms'; // 1. Importar

@NgModule({
  // ...
  imports: [
    BrowserModule,
    // FormsModule, // Já não precisamos deste para o nosso novo formulário
    ReactiveFormsModule // 2. Adicionar aos imports
  ],
  // ...
})
export class AppModule { }
```

## Passo 2: Criar o Formulário na Classe do Componente

Vamos recriar o nosso formulário de login, mas desta vez com a abordagem reativa.

1.  Gera um novo componente: `ng g c reactive-login-form`
2.  No ficheiro `.ts`, vamos construir o nosso modelo de formulário.

**`src/app/reactive-login-form/reactive-login-form.component.ts`**
```typescript
import { Component } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';

@Component({
  selector: 'app-reactive-login-form',
  templateUrl: './reactive-login-form.component.html',
  styleUrls: ['./reactive-login-form.component.scss']
})
export class ReactiveLoginFormComponent {
  // A propriedade que vai guardar o nosso FormGroup
  loginForm: FormGroup;

  // Injetamos o FormBuilder para nos ajudar
  constructor(private fb: FormBuilder) {
    // Criamos o nosso modelo de formulário
    this.loginForm = this.fb.group({
      email: [''], // O valor inicial é uma string vazia
      password: ['']
    });
  }

  onSubmit() {
    console.log('Formulário submetido!');
    console.log('Valores:', this.loginForm.value);
  }
}
```

## Passo 3: Ligar o Template ao Modelo

O nosso template agora fica muito mais "burro". Ele apenas se liga ao `FormGroup` que já existe na classe.

**`src/app/reactive-login-form/reactive-login-form.component.html`**
```html
<!-- Ligamos a tag <form> ao nosso FormGroup 'loginForm' -->
<form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
  <div class="form-group">
    <label for="email">Email</label>
    <!-- Ligamos este input ao control 'email' dentro do FormGroup -->
    <input type="email" id="email" formControlName="email">
  </div>

  <div class="form-group">
    <label for="password">Password</label>
    <input type="password" id="password" formControlName="password">
  </div>

  <button type="submit">Login</button>
</form>
```

Repara que já não usamos `[(ngModel)]`. Em vez disso, usamos `[formGroup]` no formulário e `formControlName` em cada input para criar as ligações.

A grande vantagem é que toda a "verdade" sobre o formulário (os seus valores, o seu estado de validade, etc.) vive agora na nossa classe TypeScript, onde é fácil de aceder, manipular e testar.

Na próxima lição, vamos usar o poder dos Reactive Forms para adicionar validações complexas e mostrar mensagens de erro ao utilizador.