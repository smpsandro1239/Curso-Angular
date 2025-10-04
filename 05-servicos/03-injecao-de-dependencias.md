# Lição 5.3: Injeção de Dependências: A Magia do Angular

!Mágica

Temos o nosso `TaskService` pronto a ser usado, e o nosso `TodoListComponent` a precisar desesperadamente dele. Como é que os ligamos?

Não fazemos `new TaskService()` dentro do componente. Isso criaria uma nova instância do serviço, e perderíamos a vantagem de ter uma fonte única de dados. Em vez disso, nós simplesmente **pedimos** ao Angular para nos dar o serviço. A este padrão chama-se **Injeção de Dependências (Dependency Injection - DI)**.

**Analogia:** A tua app do Uber (`Componente`) precisa do serviço de GPS. Ela não constrói uma antena de GPS nova. Ela simplesmente diz ao sistema operativo (`Angular`): "Eu preciso de acesso ao serviço de GPS". O sistema operativo, que gere um único serviço de GPS para todo o telemóvel, "injeta" ou fornece esse acesso à tua app.

## Como Pedir um Serviço: O `constructor`

Em Angular, pedimos as dependências (como os serviços) no **`constructor`** da nossa classe de componente. O `constructor` é um método especial que é chamado quando uma nova instância da classe é criada.

A sintaxe em TypeScript torna isto muito elegante:

**`src/app/todo-list/todo-list.component.ts`**
```typescript
import { Component } from '@angular/core';
import { TaskService } from '../services/task.service'; // 1. Importar o serviço

@Component({
  // ...
})
export class TodoListComponent {
  // ...

  // 2. Pedir o serviço no constructor
  constructor(private taskService: TaskService) {}

  // ...
}
```

O que é que `private taskService: TaskService` está a fazer?
-   Diz ao Angular: "Quando criares este componente, por favor injeta uma instância do `TaskService` aqui."
-   O `private` é um atalho do TypeScript que automaticamente cria uma propriedade privada na nossa classe chamada `taskService` e atribui-lhe a instância injetada.

Agora, dentro do nosso componente, podemos aceder ao serviço e a todos os seus métodos públicos através de `this.taskService`.

## Refatorar o `TodoListComponent`

Vamos agora refatorar completamente o nosso componente para delegar toda a gestão de dados ao serviço.

### O Hook `ngOnInit`

Onde é que vamos buscar a lista inicial de tarefas? Não o fazemos no `constructor`. O `constructor` deve ser mantido simples, apenas para a injeção de dependências.

Para tarefas de inicialização, usamos um **Lifecycle Hook** (Gancho de Ciclo de Vida) do Angular chamado `ngOnInit`. É um método que o Angular chama automaticamente **depois** de o componente ser criado. É o local perfeito para ir buscar dados iniciais.

**`src/app/todo-list/todo-list.component.ts` (Versão Final Refatorada)**
```typescript
import { Component, OnInit } from '@angular/core'; // Importar OnInit
import { TaskService } from '../services/task.service';

@Component({
  selector: 'app-todo-list',
  templateUrl: './todo-list.component.html',
  styleUrls: ['./todo-list.component.scss']
})
export class TodoListComponent implements OnInit { // Implementar a interface OnInit
  // Este componente já não tem o seu próprio array de tarefas.
  // Ele vai buscar os dados ao serviço.
  tarefas: any[] = []; // Começa como um array vazio
  novaTarefaDescricao = '';

  constructor(private taskService: TaskService) {}

  ngOnInit(): void {
    // Quando o componente inicializa, vamos buscar as tarefas ao serviço.
    this.tarefas = this.taskService.getTarefas();
  }

  adicionarTarefa() {
    if (this.novaTarefaDescricao.trim() === '') return;
    // Delega a lógica de adição ao serviço
    this.taskService.adicionarTarefa(this.novaTarefaDescricao);
    this.novaTarefaDescricao = '';
  }

  toggleConcluida(tarefaId: number) {
    // Delega a lógica de alteração ao serviço
    this.taskService.toggleConcluida(tarefaId);
  }
}
```

Repara como o nosso componente ficou muito mais "limpo". A sua única responsabilidade é agora mostrar os dados que o serviço lhe dá e dizer ao serviço quando algo acontece (um clique). Ele não sabe (nem precisa de saber) como as tarefas são guardadas ou manipuladas.

---

**Módulo 5 Concluído!** 🏆

Dominaste um dos conceitos mais importantes e poderosos do Angular. Agora sabes como separar as responsabilidades na tua aplicação, criando serviços reutilizáveis e usando a Injeção de Dependências para os fornecer aos teus componentes.

No próximo módulo, vamos aprender a criar uma aplicação com múltiplas "páginas" usando o **Router** do Angular.