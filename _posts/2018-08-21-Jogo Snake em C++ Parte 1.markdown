---
layout: post
comments: true
title:  "Jogo Snake em C++ - implementações iniciais"
date:   2018-08-21 07:45:38 -0300
categories: jekyll update
---

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

 

# INTRODUÇÃO

  Esse *post* tem o objetivo de apresentar a forma de iniciar o desenvolvimento do jogo snake em C++. Algumas características da linguagem serão utilizadas de forma contextualiza durante a criação do projeto, então o desenvolvimento do jogo busca ajudar, de forma divertida, na consolidação de conceitos, como: funcionalidades de POO (progração orientada à objetos), sintaxe do C++, bibliotecas (para windows) que não são usadas normalmente, manipulação de arquivos e estratégias para formatar um jogo. Segue, abaixo, um exemplo de como ficará o projeto após os três *posts*.
  
  ![snake4](http://i350.photobucket.com/albums/q430/pedro048/Snake6_zpsj2wwkyln.png)

# ESTRUTURANDO O PROJETO

  Algumas bibliotecas precisam ser incluidas no projeto

  ```cpp
   #include <stdlib.h>  // para usar o rand(), srand(), system()
   #include <windows.h> // para usar sleep(), Beep()
   #include <conio.h>   // para usar kbhit() e o getch()
   #include <time.h>    // para poder usar time(0)
   #include <cstring>   // para usar strcpy()
   #include <fstream>   // para usar a manipulacao de arquivo
 ```

  Inicialmente será necessário criar uma classe, onde será definida todas características do projeto que estamos criando. 

  ```cpp
  class JogoSnake{
  public:
     //atributos

     //metodos
};
```
  Os atributos necessários para definir essa classe precisam ser variáveis que auxiliem na criação da matriz do campo, inserção das frutas, contagem do tempo, marcação da pontuação, manipulação e criação da personagem. Entre os atributos também é necessario definir se o jogo pode permanecer executando ou deve ser finalizado, ou seja, teria ocorrido um *game over* por causa da cobra ter tocado a cauda, sendo essa a única forma de perder a partida.
  
  A cabeça da cobra é criada com coordenadas (x,y) para ser situada na matriz do jogo, qualquer movimentação da cabeça é feita incrementado ou decrementando essas coordenadas. As frutas são criadas em posições aleatórias e utilizadas para marcar quando será necessário gerar novas frutas, incrementar a cauda e os pontos. A contagem do tempo é usada apenas para definir o momento em que a fruta extra permanecerá no campo. 

  ```cpp
    // atributos 
    bool perdeu;
    int const largura=20, altura=20; // a altura se refere as linhas e a largura as colunas. Essa e a dimensao da matriz do jogo
    int caudax[100], cauday[100]; // variaveis para as coordenadas em x e y dos fragmentos da cauda
    int x, y, frutaX, frutaY, extraX, tempo, extraY, contarCauda, pontos, auxPontos=0; /* variaveis para as coordenadas referentes a
                                                                             cabeca da cobra, a fruta, a fruta extra,
                                                                             pontuacao
                                                                            */

    enum direcao{parar=0, esquerda, direita, cima, baixo}; /* usado na movimentacao da cobra no jogo. Esse he um tipo de dado
    definido pelo usuário que define uma variável que vai receber apenas um conjunto restrito de valores */
   

    direcao pos; // pos pode ser parar, esquerda, direita, cima ou baixo
    bool tesquer, tdireita, tcima, tbaixo, acabouFextra=false, mostrarCauda; /* variaveis usadas para a cobra nao mudar de sentido (direita ou esquerda) quando estiver na horizontal e
                                                                                permancer na mesmo sentido (cima ou baixo) para o movimento vertical. acabouFextra auxilia quando o tempo
                                                                                da fruta extra termina e mostrarCauda ajuda para colocar espacos vazios quando nao esta sendo mostrada a
                                                                                cabeca da cobra, a fruta, a fruta extra e a cauda da cobra
                                                                               */ 

  ```

  Os métodos são responsáveis por definir o comportamento da classe. No caso de JogoSnake serão criados seis métodos, sendo que entre eles estará o método construtor. Toda vez que um objeto da classe for instanciado o construtor é chamado e o objeto recebe as informações contidas nele, ou seja, é responsável por fazer a configuração inicial do jogo. A parte visual é criada no método desenho, isso inclui a moldura do campo, a cobra (com a extensão da cauda), a fruta, o placar de pontuação. As interações com o teclado para manipulação da cobra são feitas, no método entrada, por meio das teclas: a, w, s, d. 
  
  Toda a lógica de utilização do jogo é estruturado em um método, ajudando na criação e movimentação ordenada da cauda, ela precisa seguir a cabeça com fluidez. O método lógica também trata a passagem da cobra pelas paredes e o momento que ocorre a captura das frutas normais e extras. *game over* e gerarFrutasExtra são respectivamente responsáveis por: passar a variável que marca o estado de derrota para *true* e gerar as posições aleatórias para a fruta extra.

  ```cpp
    // metodos 
         JogoSnake(); // construtor padrão da classe
    void desenho();
    void entrada();
    void logica();
    void gerarFrutaExtra();
    bool gameOver();
 ```
##### ANALISANDO O MÉTODO CONSTRUTOR

 Esse método inicializa o jogo, fazendo com que a cabeça da cobra apareça, parada, no meio do campo, sem nenhum fragmento da cauda. A variável reponsável por marcar se o jogador perdeu ou não precisa ser *false*, porque inicialmente ele não foi derrotado. Nessa parte da configuração também são tratadas variáveis *bool* com a finalidade de evitar a mudança de direção da cobra. As coordenadas da fruta extra são geradas, mas só serão utilizadas se a pontuação chegar ao valor estipulado. O contador do tempo em que a fruta extra ficará em tela começa zerado.

 ```cpp
 JogoSnake::JogoSnake(){ // metodo responsavel pelas configuracoes iniciais do jogo

    srand(time(0));   // usundo o relogio do computador para auxiliar na obtencao de numeros aleatorios
    perdeu=false; // essa variavel comeca em falso, porque o usuario ainda nao perdeu
    x=largura/2; // a cabeca da cobra inicia centralizada em x
    y=altura/2;  // a cabeca da cobra inicia centralizada em y
    frutaX=(rand()%(largura-3))+1; // gera uma coordenada aleatoria em x para a fruta
    frutaY=(rand()%(altura-3))+1;  // gera uma coordenada aleatoria em y para a fruta
    extraX=(rand()%(largura-3))+1; // gera uma coordenada aleatoria em x para a fruta extra
    extraY=(rand()%(altura-3))+1;  // gera uma coordenada aleatoria em y para a fruta extra
    contarCauda=0; //o crescimento da cauda comeca em 0
    pos=parar; // a cabeca da cobra comeca parada
    pontos=0; // o jogo inicia com a pontuacao zerada
    tempo=0; // o tempo da fruta extra inicia zerado, porque o jogador ainda nao esta com a pontuacao adequada
    tesquer=false;
    tdireita=false;
    tcima=false;
    tbaixo=false;

}
```
##### DESENHANDO A MATRIZ DO CAMPO DE JOGO



```cpp
void JogoSnake::desenho(){ // metodo responsavel pela parte visual do jogo

    system("cls"); // limpa a tela
    cout<<"pontuacao: "<<pontos<<endl; // mostra a pontuacao
    cout<<"tempo da fruta extra: "<<tempo<<endl; // mostra o tempo da fruta extra
    cout<<endl;
    for(int i=0; i<largura+1; i++){ // mostra os quadrados da parte de cima da matriz
        cout<<char(176);
    }
    // percorre a matriz do jogo
    for(int i=0; i<altura; i++){
        for(int j=0; j<largura; j++){
            //formacao das paredes da matriz do jogo
            if(j==0 || j==largura-1){         // colocar um quadrado na primeira e na ultima posicao de cada linha
                if(i!=0){
                    cout<<char(176);
                }
            }
            if(j==x && i==y){                 //desenha a cabeca da cobra na posicao indicada por suas coordenadas
                cout<<char(219);
            }else if(j==frutaX && i==frutaY){ //desenha a fruta na posicao indicada por suas coordenadas
                cout<<char(184);
            }else if(pontos!=0 && pontos%50==0 && tempo<=50 && acabouFextra==false && j==extraX && i==extraY){  /*  condicoes para a fruta extra aparecer
                                                                                                                   (a cada 50 pontos, a contagem ser menor que 50 e a contagem nao ter estourado)
                                                                                                                */
                    cout<<char(206);
                    tempo++;                        // contagem do tempo em que a fruta extra fica disponivel

            }else{                                  /* quando nao estiver desenhando a cabeca da cobra, a fruta, a fruta extra
                                                       é verificado se a cauda da cobra precisa ser desenhada caso não precise é
                                                       colocado um espaco vazio na posicao, pois isso preenche o campo em torno da cobra,
                                                       fruta e fruta extra com espacos vazios dando forma a matriz que sera percorrida
                                                    */

                mostrarCauda=false;                /* inicialmente nao precisa mostrar a cauda, porque pode ser que ela nao exista
                                                      ou nao esteja no momento
                                                   */
                for(int p=0; p<contarCauda; p++){
                    if(caudax[p]==j && cauday[p]==i){
                        cout<<char(219);             // desenha a cauda da cobra
                        mostrarCauda=true;           // foi necessario mostrar a cauda

                    }
                }

                if(mostrarCauda==false){           // coloca espacos vazios quando nao e preciso mostar a cobra, fruta e fruta extra

                    cout<<" ";
                }
            }
        }
        cout<<endl;
    }
    for(int i=0; i<largura+1; i++){ // mostra os quadrados da parte de baixo da matriz do jogo
        cout<<char(176);
    }
    cout<<endl;
}
 ```
 Após as implementações feitas é possível realizar o primeiro teste. Na função *main()* um objeto da classe JogoSnake precisa ser instanciado, com isso a chamada do método desenho mostra na tela o visual do nosso jogo. Não é possível fazer nenhuma movimentação com a cobra, porque as únicas funções da classe criadas até agora foram o construtor e desenho, respectivamente: inicializa características e cria a parte gráfica.  

 ```cpp
 int main(){
   JogoSnake *partida = new JogoSnake(); // instanciando um objeto, partida, da classe JogoSnake
   while(true){
      partida.desenho();
   }
   return 0;
}
 ``` 
  Saída do jogo até o momento:





 


 ![snake3](http://i350.photobucket.com/albums/q430/pedro048/Snake3_zpsx0iil88a.png)

 









  



 

 
 

  

