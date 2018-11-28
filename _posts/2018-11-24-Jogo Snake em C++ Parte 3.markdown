---
layout: post
comments: true
title:  "Jogo Snake em C++ - Resultados e implementações finais"
date:   2018-11-24 11:09:00 -0200
categories: jekyll update
---

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

 
## Inserindo funcioladidades finais (menu inicial e rank dos jogadores)

Nesse *post* serão abordados aspectos complementares do jogo, ou seja, já é possível usar o projeto com as funcionalidades implementadas até agora, vamos apenas deixar mais configurável e interativo. A cor do jogo pode ser alterada por meio da função *system()* da biblioteca *stdlib.h*, isso é feito pela seguinte relação:

Parâmetro da função *system()* => Cor associada

"color 09"  =>   Azul
"color A"   =>   Verde
"color C"   =>   Vermelho
"color E"   =>   Amarelo
"color F"   =>   Branco
"color 08"  =>   Cinza
"color D"   =>   Lilas
"color B"   =>   Verde-agua

Existem três níveis de dificuldes diferenciados pela velocidade que o jogo acontece. A função sleep() da biblioteca *windows.h* é reponsável pelo atraso na execução do programa, podendo ser aplicada da seguinte forma:

```cpp
   switch(nivel){
            case 1:
                Sleep(500); // atraso de 1/2 segundo
                break;
            case 2:
                Sleep(250); // atraso de 1/4 de segundo
                break;
            case 3:
                Sleep(75);  // atraso muito pequeno
                break;
            default:
                 system("cls");
                 cout<<"Esse nivel nao e valido!"<<endl;
                 cout<<"Escolha novamente"<<endl;
                 cout<<endl;
                 goto volta;
            }
        }
``` 
O menu inicial tem a função de mostrar as informações que estão sendo configuradas. Tela para a escolha das cores:

```cpp
    system("cls");
    cout<<"########### JOGO SNAKE #############"<<endl;
    cout<<"#                                  #"<<endl;
    cout<<"# Escolha a coloracao do jogo:     #"<<endl;
    cout<<"#                                  #"<<endl;
    cout<<"# 1 - azul         2 - verde       #"<<endl;
    cout<<"# 3 - vermelho     4 - amarelo     #"<<endl;
    cout<<"# 5 - branco       6 - cinza       #"<<endl;
    cout<<"# 7 - lilas        8 - verde-agua  #"<<endl;
    cout<<"#                                  #"<<endl;
    cout<<"####################################"<<endl;
    cout<<endl;
```
Escolha do nível de dificuldade:

```cpp
    system("cls");
    cout<<"######## JOGO SNAKE #########"<<endl;
    cout<<"#                           #"<<endl;
    cout<<"# - 0 - iniciar             #"<<endl;
    cout<<"# - 1 - sair                #"<<endl;
    cout<<"#                           #"<<endl;
    cout<<"# Niveis de dificuldade:    #"<<endl;
    cout<<"#                           #"<<endl;
    cout<<"# - 1 - nivel 1             #"<<endl;
    cout<<"# - 2 - nivel 2             #"<<endl;
    cout<<"# - 3 - nivel 3             #"<<endl;
    cout<<"#                           #"<<endl;
    cout<<"#***************************#"<<endl;
    cout<<"# A tecla f reinicia o jogo #"<<endl;
    cout<<"#                           #"<<endl;
    cout<<"# A tecla p paralisa o jogo #"<<endl;
    cout<<"#***************************#"<<endl;
    cout<<"#                           #"<<endl;
    cout<<"############################"<<endl;
    cout<<endl;
``` 
Informação de *Game Over*:

```cpp
 while(perdeu==false){ // enquanto o usuario nao perdeu o jogo continua
            desenho();
            entrada();
            logica();
            if(perdeu==true){
                system("cls");
                cout<<"####################"<<endl;
                cout<<"##                ##"<<endl;
                cout<<"##                ##"<<endl;
                cout<<"##   GAME OVER!   ##"<<endl;
                cout<<"##                ##"<<endl;
                cout<<"##                ##"<<endl;
                cout<<"####################"<<endl;
                cout<<endl;
                system("pause");
                system("cls");
                main();
                if(escolhas==1){
                    goto sair;
                }
                perdeu=false;
            }
``` 
Um som de *beep* pode ser associado a ação da cobra comer uma fruta, isso no método responsável pela lógica, por meio da função *Beep(frequência, duração)* da biblioteca *windows.h*.

O sistema de *rank* do jogo *Snake* foi implementado como um método que salva as informações (o nome de 3 jogadores) em um arquivo .txt que fica armazenado na mesma pasta do projeto. Para iniciar o *rank* é necessário colocar a biblioteca *iostream* para pode manipular arquivos em C++. O histório dos maiores pontuadores é estabelecido com três posições (1º, 2º e 3º), sendo necessário inserir o nome quando a pontuação supera alguma pontuaçao do *rank*. Fluxograma do sistema de *rankeamento*: 

![rank](https://beta-static.photobucket.com/images/q430/pedro048/0/e6021bd9-2db0-4e16-9e02-fd2af9b721df-original.png?width=1920&height=1080&fit=bounds)


*Link* para o código do método rank: [clique aqui](https://github.com/pedro048/Projetos-em-C-/blob/master/Jogo%20Snake%20em%20C%2B%2B/rank.h)

## Resultado

Clicando na imagem abaixo ficará disponível um vídeo do jogo Snake com todas as funcionalidades apresentadas nos *posts*.

[![Resultado do Projeto](http://i350.photobucket.com/albums/q430/pedro048/IMG_20181014_151323_127_zpsrlpuexfi.jpg)](https://www.youtube.com/watch?v=Q5GoZOooUr8&feature=youtu.be)

[clique aqui para ter acesso ao executável do jogo Snake](https://github.com/pedro048/Projetos-em-C-/blob/master/Jogo%20Snake%20em%20C%2B%2B/snake3.exe)











  



 

 
 

  

