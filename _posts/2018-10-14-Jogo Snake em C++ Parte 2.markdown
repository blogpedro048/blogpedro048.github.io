---
layout: post
comments: true
title:  "Jogo Snake em C++ - Inserindo Funcionalidades (movimentação e lógica)"
date:   2018-10-14 11:09:00 -0200
categories: jekyll update
---

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

 

# Dando monvimentação e lógica ao jogo

Esse *Post* tem como objetivo introduzir novas funcionalidades ao jogo snake, sendo elas: a movimentação da cobra por meio do teclado e a criação de uma lógica responsável por dar sentido e estabelecer regras para as ações da personagem. 

O método entrada da classe JogoSnake é desenvolvido com base nas funções: kbhit() e getch(), sendo respectivamente responsáveis por identificar se alguma tecla foi pressionada e reconhcer qual é a tecla reconhecida, caso tenha recido alguma informação. Essas duas funções fazem parte da biblioteca conio.h.

No jogo Snake a movimentação da cobra não pode mudar para a direção contrária, ou seja, se tiver indo para cima não pode mudar para baixo e se tiver caminhando para a direita é proibido ir para a esquerda, e o contrário também não pode. As teclas p e f funcionam respectivamente para paralisar e finalizar o jogo.

![entrada](https://beta-static.photobucket.com/images/q430/pedro048/0/7308071a-8511-40ad-901c-7c439e2f70f2-original.png?width=1920&height=1080&fit=bounds)

*Link* para o código do método entrada: [clique aqui](https://github.com/pedro048/Projetos-em-C-/blob/master/Jogo%20Snake%20em%20C%2B%2B/entrada.h)


A lógica do jogo é construida para ser possível fazer a formação da cauda, movimentação (direita, esquerda, para cima, para baixo), atravessar as paredes (laterais, superiores e inferiores), derrota quando a cabeça toca a cauda e incrementa a pontuação quando a cobra comer a fruta normal ou a fruta extra.

![logica](https://beta-static.photobucket.com/images/q430/pedro048/0/143b5651-fdf7-4e9b-9005-46fd777f09ab-original.png?width=1920&height=1080&fit=bounds)

*Link* para o código do método lógica: [clique aqui](https://github.com/pedro048/Projetos-em-C-/blob/master/Jogo%20Snake%20em%20C%2B%2B/logica.h)


Clicando na imagem abaixo ficará disponível um vídeo com a demonstração das implementação feitas.

[![Demostração das implementações](http://i350.photobucket.com/albums/q430/pedro048/IMG_20181014_151323_127_zpsrlpuexfi.jpg)](https://www.youtube.com/watch?v=QZ23wBr076o)



 









  



 

 
 

  

