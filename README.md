# Missão 1 — Implementação do Quick Sort em Python

## 1. Objetivo da atividade

Implementar o algoritmo **Quick Sort** para ordenar um vetor com **100.000 números aleatórios**, executar a ordenação **3 vezes**, medir o tempo de cada execução e calcular a média dos tempos.

Bibliotecas utilizadas: `random` e `time`.

---

## 2. Importação das bibliotecas

```python
import random
import time
```

- `random`: gera números aleatórios e escolhe pivôs.
- `time`: mede o tempo de execução com `time.perf_counter()`.

---

## 3. Entendendo o Quick Sort

O Quick Sort usa a estratégia **dividir para conquistar**:

1. Escolhe um elemento chamado **pivô**.
2. Coloca os valores menores ou iguais ao pivô à esquerda e os maiores à direita.
3. Repete o procedimento nas duas partes.
4. Continua até que tudo esteja ordenado.

Exemplo:

```text
Vetor inicial: [8, 3, 7, 4, 9, 2, 5]
Pivô: 5
Após particionar: [3, 4, 2] [5] [8, 7, 9]
Resultado final: [2, 3, 4, 5, 7, 8, 9]
```

---

## 4. Criando a função de particionamento

```python
def particionar(vetor, inicio, fim):
    indice = random.randint(inicio, fim)
    vetor[indice], vetor[fim] = vetor[fim], vetor[indice]

    pivo = vetor[fim]
    i = inicio - 1

    for j in range(inicio, fim):
        if vetor[j] <= pivo:
            i += 1
            vetor[i], vetor[j] = vetor[j], vetor[i]

    vetor[i + 1], vetor[fim] = vetor[fim], vetor[i + 1]
    return i + 1
```

### Explicação passo a passo

**Passo 1 — Escolher o pivô aleatório**

```python
indice = random.randint(inicio, fim)
```

Seleciona uma posição aleatória na partição, reduzindo a chance de divisões muito desequilibradas.

**Passo 2 — Mover o pivô para o final**

```python
vetor[indice], vetor[fim] = vetor[fim], vetor[indice]
```

**Passo 3 — Guardar o pivô e iniciar o índice de separação**

```python
pivo = vetor[fim]
i = inicio - 1
```

**Passo 4 — Comparar os elementos**

```python
for j in range(inicio, fim):
    if vetor[j] <= pivo:
        i += 1
        vetor[i], vetor[j] = vetor[j], vetor[i]
```

Cada elemento menor ou igual ao pivô é movido para a região à esquerda.

**Passo 5 — Colocar o pivô na posição definitiva**

```python
vetor[i + 1], vetor[fim] = vetor[fim], vetor[i + 1]
```

**Passo 6 — Retornar a posição do pivô**

```python
return i + 1
```

---

## 5. Implementando o Quick Sort

```python
def quicksort(vetor, inicio, fim):
    while inicio < fim:
        pivo = particionar(vetor, inicio, fim)

        if pivo - inicio < fim - pivo:
            quicksort(vetor, inicio, pivo - 1)
            inicio = pivo + 1
        else:
            quicksort(vetor, pivo + 1, fim)
            fim = pivo - 1
```

- `while inicio < fim`: continua enquanto a partição tiver pelo menos dois elementos.
- `particionar(...)`: reorganiza os valores e encontra a posição final do pivô.
- A menor partição é ordenada recursivamente; a maior é processada pelo `while`. Essa otimização reduz a profundidade da recursão.

---

## 6. Criando o vetor de 100.000 posições

```python
vetor = [random.randint(1, 100000) for _ in range(100000)]
```

`random.randint(1, 100000)` gera um inteiro entre 1 e 100.000. A expressão é repetida 100.000 vezes. Podem existir valores repetidos.

---

## 7. Medindo o tempo de execução

```python
inicio = time.perf_counter()
quicksort(vetor, 0, len(vetor) - 1)
fim = time.perf_counter()
tempo = fim - inicio
```

O tempo registrado corresponde **somente à ordenação**, sem incluir a criação do vetor.

---

## 8. Executando três vezes

```python
tempos = []

for i in range(3):
    vetor = [random.randint(1, 100000) for _ in range(100000)]

    inicio = time.perf_counter()
    quicksort(vetor, 0, len(vetor) - 1)
    fim = time.perf_counter()

    tempo = fim - inicio
    tempos.append(tempo)
    print(f"Execução {i + 1}: {tempo:.6f} segundos")
```

Em cada repetição é criado um novo vetor aleatório, a ordenação é executada e seu tempo é armazenado.

---

## 9. Calculando a média

A média aritmética dos três tempos é:

\[
\text{Média} = \frac{T_1 + T_2 + T_3}{3}
\]

```python
media = sum(tempos) / len(tempos)
print(f"\nTempo médio: {media:.6f} segundos")
```

### Exemplo de saída (valores ilustrativos)

```text
Execução 1: 0.182345 segundos
Execução 2: 0.190123 segundos
Execução 3: 0.185678 segundos

Tempo médio: 0.186049 segundos
```

Os tempos reais dependem do computador e dos valores sorteados.

---

## 10. Código completo

```python
import random
import time


def particionar(vetor, inicio, fim):
    indice = random.randint(inicio, fim)
    vetor[indice], vetor[fim] = vetor[fim], vetor[indice]

    pivo = vetor[fim]
    i = inicio - 1

    for j in range(inicio, fim):
        if vetor[j] <= pivo:
            i += 1
            vetor[i], vetor[j] = vetor[j], vetor[i]

    vetor[i + 1], vetor[fim] = vetor[fim], vetor[i + 1]
    return i + 1


def quicksort(vetor, inicio, fim):
    while inicio < fim:
        pivo = particionar(vetor, inicio, fim)

        if pivo - inicio < fim - pivo:
            quicksort(vetor, inicio, pivo - 1)
            inicio = pivo + 1
        else:
            quicksort(vetor, pivo + 1, fim)
            fim = pivo - 1


tempos = []

for i in range(3):
    vetor = [random.randint(1, 100000) for _ in range(100000)]

    inicio = time.perf_counter()
    quicksort(vetor, 0, len(vetor) - 1)
    fim = time.perf_counter()

    tempo = fim - inicio
    tempos.append(tempo)
    print(f"Execução {i + 1}: {tempo:.6f} segundos")

    # Verifica a ordenação fora do intervalo medido
    assert all(vetor[j] <= vetor[j + 1] for j in range(len(vetor) - 1))

media = sum(tempos) / len(tempos)
print(f"\nTempo médio: {media:.6f} segundos")
```

---

## 11. Complexidade do Quick Sort

| Situação | Complexidade |
|---|---|
| Melhor caso | `O(n log n)` |
| Caso médio esperado | `O(n log n)` |
| Pior caso | `O(n²)` |

No melhor caso, o pivô divide o vetor em partes equilibradas. No pior caso, ocorrem divisões muito desiguais. A escolha aleatória do pivô diminui a chance de chegar ao pior caso, mas não o elimina.

---

## 12. Conclusão

Nesta atividade, implementamos o **Quick Sort em Python** para ordenar um vetor de **100.000 posições** preenchido com números aleatórios. Utilizamos `random` para gerar os valores e `time.perf_counter()` para medir o tempo de ordenação. O programa executa três testes, verifica se cada vetor ficou ordenado e calcula a média dos tempos, permitindo analisar o desempenho do algoritmo.
