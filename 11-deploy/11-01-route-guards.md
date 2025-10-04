# Lição 11.1: Route Guards: O Segurança das Tuas Rotas

!Segurança

A nossa aplicação tem várias páginas, mas e se quisermos que algumas delas sejam de acesso restrito? Por exemplo, uma página de "Checkout" ou um painel de "Admin" só devem ser acessíveis a utilizadores autenticados.

Para implementar esta lógica, o Angular Router fornece os **Route Guards** (Guardas de Rota).

**Analogia:** Pensa num segurança à porta de uma área VIP num festival. Antes de te deixar entrar (ativar a rota), ele verifica se tens a pulseira correta (se estás autenticado). Se tiveres, entras. Se não, ele barra-te a passagem e talvez te mande para a bilheteira (a página de login).

O guarda mais comum é o `CanActivate`.

## `CanActivate`: O Guarda Principal

Um guarda `CanActivate` é uma função que corre **antes** de uma rota ser ativada. A função decide se a navegação pode continuar. Ela pode retornar:
-   `true`: A navegação é permitida.
-   `false`: A navegação é cancelada.
-   Um `UrlTree`: A navegação é cancelada e o utilizador é redirecionado para outra rota (ex: a página de login).

## Passo 1: Simular um Serviço de Autenticação

Primeiro, vamos criar um serviço simples que simula o estado de autenticação do utilizador.

**`src/app/services/auth.service.ts`**
```typescript
import { Injectable, signal } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  // Usamos um signal para guardar o estado de login
  isLoggedIn = signal<boolean>(false);

  login(): void {
    this.isLoggedIn.set(true);
  }

  logout(): void {
    this.isLoggedIn.set(false);
  }
}
```

## Passo 2: Criar o Route Guard

Vamos usar o Angular CLI para gerar o nosso guarda.

```sh
ng g guard auth
```

O CLI vai perguntar que tipo de guarda queremos criar. Escolhe `CanActivate` e prime Enter. Ele vai gerar uma função no ficheiro `src/app/auth.guard.ts`.

Agora, vamos implementar a lógica do guarda.

**`src/app/auth.guard.ts`**
```typescript
import { inject } from '@angular/core';
import { CanActivateFn, Router } from '@angular/router';
import { AuthService } from './services/auth.service';

export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isLoggedIn()) {
    return true; // Se o utilizador está logado, permite o acesso.
  }

  // Se não está logado, redireciona para a página de login
  console.warn('Acesso negado! Redirecionando para o login.');
  return router.createUrlTree(['/login']); 
};
```

## Passo 3: Aplicar o Guarda a uma Rota

Finalmente, vamos ao nosso "mapa" (`app-routing.module.ts`) e dizemos ao segurança em que "porta" (rota) ele deve ficar.

**`src/app/app-routing.module.ts` (Exemplo)**
```typescript
import { Routes } from '@angular/router';
import { CheckoutComponent } from './pages/checkout/checkout.component';
import { authGuard } from './auth.guard'; // Importar o nosso guarda

export const routes: Routes = [
  // ... outras rotas ...
  {
    path: 'checkout',
    component: CheckoutComponent,
    canActivate: [authGuard] // Aplicar o guarda a esta rota
  },
];
```

Agora, sempre que um utilizador tentar aceder a `/checkout`, a função `authGuard` será executada. Se `isLoggedIn()` for `false`, o acesso é negado e ele é redirecionado para `/login`.