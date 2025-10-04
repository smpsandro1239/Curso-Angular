# Módulo 11: Routing Avançado e Deployment

![Foguetão](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExbWw5a2N5c3JqY254b250b294c251b294c251b294c251b294/mi6DsBqWEpGSs/giphy.gif)

A nossa aplicação está a ficar complexa e poderosa. Agora, vamos dar-lhe os toques finais de um projeto profissional: otimizar o seu carregamento, proteger certas áreas e, finalmente, lançá-la para o mundo!

Este módulo foca-se em levar a tua aplicação do ambiente de desenvolvimento para um ambiente de produção real.

**Analogia:** Já construíste o teu carro de corrida. Agora, vamos afiná-lo para a performance máxima, instalar sistemas de segurança para o piloto e transportá-lo para o circuito para a grande corrida.

---

## 🎯 Objetivos de Aprendizagem

No final deste módulo, serás capaz de:

- ✅ Proteger rotas com **Route Guards** (ex: `CanActivate`), impedindo o acesso a utilizadores não autenticados.
- ✅ Otimizar o tempo de carregamento inicial da aplicação com **Lazy Loading** de módulos.
- ✅ Pré-carregar dados para uma rota antes de o componente ser ativado com **Resolvers**.
- ✅ Entender a diferença entre um build de desenvolvimento e um de produção.
- ✅ Fazer o **build de produção** da tua aplicação com o comando `ng build`.
- ✅ Fazer o **deploy** (publicar) da tua aplicação Angular para uma plataforma global como a Vercel ou o Netlify.

## 🛠️ Pré-requisitos

- Domínio de Rotas (Módulo 6).
- Domínio de Serviços e `HttpClient` (Módulo 8).

---

## 📚 Lições

1.  **Route Guards: O Segurança das Tuas Rotas**
2.  **Lazy Loading: Carregar Código Só Quando é Preciso**
3.  **Build de Produção: Otimizar para a Performance**
4.  **Deployment: A Tua App no Mundo!**

## 🏆 Projeto do Módulo

**Finalizar a Sneaker Store:** Vamos aplicar estes conceitos à nossa loja.

-   Criaremos uma página de "Checkout" e vamos protegê-la com um `AuthGuard`. O utilizador só poderá aceder se tiver "feito login" (simularemos um serviço de autenticação).
-   Vamos refatorar a nossa aplicação para que o módulo de "Checkout" e o de "Admin" (hipotético) sejam carregados com *lazy loading*, melhorando o tempo de arranque da loja.
-   Finalmente, vamos gerar a versão de produção da nossa loja e publicá-la online para que possas partilhá-la no teu portfólio.

---

Prepara-te para o lançamento!