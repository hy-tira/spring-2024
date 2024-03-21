---
title: Linked list
permalink: /linked-list
hide: true
---

# Linked list

_Linked list_ is a data structure, where the elements of the list are stored so that each element contains a reference to the preceding and the succeeding elements. Unlike in a standard list, the elements do not need to be consecutive in memory.

As an example, let us look at the list $$[1,2,3]$$ stored as a standard list and as a linked list. A standard list looks like this:

_Linkitetty lista_ (_linked list_) on tietorakenne, jossa listan alkiot tallennetaan muistiin niin, että jokaisessa alkiossa on viittaus edelliseen tai seuraavaan alkioon. Toisin kuin tavallisessa listassa, alkioiden ei tarvitse olla peräkkäin muistissa.

Tarkastellaan esimerkkinä listan $$[1,2,3]$$ tallentamista tavallisena listana ja linkitettynä listana. Tavallisena listana tilanne voi näyttää tältä:

100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111
1 | 2 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0

Here the elements of the list are stored in memory starting from the location 100. The elements are in consecutive memory locations, and additions and removals are efficient only at the end of the list.

A linked list might look like this:

Tässä listan alkiot on tallennettu muistiin alkaen kohdasta 100. Alkiot ovat peräkkäin muistissa ja lisäykset ja poistot ovat tehokkaita vain listan lopussa.

Linkitettynä listana tilanne voi puolestaan näyttää seuraavalta:

100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111
1 | 0 | 106 | 3 | 106 | 0 | 2 | 100 | 103 | 0 | 0 | 0

Here the elements are in the memory locations 100, 106 and 103. Each element consists of three components: the value of the element, a reference to the preceding element and a reference to the succeeding element. For example, the components at the location 106 are 1, 100 and 103, because the value of the element is 2, the preceding element is at the location 100 and the following element is at the location 103. A reference value 0 means that the element has no preceding or succeeding element.

The advantage of a linked list is that elements can be added and removed easily, because they can be located anywhere in the memory. For example, if we want to add an element to the beginning of the above list, it can be inserted in the memory location 109:

Tässä listan alkiot ovat kohdissa 100, 106 ja 103. Jokaisessa kohdassa on kolme tietoa: alkion sisältö, viittaus edelliseen alkioon ja viittaus seuraavaan alkioon. Esimerkiksi kohdassa 106 on arvot 2, 100, 103, koska alkion sisältö on 2, edellinen alkio on kohdassa 100 ja seuraava alkio on kohdassa 103. Viittaus 0 tarkoittaa, että alkiolla ei ole edellistä tai seuraavaa alkiota.

Linkitetyn listan etuna on, että alkioita voi lisätä ja poistaa joustavasti, koska alkiot voivat olla missä tahansa kohdissa muistissa. Esimerkiksi jos yllä olevassa tilanteessa listan alkuun halutaan lisätä uusi alkio, se voidaan lisätä muistiin kohtaan 109:

100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111
1 | 109 | 106 | 3 | 106 | 0 | 2 | 100 | 103 | 4 | 0 | 100

This corresponds to the list $$[4,1,3,2]$$ beginning at the memory location 109.

The disadvantage of a linked list is that finding the memory location of an element can be slow. To find the location of an element, we have to follow the references starting from the beginning or the end of the list. Thus accessing an element in the middle of a list needs $$O(n)$$ time.

Tämä vastaa listaa $$[4,1,3,2]$$, jonka aloituskohta muistissa on 109.

Linkitetyn listan heikkoutena on, että ei ole tehokasta tapaa laskea, missä kohdassa muistia mikäkin alkio sijaitsee. Jos halutaan päästä käsiksi tietyssä kohdassa olevaan alkioon, täytyy seurata viittauksia listan alusta tai lopusta. Tämän vuoksi indeksointi vie aikaa $$O(n)$$, jos alkio sijaitsee listan keskellä.
