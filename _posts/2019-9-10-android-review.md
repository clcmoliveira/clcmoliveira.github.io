---
layout: post
title: Revisão de Android
description: Revisão de itens utilizados em um projeto Android
author: Camila L. Oliveira
---

Revisão de itens muito utilizados em um projeto android e como implementar.

## Activity Lifecycle
Os sete passos do ciclo de vida de uma activity de um aplicativo android são:
- onCreate: quando o app é criado pela primeira vez;
- onStart: quando o app está para ser visualizado pelo usuário;
- onResume: chamado quando existe uma interação com o usuário (ex. abrir a galeria para importar uma foto);
- onPause: chamado quando a activity é pausada, logo, deixa de ser visível ao usuário;
- onStop: chamado quando a activity deixa de ser visível ao usuário;
- onRestart: quando paramos, para depois reinicializar a activity; e
- onDestroy: chamado quando a activity é destruída, finalizada.

![Image of Activity Lifecycle](https://static.javatpoint.com/images/androidimages/Android-Activity-Lifecycle.png)

----
No [JavaTPoint](https://www.javatpoint.com/android-life-cycle-of-activity), faça um app que retrate o ciclo de vida de uma activity. 
Confira os logs no Debug.
----



## Métodos AsyncTask

Os métodos [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask?hl=en) recebem três parâmetros, que podem ter seu retorno em Void. Estes parâmetros são:

  1. Progress Bar, 
  2. Tipo de Método,
  3. Resultado
  
  Estes métodos são:
- On Pre Excecute: executado antes da Thread ser iniciada, com uma mensagem dentro da mesma activity;
- Do In Background: *obrigatória*, é a responsável pelos processamentos prévios de **On Post Execute** em outra thread e, depois, devolve os valores obtidos;
- On Post Excute: *obrigatória*, recebe o retorno de **Do In Background**; e
- On Progress Update: informa o carregamento, através de um Loop.

Ela é executada através do .Execute

### Exemplo:
<pre><code> 
//NomeDaClasse().método
Login().execute 
</code></pre>

---
## JSON

É uma linguagem de programação que é utilizada para as requisições externas de dados.

### Exemplo: 
Eu tenho um app de previsão de tempo, aonde informo o meu CEP e ele me devolve os valores de temperatura mínima e máxima neste exato momento. Para tanto, eu uso o JSON nesta obtenção dos dados e encaminho aos métodos que lhes peçam.

---
## Retrofit
Também é utilizada para requisições externas, porém é voltada a serviços de conexão ao servidor e utiliza o JSON. Sua diferença para o HTTP Url Connection é que ele é mais rápido.

- [API](https://square.github.io/retrofit/)
- [Aulas](https://www.youtube.com/user/SimplifiedCoding/search?query=retrofit)

---
## Media Recorder
Manipulação de mídias em seu projetos, através da classe **Media Player**.

### Exemplo:

Eu tenho uma aplicação que executará um instant meme que baixei no [Soundbible](soundbible.com). Eu a manipulo através de botões de Iniciar, Pausar e Parar e seus métodos serão executados através delas.

---
## List View x Recycler View

A diferença entre elas é que o **List View** trata de uma lista simples, sem necessitar de memória RAM. Já o **Recycler View**, você possui uma reciclágem dos componentes, com dados atualizados.

---
## Notificação
- [FAB](http://technxt.net/how-to-create-a-floating-action-button-in-android-app/)
- [Criar Notificação](https://developer.android.com/training/notify-user/build-notification)

---
## Dagger 2

Dependências entre classes, como requisisções de módulos e métodos.

---
## Unit Test

Teste de lógica entre os componentes, se estes são válidos ou inválidos.

### Exemplo:
Quero saber se o email informado pelo usuário é válido

<pre><code>
//estrutura do email

//username [a-Z ou A-Z com 0-9 ou caracteres especiais] @ [domínio].[domínio de topo (comercial ou regional)]
clcmoliveira@github.io
</code></pre>

---
