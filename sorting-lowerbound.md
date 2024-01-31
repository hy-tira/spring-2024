---
title: Sorting lower bound
permalink: /sorting-lowerbound
hide: true
---

# Sorting lower bound

It is possible to prove that $$O(n \log n)$$ is the best possible time complexity of a general purpose sorting algorithm. The assumption here is that the algorithm determines the correct order by comparing elements to each other.

Let us consider a situation, where the algorithm is given a list of length $$n$$, where each element is the range $$1 \dots n$$ occurs exactly once. There are $$n!$$ possibilities for the initial order of the elements, and in each case the algorithm has to do a different set of moves to accomplish the sorting.

Whenever the algorithm compares two elements to each other, it obtains some information that can be used in sorting. Consider a situation, where $$n=3$$ and
the algorithm has not made any comparisons yet. Since the algorithm has no information about the order yet, it has to consider all of the following orders as possible:

$$[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]$$

Suppose then that the algorithm compares the first two elements of the list to find out that the first element is bigger than the second element. Now the algorithm can narrow down the set of possible orders to the following:

$$[2,1,3], [3,1,2], [3,2,1]$$

For the algorithm to operate correctly it has to continue comparisons until the set of possible orders is reduced to a single order. Initially, there are $n!$ possible orders, and after each comparison, at least half of the orders may remain possible. Thus the algorithm may need at least $$\log(n!)$$ comparisons before it has enough information to complete the sorting. This gives us the lower bound

On mahdollista osoittaa, että $$O(n \log n)$$ on paras mahdollinen aikavaativuus yleiskäyttöiselle järjestämisalgoritmille. Tässä oletuksena on, että algoritmi vertailee alkioita toisiinsa ja muuttaa sen perusteella niiden järjestystä.

Tarkastellaan tilannetta, jossa algoritmille annetaan $$n$$ alkion lista, jossa jokainen luku väliltä $$1 \dots n$$ esiintyy tasan kerran. Listan alkiot voivat olla $$n!$$ eri järjestyksessä, ja kussakin tapauksessa algoritmin tulee toimia eri tavalla, jotta lista tulee järjestykseen.

Kun algoritmi vertailee kahta alkiota toisiinsa, tämä antaa algoritmille tietoa, jota se voi käyttää järjestämisessä. Tarkastellaan tilannetta, jossa $$n=3$$ ja algoritmi ei ole vielä tehnyt vertailuja. Koska algoritmilla ei ole vielä tietoa listasta, algoritmin näkökulmasta lista voi olla mikä tahansa seuraavista:

$$[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]$$

Oletetaan sitten, että algoritmi vertailee listan kahta ensimmäistä alkiota toisiinsa ja saa tietää, että ensimmäinen alkio on suurempi kuin toinen alkio. Tämän perusteella algoritmi pystyy rajaamaan tilannetta niin, että lista on jokin seuraavista:

$$[2,1,3], [3,1,2], [3,2,1]$$

Jotta algoritmi toimii oikein, sen täytyy päästä vertailuja tekemällä tilanteeseen, jossa on vain yksi mahdollinen lista ja algoritmi voi järjestää sen oikealla tavalla. Listojen määrä alussa on $$n!$$ ja jokaisen vertailun jälkeen ainakin puolet listoista voi jäädä jäljelle. Niinpä tarvitaan ainakin $$\log_2(n!)$$ vertailua, ennen kuin algoritmi on saanut varmasti riittävästi tietoa listasta. Tästä saadaan alaraja

$$\log_2(n!) = \log_2(1)+\log_2(2)+\dots+\log_2(n) \ge (n/2) \log_2(n/2)$$

which means that the algorithm may need at least $$O(n \log n)$$ comparisons.

Notice that sorting can be done more efficiently if the type of elements is restricted. For example, if the elements are small integers, sorting can be done in $$O(n)$$ time. One such algorithm is _counting sort_.

eli algoritmin täytyy tehdä luokkaa $$n \log n$$ vertailua.

Huomaa, että järjestäminen voi olla mahdollista toteuttaa tehokkaammin, jos sen riittää toimia vain tietyn tyyppisillä alkioilla. Jos esimerkiksi alkiot ovat pieniä kokonaislukuja, järjestäminen on mahdollista toteuttaa ajassa $$O(n)$$. Yksi tällainen algoritmi on _laskemisjärjestäminen_ (_counting sort_).
