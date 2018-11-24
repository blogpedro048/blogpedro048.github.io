---
layout: post
comments: true
title:  "Jogo Snake em C++ - Implementações Finais"
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
Informação de Game Over:

'''cpp
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
'''








  



 

 
 

  

