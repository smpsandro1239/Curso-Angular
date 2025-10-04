# Lição 6.1: Configurar o Router: O Mapa da Nossa App

!Mapa

Quando criámos o nosso projeto com `ng new`, respondemos "sim" à pergunta sobre o *routing*. O Angular CLI foi simpático e criou-nos um ficheiro `src/app/app-routing.module.ts`. Este ficheiro é o "mapa" da nossa aplicação. É aqui que dizemos ao Angular: "quando o utilizador for para o URL `/sobre`, mostra o `SobreComponent`".

## A Anatomia do `app-routing.module.ts`

Vamos abrir o ficheiro. Ele parece-se com isto:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

// 1. O Array de Rotas
const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

A parte mais importante para nós é a constante `routes`. É um array de objetos, onde cada objeto define uma rota.

## Passo 1: Criar os Componentes das Páginas

Uma boa prática é ter componentes que representam páginas inteiras. Vamos criar três: `Home`, `Tarefas` e `Sobre`. Para as manter organizadas, vamos criá-las dentro de uma pasta `pages`.

Corre os seguintes comandos no terminal:

```sh
ng g c pages/home
ng g c pages/tasks
ng g c pages/about
```

## Passo 2: Definir as Rotas

Agora, vamos preencher o nosso "mapa" no `app-routing.module.ts`.

Cada objeto de rota tem, no mínimo, duas propriedades:
-   `path`: A string que aparece no URL (sem a barra `/`).
-   `component`: O componente que deve ser renderizado para esse `path`.

**`src/app/app-routing.module.ts` (Atualizado)**
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

// Importar os nossos novos componentes de página
import { HomeComponent } from './pages/home/home.component';
import { TasksComponent } from './pages/tasks/tasks.component';
import { AboutComponent } from './pages/about/about.component';

const routes: Routes = [
  // Rota para a página de início
  { path: 'inicio', component: HomeComponent },

  // Rota para a lista de tarefas
  { path: 'tarefas', component: TasksComponent },

  // Rota para a página "Sobre"
  { path: 'sobre', component: AboutComponent },

  // Rota de Redirecionamento: se o URL estiver vazio, redireciona para '/inicio'
  { path: '', redirectTo: '/inicio', pathMatch: 'full' },

  // Rota Wildcard (Coringa): se o URL não corresponder a nenhuma rota, mostra um componente de "Página não encontrada"
  // (teríamos de criar um componente para isto, por agora podemos ignorar)
  // { path: '**', component: NotFoundComponent } 
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

Definimos o nosso mapa! Agora, se visitares `http://localhost:4200/tarefas`, o Angular sabe que deve usar o `TasksComponent`.

Mas... nada aparece no ecrã. Porquê? Porque já dissemos ao Angular *o que* mostrar, mas ainda não lhe dissemos *onde* o deve mostrar. É o que vamos ver na próxima lição com a diretiva `<router-outlet>`.