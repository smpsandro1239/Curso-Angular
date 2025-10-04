# Lição 7.3: Validação de Formulários

!Segurança a verificar

Um formulário sem validação é como uma festa sem segurança na porta: qualquer um entra. A validação garante que os dados que recebemos do utilizador estão no formato correto antes de os processarmos.

Com Reactive Forms, a validação é configurada diretamente na nossa classe TypeScript, o que a torna explícita e fácil de testar.

## Adicionar Validadores com `FormBuilder`

O Angular oferece uma classe `Validators` com vários validadores incorporados. Passamo-los como o segundo argumento no array ao criar um `FormControl`.

Vamos adicionar validações ao nosso formulário de login.

**`src/app/reactive-login-form/reactive-login-form.component.ts` (Atualizado)**
```typescript
import { Component } from '@angular/core';
// Importar também o Validators
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  // ...
})
export class ReactiveLoginFormComponent {
  loginForm: FormGroup;

  constructor(private fb: FormBuilder) {
    this.loginForm = this.fb.group({
      // [valorInicial, validadoresSincronos, validadoresAssincronos]
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(8)]]
    });
  }

  onSubmit() {
    // Boa prática: só processar se o formulário for válido
    if (this.loginForm.invalid) {
      console.log('Formulário inválido!');
      return;
    }

    console.log('Formulário submetido!');
    console.log('Valores:', this.loginForm.value);
  }
}
```

Agora, o nosso `email` tem de ser um email válido e não pode estar vazio, e a `password` tem de ter no mínimo 8 caracteres.

## Mostrar Mensagens de Erro na UI

Como é que mostramos ao utilizador que algo está errado? Podemos aceder ao estado de cada `FormControl` para verificar se ele tem erros.

Um `FormControl` tem várias propriedades úteis:
-   `valid` / `invalid`: Se o controlo é válido ou inválido.
-   `touched` / `untouched`: Se o utilizador já interagiu com o campo (clicou e saiu).
-   `dirty` / `pristine`: Se o valor do campo foi alterado.
-   `errors`: Um objeto que contém os erros de validação (ex: `{ required: true }`).

A melhor prática é mostrar uma mensagem de erro apenas se o campo for **inválido E já tiver sido tocado** (`invalid && touched`).

### Criar um Getter para Acesso Fácil

Para evitar escrever `this.loginForm.controls.email` muitas vezes, podemos criar um *getter* na nossa classe.

**`...component.ts` (Adicionar o getter)**
```typescript
export class ReactiveLoginFormComponent {
  // ...
  
  // Getter para aceder facilmente aos controlos do formulário no template
  get f() { return this.loginForm.controls; }

  // ...
}
```

### Atualizar o Template

**`src/app/reactive-login-form/reactive-login-form.component.html` (Atualizado)**
```html
<form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
  <div class="form-group">
    <label for="email">Email</label>
    <input type="email" id="email" formControlName="email">
    
    <!-- Mensagens de Erro para o Email -->
    <div *ngIf="f.email.invalid && (f.email.dirty || f.email.touched)" class="error-message">
      <div *ngIf="f.email.errors?.['required']">O email é obrigatório.</div>
      <div *ngIf="f.email.errors?.['email']">Por favor, insere um email válido.</div>
    </div>
  </div>

  <!-- ... campo da password com lógica de erro semelhante ... -->

  <!-- Desativar o botão se o formulário for inválido -->
  <button type="submit" [disabled]="loginForm.invalid">Login</button>
</form>
```

Agora temos um formulário robusto com validação em tempo real e feedback claro para o utilizador.

---

**Módulo 7 Concluído!** 🏆

Dominaste as duas estratégias de formulários em Angular e sabes como implementar validações robustas. Estás pronto para o projeto do módulo: o **Formulário de Registo de Sneakerhead**.