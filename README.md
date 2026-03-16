# App-blog-em-angular

Para criar uma aplicação de blog funcional em Angular, você pode seguir uma arquitetura organizada e usar componentes inteligentes. Aqui está um passo a passo básico para ajudá-lo a configurar seu projeto.

# Estrutura do Projeto

1. Criar um Novo Projeto:
   ```bash
   ng new blog-app
   cd blog-app
   ```

2. Gerar Componentes:
   - Componentes que podem ser úteis:
     ```bash
     ng generate component components/header
     ng generate component components/footer
     ng generate component components/post-list
     ng generate component components/post-detail
     ng generate component components/create-post
     ```

3. Criar um Serviço para Gerenciar Postagens:
   ```bash
   ng generate service services/post
   ```

# Organizando o Projeto

# 1. Componentes:
   - Header: Componente para a barra de navegação.
   - Footer: Componente de rodapé.
   - Post List: Componente que mostra todos os posts.
   - Post Detail: Componente para exibir detalhes de um post específico.
   - Create Post: Componente para criar novos posts.

# 2. Serviços:
   O serviço `PostService` deve gerenciar a lógica de negócios, como obter e criar postagens. Aqui está um exemplo simples de como ele pode ser estruturado:

```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

export interface Post {
  id: number;
  title: string;
  content: string;
}

@Injectable({
  providedIn: 'root'
})
export class PostService {
  private posts: Post[] = [];
  private postsSubject = new BehaviorSubject<Post[]>(this.posts);

  getPosts() {
    return this.postsSubject.asObservable();
  }

  addPost(post: Post) {
    this.posts.push(post);
    this.postsSubject.next(this.posts);
  }
}
```

# 3. Routing:
   Configure o roteamento para navegar entre componentes. Edite `app-routing.module.ts`:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { PostListComponent } from './components/post-list/post-list.component';
import { CreatePostComponent } from './components/create-post/create-post.component';
import { PostDetailComponent } from './components/post-detail/post-detail.component';

const routes: Routes = [
  { path: '', component: PostListComponent },
  { path: 'create', component: CreatePostComponent },
  { path: 'post/:id', component: PostDetailComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

# Implementação dos Componentes

# 1. Header Component:
   Exiba links de navegação.

```html
<!-- header.component.html -->
<nav>
  <a routerLink="/">Home</a>
  <a routerLink="/create">Create Post</a>
</nav>
```

# 2. Post List Component:
   Exiba a lista de postagens.

```typescript
// post-list.component.ts
import { Component, OnInit } from '@angular/core';
import { PostService, Post } from '../../services/post.service';

@Component({
  selector: 'app-post-list',
  templateUrl: './post-list.component.html',
})
export class PostListComponent implements OnInit {
  posts: Post[] = [];

  constructor(private postService: PostService) {}

  ngOnInit() {
    this.postService.getPosts().subscribe((posts) => {
      this.posts = posts;
    });
  }
}
```

# 3. Criar Post Component:
   Formulário para adicionar novos posts.

```typescript
// create-post.component.ts
import { Component } from '@angular/core';
import { PostService, Post } from '../../services/post.service';

@Component({
  selector: 'app-create-post',
  templateUrl: './create-post.component.html',
})
export class CreatePostComponent {
  title: string = '';
  content: string = '';

  constructor(private postService: PostService) {}

  addPost() {
    const newPost: Post = { id: Date.now(), title: this.title, content: this.content };
    this.postService.addPost(newPost);
    this.title = '';
    this.content = '';
  }
}
```

# Finalizando

1. Adicionar Estilos: Use CSS ou frameworks como Bootstrap ou Tailwind CSS para estilizar sua aplicação.
2. Testar: Execute sua aplicação com:
   ```bash
   ng serve
   ```
   Acesse `http://localhost:4200` para ver seu blog em ação.

3. Implementar Funcionalidades: Adicione funcionalidades como edição, exclusão e persistência em um banco de dados real, usando APIs.

Seguindo esses passos, você terá uma aplicação de blog funcional com Angular, utilizando componentes inteligentes e uma estrutura organizada.
