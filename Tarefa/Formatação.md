# Atividade — Formatação e Instalação de um Sistema Operacional Windows

## 1. Introdução

A instalação de um sistema operacional não consiste apenas em copiar arquivos para um disco. Durante esse processo, diversos componentes trabalham em conjunto para controlar a CPU, a memória, os dispositivos, os arquivos e a execução de programas.

Neste trabalho será considerada uma **instalação limpa do Windows 11 em um computador moderno, inicializado em modo UEFI e utilizando GPT no disco de destino**.

Antes da instalação, pressupõe-se que o computador atenda aos requisitos mínimos do Windows 11, como:

- processador compatível de 64 bits;
- pelo menos 4 GB de memória RAM;
- pelo menos 64 GB de armazenamento;
- firmware UEFI compatível com Secure Boot;
- TPM 2.0.

O objetivo é descrever o processo desde o momento em que o computador é ligado até o Windows estar instalado e pronto para utilização, relacionando cada etapa aos conceitos estudados em Sistemas Operacionais.

---

# 2. Componentes do Sistema Operacional envolvidos

Durante a instalação, diferentes componentes do sistema operacional precisam trabalhar de maneira integrada.

## 2.1. Gerenciamento de processos e threads

O sistema operacional controla os programas em execução e distribui tempo de CPU entre suas threads.

Durante a instalação existem processos responsáveis por tarefas como:

- executar a interface do Windows Setup;
- detectar dispositivos;
- preparar o disco;
- aplicar os arquivos do Windows;
- carregar drivers;
- configurar o sistema.

Entre os principais elementos que podem participar estão:

- **Windows Setup**, responsável pela instalação;
- processos e serviços pertencentes ao **Windows PE**;
- ferramentas auxiliares, como o **DiskPart**, quando utilizado para gerenciamento de discos;
- processos responsáveis pela aplicação e configuração da imagem do Windows.

Cada processo utiliza recursos fornecidos pelo sistema operacional, como CPU, memória, arquivos e dispositivos.

---

## 2.2. Gerenciamento de memória

O sistema operacional controla a utilização da memória RAM.

Durante a instalação, a RAM é utilizada para armazenar temporariamente:

- partes do ambiente de instalação;
- código dos programas;
- drivers;
- dados temporários;
- buffers utilizados em operações de leitura e escrita;
- partes da imagem do Windows durante seu processamento.

O gerenciamento de memória também ajuda a impedir que um processo interfira indevidamente na memória utilizada por outro.

---

## 2.3. Gerenciamento de Entrada e Saída

As operações de **Entrada/Saída**, também chamadas de **I/O — Input/Output**, permitem a comunicação entre software e dispositivos.

Durante a instalação, vários dispositivos participam desse processo:

| Dispositivo | Tipo de operação | Utilização durante ou após a instalação |
|---|---|---|
| **Teclado** | Entrada | Digitação e navegação pelas opções |
| **Mouse** | Entrada | Interação com a interface gráfica |
| **Monitor** | Saída | Exibição da interface do instalador e do Windows |
| **SSD/HD** | Entrada e saída | Leitura e gravação dos arquivos do sistema |
| **Pendrive** | Entrada e saída | Fornecimento da mídia de instalação e possível armazenamento de dados |
| **Rede** | Entrada e saída | Atualizações, autenticação e download de drivers |
| **Áudio** | Entrada e/ou saída | Reprodução ou captura de som após a instalação |

O sistema operacional não precisa conhecer diretamente todos os detalhes físicos desses equipamentos em suas aplicações. A comunicação é intermediada pelo subsistema de entrada/saída e pelos respectivos drivers.

---

## 2.4. Sistema de arquivos

O sistema de arquivos organiza os dados presentes em uma unidade de armazenamento.

Na instalação do Windows, ele permite:

- criar diretórios;
- armazenar arquivos;
- localizar dados;
- manter metadados;
- controlar espaço livre e utilizado;
- aplicar permissões.

A partição principal do Windows normalmente utiliza **NTFS**.

---

## 2.5. Plug and Play e drivers

O Windows utiliza mecanismos de **Plug and Play** para identificar dispositivos e associá-los a drivers compatíveis.

Os drivers permitem que o sistema operacional utilize componentes como:

- SSD e HD;
- controladores SATA e NVMe;
- dispositivos USB;
- placa de vídeo;
- placa de rede;
- áudio;
- Bluetooth.

Sem um driver compatível com determinado controlador de armazenamento, por exemplo, o instalador pode não conseguir detectar o SSD.

---

# 3. Kernel

O **kernel** é o núcleo do sistema operacional.

No Windows, o kernel e os demais componentes centrais executados em modo kernel fornecem a base para funções como:

- escalonamento de threads;
- gerenciamento de memória;
- entrada e saída;
- tratamento de interrupções;
- segurança;
- comunicação com drivers.

Durante a instalação, essas funções inicialmente são fornecidas pelo ambiente de pré-instalação do Windows.

Quando o Windows definitivo é instalado e inicializado, seu próprio kernel e os demais componentes centrais passam a gerenciar os recursos do computador.

## Exemplo: gravação no SSD

Quando o instalador precisa gravar um arquivo no SSD, a operação pode ser representada de maneira simplificada:

```text
Instalador
    ↓
Sistema Operacional
    ↓
Sistema de arquivos
    ↓
Subsistema de Entrada/Saída
    ↓
Driver de armazenamento
    ↓
Controlador
    ↓
SSD
```

O programa instalador não precisa saber como controlar fisicamente cada modelo de SSD.

O sistema operacional fornece essa abstração e coordena a utilização do dispositivo.

---

# 4. Modo usuário e modo kernel

O Windows separa a execução de código principalmente entre **modo usuário** e **modo kernel**.

## 4.1. Modo usuário

Aplicações comuns executam com privilégios limitados.

Um programa em modo usuário não pode livremente:

- acessar a memória interna do sistema;
- alterar a memória pertencente a outro processo;
- executar determinadas instruções privilegiadas;
- controlar diretamente qualquer dispositivo físico.

Quando necessita de uma operação protegida, o programa solicita o serviço ao sistema operacional.

---

## 4.2. Modo kernel

O modo kernel possui privilégios elevados e é utilizado pelos componentes centrais do sistema.

Muitos drivers também executam nesse modo, embora o Windows possua modelos que permitem que determinados drivers funcionem em modo usuário.

A separação entre os dois modos aumenta:

- segurança;
- estabilidade;
- isolamento entre aplicações;
- controle dos recursos.

Se qualquer programa pudesse controlar diretamente toda a memória ou qualquer dispositivo, um erro simples poderia comprometer todo o sistema.

---

# 5. Programa × Processo × Thread

Os conceitos de programa, processo e thread estão relacionados, mas representam coisas diferentes.

## 5.1. Programa

Um **programa** é um conjunto de instruções armazenado em um arquivo.

Enquanto não está sendo executado, ele é apenas código armazenado.

---

## 5.2. Processo

Quando um programa é carregado e executado, o sistema operacional cria um **processo**.

Um processo possui recursos como:

- identificador;
- espaço de memória;
- arquivos abertos;
- informações de segurança;
- uma ou mais threads.

---

## 5.3. Thread

Uma **thread** é uma unidade de execução existente dentro de um processo.

Um mesmo processo pode possuir diversas threads.

### Exemplo relacionado à instalação

O Windows Setup existe inicialmente como programa armazenado na mídia de instalação.

Quando é iniciado pelo Windows PE, passa a existir como processo.

De maneira ilustrativa, um instalador multithread poderia dividir suas atividades da seguinte forma:

```text
Processo do instalador
├── Thread da interface
├── Thread de leitura da mídia
├── Thread de processamento
└── Thread de gravação
```

O uso de múltiplas threads pode:

- manter a interface responsiva;
- permitir tarefas concorrentes;
- melhorar a utilização de CPUs com vários núcleos.

---

# 6. Apagar dados × particionar × formatar

Esses três conceitos são diferentes.

## 6.1. Apagar dados

Apagar dados significa remover arquivos ou estruturas existentes.

Dependendo da operação realizada, os dados podem continuar fisicamente presentes até serem sobrescritos.

Em uma instalação limpa, arquivos importantes devem ser copiados para outro dispositivo antes da exclusão de partições ou da formatação.

---

## 6.2. Particionar

Particionar significa dividir logicamente um dispositivo de armazenamento em regiões chamadas **partições**.

Um único SSD pode conter diversas partições.

Em uma instalação do Windows utilizando UEFI e GPT, podem existir:

```text
SSD
├── Partição EFI
├── Partição Reservada da Microsoft
├── Partição do Windows
└── Partição de Recuperação
```

A **EFI System Partition — ESP** utiliza FAT32 e contém arquivos necessários ao processo de boot UEFI.

A partição principal do Windows normalmente utiliza NTFS.

---

## 6.3. Formatar

Formatar significa criar a estrutura de um sistema de arquivos dentro de uma partição.

Por exemplo:

```text
Partição
    ↓
Formatação NTFS
    ↓
Volume capaz de organizar arquivos e diretórios
```

Portanto:

```text
Apagar ≠ Particionar ≠ Formatar
```

---

# 7. Processo de instalação do Windows

## Etapa 1 — Inicialização

O computador é ligado.

Nesse momento, o Windows ainda não está executando.

O firmware da placa-mãe, normalmente **UEFI**, inicializa os componentes essenciais do computador, como:

- CPU;
- memória RAM;
- controladores;
- dispositivos necessários ao boot.

Depois disso, procura um dispositivo inicializável.

Se o pendrive de instalação estiver selecionado, o firmware transfere o controle para os arquivos presentes nessa mídia.

### Conceitos envolvidos

- hardware;
- firmware;
- entrada/saída;
- armazenamento.

---

## Etapa 2 — Inicialização do instalador

A mídia carrega o **Windows Preinstallation Environment — WinPE**.

O WinPE é um sistema operacional reduzido utilizado para instalar, implantar, manter e reparar instalações do Windows.

Ele fornece recursos como:

- kernel e componentes básicos do sistema;
- gerenciamento de memória;
- processos;
- sistema de arquivos;
- drivers;
- rede;
- acesso ao armazenamento.

Em seguida, o Windows Setup é iniciado.

Dependendo da mídia e da configuração, o usuário pode encontrar opções como:

- idioma;
- formato regional;
- layout de teclado;
- chave do produto, quando aplicável;
- edição do Windows;
- termos de licença;
- tipo de instalação.

Para uma instalação limpa é utilizada a instalação personalizada.

### Conceitos envolvidos

- kernel;
- processos;
- memória;
- modo usuário e modo kernel.

---

## Etapa 3 — Reconhecimento do hardware

O WinPE identifica os dispositivos necessários para continuar a instalação.

Entre eles:

- CPU;
- memória RAM;
- controlador USB;
- armazenamento;
- teclado;
- mouse;
- adaptador gráfico.

O sistema utiliza drivers para controlar esses dispositivos.

### Exemplo

Se o ambiente de instalação não possuir um driver compatível com determinado controlador de armazenamento:

```text
SSD existe fisicamente
        ↓
Driver adequado não está disponível
        ↓
Sistema não consegue acessar corretamente o controlador
        ↓
SSD pode não aparecer no instalador
```

Nesse caso pode ser necessário fornecer um driver compatível.

### Conceitos envolvidos

- drivers;
- Plug and Play;
- entrada/saída;
- hardware.

---

## Etapa 4 — Seleção da unidade

O instalador apresenta os dispositivos de armazenamento encontrados.

O usuário seleciona o SSD ou HD onde o Windows será instalado.

Em uma instalação limpa, é importante confirmar cuidadosamente qual dispositivo está sendo utilizado, pois remover partições da unidade errada pode provocar perda de dados.

O acesso ao dispositivo depende do subsistema de entrada/saída e dos drivers de armazenamento.

### Conceitos envolvidos

- drivers;
- armazenamento;
- entrada/saída;
- dispositivos.

---

## Etapa 5 — Particionamento e formatação

A unidade é preparada para receber o Windows.

Em uma instalação limpa, as partições anteriores podem ser removidas, deixando o espaço disponível como:

```text
Espaço não alocado
```

Em um sistema iniciado em modo UEFI, o Windows pode utilizar GPT e criar automaticamente as estruturas necessárias.

Entre elas podem existir:

- **EFI System Partition (ESP)**;
- **Microsoft Reserved Partition (MSR)**;
- partição principal do Windows;
- partição de recuperação.

A partição principal é preparada normalmente com NTFS.

Se as partições anteriores forem excluídas e recriadas, seus arquivos deixam de estar disponíveis por meio da estrutura anterior. Por isso, dados importantes devem ser copiados antes da operação.

### Conceitos envolvidos

- particionamento;
- sistema de arquivos;
- armazenamento;
- drivers;
- entrada/saída.

---

## Etapa 6 — Cópia dos arquivos

O Windows Setup transfere e aplica os arquivos necessários ao sistema na unidade escolhida.

Durante essa etapa ocorrem operações como:

- leitura;
- processamento;
- descompressão;
- criação de diretórios;
- criação de arquivos;
- gravação no SSD.

Uma representação simplificada é:

```text
Pendrive
   ↓
Sistema Operacional
   ↓
Memória RAM
   ↓
Sistema de arquivos
   ↓
Driver de armazenamento
   ↓
SSD
```

Nesse processo trabalham conjuntamente:

- processos;
- threads;
- memória;
- entrada e saída;
- sistema de arquivos;
- drivers.

### Organização dos arquivos instalados

Depois da instalação, o volume principal do Windows passa a possuir uma estrutura semelhante a:

```text
C:\
├── Windows\
├── Program Files\
├── Program Files (x86)\
├── Users\
└── ProgramData\
```

Alguns dos principais diretórios possuem funções específicas:

- **`C:\Windows`** — arquivos principais do sistema operacional;
- **`C:\Program Files`** — aplicações instaladas, principalmente de 64 bits;
- **`C:\Program Files (x86)`** — aplicações de 32 bits em instalações Windows de 64 bits;
- **`C:\Users`** — perfis e arquivos pertencentes aos usuários;
- **`C:\ProgramData`** — dados compartilhados utilizados por aplicações e serviços.

O sistema de arquivos mantém toda essa organização e permite que o sistema e as aplicações localizem seus dados.

---

## Etapa 7 — Instalação do Windows

Depois que os arquivos principais estão no disco, o Windows Setup prepara o sistema para funcionar de forma independente.

São configurados:

- componentes do sistema;
- informações necessárias ao boot;
- configurações iniciais;
- serviços;
- dispositivos;
- dados necessários para as próximas fases da instalação.

Em sistemas UEFI, os arquivos necessários à inicialização são armazenados na partição EFI.

Depois dessa etapa, o computador pode reiniciar e passar a utilizar o sistema presente no armazenamento interno.

### Conceitos envolvidos

- kernel;
- serviços;
- sistema de arquivos;
- segurança;
- boot.

---

## Etapa 8 — Instalação e configuração de drivers

Durante a configuração do Windows instalado, o sistema identifica dispositivos e associa drivers compatíveis.

Podem ser configurados drivers para:

- chipset;
- vídeo;
- rede;
- áudio;
- armazenamento;
- USB;
- Bluetooth.

Muitos drivers de baixo nível executam em modo kernel, embora o Windows também possua modelos de drivers capazes de executar em modo usuário.

Drivers adicionais podem ser obtidos posteriormente por meio de:

- Windows Update;
- fabricante do computador;
- fabricante da placa-mãe;
- fabricante do dispositivo.

### Como o Windows se comunica com o hardware?

De maneira simplificada:

```text
Aplicação
    ↓
Serviços do Sistema Operacional
    ↓
Subsistema de Entrada/Saída
    ↓
Driver
    ↓
Controlador
    ↓
Dispositivo físico
```

O driver traduz as operações esperadas pelo sistema operacional para as operações compreendidas pelo dispositivo.

### Conceitos envolvidos

- drivers;
- Plug and Play;
- entrada/saída;
- componentes privilegiados do sistema.

---

## Etapa 9 — Inicialização do sistema

Depois de reiniciar, o firmware passa a iniciar o Windows presente no armazenamento interno.

De maneira simplificada:

```text
UEFI
 ↓
Windows Boot Manager
 ↓
Carregador do Windows
 ↓
Kernel e componentes centrais
 ↓
Drivers essenciais
 ↓
Serviços
 ↓
Sistema
```

Nesse momento, o Windows instalado assume efetivamente o gerenciamento da máquina.

O sistema começa a:

- administrar memória;
- criar processos;
- escalonar threads;
- carregar drivers;
- iniciar serviços;
- controlar os dispositivos.

### Conceitos envolvidos

- kernel;
- processos;
- threads;
- memória;
- drivers;
- boot.

---

## Etapa 10 — Windows pronto para utilização

Depois das etapas técnicas da instalação, o Windows apresenta a experiência inicial chamada **OOBE — Out-of-Box Experience**.

Nessa etapa podem ser definidos:

- país ou região;
- layout do teclado;
- rede;
- conta;
- nome do dispositivo;
- preferências do sistema.

Depois do OOBE, o desktop é carregado.

Para considerar a instalação realmente concluída, é recomendável:

1. executar o Windows Update;
2. verificar os drivers instalados;
3. confirmar o funcionamento da rede;
4. verificar se existem dispositivos sem driver;
5. reiniciar o computador quando solicitado;
6. verificar a situação da ativação do Windows, quando aplicável.

Depois dessas verificações, o computador está pronto para receber aplicações e ser utilizado normalmente.

### Conceitos envolvidos

- processos;
- modo usuário;
- segurança;
- sistema de arquivos;
- drivers;
- rede.

---

# 8. Linha do tempo da instalação

```text
1. Inicialização
       ↓
2. Inicialização do instalador
       ↓
3. Reconhecimento do hardware
       ↓
4. Seleção da unidade
       ↓
5. Particionamento/formatação
       ↓
6. Cópia dos arquivos
       ↓
7. Instalação do Windows
       ↓
8. Instalação/configuração de drivers
       ↓
9. Inicialização do sistema
       ↓
10. Windows pronto para utilização
```

---

# 9. Relação entre cada etapa e os conceitos estudados

| Etapa | O que acontece? | Conceito envolvido | Por que é importante? |
|---|---|---|---|
| **1. Inicialização** | O computador é ligado e o UEFI inicializa o hardware básico. | Firmware, hardware e I/O | O hardware precisa estar minimamente inicializado antes que um sistema operacional possa ser carregado. |
| **2. Inicialização do instalador** | O WinPE é carregado e executa o Windows Setup. | Kernel, processos e memória | O instalador precisa de um sistema operacional mínimo para executar e controlar recursos. |
| **3. Reconhecimento do hardware** | Os principais dispositivos são identificados. | Drivers, Plug and Play e I/O | O sistema precisa saber como se comunicar com os dispositivos necessários. |
| **4. Seleção da unidade** | O usuário escolhe o armazenamento de destino. | Drivers, armazenamento e I/O | O dispositivo precisa ser corretamente detectado e disponibilizado ao instalador. |
| **5. Particionamento/formatação** | São criadas as divisões do disco e os sistemas de arquivos necessários. | Sistema de arquivos e armazenamento | Cria as estruturas utilizadas para organizar os dados e preparar o sistema para instalação. |
| **6. Cópia dos arquivos** | A imagem e os arquivos do Windows são processados e gravados. | Processos, threads, memória, sistema de arquivos e I/O | Permite ler, processar, organizar e gravar os arquivos necessários ao sistema. |
| **7. Instalação do Windows** | Componentes, configurações, serviços e informações de boot são preparados. | Kernel, sistema de arquivos e segurança | Faz com que o Windows instalado possa posteriormente iniciar e funcionar de forma autônoma. |
| **8. Instalação/configuração de drivers** | Drivers são associados aos dispositivos detectados. | Drivers, Plug and Play e I/O | Permite que o sistema utilize corretamente o hardware existente. |
| **9. Inicialização do sistema** | O Windows instalado carrega kernel, drivers, serviços e processos. | Kernel, memória, processos e threads | É quando o sistema instalado assume efetivamente o controle do computador. |
| **10. Windows pronto para utilização** | O OOBE termina, atualizações e drivers são verificados e o desktop fica disponível. | Modo usuário, processos, segurança e drivers | O computador passa a oferecer um ambiente estável para usuários e aplicações. |

---

# 10. Relação entre os conceitos

Os componentes estudados não funcionam isoladamente.

Considere uma única operação: **gravar um arquivo do Windows no SSD**.

Para que isso aconteça:

1. um **processo** executa o instalador;
2. uma ou mais **threads** executam suas instruções;
3. o gerenciamento de **memória** fornece espaço em RAM;
4. o programa solicita uma operação de **entrada/saída**;
5. os componentes privilegiados do sistema processam a solicitação;
6. o **sistema de arquivos** determina como o arquivo será organizado;
7. o **driver** controla o dispositivo de armazenamento;
8. o SSD realiza fisicamente a gravação.

De maneira simplificada:

```text
Processo
   ↓
Thread
   ↓
Memória
   ↓
Sistema Operacional
   ↓
Sistema de arquivos
   ↓
Driver
   ↓
Hardware
```

Uma operação aparentemente simples depende, portanto, da integração entre vários componentes do sistema operacional.

---

# 11. Desafio Final

## 11.1. Se não existisse um Sistema Operacional, quais partes desse processo precisariam ser realizadas diretamente pelo usuário ou pelos programas?

O firmware ainda poderia realizar a inicialização básica do computador. Depois dessa etapa, porém, o software executado diretamente sobre a máquina teria que implementar diversas funções que atualmente são fornecidas pelo sistema operacional.

Os programas precisariam controlar diretamente:

- memória;
- processador;
- armazenamento;
- dispositivos;
- interrupções;
- entrada e saída;
- organização dos arquivos;
- execução concorrente;
- segurança.

Por exemplo, atualmente um programa pode solicitar ao sistema operacional:

```text
Grave este arquivo.
```

Sem um sistema operacional, o próprio programa precisaria conhecer:

- qual controlador existe;
- qual protocolo o dispositivo utiliza;
- quais blocos de armazenamento estão livres;
- onde os dados devem ser gravados;
- como criar diretórios;
- como atualizar os metadados;
- como evitar conflitos com outros programas.

Cada aplicação também precisaria possuir suporte específico para diversos modelos de hardware.

Uma das principais funções de um sistema operacional é justamente fornecer **abstrações**.

Em vez de aplicações trabalharem diretamente com blocos físicos de um SSD, elas utilizam conceitos como:

```text
arquivos
diretórios
volumes
```

Em vez de controlar diretamente a CPU, trabalham com abstrações como:

```text
processos
threads
```

Isso reduz a complexidade dos programas e melhora a segurança, a compatibilidade e a estabilidade do computador.

---

## 11.2. Qual dos conceitos estudados considero mais importante?

Entre os conceitos estudados, considero o **kernel** o mais importante para que o computador passe de um conjunto de componentes de hardware para um sistema capaz de executar aplicações.

No Windows, ele atua em conjunto com outros componentes centrais executados em modo kernel para fornecer os mecanismos necessários ao funcionamento do sistema.

Isso não significa que drivers, processos, memória ou sistemas de arquivos sejam dispensáveis.

Todos são fundamentais.

Porém, o kernel ocupa uma posição central na coordenação de funções como:

- uso da CPU;
- escalonamento de threads;
- tratamento de interrupções;
- execução de operações privilegiadas;
- interação com componentes responsáveis por memória e entrada/saída;
- comunicação com drivers e outros componentes internos do sistema.

Uma representação simplificada seria:

```text
Aplicações
    ↓
Serviços do Sistema Operacional
    ↓
Kernel e componentes centrais
    ↓
Drivers
    ↓
Hardware
```

Sem essa camada central, cada aplicação teria que interagir muito mais diretamente com os recursos físicos do computador.

Isso aumentaria significativamente a complexidade e reduziria:

- segurança;
- isolamento;
- estabilidade;
- compatibilidade.

Por isso, considero o kernel o conceito central entre os estudados, pois ele participa diretamente da base que permite transformar recursos físicos em serviços controlados utilizados pelo restante do sistema operacional e pelas aplicações.

---

# 12. Conclusão

A instalação do Windows demonstra de forma prática os principais conceitos estudados em Sistemas Operacionais.

Desde o momento em que o computador é ligado até o carregamento do desktop, participam do processo:

- firmware;
- kernel;
- processos;
- threads;
- memória;
- sistema de arquivos;
- drivers;
- entrada e saída;
- mecanismos de segurança.

A instalação também demonstra que um sistema operacional não é apenas uma interface gráfica.

Sua função fundamental é **gerenciar e abstrair os recursos do hardware**, oferecendo serviços que podem ser utilizados pelos programas sem que cada aplicação precise controlar diretamente todos os componentes físicos.

Ao final do processo, o computador deixa de ser apenas um conjunto de componentes de hardware e passa a oferecer um ambiente organizado, controlado e adequado para executar aplicações.

---

# 13. Referências

- MICROSOFT. **Windows 11 requirements**. Microsoft Learn. Disponível em: <https://learn.microsoft.com/windows/whats-new/windows-11-requirements>. Acesso em: 20 set. 2026.
- MICROSOFT. **Windows Setup Installation Process**. Microsoft Learn. Disponível em: <https://learn.microsoft.com/windows-hardware/manufacture/desktop/windows-setup-installation-process>. Acesso em: 20 set. 2026.
- MICROSOFT. **Windows Preinstallation Environment (Windows PE)**. Microsoft Learn. Disponível em: <https://learn.microsoft.com/windows-hardware/manufacture/desktop/winpe-intro>. Acesso em: 20 set. 2026.
- MICROSOFT. **Windows Setup: Installing using the MBR or GPT partition style**. Microsoft Learn. Disponível em: <https://learn.microsoft.com/windows-hardware/manufacture/desktop/windows-setup-installing-using-the-mbr-or-gpt-partition-style>. Acesso em: 20 set. 2026.
- MICROSOFT. **UEFI/GPT-based hard drive partitions**. Microsoft Learn. Disponível em: <https://learn.microsoft.com/windows-hardware/manufacture/desktop/configure-uefigpt-based-hard-drive-partitions>. Acesso em: 20 set. 2026.
- MICROSOFT. **User Mode and Kernel Mode**. Microsoft Learn. Disponível em: <https://learn.microsoft.com/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode>. Acesso em: 20 set. 2026.
- MICROSOFT. **Windows Drivers**. Microsoft Learn. Disponível em: <https://learn.microsoft.com/windows-hardware/drivers/>. Acesso em: 20 set. 2026.
- TANENBAUM, Andrew S.; BOS, Herbert. **Sistemas Operacionais Modernos**. Pearson.
