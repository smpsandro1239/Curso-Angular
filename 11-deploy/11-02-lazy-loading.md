# Lição 11.2: Lazy Loading: Carregar Código Só Quando é Preciso

!Netflix

Quando a nossa aplicação cresce, o ficheiro JavaScript principal (o *bundle*) também cresce. Por defeito, quando um utilizador visita o nosso site, ele descarrega o código de **toda** a aplicação, mesmo das páginas que ele talvez nunca visite, como um painel de administração. Isto torna o carregamento inicial mais lento.

O **Lazy Loading** (Carregamento Preguiçoso) é a solução. É uma técnica que nos permite dividir a nossa aplicação em pedaços mais pequenos (chamados *chunks*) e carregar esses pedaços apenas quando são necessários.

**Analogia:** Pensa na Netflix. Quando abres a app, não descarregas todas as temporadas de todas as séries. Apenas descarregas os dados e o vídeo do episódio que clicas para ver. O Lazy Loading faz o mesmo com as funcionalidades da tua aplicação.

## Como Funciona?

Em vez de associarmos um componente diretamente a uma rota, dizemos ao Angular para carregar um conjunto de rotas ou um componente de forma assíncrona quando o utilizador navega para um determinado `path`.

## Passo 1: Organizar a Aplicação em Funcionalidades (Features)

O Lazy Loading funciona melhor quando agrupamos as nossas rotas por funcionalidade. Vamos imaginar que temos uma secção de "Admin" na nossa loja, com várias páginas (Dashboard, Gerir Produtos, etc.).

1.  Primeiro, criamos os componentes para esta secção:
    ```sh
    ng g c pages/admin/dashboard
    ng g c pages/admin/manage-products
    ```

2.  Depois, criamos um ficheiro de rotas **apenas para a secção de admin**.

**`src/app/pages/admin/admin.routes.ts` (Novo Ficheiro)**
```typescript
import { Routes } from '@angular/router';
import { DashboardComponent } from './dashboard/dashboard.component';
import { ManageProductsComponent } from './manage-products/manage-products.component';

export const ADMIN_ROUTES: Routes = [
  {
    path: '', // O path vazio (ex: /admin) vai para o dashboard
    component: DashboardComponent,
    title: 'Admin Dashboard'
  },
  {
    path: 'products', // O path /admin/products
    component: ManageProductsComponent,
    title: 'Gerir Produtos'
  }
];
```

## Passo 2: Configurar o Lazy Loading na Rota Principal

Agora, no nosso ficheiro de rotas principal (`app-routing.module.ts`), em vez de definirmos todas as rotas de admin, vamos apenas apontar para este novo ficheiro usando a propriedade `loadChildren`.

**`src/app/app-routing.module.ts` (Atualizado)**
```typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  // ... outras rotas ...
  {
    path: 'admin',
    // Em vez de 'component', usamos 'loadChildren'.
    // Usamos uma função que faz um import() dinâmico do nosso ficheiro de rotas.
    loadChildren: () => import('./pages/admin/admin.routes').then(m => m.ADMIN_ROUTES)
  },
];
```

## O que Acontece Agora?

1.  Quando a aplicação carrega, o código dos componentes `DashboardComponent` e `ManageProductsComponent` **não é incluído** no *bundle* principal. A aplicação carrega mais rápido.
2.  Apenas quando o utilizador tenta navegar para um URL que começa com `/admin` (ex: clicando num link), o Angular:
    a.  Vai buscar o ficheiro JavaScript correspondente a essa funcionalidade (o *chunk* de admin).
    b.  Carrega as rotas definidas em `ADMIN_ROUTES`.
    c.  Renderiza o componente correto.

## Verificação

Abre as ferramentas de developer do teu browser (F12) e vai ao separador "Network". Quando navegares para `/admin` pela primeira vez, verás um novo ficheiro JavaScript a ser descarregado. Nas visitas seguintes, ele já estará em cache.

Usar Lazy Loading é uma das melhores práticas e uma das formas mais eficazes de garantir que a tua aplicação Angular é rápida e escalável.