## Funções de redução/mapeamento

Você já achou que estava super avançado, eu sei, mas porém contudo, entretanto, todavia. Agora que você já sabe como os iteráveis funcionam, nós podemos avançar mais e fazer melhor uso de funções embutidas do python. Como:

- any()
- all()
- len()
- sum()
- zip()
- enumerate()
- map()
- reversed()

Mas, primeiro, vamos reduzir tudo a categorias. Como assim?

any, all, len e sum (vamos lembrar, não estamos importando nada, AINDA). São funções de redução. Dando uma prévia, são funções que consomem iteráveis e retornam um único valor.

Por outro lado, as outras funções (zip, enumerate, map e reversed) produzem uma nova coleção porém modificada.

Tá bom, vamos explicar detalhadamente.

## Funções de redução

Funções de redução recebem um iterável e retornam um único elemento. Ok, já disse isso, mas é só isso. Juro.

Vamos exemplificar com a função any

```Python
lista = [1, 2, 3, 4, 5]

any(lista) # True
```

Viu, nem doeu nada. Mas ok, nós vamos explicar. A função any itera sobre o iterável, que nesse caso é uma lista e verifica se seu valor é `True`. Tá, mas como ela faz isso?

```Python
for elemento in lista:
    print(elemento, bool(elemento))

# 1 True
# 2 True
# 3 True
# 4 True
```

Ou, seja ele executa a função `bool()` para cada elemento da lista.

Tá, mas como ele decide o que é Verdadeiro ou Falso?

A [documentação do python](https://docs.python.org/3.6/library/stdtypes.html#truth-value-testing) diz que 'qualquer objeto pode ser testado (...) e que os seguintes valores são considerados falsos:

- None
- False
- zeros
    - 0 (int)
    - 0.0 (float)
    - 0j (complex)
- Sequências vazias
    - [] (lista)
    - '' (string)
    - {} (Dicionários/conjuntos)

Ou seja, qualquer coisa que não seja uma sequência vazia, None/False e zero é True. Olha só, você acabou de aprender um truque grátis e nem doeu:

```Python

if var:
    (...)
```

Você nunca mais vai precisar perguntar se algo é True, por exemplo.

```Python
# um péssimo exemplo
if var == True:
    (...)
```

Bom, agora que você já sabe como o any verifica os valores, vamos continuar a explicar sua função.

`any()` retorna True se qualquer elemento da seqûencia for `True` (usando `bool(elemento)`) para todos os ítens da sequência. Ou seja, a única maneira de ele ser `False` é que todos os elementos no iterável sejam falsos.

```python

lista = [[],'', {}, 0]

any(lista) # False
```

```python
lista = [[1],'', {}, 0]

any(lista) # True
```

Olha que legal, você acabou de aprender a lidar com sua primeira função de redução. Mas não vamos parar por aí.

Jaber diz: `por que??? Agora que eu tinha entendido tudo. Estava tudo tão fácil, por favor. Pare`

Só mais um pouco, eu sei que você consegue. Vamos lá

### `all()`

Diferente do `any()` o `all()` só retorna `True`, se todos os elementos da sequência, aplicados a `bool()` retornarem `True`.

```python
lista = [1, 2, 3, 4, 5]

all(lista) # True
```

```python
lista = [0, 1, 2, 3, 4, 5]

all(lista) # True
```

Viu, foi tão simples. Agora, vamos a mais uma de redução

### `len()`

`len()` diferente das outras sequências, efetua uma soma de números existente em uma sequência. vamos tentar implementar um `len()`?

```Python

def _len(sequência):
    """
    Vamos usar _len pois len é uma palavra reservada
    """
    contador = 0
    for x in sequência:
        contador += 1

    return contador

_len([1,2,3,4])

```

Viu, não é nada tão absurdo. A função `len()` conta a quantidade de elementos existentes em uma sequência.

```python
len([1,2,3,4,5]) # 5

len('string') # 6
```

Vale lembrar que para que nossas sequências, sabe, aqueles que a gente mesmo implementa? elas só precisam ter o método `__len__()`

o interpretador python pega o nosso objeto do tipo sequência e chama o seu método especial.

Podemos ver, como já foi dito antes, todas as classes de sequência tem que ter um `__iter__()` ou um `__getitem__()`. Mas, se você parar para notar, todas as sequências implementam `__len__()` também. Vamos conferir:

```python

'__len__' in dir([1,2,3]) # True
'__len__' in dir('string') # True
'__len__' in dir((1,2,3)) # True
'__len__' in dir({1,2,3}) # True
'__len__' in dir(4) # False

```

Viu só, ele está em toda sequência e não está em objetos que não podem ser iterados, como por exemplo um número inteiro.

Pronto, agora você está preparado para aprender mais um pouco sobre funções de redução. Existem outras funções, mas esse não é momento para falarmos delas. Talvez depois de funções de ordem superior.


## Funções de mapeamento

As funções de mapeamento padrões da biblioteca padrão (zip(), enumerate() ,reversed()) são maneiras super interessantes de trabalhar com iteráveis, vamos lá.

A função zip não é uma função de compressão, como pode parecer. Ela funciona como um zipper, sabe, aquele da sua calça jeans? é tipo isso.

```python
lista = [1, 2, 3, 4, 5]
lista_reversa = reversed(lista)

list(zip(lista, lista_reversa)) # [(1, 5), (2, 4), (3, 3), (4, 2), (5, 1)]
```

Você acaba de ser surpreendido, você aprender duas funções em um único exemplo. Falei que aprender programação funcional ia te fazer bem, eu disse.

Como podemos ver, a função zip juntou os elementos de mesmo index de cada sequência e os agrupou em tuplas. Como podemos ver:

```Python
(lista[0], lista_reversa[0]) # TypeError
```

Jaber diz: `Você não disse que elas eram acessadas pelo index, não acredito mais em você!`

Calma Jaber, lembra quando eu disse que uma sequência podia ser acessada usando `__iter__()`? Pense comigo. Para consumir um iterador preguiçoso, é, é isso mesmo, é assim que chamamos esse tipo de iterador que em python não pode ser acessado por index.

Isso explica quase tudo sobre a linha `list(zip(..., ...))`. Como o zip produz um iterador preguiçoso, não conseguimos acessar nenhum item pelo index, mas chamando a função embutida `next()`. Com isso podemos consumir um iterador sem acessar ele pelo index.

Mas por que estamos falando disso outra vez? Todas as nossas funções de mapeamento retornam esse tipo de sequência preguiçosa.

Então, caso a gente queira ver o que tem na sequência, a sua representação, precisamos construir uma sequência não preguiçosa.

```python
print(zip(lista, lista_reversa)) # <zip object at xpto>
print(reversed(lista)) # <list_reverseiterator object at xpto>
```

A resposta é exatamente essa. Quer dizer que não existe uma representação 'visível' desse tipo de iterável.

```Python
'__str__' in reversed(lista) # False
'__repr__' in reversed(lista) # False
```

Não temos um método para representar a saída do nosso objeto. Então, como faço pra ver?

```Python
# construtor de uma lista
print(list(reversed(lista))) # [5, 4, 3, 2, 1]

# construtor de um conjunto
print(set(reversed(lista))) # {5, 4, 3, 2, 1}

# construtor de uma lista
print(tuple(reversed(lista))) # (5, 4, 3, 2, 1)
```

A essa altura do campeonato, você já deve ter notado o que faz a função `reversed()`. Ele devolve um iterável preguiçoso e invertido do que entrou.

```Python
list(reversed([1,2,3])) # [3, 2, 1]
list(reversed('String')) # ['g', 'n', 'i', 'r', 't', 'S']
```

Então... depois desse grande devaneio, vamos voltar ao `zip()`. Não esqueci dele, só precisava te ensinar o que ele vai retornar, vai que você se assusta. Não?


Então... o zip retorna um iterável preguiçoso que casa valores de iteráveis.

```Python
zip([1,2,3], [4,5,6]) # [(1, 4), (2, 5), (3, 6)]

# E você achando que ele só zipava dois? estava enganado
zip([1,2,3], [4,5,6], [7,8,9]) # [(1, 4, 7), (2, 5, 8), (3, 6, 9)]
```

Não é muito complicado de entender, o único problema do `zip()` é que todas as sequências tem que ter o mesmo `len()`. (Nossa você já está entendo tudo, eu sei). Vamos voltar a falar mais sobre essa quantidade de argumentos infinitos, só que mais tarde. Agora é o enumerate.

A função `enumerate()` faz uma coisa muito parecida com o zip, só que ele gera a sequência a ser zipada pra você. Olha que legal.

```Python
list(enumerate([1,2,3])) # [(0, 1), (1, 2), (2, 3)]
```

Olha só que lindo, você não esperava por isso, né? Toda a sua sequência é agrupada com o número do index dela. Fantástico.

Algum tempo atrás falamos sobre iteração implícita, lembra?

```python
lista = [1, 2, 3, 4, 5]

# iteração
for x in lista:
    print(x)

# 1
# 2
# 3
# 4
# 5
```

Era exatamente esse trecho de código e a gente comparou com o código em C, que usa o index para navegar pelo vetor. Só que as vezes não precisamos de um foreach, a gente quer mesmo é os valores. Então segue uma outra dica bonita:

```python
lista = [1, 2, 3, 4, 5]

# iteração
for x, y in enumerate(lista):
    print(x, y)

# 0 1
# 1 2
# 2 3
# 3 4
# 4 5
```

É explícito? não, mas tem os index, as vezes a gente precisa deles.

Bom, enumerate é bem simples. Mas temos uma função lá no começo que deixamos de falar. Na verdade vamos só dar uma olhada nela, pois ela é a ponte entre esse e próximo vídeo. Enfim, `map()`.

Vamos, falta pouco pra acabar por hoje, você aguenta.

Embora já tenhamos usado a função `map()` em quase todos os vídeos anteriores, agora o seu grande segredo será revelado.

`map()` é uma função de MAPeamento, é essa não foi tão difícil. Porém, ela é um pouco diferente das nossas funções usadas agora. Ela é uma função que chamamos de __Função de ordem superior__. Isso não é um conceito complicado, quer dizer que ela é uma função que recebe uma função como argumento. Existem muitos casos de como usar map, mas uma coisa é certa, a função usada por `map()` só pode receber um argumento. Vamos ao código:


```Python
def func(x):
    """
    Já usamos essa função, lembra?
    """
    return x +2

list(map(func, [1,2,3])) # [3, 4, 5]
```

Também já contei pra você que ela executa a função em todos os elementos da sequência. Tá, você já sabe como usar o map, eu sei.

Porém você já pensou que falamos hoje da função `bool()`? já pensou usar ela em todos os elementos da sequência?

```Python

map(bool, [0, 1, 2]) # [False, True, True]
```

Era só nesse ponto que eu queria tocar, todas as funções embutidas do python que recebem só um único argumento podem ser usadas com map. Mas o gostinho das funções que recebem funções ficou na pontinha da língua? Até o próximo vídeo.