# Módulo 11: Soluções dos Exercícios – Finalizar a Sneaker Store

Aqui está a solução completa para o projeto, que adiciona uma camada de profissionalismo à nossa aplicação com proteção de rotas e otimização de carregamento.

---

### `src/app/services/auth.service.ts` (Solução Bronze)

Um serviço simples para simular o estado de autenticação.

```typescript
import { Injectable, signal } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  isLoggedIn = signal<boolean>(false);

  login(): void {
    this.isLoggedIn.set(true);
    console.log('Utilizador autenticado.');
  }

  logout(): void {
    this.isLoggedIn.set(false);
    console.log('Sessão terminada.');
  }
}
```

---

### `src/app/guards/auth.guard.ts` (Solução Bronze)

O nosso "segurança" que verifica se o utilizador pode ou não aceder a uma rota.

```typescript
import { inject } from '@angular/core';
import { CanActivateFn, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isLoggedIn()) {
    return true; // Acesso permitido
  }

  // Se não estiver logado, redireciona para a página de login e bloqueia a navegação atual.
  return router.createUrlTree(['/login']);
};
```

---

### `src/app/pages/checkout/checkout.routes.ts` (Solução Prata)

Criamos um ficheiro de rotas dedicado à funcionalidade de checkout para permitir o Lazy Loading.

```typescript
import { Routes } from '@angular/router';
import { CheckoutComponent } from './checkout.component';

export const CHECKOUT_ROUTES: Routes = [
  {
    path: '', // O path vazio corresponde a '/checkout' por causa da rota pai
    component: CheckoutComponent
  }
];
```

---

### `src/app/app.routes.ts` (Ficheiro de Rotas Principal - Solução Prata)

O ficheiro principal agora carrega a secção de checkout de forma "preguiçosa" e aplica o guarda a essa secção.

```typescript
import { Routes } from '@angular/router';
import { authGuard } from './guards/auth.guard';
import { HomeComponent } from './pages/home/home.component';
import { LoginComponent } from './pages/login/login.component';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'login', component: LoginComponent },
  // ... outras rotas ...

  {
    path: 'checkout',
    loadChildren: () => import('./pages/checkout/checkout.routes').then(m => m.CHECKOUT_ROUTES),
    canActivate: [authGuard] // O guarda protege o acesso a todo o grupo de rotas
  }
];
```

### Projeto Nível Ouro: Deploy

A solução para o nível Ouro não é código, mas sim o processo descrito na lição 11.4. O resultado final é teres a tua aplicação a correr num URL público da Vercel (ex: `sneaker-store-teunome.vercel.app`), com o teu repositório do GitHub ligado para deployments automáticos.