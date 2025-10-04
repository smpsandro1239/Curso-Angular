# Lição 6.3: Rotas com Parâmetros: Apanhar Dados do URL

!ID no URL

As nossas rotas atuais são estáticas. `/tarefas` mostra sempre o mesmo componente. Mas e se quisermos ter uma página de detalhe para cada tarefa? Precisamos de um URL dinâmico, como `/tarefas/1` ou `/tarefas/2`. A parte do URL que muda (`1`, `2`) é um **parâmetro de rota**.

**Analogia:** Pensa no site da Zara. `/mulher/casacos` mostra-te todos os casacos. `/mulher/casacos/87654321` mostra-te um casaco específico. O número `87654321` é o parâmetro que diz à aplicação qual o produto a mostrar.

## Passo 1: Definir uma Rota com Parâmetro

Definimos um parâmetro no nosso ficheiro `app-routing.module.ts` usando a sintaxe de dois pontos `:`.

Vamos criar um componente de detalhe para as nossas tarefas.

1.  Gera um novo componente: `ng g c pages/task-detail`
2.  Atualiza o `app-routing.module.ts`:

**`src/app/app-routing.module.ts` (Atualizado)**
```typescript
// ... imports ...
import { TaskDetailComponent } from './pages/task-detail/task-detail.component';

const routes: Routes = [
  // ... outras rotas ...
  
  // Nova rota com um parâmetro 'id'
  { path: 'tarefas/:id', component: TaskDetailComponent },
];
```

A parte `:id` diz ao Angular: "qualquer coisa que venha depois de `/tarefas/` deve ser tratada como um parâmetro chamado `id`".

## Passo 2: Navegar para a Rota com Parâmetro

No nosso `TodoListComponent`, podemos agora criar um link para a página de detalhe de cada tarefa.

**`src/app/todo-list/todo-list.component.html`**
```html
<ul>
  <li *ngFor="let tarefa of tarefas">
    <!-- Usamos property binding no routerLink para passar um array.
         O primeiro elemento é o path estático, os seguintes são os parâmetros. -->
    <a [routerLink]="['/tarefas', tarefa.id]">{{ tarefa.descricao }}</a>
  </li>
</ul>
```

## Passo 3: Ler o Parâmetro no Componente

Ok, o utilizador navegou para `/tarefas/2`. Como é que o `TaskDetailComponent` sabe que o `id` é `2`?

Para isso, injetamos um serviço especial do Router chamado `ActivatedRoute`. Ele dá-nos acesso à informação sobre a rota atualmente ativa, incluindo os seus parâmetros.

**`src/app/pages/task-detail/task-detail.component.ts`**
```typescript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router'; // 1. Importar

@Component({
  selector: 'app-task-detail',
  templateUrl: './task-detail.component.html',
  styleUrls: ['./task-detail.component.scss']
})
export class TaskDetailComponent implements OnInit {
  tarefaId: string | null = null;

  // 2. Injetar o ActivatedRoute
  constructor(private route: ActivatedRoute) { }

  ngOnInit(): void {
    // 3. Ler o parâmetro do URL
    this.tarefaId = this.route.snapshot.paramMap.get('id');
  }
}
```

Agora, a propriedade `tarefaId` terá o valor que veio do URL, e podemos usá-la no nosso template ou para ir buscar os dados específicos dessa tarefa a um serviço.

---

**Módulo 6 Concluído!** 🏆

Agora sabes como estruturar uma aplicação Angular com múltiplas páginas, navegar entre elas e criar rotas dinâmicas com parâmetros. Estás pronto para o projeto do módulo: construir o teu primeiro mini-site com navegação!