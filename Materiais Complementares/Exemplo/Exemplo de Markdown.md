# 🌟 Exemplo Completo e Avançado de Markdown

Este documento mostra **diversos recursos do Markdown**, do básico ao avançado, para deixar seus arquivos mais interessantes, profissionais e interativos.

---

## 📑 Índice
1. [Texto e Listas](#-texto-e-listas)
2. [Links e Imagens](#-links-e-imagens)
3. [Códigos](#-códigos)
4. [Tabelas](#-tabelas)
5. [Checklists](#-checklists)
6. [Citações](#-citações)
7. [Diagramas Mermaid](#-diagramas-mermaid)
8. [Matemática (LaTeX)](#-matemática-latex)
9. [Mídia Avançada](#-mídia-avançada)
10. [Badges e Status](#-badges-e-status)
11. [Spoilers/Colapsáveis](#-spoilerscolapsáveis)
12. [Tabelas Avançadas](#-tabelas-avançadas)
13. [Diagramas Extras](#-diagramas-extras)
14. [ASCII/Unicode](#-asciiunicode)
15. [Blocos de Alerta](#-blocos-de-alerta)
16. [Estrutura de Pastas](#-estrutura-de-pastas)

---

## 📝 Texto e Listas

### Listas não ordenadas:
- Item comum
- **Item em negrito**
- *Item em itálico*
  - Subitem
    - Sub-subitem

### Listas ordenadas:
1. Primeiro
2. Segundo
3. Terceiro

---

## 🔗 Links e Imagens

[Visite o GitHub](https://github.com)  

Imagem exemplo:  

![Sistemas Operacionais](['https://markdown-here.com/img/icon256.png](https://hermes.dio.me/articles/cover/0d03528c-4207-4f42-8dbc-d4e2548a3cd4.png'])


---

## 💻 Códigos

Exemplo de código em **Python**:

```python
def soma(a, b):
    return a + b

print(soma(3, 5))
```

---

## 📊 Tabelas

| Nome       | Idade | Profissão       |
|------------|-------|----------------|
| Ana        | 25    | Engenheira     |
| Bruno      | 30    | Designer       |
| Carlos     | 28    | Desenvolvedor  |

---

## ✅ Checklists

- [x] Criar exemplo Markdown  
- [ ] Revisar formatação  
- [ ] Compartilhar com a equipe  

---

## 💬 Citações

> "Markdown é simples, mas poderoso!"  
> — Alguém inspirado

---

## 📈 Diagramas Mermaid

### Fluxograma
```mermaid
flowchart TD
    A[Início] --> B{Decisão?}
    B -- Sim --> C[Executa ação]
    B -- Não --> D[Fim]
```

### Diagrama de Gantt
```mermaid
gantt
    title Exemplo de Projeto
    dateFormat  YYYY-MM-DD
    section Planejamento
    Analisar requisitos :done, a1, 2025-10-01, 2d
    Criar plano :active, a2, after a1, 3d
    section Execução
    Desenvolvimento : a3, after a2, 7d
    Testes : a4, after a3, 5d
```

---

## 🔢 Matemática (LaTeX)

Equação de segundo grau:  

$$x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$  

Integral:  

$$\int_0^\infty e^{-x^2} dx = \frac{\sqrt{\pi}}{2}$$

---

## 🎬 Mídia Avançada

GIF animado:  
![Funny Cat](https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif)

Vídeo embutido (HTML):  
<video src="https://www.w3schools.com/html/mov_bbb.mp4" controls width="300"></video>

---

## 🏅 Badges e Status

![Build](https://img.shields.io/badge/build-passing-brightgreen)
![Versão](https://img.shields.io/badge/version-1.0.0-blue)
![Licença](https://img.shields.io/badge/license-MIT-yellow)

---

## 📂 Spoilers/Colapsáveis

<details>
  <summary>Ver mais detalhes</summary>
  Aqui dentro pode ter texto, código, imagens etc.
</details>

---

## 📊 Tabelas Avançadas

| Funcionalidade | Status   | Observações       |
|----------------|----------|------------------|
| Login          | ✅ Ok    | Em produção      |
| Pagamentos     | 🚧 Em dev | Falta integração |
| Relatórios     | ❌ Erro  | Revisar queries  |

---

## 📐 Diagramas Extras

### Diagrama de sequência
```mermaid
sequenceDiagram
    Alice->>Bob: Mensagem 1
    Bob-->>Alice: Resposta
```

### Diagrama de classes
```mermaid
classDiagram
    Animal <|-- Cachorro
    Animal <|-- Gato
    Animal : +String nome
    Cachorro : +latir()
    Gato : +miar()
```

---

## 🔡 ASCII/Unicode

Exemplo de menu com **ASCII Art**:  

```text
+---------+
|  MENU   |
+---------+
| Opção 1 |
| Opção 2 |
+---------+
```

---

## ⚠️ Blocos de Alerta

> [!NOTE]  
> Este é um bloco de nota estilizado!  

> [!WARNING]  
> Atenção: algo importante aqui.  

---

# 🌐 Estrutura de Pastas — Projeto Web

```bash
meu-projeto/
├── public/                     # Arquivos públicos acessíveis diretamente
│   ├── index.html              # Página principal
│   ├── favicon.ico             # Ícone do site
│   └── assets/                 # Imagens, ícones, etc.
│       ├── logo.png
│       └── banner.jpg
│
├── src/                        # Código-fonte principal
│   ├── components/             # Componentes reutilizáveis
│   │   ├── Header.jsx
│   │   ├── Footer.jsx
│   │   └── Button.jsx
│   │
│   ├── pages/                  # Páginas da aplicação
│   │   ├── Home.jsx
│   │   ├── About.jsx
│   │   └── Contact.jsx
│   │
│   ├── services/               # Comunicação com APIs / back-end
│   │   └── api.js
│   │
│   ├── hooks/                  # Hooks customizados
│   │   └── useAuth.js
│   │
│   ├── context/                # Contextos globais (ex: autenticação, tema)
│   │   └── AuthContext.jsx
│   │
│   ├── styles/                 # Estilos globais e temas
│   │   ├── global.css
│   │   └── variables.css
│   │
│   ├── App.jsx                 # Componente raiz
│   └── main.jsx                # Ponto de entrada da aplicação
│
├── .gitignore                  # Arquivos ignorados pelo Git
├── package.json                # Dependências e scripts do projeto
├── vite.config.js              # Configuração do bundler (Vite)
└── README.md                   # Documentação do projeto
```

---

## 🎉 Conclusão

Este arquivo `.md` reúne **recursos do básico ao avançado** para deixar sua documentação mais **rica, interativa e atraente**.
