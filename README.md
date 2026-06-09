# 🐍 Jogo da Cobrinha - Abordagens de Inteligência Artificial

[![Python 3.8+](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0-red.svg)](https://pytorch.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.2-orange.svg)](https://scikit-learn.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📌 Sobre o Projeto

Este projeto implementa e compara **três abordagens de Inteligência Artificial** para resolver o Jogo da Cobrinha em matrizes de diferentes tamanhos:

1. **Busca Heurística** - A* com distância de Manhattan + BFS para segurança
2. **Algoritmo Genético** - Evolução de sequências de movimentos com crossover e mutação
3. **Aprendizado Supervisionado** - Rede neural treinada com dados da heurística

**Disciplina:** Inteligência Artificial - UFSCar  
**Professor:** Murilo Coelho Naldi  
**Período:** 2025/2 - Turma B

## 🎯 Objetivos

- Construir um ambiente determinístico do Jogo da Cobrinha em Python
- Implementar um agente heurístico usando A* e BFS
- Desenvolver um Algoritmo Genético para otimizar pontuação e movimentos
- Treinar uma Rede Neural para imitar o comportamento da heurística
- Comparar o desempenho das três abordagens

## 🎮 Implementação do Jogo

### Características
- Tabuleiro N×N (ímpar para evitar ciclos perfeitos)
- Cobra representada como vetor de coordenadas
- Geração determinística de comidas (all_positions shufflado)
- Sistema de colisão com paredes e corpo
- Detecção de vitória (cobra preenche todo tabuleiro)

### Movimentos
| Comando | Direção |
|---------|---------|
| `w` | Cima (-1, 0) |
| `s` | Baixo (+1, 0) |
| `a` | Esquerda (0, -1) |
| `d` | Direita (0, +1) |

## 🤖 Abordagem 1: Busca Heurística (A* + BFS)

### Funcionamento
1. **A*** - Encontra caminho mais curto até a comida (heurística: Manhattan)
2. **Safe-to-eat** - Simula se comer a comida não levará a um beco sem saída
3. **BFS Tail Length** - Se não for seguro comer, escolhe movimento que maximiza distância até a cauda

### Funções Principais
- `astar()` - Busca A* com desempate aleatório
- `bfs_tail_length()` - Calcula distância da nova cabeça até a cauda
- `safe_to_eat()` - Verifica se o caminho até a comida é seguro
- `agent_move()` - Orquestra a decisão do movimento

### Resultados (100 jogos - 13×13)

| Métrica | Valor |
|---------|-------|
| Pontuação Média | 105.95 |
| Pontuação Máxima | 129 |
| Pontuação Mínima | 46 |

## 🧬 Abordagem 2: Algoritmo Genético

### Representação do Indivíduo
- Genótipo: sequência de movimentos (lista de caracteres 'w','s','a','d')
- Tamanho variável (permite crescimento/encolhimento)

### Operadores Genéticos
- **Seleção**: Torneio (k=3)
- **Crossover**: Ponto único (apenas segmento final - 10% do genótipo)
- **Mutação**: Substituição, inserção e deleção (apenas segmento final)
- **Elitismo**: Mantém os melhores indivíduos

### Função de Fitness
- fitness = (score × 100) - (len(genotype) / 500)
- Maximiza pontuação
- Penaliza sequências longas (incentiva eficiência)

### Parâmetros
| Parâmetro | Valor |
|-----------|-------|
| População | 20 |
| Gerações | 30 |
| Crossover Rate | 0.7 |
| Mutação Rate | 0.01 |
| Elitismo | 1 |
| Segmento Final | 10% |

### Resultados (Matriz 17×17)

| Métrica | Inicial | Final | Melhoria |
|---------|---------|-------|-----------|
| Pontuação Máxima | 288 | 288 | - |
| Movimentos | 4572 | 3798 | ↓ 16.93% |

## 🧠 Abordagem 3: Aprendizado Supervisionado

### Dataset
- Gerado pelo agente heurístico (300 jogos)
- **Total de amostras**: 4.500
- **Entrada**: 9 features (posição cabeça, distância comida, direções livres, tamanho cobra)
- **Saída**: 4 classes (movimentos w/s/a/d)

### Arquitetura da Rede Neural
- Input (9) → FC(32) → ReLU → Dropout(0.2) → FC(16) → ReLU → FC(4)

### Treinamento
- **Loss**: CrossEntropyLoss
- **Otimizador**: Adam (lr=0.001)
- **Batch Size**: 16
- **Épocas**: 30
- **Divisão**: 80% treino / 20% validação

### Resultados (Tabuleiro 3×3)

| Métrica | Valor |
|---------|-------|
| Acurácia Treino | 95.08% |
| Acurácia Validação | 94.67% |
| Score Médio | 7.0 (máximo possível) |

## 📊 Comparação das Abordagens

| Critério | Heurística | Algoritmo Genético | Superv. Learning |
|----------|------------|--------------------|--------------------|
| Tempo de execução | ⚡ Rápido | 🐢 Lento (evolução) | ⚡ Rápido (inferência) |
| Qualidade | ✅ Muito boa | ✅ Excelente (otimizada) | ✅ Boa |
| Generalização | ✅ Alta | 🟡 Depende da população | 🟡 Limitada ao treino |
| Esforço implementação | 🟡 Médio | 🐢 Alto | 🐢 Alto |

## 🛠️ Tecnologias Utilizadas

- Python 3.8+
- PyTorch (redes neurais)
- NumPy (operações numéricas)
- Matplotlib (visualizações)
- Scikit-learn (métricas - no notebook de spam)


## 🚀 Como Executar

1. Clone o repositório:
```bash
git clone https://github.com/seu-usuario/projeto-ia-cobrinha.git
cd projeto-ia-cobrinha
```

2. Instale as dependências:
```bash
pip install -r requirements.txt
```

3. Execute o Jupyter Notebook:
```bash
jupyter notebook jogo_cobrinha.ipynb
```

## 📈 Experimentos Realizados
1. Heurística vs Tamanho da Matriz
- Testado para matrizes de 3×3 até 29×29
- Atingiu 100% da pontuação máxima em matrizes ≤ 7×7
-Desempenho varia em matrizes maiores (limite de movimentos)

2. GA para Otimização de Movimentos
- População inicial: 100 indivíduos (jogos completos)
- Evolução por 50 gerações
- Redução de 16.93% no número de movimentos

3. Supervised Learning
- Dataset: 300 jogos, 4.500 amostras
- Acurácia de validação: 94.67%
- Tabuleiro 3×3 totalmente resolvido

## 🧠 Aprendizados
- Heurísticas são eficientes, mas podem usar movimentos redundantes
- Algoritmos Genéticos são excelentes para otimização de soluções existentes
- Aprendizado Supervisionado consegue imitar o comportamento da heurística com boa precisão
- A combinação de abordagens pode produzir resultados superiores
- Espaço de busca do Jogo da Cobrinha é enorme - estratégias de otimização são essenciais

## 👨‍💻 Autores
- Gustavo Cesar Bento Laurindo (821402)
- Ana Carolina Santos Barbizan (759089)
- Lucas de Andrade Marin (791663)
- Kailayni Rodrigues Janez (824751)
