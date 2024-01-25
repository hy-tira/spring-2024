---
title: Hash table
permalink: /hashtable
hide: true
---
    
# Hash table

A _hash table_ is a data structure for efficiently maintaining a set of elements. For example, the Python data structures `set` and `dict` are implemted using a hash table.

A hash table consists of $$N$$ locations indexed $$0,1,\dots,N-1$$. Each element has a specific location in the hash table based on its hash value.

A _hash function_ determines the location of an element in the hash table. The function takes an element $$x$$ as a parameter and returns an integer $$h(x)$$ as the _hash value_ of $$x$$.  The location of $$x$$ in the hash table is $$h(x) \bmod N$$, i.e., the remainder from dividing $$h(x)$$ by $$N$$.

When the element is added to the hash table, it will be inserted at the location $$h(x) \bmod N$$. Similarly, when checking if the hash table contains the element $$x$$, it will be searched at the location $$h(x) \bmod N$$.

_Hajautustaulu_ (_hash table_) on tietorakenne, jonka avulla voidaan pitää yllä tehokkaasti alkioiden joukkoa. Esimerkiksi Pythonin tietorakenteet `set` ja `dict` on toteutettu hajautustaulun avulla.

Hajautustaulussa on $$N$$ kohtaa, joilla on indeksit $$0,1,\dots,N-1$$. Jokaisella alkiolla on hajautustaulussa tietty sijainti, joka lasketaan alkion arvon perusteella.

_Hajautusfunktio_ (_hash function_) määrittää alkion sijainnin hajautustaulussa. Funktiolle annetaan alkio $$x$$ ja se palauttaa kokonaisluvun $$h(x)$$ eli _hajautusarvon_ (_hash value_). Alkion sijainti hajautustaulussa on $$h(x) \bmod N$$ eli $$h(x)$$:n jakojäännös $$N$$:llä.

Kun alkio $$x$$ lisätään hajautustauluun, se lisätään kohtaan $$h(x) \bmod N$$. Vastaavasti kun halutaan tarkastaa, onko hajautustaulussa alkiota $$x$$, sitä etsitään hajautustaulun kohdasta $$h(x) \bmod N$$.

## Example

Consider an example, where $$N=10$$ and $$h(x)=7x$$. We will store the elements of the set $$\{2,4,5,9,18,30\}$$ into the hash table. We obtain the following hash values for the elements:

Tarkastellaan esimerkkiä, jossa $$N=10$$ ja $$h(x)=7x$$. Tallennetaan hajautustauluun joukon $$\{2,4,5,9,18,30\}$$ sisältö. Alkioille saadaan seuraavat hajautusarvot:

Element | Hash value
-- | --
$$2$$ | $$7 \cdot 2 \bmod 10 = 4$$
$$4$$ | $$7 \cdot 4 \bmod 10 = 8$$
$$5$$ | $$7 \cdot 5 \bmod 10 = 5$$
$$9$$ | $$7 \cdot 9 \bmod 10 = 3$$
$$18$$ | $$7 \cdot 18 \bmod 10 = 6$$
$$30$$ | $$7 \cdot 30 \bmod 10 = 0$$

Thus the elements are located in the hash table as follow:

Tämän perusteella alkiot sijoitetaan hajautustauluun seuraavasti:

Location | $$0$$ | $$1$$ | $$2$$ | $$3$$ | $$4$$ | $$5$$ | $$6$$ | $$7$$ | $$8$$ | $$9$$
Elements | $$30$$ | -- | -- | $$9$$ | $$2$$ | $$5$$ | $$18$$ | -- | $$4$$ | --

When we want to check if the hash table contains an element $$x$$, we search for it at the location $$7x \bmod 10$$. For example, we will search for the element $$4$$ at the location $$7 \cdot 4 \bmod 10 = 8$$.

Kun halutaan tarkastaa, onko hajautustaulussa alkiota $$x$$, sitä etsitään hajautustaulun kohdasta $$7x \bmod 10$$. Esimerkiksi kun alkio on $$4$$, sitä etsitään hajautustaulun kohdasta $$7 \cdot 4 \bmod 10 = 8$$.

## Collisions

A _collision_ is a situation, where two elements have the same location in the hash table. An implementation of a hash table has to be prepared for collisions, because the size $$N$$ of the hash table is typically much smaller than the number of possible element values.

For example, if we add the elements $$4$$ and $$14$$ into our example hash table, both will be assigned to the location $$8$$, because $$7 \cdot 4 \bmod 10 = 7 \cdot 14 \bmod 10 = 8$$.

There are two commonly used techniques for handling collisions:

* _Chaining_: Each hash table location contains a list that stores all the elements assigned to that location.

* _Open addressing_: Each location contains at most one element. The location of an element $$x$$ is determined by computing the function $$p(x,i)$$ for $$i=0,1,2,\dots$$ until an unoccupied location is found.

_Törmäys_ (_collision_) on tilanne, jossa kahdella alkiolla on sama sijainti hajautustaulussa. Tällaiseen tilanteeseen tulee varautua hajautustaulun toteutuksessa, koska hajautustaulun koko $$N$$ on yleensä aina pienempi kuin mahdollisten alkioiden määrä.

Esimerkiksi jos äskeiseen hajautustauluun sijoitetaan alkiot $$4$$ ja $$14$$, kummankin alkion sijainniksi tulee $$8$$, koska $$7 \cdot 4 \bmod 10 = 7 \cdot 14 \bmod 10 = 8$$.

Törmäysten käsittelyyn on kaksi tavallista tapaa:

* _Ketjuhajautus_ (_chaining_): Jokaisessa hajautustaulun kohdassa on lista, joka sisältää kaikki siihen kohtaan kuuluvat alkiot.

* _Avoin hajautus_ (_open addressing_): Jokaisessa hajautustaulun kohdassa voi olla enintään yksi alkio. Alkion $$x$$ paikka etsitään funktiolla $$p(x,i)$$ kokeilemalla arvoja $$i=0,1,2,\dots$$ kunnes löytyy vapaa paikka.

## Efficiency

A hash table operates efficiently if the elements are fairly evenly distributed over the whole hash table. The distribution is determined by the hash table size and the choice of the hash function.

Chaining is efficient if each location contains only a small number of elements. Then the lists are short and the list operations are fast.

Open addressing is efficient if a small number of tries is sufficient for finding the location of an element, i.e., only a few calls to the function $$p(x,i)$$ is needed.

However, in the worst case, hash table can become slow and the operations can take $$O(n)$$ time. For example, this happens with chaining if every element is assigned into the same location.

Hajautustaulu toimii tehokkaasti, jos alkiot sijoittuvat tasaisesti eri puolille hajautustaulua. Tähän vaikuttavat hajautustaulun koko ja hajautusfunktion valinta.

Ketjuhajautuksessa hajautustaulu on tehokas, jos jokaisessa hajautustaulun kohdassa on pieni määrä alkioita, jolloin listat ovat lyhyitä ja niiden käsittely on tehokasta.

Avoimessa hajautuksessa hajautustaulu on tehokas, jos riittää pieni määrä kokeiluja alkion paikan löytämiseen eli funktiota $$p(x,i)$$ kutsutaan pieni määrä kertoja.

Jos alkiot sijoittuvat tasaisesti hajautustauluun ja hajautustaulu on riittävän suuri, hajautustaulun operaatiot (lisäys, haku, poisto) vievät aikaa $$O(1)$$. Käytännössä yleensä on aina näin, minkä takia voidaan olettaa, että hajautustaulun operaatiot vievät aikaa $$O(1)$$.

Kuitenkin pahimmassa tapauksessa hajautustaulu voi toimia hitaasti ja operaatiot vievät aikaa $$O(n)$$. Näin käy esimerkiksi ketjuhajautuksessa silloin, kun jokainen alkio saa saman hajautusarvon.
