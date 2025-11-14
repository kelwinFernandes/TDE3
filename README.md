# TDE3

## Problema dos Filósofos

### Problema e Solução
O problema do jantar dos filósofos representa cinco filósofos sentados em uma mesa, onde cada um alterna entre pensar e comer. Para comer, cada filósofo precisa de dois garfos: o da esquerda e o da direita. O problema é que, se todos tentarem pegar os garfos ao mesmo tempo, pode ocorrer um impasse (deadlock), em que cada um segura um garfo e fica esperando o outro, sem que ninguém consiga prosseguir.
A solução adotada aqui foi o uso de um “garçom”, que atua como um semáforo contador inicializado com N−1 (no caso, 4). Esse garçom limita a quantidade de filósofos que podem tentar comer ao mesmo tempo. Antes de pegar os garfos, cada filósofo pede permissão ao garçom com a operação adquirir(garçom). Se ainda houver permissão disponível, ele entra e tenta pegar os garfos; caso contrário, ele fica bloqueado esperando outro filósofo terminar. Quando termina de comer, o filósofo libera os garfos e avisa o garçom com liberar(garçom), permitindo que outro entre.
Essa estratégia impede o deadlock porque garante que sempre sobra pelo menos um garfo livre, eliminando a condição de espera circular das quatro condições de Coffman. Cada garfo é controlado por um semáforo binário que assegura exclusão mútua, e o garçom atua como uma camada adicional de controle, garantindo que o sistema nunca fique completamente travado. Dessa forma, os filósofos se alternam de forma justa entre pensar e comer, e todos eventualmente conseguem acesso aos recursos sem bloqueio permanente

### Pseudocodigo
    Dados:
    N = 5 filósofos
    garfos[0..N-1] → semáforos binários inicializados em 1
    garçom → semáforo contador inicializado em N-1 (4)
    
    Para cada Filósofo p:
      enquanto verdadeiro:
  
          pensar()
  
          estado[p] ← "com fome"
          adquirir(garçom)                    // pede permissão para tentar comer
  
          adquirir(garfo[p])                  // pega garfo da esquerda
          adquirir(garfo[(p + 1))             // pega garfo da direita
  
          estado[p] ← "comendo"
          comer()
  
          liberar(garfo[(p + 1)])             // solta garfo da direita
          liberar(garfo[p])                   // solta garfo da esquerda
          liberar(garçom)                     // libera permissão para outro filósofo

          estado[p] ← "pensando"
