---
layout: post
title: Layouts no Android
description: Os modelos de XML de uma activity
author: Camila L. Oliveira
image: https://miro.medium.com/max/1286/1*_-Xrmn7afj_HzxGOlsRqig.png
---

Existem diversos tipos de layout para fazer a visualização de uma activity ou fragment no Android. No entanto, vamos focar nas que mais serão utilizadas nos projetos.
 - Constraint
 - Linear
 - Grid
 - Relative

## Constraint Layout
Segundo o [guia de desenvolvimento android](https://developer.android.com/training/constraint-layout/index.html), o Constraint Layout opera de uma maneira idêntica ao RelativeLayout, assim,
todo o alinhamento é feito de relações entre as Views. A única diferença é que somos capazes de indicar o posicionamento que queremos manter as Views por meio dos seus eixos.
Os eixos são definidos como X e Y, sendo X o eixo de ínicio (esquerdo) e fim (direito) e o Y o de topo (cima) e inferior (baixo).

![Image of Constraint Alignment](https://miro.medium.com/max/429/1*3jIUT0p0bf0-2baw_K68QQ.png)
----
Um bom exemplo de como você pode posicionar um [componente](/posts/2019-11-14-components-in-android) no Constraint Layout:
<script src="https://gist.github.com/clcmoliveira/f89d5ed420f9749f06caf4e2e726b14f.js"></script>
----

## Linear Layout
O Linear Layout é um ViewGroup que pode posicionar as Views em uma única coluna vertical ou horizontal, atribuídas através do **orientation**.
Um exemplo é quando você quer que uma mensagem seja mostrada ao usuário, como este abaixo:
<script src="https://gist.github.com/clcmoliveira/aa64bc7f6e60133e2c6166ef714b60ed.js"></script>

Um outro importante atribuuto do LinearLayout é o layout_weight, responsável por definir o peso que cada View tem referente a distribuição dentro dela. Quando não utilizadas, 
as Views não ocupam totalmente o espaço disponível existente no layout, deixando o design da nossa interface muito mal feito.
Um outro [exemplo](https://gist.github.com/clcmoliveira/aa64bc7f6e60133e2c6166ef714b60ed#file-act_linear_layout_example_2-xml) é quando você atribui os pesos de cada componente (View) para layout.

------

## Grid Layout

---------
# Bibliografia
- @alexfelipe - [Implementando telas no Android com Constraint Layout](https://medium.com/collabcode/implementando-telas-no-android-com-constraint-layout-13a90e44622f)
- AndroidPro - [Como dominar os Android Layouts em 07 passos](https://www.androidpro.com.br/blog/desenvolvimento-android/android-layouts-viewgroups-intro/#LinearLayout_Horizontal_e_Vertical)
