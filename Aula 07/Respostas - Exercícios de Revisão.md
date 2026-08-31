# Respostas — Exercícios de Revisão

## 1. Explique a diferença entre modo usuário e modo kernel.

O **modo usuário** é o modo em que executam os programas comuns, como navegadores, editores de texto, jogos, editores de código e outros aplicativos. Nesse modo, o acesso ao hardware é limitado. O programa não pode acessar diretamente dispositivos, memória de outros processos ou instruções privilegiadas da CPU.

O **modo kernel** é o modo privilegiado usado pelo núcleo do sistema operacional. Nesse modo, o sistema operacional tem acesso direto ao hardware e pode executar operações críticas, como gerenciar memória, controlar dispositivos, escalonar processos, manipular arquivos e tratar interrupções.

A diferença principal está no nível de privilégio:

| Modo | Quem usa | Permissões |
|---|---|---|
| Modo usuário | Aplicativos comuns | Acesso limitado |
| Modo kernel | Núcleo do sistema operacional | Acesso privilegiado ao hardware |

Quando um programa em modo usuário precisa realizar uma operação protegida, como abrir um arquivo ou acessar um dispositivo, ele faz uma **chamada de sistema** para solicitar esse serviço ao kernel.

---

## 2. O que é um processo?

Um **processo** é um programa em execução.

Um programa armazenado no disco é apenas um arquivo. Quando esse programa é carregado na memória e começa a ser executado pela CPU, ele se torna um processo.

Um processo possui seu próprio contexto de execução, incluindo:

- código do programa;
- dados;
- pilha de execução;
- registradores da CPU;
- arquivos abertos;
- espaço de endereçamento;
- estado do processo;
- identificador do processo, chamado PID.

O sistema operacional é responsável por criar, controlar, pausar, retomar e finalizar processos. Ele também decide qual processo usará a CPU em determinado momento.

Exemplo: quando abrimos um navegador, o sistema operacional cria um ou mais processos para executar esse programa.

---

## 3. Qual a diferença entre processo e thread?

Um **processo** é uma instância de um programa em execução, com seu próprio espaço de memória e seus próprios recursos.

Uma **thread** é uma unidade de execução dentro de um processo. Um mesmo processo pode possuir uma ou várias threads executando tarefas diferentes ao mesmo tempo.

A principal diferença é que processos são mais isolados entre si, enquanto threads do mesmo processo compartilham memória e recursos.

| Conceito | Característica |
|---|---|
| Processo | Possui espaço de memória próprio |
| Thread | Compartilha o espaço de memória do processo |
| Processo | Mais pesado para criar e alternar |
| Thread | Mais leve e rápida |
| Processo | Maior isolamento |
| Thread | Maior compartilhamento |

Exemplo: um navegador pode ter threads para carregar páginas, executar scripts, tocar áudio, baixar arquivos e manter a interface responsiva.

Threads melhoram o desempenho e a organização de programas concorrentes, mas exigem cuidado, pois várias threads podem acessar os mesmos dados ao mesmo tempo.

---

## 4. Explique o funcionamento do Round Robin.

O **Round Robin** é um algoritmo de escalonamento de processos que distribui o tempo da CPU entre vários processos de forma justa.

Ele funciona usando uma fila de processos prontos. Cada processo recebe uma pequena fatia de tempo da CPU, chamada **quantum**. Quando o quantum termina, o processo é interrompido e colocado no final da fila, permitindo que o próximo processo execute.

Exemplo:

```text
P1 → P2 → P3 → P1 → P2 → P3
```

Se um processo terminar antes do fim do quantum, ele libera a CPU. Caso contrário, ele volta para o final da fila e aguarda sua próxima vez.

Esse algoritmo é útil em sistemas interativos, pois evita que um único processo monopolize a CPU e melhora a resposta percebida pelo usuário.

Um quantum muito pequeno gera muitas trocas de contexto, reduzindo o desempenho. Um quantum muito grande pode fazer o sistema parecer menos responsivo.

---

## 5. O que é memória virtual?

**Memória virtual** é uma técnica usada pelo sistema operacional para dar a cada processo a impressão de possuir um espaço de memória próprio, contínuo e isolado, mesmo que a memória física real seja limitada.

Com memória virtual, o processo trabalha com endereços virtuais. O sistema operacional, com apoio do hardware, traduz esses endereços virtuais para endereços físicos na memória RAM.

Representação simplificada:

```text
Endereço virtual → tradução → endereço físico
```

A memória virtual permite:

- isolar a memória de cada processo;
- proteger um processo contra outro;
- executar vários programas ao mesmo tempo;
- usar a RAM de forma mais eficiente;
- mover partes da memória para o disco quando necessário;
- criar a impressão de que há mais memória disponível do que a RAM física.

Um conceito importante relacionado à memória virtual é a **paginação**, em que a memória é dividida em blocos chamados páginas. Quando uma página necessária não está na RAM, ocorre uma **falta de página**, e o sistema operacional precisa carregá-la.

---

## 6. Qual a função de um driver de dispositivo?

Um **driver de dispositivo** é um software que permite a comunicação entre o sistema operacional e um hardware específico.

Cada dispositivo possui características próprias. Por isso, o sistema operacional usa drivers para controlar corretamente dispositivos como:

- teclado;
- mouse;
- monitor;
- disco;
- impressora;
- placa de rede;
- placa de vídeo;
- dispositivos USB.

A função do driver é traduzir comandos genéricos do sistema operacional para comandos específicos compreendidos pelo hardware.

Representação:

```text
Sistema Operacional ↔ Driver ↔ Hardware
```

Exemplo: quando o sistema operacional precisa gravar dados em um disco, ele não controla diretamente todos os detalhes físicos do dispositivo. Ele envia solicitações ao driver, e o driver se comunica com o controlador do disco.

Sem drivers, o sistema operacional não conseguiria usar corretamente os dispositivos de entrada e saída. Por isso, drivers são essenciais para o funcionamento completo do computador.
