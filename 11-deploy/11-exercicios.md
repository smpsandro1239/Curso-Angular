# Módulo 11: Exercícios Práticos – Finalizar a Sneaker Store

Vamos aplicar os conceitos avançados de routing e deployment para dar os toques finais de profissionalismo à nossa loja de sneakers.

---

## 🥉 Exercício Nível Bronze: Proteger a Rota de Checkout

**O Problema:** Queremos ter uma página de "Checkout" que só seja acessível a utilizadores autenticados.

**A tua Missão:**

1.  Cria um serviço de autenticação simples: `ng g s services/auth`.
2.  No `auth.service.ts`, cria um `signal` `isLoggedIn` que começa como `false`, e dois métodos: `login()` (que o define para `true`) e `logout()` (que o define para `false`).
3.  Cria um novo componente para a página de checkout: `ng g c pages/checkout`.
4.  No teu ficheiro de rotas principal, adiciona a rota para `/checkout` que aponta para o `CheckoutComponent`.
5.  Cria um guarda de rota: `ng g guard guards/auth`. Escolhe `CanActivate`.
6.  No `auth.guard.ts`, implementa a lógica: injeta o `AuthService` e o `Router`. Se `isLoggedIn()` for `true`, retorna `true`. Caso contrário, redireciona para a página de login (`/login`) e retorna `false`.
7.  Aplica o guarda à tua rota de checkout: `canActivate: [authGuard]`.

---

## 🥈 Exercício Nível Prata: Otimizar com Lazy Loading

**O Problema:** A nossa secção de checkout não precisa de ser carregada quando o utilizador abre a loja. Vamos carregá-la apenas quando for necessária.

**A tua Missão:**

1.  Cria um novo ficheiro de rotas dedicado apenas à funcionalidade de checkout, por exemplo, `src/app/pages/checkout/checkout.routes.ts`.
2.  Move a definição da rota de checkout do ficheiro principal para este novo ficheiro. Exporta a constante com as rotas (ex: `export const CHECKOUT_ROUTES: Routes = [...]`).
3.  No ficheiro de rotas principal, altera a rota `path: 'checkout'`. Em vez de usar `component`, usa `loadChildren` para carregar dinamicamente o teu novo ficheiro de rotas.
    ```typescript
    loadChildren: () => import('./pages/checkout/checkout.routes').then(m => m.CHECKOUT_ROUTES)
    ```
4.  Verifica nas ferramentas de developer do browser (separador Network) que, ao navegar para `/checkout` pela primeira vez, um novo ficheiro JavaScript é descarregado.

---

## 🥇 Projeto Nível Ouro: Build e Deploy para o Mundo

**O Problema:** A nossa aplicação só existe no nosso computador. É hora de a partilhar.

**A tua Missão:**

1.  Certifica-te de que o teu projeto está num repositório do GitHub e que todas as alterações estão "pushadas".
2.  Cria uma conta na Vercel usando a tua conta do GitHub.
3.  No dashboard da Vercel, importa o teu repositório da Sneaker Store.
4.  A Vercel deve detetar automaticamente que é um projeto Angular. Confirma as configurações (não deves precisar de alterar nada) e clica em "Deploy".
5.  Aguarda a conclusão do processo. Quando terminar, a Vercel vai dar-te um URL público.
6.  Partilha o link da tua loja online no Discord do curso! Parabéns, publicaste a tua primeira aplicação Angular profissional!