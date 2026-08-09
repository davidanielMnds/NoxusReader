# Requisitos — NoxusReader

## 1. Visão geral

O NoxusReader é uma aplicação desktop para leitura de arquivos PDF, com foco em
simplicidade, desempenho e conforto de leitura. O diferencial central é a
retomada automática de leitura: o aplicativo lembra exatamente onde o usuário
parou em cada livro (página e nível de zoom), sem exigir ação manual.

Esta versão (v1 / MVP) tem como público-alvo o próprio desenvolvedor, rodando
em ambiente Linux (dual-boot Ubuntu).

## 2. Requisitos funcionais

| ID     | Descrição |
|--------|-----------|
| RF-01  | O sistema deve permitir ao usuário selecionar um arquivo PDF do disco através de um seletor de arquivos. |
| RF-02  | O sistema deve renderizar e exibir a página atual do PDF selecionado. |
| RF-03  | O sistema deve permitir navegar para a próxima página. |
| RF-04  | O sistema deve permitir navegar para a página anterior. |
| RF-05  | O sistema deve permitir ir diretamente para um número de página específico. |
| RF-06  | O sistema deve permitir aumentar o nível de zoom da página exibida. |
| RF-07  | O sistema deve permitir diminuir o nível de zoom da página exibida. |
| RF-08  | O sistema deve salvar automaticamente o progresso (página atual e zoom) em dois momentos: a cada 10 segundos (autosave periódico) e ao fechar o livro/aplicação. |
| RF-09  | Ao reabrir um PDF já lido anteriormente, o sistema deve restaurar automaticamente a página e o zoom salvos. |
| RF-10  | O progresso de leitura de todos os livros deve ser persistido em um único arquivo JSON, contendo uma entrada por livro identificada pelo caminho do PDF. |

## 3. Requisitos não funcionais

| ID     | Descrição |
|--------|-----------|
| RNF-01 | A aplicação deve rodar em ambiente Linux (Ubuntu), usando Java 21+. |
| RNF-02 | A interface deve utilizar tema escuro por padrão. |
| RNF-03 | A renderização de páginas não deve travar a interface (thread principal do Swing deve permanecer responsiva). |
| RNF-04 | A abertura de um PDF de tamanho médio (até ~50MB) deve ocorrer em tempo aceitável para uso interativo (referência subjetiva: poucos segundos). |
| RNF-05 | O código deve ser organizado em camadas (UI, aplicação, domínio, persistência, PDF), sem lógica de negócio implementada diretamente nas classes de interface. |
| RNF-06 | O formato de persistência (JSON) deve ser legível por humanos, para facilitar depuração manual durante o desenvolvimento. |
| RNF-07 | A navegação entre páginas próximas (anterior/próxima) deve ocorrer sem travamento perceptível da interface, mesmo em PDFs com páginas visualmente pesadas. A estratégia de cache/pré-renderização para atender este requisito será definida em `ARCHITECTURE.md`. |

## 4. Fora de escopo (v1)

Itens abaixo são conscientemente adiados — não são esquecimentos, são decisões
de escopo para manter o MVP entregável:

- Suporte a Windows/macOS (fica para uma iteração futura).
- Suporte a PDFs protegidos por senha ou corrompidos (assume-se PDF válido e sem proteção).
- Conceito de "biblioteca" com lista de livros já abertos (v1 usa apenas seleção manual via `JFileChooser`).
- Anotações, marcadores (bookmarks) ou destaques no texto.
- Busca de texto dentro do PDF.
- Múltiplas abas ou múltiplos livros abertos simultaneamente.
- Sincronização de progresso entre dispositivos.

## 5. Suposições e restrições

- O usuário sempre fornecerá um caminho de arquivo PDF válido e acessível.
- O ambiente de desenvolvimento e execução é Linux (Ubuntu), com Java 21 já instalado.
- O arquivo JSON de progresso não será editado manualmente pelo usuário final (embora seja legível, isso é uma facilidade para debug, não um requisito funcional).
- Por ser um arquivo único compartilhado entre todos os livros, um crash durante a escrita é um risco aceito nesta versão (v1) — estratégias de escrita atômica ou backup poderão ser avaliadas futuramente, mas não são requisito do MVP.
- A biblioteca de renderização de PDF (a ser definida em `ARCHITECTURE.md`) é uma dependência externa e seu comportamento interno não é responsabilidade deste documento.
