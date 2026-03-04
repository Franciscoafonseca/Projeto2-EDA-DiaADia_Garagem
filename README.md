# Oficina de Reparações — Simulador (C++)

Projeto em C++ que simula a gestão de uma **oficina automóvel** com:
- **Fila de espera** de carros (com possibilidade de **prioridade**)
- **Estações de Trabalho (ETs)** com **mecânico**, **capacidade** e **faturação**
- Processo diário de **reparação** com componente probabilística
- Registo de **carros reparados** numa **árvore binária de pesquisa** (ordenada por `modelo`)
- Menu de **gestão** (alterar tempos, adicionar prioridade, remover mecânico, adicionar ET, gravar/carregar)

---

## Estrutura do projeto

Ficheiros principais (conforme o repositório):

- `Main.cpp` — ponto de entrada, inicialização e menus
- `Oficina.cpp / Oficina.h` — criação da oficina, fila de espera, colocar carros nas ETs, prioridade
- `ET.cpp / ET.h` — criação das ETs e lógica de reparação diária
- `Mecanico.cpp / Mecanico.h` — criação/validação de mecânicos e utilitários (`verificarnumero`)
- `Carro.cpp / Carro.h` — criação de carros, probabilidade, operações em listas (adicionar/remover)
- `Gestao.cpp / Gestao.h` — menu de gestão, ações administrativas, gravar/carregar
- `Arvore.cpp / Arvore.h` — árvore de carros reparados (inserção, altura, impressão)

Ficheiros de dados:
- `marcas.txt` — lista de marcas
- `modelos.txt` — lista de modelos

Projeto/IDE:
- `2fase.sln`, `2fase.vcxproj`, `2fase.vcxproj.filters` (Visual Studio)

---

## Funcionalidades

### Simulação diária
- Cada “dia” (ciclo) adiciona novos carros à fila de espera.
- A fila é reorganizada para colocar **prioritários primeiro**.
- A partir de certo ciclo, ocorre a **reparação** de carros em cada ET.

### Estações de Trabalho (ET)
Cada ET tem:
- `mecanico` (marca, nome, preço/dia)
- `capacidade` aleatória (entre 2 e 4)
- lista de `carros_a_ser_reparados`
- `faturacao`
- árvore `Carrosreparados` para registar carros concluídos

### Reparação
- Cada carro tem `tempo_reparacao_max` (2 a 4 dias) e `dias_em_reparacao`.
- Existe uma probabilidade (ex.: 0.15) de um carro terminar antes do tempo máximo (após já ter pelo menos 1 dia).
- Quando termina:
  - calcula-se custo: `dias_em_reparacao * preco_reparacao_por_dia`
  - adiciona-se o carro à árvore de “reparados”
  - atualiza-se a faturação da ET

### Gestão (menu do gestor)
Disponibiliza ações como:
1. Atualizar tempo máximo de reparação por **marca+modelo**
2. Definir carro como **prioritário** (por ID)
3. Remover mecânico de uma ET (e substituir por novo)
4. Gravar estado da oficina para ficheiro (`oficina.txt`)
5. Carregar oficina de ficheiro (há função implementada)
6. Adicionar nova ET
7. Imprimir carros reparados (representação por árvore)

---

## Como executar

### Requisitos
- Windows + Visual Studio (projeto `.sln`)
- Compilador C++ compatível (MSVC recomendado)

### Abrir no Visual Studio
1. Abrir `2fase.sln`
2. Confirmar que `marcas.txt` e `modelos.txt` estão no mesmo diretório do executável (ou copiar para a pasta `Debug/Release`)
3. Compilar e correr (`Local Windows Debugger`)

---

## Como usar (menus)

Ao iniciar:
- O sistema carrega `modelos.txt` e `marcas.txt`
- Cria uma oficina com número de ETs aleatório
- Cria fila inicial (ex.: 10 carros)
- Mostra o estado atual (`MenuInfo`)
- Abre o menu principal

### Menu principal
- **s**: avançar para o dia seguinte (gera carros, organiza prioridade, repara, aloca carros)
- **g**: abrir menu de gestão
- **t**: terminar

### Gestão
Inclui as opções descritas acima (tem validação de inputs numéricos e limites).

---

## Formato do ficheiro `oficina.txt`

A gravação (`gravar_oficina`) usa separador `|` e inclui:
1. Dados gerais da oficina: `ciclos | carrostotais | numero_ets`
2. Para cada ET:
   - `ID | capacidade | faturacao | num_carros_a_ser_reparados | num_carros_reparados`
   - depois grava os carros em reparação (um por linha)
   - depois grava os dados do mecânico
3. Fila de espera:
   - tamanho da fila
   - lista de carros (um por linha)

> Nota: a função `carregar_oficina` está implementada, mas é importante garantir que a ordem de leitura corresponde exatamente à ordem de gravação.

---

## Árvore de carros reparados

- Inserção ordenada por `modelo` (string), indo para a direita quando `modelo >= atual->modelo`.
- A função `imprimirarvore` imprime a árvore “deitada”, útil para visualização rápida.
- O `numeroniveis` calcula a altura.

---
# Inicialização do Programa

Quando o programa inicia, executa os seguintes passos principais:

1. Inicializa o gerador de números aleatórios (`srand(time(NULL))`).
2. Carrega os ficheiros de dados `marcas.txt` e `modelos.txt`.
3. Cria a estrutura da oficina com um número aleatório de **Estações de Trabalho (ETs)**.
4. Inicializa os **mecânicos** e a **capacidade** de cada ET.
5. Cria os espaços para carros que estão a ser reparados em cada ET.
6. Inicializa a **fila de espera** da oficina.
7. Gera os primeiros **10 carros** para a fila de espera.
8. Organiza a fila colocando **carros prioritários primeiro**.
9. Mostra o estado inicial da oficina.
10. Inicia o **menu principal**, onde o utilizador pode avançar dias, gerir a oficina ou terminar o programa.
