# 📘 Conceito de Processos  

## 🎯 Objetivo da Unidade

Compreender **o que é um processo**, como ele é criado, executado e finalizado, além de entender como o sistema operacional gerencia múltiplos processos de forma eficiente.

---

## 1. O que é um Processo

Um **processo** é um **programa em execução**, juntamente com todo o seu contexto de execução.  
Enquanto um programa é algo **estático** (um arquivo armazenado em disco), o processo é **dinâmico**, existindo apenas enquanto está sendo executado.

### Programa × Processo

- **Programa**
  - Código estático armazenado em disco
  - Não consome CPU diretamente
- **Processo**
  - Programa em execução
  - Possui estado, memória, registros e recursos alocados

👉 Um mesmo programa pode gerar **vários processos diferentes**.

---

## 2. Criação e Finalização de Processos

### Criação de Processos

Um processo pode ser criado quando:
- O sistema é iniciado
- Um usuário executa um programa
- Um processo cria outro processo

Durante a criação, o SO:
- Aloca memória
- Inicializa estruturas de controle
- Define o estado inicial do processo

### Finalização de Processos

Um processo pode terminar quando:
- Conclui sua execução normalmente
- Ocorre um erro
- É finalizado pelo sistema ou por outro processo

Ao finalizar, o SO libera todos os recursos associados.

---

## 3. Estados de um Processo

Durante sua vida, um processo passa por diferentes **estados**, que representam sua situação em relação à CPU.

### Estados Principais

- **Novo (New)**  
  Processo acabou de ser criado

- **Pronto (Ready)**  
  Está apto a executar, aguardando CPU

- **Executando (Running)**  
  Está usando a CPU no momento

- **Bloqueado (Waiting)**  
  Aguardando algum evento (E/S, recurso, sinal)

- **Finalizado (Terminated)**  
  Execução encerrada

### Diagrama de Estados (descrição)

- Novo → Pronto  
- Pronto → Executando  
- Executando → Bloqueado / Pronto / Finalizado  
- Bloqueado → Pronto  

Esse ciclo é controlado pelo **escalonador do sistema operacional**.

---

## 4. PCB – Process Control Block

O **PCB (Bloco de Controle de Processo)** é a estrutura de dados usada pelo SO para armazenar todas as informações de um processo.

### Conteúdo do PCB

- Identificador do processo (PID)
- Estado atual
- Contador de programa
- Registradores da CPU
- Informações de memória
- Prioridade
- Recursos alocados

O PCB é essencial para que o sistema operacional consiga **interromper e retomar processos corretamente**.

---

## 5. Troca de Contexto (Context Switch)

A **troca de contexto** ocorre quando o SO interrompe um processo em execução para executar outro.

Durante a troca:
- O estado do processo atual é salvo no PCB
- O estado do próximo processo é restaurado
- A CPU passa a executar o novo processo

### Overhead

A troca de contexto:
- Não executa trabalho útil
- Consome tempo de CPU
- É chamada de **overhead**

Por isso, o SO deve equilibrar desempenho e justiça no uso da CPU.

---

## 6. Processos Pai e Filho

Um processo pode criar outros processos, formando uma relação de **pai e filho**.

- Processo **pai**: cria um novo processo
- Processo **filho**: processo criado

Essa relação é comum em sistemas modernos e permite:
- Organização hierárquica
- Compartilhamento controlado de recursos

---

## 7. Execução Concorrente de Processos

### Multitarefa

- Vários processos aparentam executar ao mesmo tempo
- Em sistemas com uma CPU, ocorre alternância rápida entre processos

### Multiprogramação

- Vários processos permanecem na memória
- O SO escolhe qual executa, otimizando o uso da CPU

### Multiprocessamento

- Uso de **múltiplos processadores ou núcleos**
- Execução simultânea real de processos diferentes

---

## 8. Exemplos Práticos de Processos

### Gerenciador de Tarefas

Ferramenta que permite:
- Visualizar processos em execução
- Monitorar uso de CPU e memória
- Finalizar processos

### Processos no Linux

- Cada processo possui um PID
- Comandos comuns: `ps`, `top`, `htop`
- Forte uso de processos pai e filho

### Processos no Windows

- Visualização via Gerenciador de Tarefas
- Processos associados a aplicações e serviços
- Forte integração com interface gráfica

### Comparação Geral

- Linux: maior controle e transparência
- Windows: foco em usabilidade e integração

---

## 📚 Considerações Finais

O conceito de processo é **fundamental** para o entendimento de sistemas operacionais. Ele explica como programas são executados, como a CPU é compartilhada e como o sistema mantém organização e desempenho.

Este conteúdo serve como base para estudos posteriores sobre:
- Threads
- Escalonamento de processos
- Sincronização
- Concorrência e paralelismo
