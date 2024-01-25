---
title: Slow hashing
permalink: /hashing-slow
hide: true
---
    
# Slow hashing

Operations on data structures based on hashing are _usually_ efficient, it is possible for the operations to be slow due to collisions. We will next see how the Python dictionary becomes slow, when the keys stored there are chosen in a specific way.

In order to choose the keys, we need to know the exact implementation of the dictionary. The relevant aspect of the implementation are the hash function, the hash table size and the collision handling. 

Hajautusta käyttävissä tietorakenteissa operaatiot toimivat _yleensä_ tehokkaasti, mutta on mahdollista, että operaatiot ovatkin hitaita törmäysten takia. Näemme seuraavaksi, miten Pythonin sanakirja muuttuu hitaaksi, kun sinne lisättävät avaimet valitaan sopivalla tavalla.

Jotta pystymme valitsemaan törmäyksiä aiheuttavat avaimet, meidän tulee tuntea sanakirjan tarkka toteutustapa. Tehokkuuteen vaikuttavia asioita ovat hajautusfunktio, hajautustaulun koko sekä törmäysten käsittelytapa. We can find these details in the [Pythonin source code](https://github.com/python/cpython/blob/main/Objects/dictobject.c).

## Hash function

The Python hash function `hash` is implemented differently for different data types. For small integers, the hash value is simply the integer itself:

Pythonin hajautusfunktio `hash` on toteutettu eri tavalla eri tietotyypeille. Tiettyä rajaa pienemmillä kokonaisluvuilla hajautusfunktio on toteutettu yksinkertaisesti niin, että kokonaisluvun hajautusarvo on sama kuin luku itse:

```console?lang=python
> hash(42)
42
> hash(123)
123
> hash(1337)
1337
```

We will use small integers as keys, so we can assume the above hash function.

Käytämme tässä sanakirjan avaimina pieniä kokonaislukuja, jolloin niiden hajautusarvot saadaan yllä olevalla tavalla.

## Hash table size

The hash table size $$N$$ used with the Python dictionary is initially $$8$$. The hash table is implemented so that at most $$2/3$$ of the locations are occupied. If this limit is exceeded, the size of the hash table is doubled.

The following code illustrates this:

Pythonin sanakirjassa hajautustaulun koko $$N$$ on alussa $$8$$. Hajautustaulu on toteutettu niin, että enintään $$2/3$$ paikoista saa olla täynnä. Jos tämä raja ylittyy alkion lisäämisen jälkeen, hajautustaulun koko kaksinkertaistetaan.

Seuraava koodi havainnollistaa asiaa:

```python
import sys

n = 1000

numbers = {}
old_size = 0

for i in range(n):
    new_size = sys.getsizeof(numbers)

    if new_size != old_size:
        print(len(numbers), sys.getsizeof(numbers))
        old_size = new_size

    numbers[i] = True
```

The code creates a dictionary `numbers` and adds elements to it one at a time. When the memory usage of the hash table changes, the code prints out the number of elements and the memeory usage of the hash table. The output of the code is as follows:

Koodi luo sanakirjan `numbers` ja lisää siihen alkioita yksi kerrallaan. Kun hajautustaulun koko muistissa muuttuu, koodi tulostaa alkioiden määrän ja hajautustaulun koon. Koodin tulostus on seuraava:

```
0 64
1 232
6 360
11 640
22 1176
43 2272
86 4696
171 9312
342 18520
683 36960
```

For example, the size of the hash table increases when the element count reaches $$6$$, and then the hash table size increases from $$8$$ to $$16$$. The size increases, because the $$6/8$$ exceeds the limit $$2/3$$. The size increases again when the element count reaches $$11$$, because $$11/16$$ exceeds $$2/3$$, and then the new size is $$32$$.

Esimerkiksi hajautustaulun koko kasvaa, kun alkioiden määräksi tulee $$6$$. Tässä tilanteessa hajautustaulun koko on ollut aiemmin $$8$$ ja siitä tulee $$16$$. Koko kasvaa, koska $$6/8$$ ylittää rajan $$2/3$$. Koko kasvaa seuraavan kerran, kun alkioiden määräksi tulee $$11$$. Tällöin kooksi tulee $$32$$, koska $$11/16$$ ylittää rajan $$2/3$$.

## Handling collisions

The following code shows how the Python dictionary chooses the location in the hash table for the pair `(key, value)`.

Seuraava koodi esittää, miten Pythonin sanakirja valitsee paikan hajautustaulussa, kun sanakirjaan tallennetaan avaimelle `key` arvo `value`.

```python
index = hash(key) % N
perturb = hash(key)
while True:
    if not table[index]:
        table[index] = (key, value)
        break
    perturb = perturb >> 5
    index = (5 * index + 1 + perturb) % N
```

Notice that this is not the actual code implementing the dictionary (Python is implemented in the C language), but this code shows the idea.

The variable `index` is the location in the hash table and its initial value is computed with the formula `hash(key) % N`. If the location `index` occupied, the next location to try is computed as with the formula `(5 * index + 1 + perturb) % N`. This continues until an unoccupied location is found.

The purpose of the variable `perturb` is to make collisions less likely. It affects the location formula for the first few tries. The initial value of `perturb' is the hash value of the key, and in each round its new value is computed as `perturb >> 5'. This bit shift operation corresponds to dividing by $$32$$ and rounding down.

Huomaa, että tämä koodi ei ole todellinen Pythonin sanakirjan taustalla oleva koodi (Python on toteutettu C-kielellä), mutta koodi esittää paikan valinnan idean.

Muuttujassa `index` on paikka hajautustaulussa ja muuttujan alkuarvo lasketaan avaimen hajautusarvon perusteella kaavalla `hash(key) % N`. Jos paikka `index` on varattu, lasketaan uusi paikka kaavalla `(5 * index + 1 + perturb) % N`. Tätä jatketaan, kunnes löytyy vapaa paikka avaimen ja arvon tallentamiseen.

Muuttujan `perturb` tavoitteena on vähentää törmäyksiä, ja se vaikuttaa paikan valintaan silmukan muutaman ensimmäisen kierroksen ajan. Muuttujan `perturb` alkuarvo on avaimen hajautusarvo, ja joka kierroksella uudeksi arvoksi tulee `perturb >> 5`. Tämä bittisiirto vastaa jakamista $$32$$:lla pyöristäen alaspäin.

## Constructing the input

Now we know enough about the Python dictionary implementation, and we can design an input that makes the dictionary operations slow. The idea is to use keys that collide with each other and cause the hash table operations slow down. The following code constructs such an input:

Nyt meillä on riittävät tiedot Pythonin sanakirjan toiminnasta, jotta voimme muodostaa syötteen, jossa sanakirja toimii hitaasti. Ideana on lisätä sanakirjan avaimia, jotka aiheuttavat törmäyksiä toisilleen ja hidastavat hajautustaulun toimintaa. Seuraava koodi muodostaa tällaisen syötteen:

```python
def find(table, key):
    N = len(table)
    index = hash(key) % N
    perturb = hash(key)
    count = 0
    while True:
        count += 1
        if not table[index]:
            return index, count
        perturb = perturb >> 5
        index = (5 * index + 1 + perturb) % N

n = 100000
chain_len = 50000
threshold = 5000

N = 2**18
table = [None] * N
keys = []

key = 1
for i in range(chain_len):
    keys.append(key)
    key = (5 * key + 1) % N

key = N
while len(keys) < n:
    index, count = find(table, key)
    if count > threshold:
        table[index] = True
        keys.append(key)
    key += 1
```
The code creates an input of `n` keys. The hash table size `N` is chosen accordingly so that the limit of $$2/3$$ is not exceeded. The list `keys` stores the chosen keys.

First, the code adds keys that form a chain of length `chain_len`. The first key in the chain is `1` and the other keys in the chain are computed iteratively with the formula `key = (5 * key + 1) % N`. Then the code adds the remaining keys so that each added key causes at least `threshold` steps along the chain before an unoccupied location is found. This makes their addition to the dictionary slow.

Koodi muodostaa syötteen, jossa on `n` avainta. Tätä vastaavasti valitaan hajautustaulun koko `N` niin, että enintään $$2/3$$ paikoista on käytössä. Listaan `keys` lisätään sanakirjaan tallennettavat avaimet.

Koodi lisää ensin hajautustauluun avaimia, jotka muodostavat ketjun, jonka pituus on `chain_len`. Tämän ketjun ensimmäinen avain on `1` ja muut avaimet muodostetaan kaavalla `key = (5 * key + 1) % N`. Tämän jälkeen koodi lisää loput avaimet niin, että jokaisen avaimen lisääminen aiheuttaa ainakin `threshold` askelta ketjussa kulkemista. Tämän ansiosta avainten lisääminen sanakirjaan tulee olemaan hidasta.

## Testing efficiency

The following code measures how much time it takes to add the keys in the list `keys` to'the dictionary:

Seuraava koodi mittaa, kauanko kestää lisätä listan `keys` avaimet sanakirjaan:

```python
import time

...

start_time = time.time()
numbers = {}
for key in keys:
    numbers[key] = True
end_time = time.time()

print(round(end_time - start_time, 2), "s")
```

Let us first do a test where the list `keys` contains $$10^5$$ random numbers in the range $$1 \dots 10^8$$. The list can be formed as follows:

Tehdään ensin testi, jossa lista `keys` sisältää $$10^5$$ satunnaisesti valittua avainta väliltä $$1 \dots 10^8$$. Lista voidaan muodostaa näin:

```python
import random

keys = random.sample(range(1, 10**8 + 1), 10**5)
```

This test took 0.02 seconds on the test computer. This is what one would expect when the dictionary operates efficiently.

Let us then do the test where the list `keys` is created with the code described above designed to cause a lot of collisions. Now the test time is 10.57 seconds, which is indeed very slow in comparison due to the collisions.

Testikoneella tässä testissä kuluu aikaa 0.02 s. Tämä on odotettava kesto tavallisessa tilanteessa, jossa sanakirja toimii tehokkaasti.

Tehdään sitten testi, jossa lista `keys` on muodostettu aiemmalla koodilla niin, että siinä olevat avaimet aiheuttavat törmäyksiä. Nyt aikaa kuluukin 10.57 s, eli sanakirja toimii hitaasti törmäysten takia.

## Entä käytännössä?

Although it is possible to construct an input that makes the dictionary slow, in practice such inputs occur very rarely. In essence, this happens only when the input was specifically designed so.

However, the existence of such designed inputs can be a problem in web programming, because a malicious attacker could try feeding such data to the site, causing the site to slow down. In python this possibility [has been addressed](https://github.com/python/cpython/issues/57912) by changing the operation of the function `hash` for strings each time Python is started:

Vaikka on mahdollista muodostaa syöte, jossa sanakirja toimii hitaasti, tällaiset syötteet ovat käytännössä hyvin harvinaisia. Niitä voi esiintyä lähinnä silloin, jos joku haluaa tahallaan saada sanakirjan toimimaan hitaasti.

Hajautuksen hitaus on kuitenkin uhka esimerkiksi web-ohjelmoinnissa, koska hyökkääjä voi koettaa lähettää sivustolle dataa, joka aiheuttaa törmäyksiä hajautuksessa. Pythonissa [tähän on varauduttu](https://github.com/python/cpython/issues/57912) niin, että funktion `hash` toiminta merkkijonoilla muuttuu aina, kun Python käynnistetään uudestaan:

```console?lang=python
$ python3
> hash("apina")
-8847049641918498300
> exit()
$ python3
> hash("apina")
5108947336973792736
> exit()
$ python3
> hash("apina")
-7637574785741815573
> exit()
```

This makes it impossible to construct a list of string keys that causes the dictionary to slow down.

Tämän vuoksi ei ole mahdollista muodostaa tässä esitetyllä tavalla listaa merkkijonoista, joka aiheuttaisi sanakirjan hidastumisen.

