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

 ```cpp
 void JogoSnake::entrada(){ // metodo responsavel por receber os comandos feitos no teclado

    if(kbhit()){ // verifica se alguma tecla foi acionada
        switch(getch()){ // identifica qual tecla foi acionada
        case 'a': // a tecla a direciona a cobra para a esquerda
            if(tdireita==true){    /*
                                   caso esteja se movimentando para a direita continua mesmo
                                   se o usuario pretender movimentar para a esquerda
                                   */
                pos=direita;
            }else{                // caso nao esteja se movendo para a direita atende o comando e se move para a esquerda
                pos=esquerda;
                tesquer=true;
            }
            if(tcima==true){      // se estava se movendo para cima para de se mover nessa posicao
                tcima=false;
            }
            if(tbaixo==true){     // se estava se movendo para baixo para de se mover nessa posicao
                tbaixo=false;
            }
            break;
        case 'd':
            if(tesquer==true){      /*
                                   caso esteja se movimentando para a esquerda continua mesmo
                                   se o usuario pretender movimentar para a direita
                                   */
               pos=esquerda;
            }else{                  // caso nao esteja se movendo para a esquerda atende o comando e se move para a direita
                pos=direita;
                tdireita=true;
            }
            if(tcima==true){        // se estava se movendo para cima para de se mover nessa posicao
                tcima=false;
            }
            if(tbaixo==true){       // se estava se movendo para baixo para de se mover nessa posicao
                tbaixo=false;
            }
            break;
        case 'w':
            if(tesquer==true){       // se estava se movendo para a esquerda para de se mover nessa posicao
                tesquer=false;
            }
            if(tdireita==true){      // se estava se movendo para a direita para de se mover nessa posicao
                tdireita=false;
            }
            if(tbaixo==true){      /*
                                   caso esteja se movimentando para baixo continua mesmo
                                   se o usuario pretender movimentar para cima
                                   */

                pos=baixo;
            }else{                 // caso nao esteja se movendo para baixo atende o comando e se move para cima
                pos=cima;
                tcima=true;
            }
            break;
        case 's':
            if(tesquer==true){      // se estava se movendo para a esquerda para de se mover nessa posicao
                tesquer=false;
            }
            if(tdireita==true){     // se estava se movendo para a direita para de se mover nessa posicao
                tdireita=false;
            }
            if(tcima==true){        /*
                                   caso esteja se movimentando para cima continua mesmo
                                   se o usuario pretender movimentar para baixo
                                   */
                pos=cima;
            }else{                 // caso nao esteja se movendo para cima atende o comando e se move para baixo
                pos=baixo;
                tbaixo=true;
            }
            break;
        case 'f':                   // finaliza a partida
            perdeu=true;
            break;
        case 'p':                   // paralisa o jogo
            system("pause");
            Sleep(1500);
            break;
        default:
            break;
        }
    }
}
```

A lógica do jogo é construida para ser possível fazer a formação da cauda, movimentação (direita, esquerda, para cima, para baixo), atravessar as paredes (laterais, superiores e inferiores), derrota quando a cabeça toca a cauda e incrementa a pontuação quando a cobra comer a fruta normal ou a fruta extra.

 ```cpp
void JogoSnake::logica(){ // metodo responsavel pela formação da cauda

    int guardar1X, guardar1Y, guardar2X, guardar2Y;
    for(int i=0; i<=contarCauda; i++){
        if(i==0){ // analise do 1 fragmento da cauda
            guardar1X=caudax[i]; // guarda a posicao em x do 1 fragmento da cauda
            guardar1Y=cauday[i]; // guarda a posicao em y do 1 fragmento da cauda
            caudax[i]=x;         // o 1 fragmento da cauda recebe a posicao em x da cabeca da cobra
            cauday[i]=y;         // o 1 fragmento da cauda recebe a posicao em y da cabeca da cobra
        }else{
            guardar2X=caudax[i]; // guarda a posicao em x de qualquer fragmento da cauda que nao seja o 1
            guardar2Y=cauday[i]; // guarda a posicao em y de qualquer fragmento da cauda que nao seja o 1
            caudax[i]=guardar1X; /* qualquer fragmento da cauda que nao seja o 1 recebe a posicao em x
                                          do fragmento anterior
                                        */
            cauday[i]=guardar1Y;  /* qualquer fragmento da cauda que nao seja o 1 recebe a posicao em y
                                          do fragmento anterior
                                        */
            guardar1X=guardar2X;        /* a posicao em x do fragmento em analise e guardada para ser recebida pelo
                                           fragmento posterior
                                        */
            guardar1Y=guardar2Y;        /* a posicao em y do fragmento em analise e guardada para ser recebida pelo
                                           fragmento posterior
                                        */
        }
    }
    switch(pos){
    case esquerda: // para a cobra se mover para a esquerda e preciso decrementar os valores da linha
        x--;
        break;
    case direita:  // para a cobra se mover para a direita e preciso incrementar os valores da linha
        x++;
        break;
    case cima:     // para a cobra se mover para cima e preciso decrementar os valores da coluna
        y--;
        break;
    case baixo:    // para a cobra se mover para baixo e preciso incrementar os valores da coluna
        y++;
    }
// atravessa as paredes laterais

    if(x>=largura-1){ /* Se a coordenada em x da cabeca da cobra chegar ao fim de
                         uma linha da matriz esse sua coordenada retornara para o
                         inicio da linha
                      */
        x=0;
    }else if(x<0){     /* Se a coordenada em x da cabeca da cobra for menor que
                         a primeira posicao de uma linha da matriz esse sua coordenada
                         ira para o fim da linha
                      */
        x=largura-2;
    }
// atravessa as paredes de cima e de baixo

    if(y<=0){           /* Se a coordenada em y da cabeca da cobra for quase menor
                         ou igual a primeira posicao de uma coluna da matriz essa coor
                         denada ira para a ultima posicao da coluna
                        */
        y=altura-1;
    }else if(y>=altura){ /* Se a coordenada em y da cabeca da cobra chegar ao fim da
                            coluna da matriz essa coordenada ira para o inicio da co
                            luna
                        */
        y=1;
    }

// se a cabeca da cobra tocar o corpo o jogador sera derrotado
    for(int i=0; i<contarCauda; i++){
        if(caudax[i]==x && cauday[i]==y){
            Beep(250, 1000); //(frequencia, duracao) produz um som depois que a cobra tocar nela mesma
            perdeu=true;
        }
    }
// quando a cobra come a fruta
    if(x==frutaX && y==frutaY){
        if(tempo==0){           // a cobra comeu uma fruta quando nao tinha fruta extra no campo
            Beep(1000, 250);    //(frequencia, duracao) produz um som depois que a cobra comer uma fruta
            pontos+=10;         // incrementa a pontuacao, porque a cobra comeu a fruta

        }else{                  // como a variavel tempo nao esta em 0 a cobra comeu uma fruta quando a fruta extra esta no campo
            Beep(1000, 250);
            auxPontos+=10;      // acumula a pontuacao referente as frutas normais enquanto a fruta extra ainda esta no campo
        }
        contarCauda++;          // cada vez que a cobra come uma fruta a sua cauda cresce
        outraPos1:
        frutaX=(rand()%(largura-3))+1; // gera uma nova fruta com coordenadas x e y aleatorias
        frutaY=(rand()%(altura-3))+1;
        for(int i=0; i<contarCauda; i++){
            if(caudax[i]==frutaX && cauday[i]==frutaY)
                goto outraPos1;
        }
        acabouFextra=false;           // o tempo da fruta extra ainda nao terminou
    }
    if(pontos!=0 && pontos%50==0 && tempo<=50 && acabouFextra==false){ // verifica as condicoes para ter fruta extra no campo
         for(int i=0; i<contarCauda; i++){
                if(caudax[i]==extraX && cauday[i]==extraY){
                   gerarFrutaExtra();
                }
            }
        if(x==extraX && y==extraY){ // verifica se a cobra comeu uma fruta extra
            Beep(1000, 250);
            pontos+=20;             // recebe uma pontucao maior que o normal
            contarCauda++;          // incrementa a cauda
            tempo=0;                // zera o tempo da fruta extra, porque ela deve desaparecer tendo em vista que a cobra ja comeu
            outraPos2:
            extraX=(rand()%(largura-3))+1;  // gera uma posicao aleatoria para a nova fruta extra, mas ela so aparecera quando os criterios forem cumpridos
            extraY=(rand()%(altura-3))+1;
            for(int i=0; i<contarCauda; i++){
                if(caudax[i]==extraX && cauday[i]==extraY){
                    goto outraPos2;
                }
            }
        }
    }
    if(tempo>50){ // o tempo da fruta extra estourou, porque vai ate 50
        tempo=0;  // zera a variavel tempo
        pontos+=auxPontos; /*a variavel pontos recebe a pontuacao das frutas normais comidas durando o tempo em que a fruta extra
                            estava no campo
                            */
        auxPontos=0;
        acabouFextra=true;  // o tempo da fruta extra estar em jogo acabou
    }
}
```

Clicando na imagem abaixo ficará disponível um vídeo com a demonstração das implementação feitas.

[![Demostração das implementações](http://i350.photobucket.com/albums/q430/pedro048/IMG_20181014_151323_127_zpsutuiuavh.jpg)](https://www.youtube.com/watch?v=QZ23wBr076o)



 









  



 

 
 

  

