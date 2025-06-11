# Datové struktury na externích paměťových médiích

## Úvod

Externí paměťová média (např. pevné disky) pracují pomaleji než vnitřní operační paměť, ale poskytují větší kapacitu. Proto se zde data organizují **blokově** – čte se celý blok naráz, ne po jednotlivých záznamech.

### Základní pojmy:

* **Blok**: jednotka přenosu mezi diskem a pamětí (např. 512 bajtů, 4 KB…).
* **Buffer**: oblast v paměti, kam se načítají/ukládají bloky.
* **Seek**: operace, kdy se hlavička disku fyzicky přesune – časově náročné.

---

## Typy souborů

### 1. **Sekvenční soubor**

* Záznamy jsou **uloženy za sebou** podle klíče.
* Přístup pouze **sekvenční** (čtení jeden po druhém).
* Efektivní pro **průchod všemi daty**, např. výpis všech záznamů.

**Operace:**

* `Najdi`: čti bloky postupně, dokud nenajdeš klíč nebo nedojdeš na konec.
* `Vlož`: často nutná **rekonstrukce** celého souboru.
* `Odeber`: záznam se **označí jako neplatný** (např. příznakem).

---

### 2. **Soubor s přímým přístupem**

* Každý záznam má pevné **pořadí a místo v souboru**.
* Umožňuje **přímý skok** na požadovaný záznam (např. podle indexu).

#### Fixované vs. nefixované záznamy:

* **Fixované**: všechny záznamy mají stejnou velikost → snadno spočítáme, kam skočit.
* **Nefixované**: proměnlivá délka záznamů → potřebujeme **index nebo značky konce záznamu**.

---

### 3. **Hashovací soubor**

Umožňuje **rychlé vyhledávání** bez třídění záznamů.

### Princip:

1. Z klíče spočítáme **adresu bloku** pomocí hashovací funkce:
   `adr = hash(klíč)`
2. Záznam uložíme na danou adresu.

### Problém: **kolize**

* Dva různé klíče mohou mít stejný výsledek hashovací funkce.

### Metody řešení kolizí:

1. **Přetečení do další oblasti** – záznam se uloží do jiného volného místa.
2. **Otevřená adresa** – zkouší se další volná místa podle určitého pravidla.
3. **Spojování v seznamech** – v jednom místě je více záznamů jako seznam.

### Použití:

* Vhodné pro **statické soubory**, kde není potřeba řazení ani procházení.

---

### 4. **Utříděný (setříděný) soubor**

Záznamy jsou uspořádány **vzestupně** podle klíče.

**Vyhledávání:**

* Efektivnější než sekvenční – lze použít:

  * **Binární vyhledávání** – dělíme prostor na poloviny.
  * **Interpolační vyhledávání** – odhaduje pozici klíče na základě jeho hodnoty.

**Vkládání/Odebírání:**

* Komplikovanější než u hashování – musí se **zachovat pořadí**.
* Může vést k **přetečení bloku** – potřeba přesunů nebo připojení pomocných bloků.

---

### 5. **Soubory s přístupovým indexem**

Skládají se ze dvou částí:

* **Bázový soubor** – obsahuje vlastní data.
* **Indexový soubor** – slouží jako mapa, která říká, kde hledat.

### Varianty:

#### a) **Index-sekvenční soubor**

* Index je **utříděný**, každý záznam indexu odkazuje na blok s daty.
* Datový soubor lze procházet sekvenčně, nebo přeskakovat pomocí indexu.

#### b) **B⁺-strom**

* Vnitřní uzly obsahují **klíče + odkazy na podstromy**.
* **Listy obsahují záznamy** a jsou **lineárně propojené**.
* Efektivní jak pro **vyhledávání bodové**, tak pro **vyhledávání rozsahové**.

---

## Záznamy proměnlivé délky

U záznamů, které nemají stejnou délku, lze použít tyto přístupy:

1. **Indikátor délky na začátku záznamu**
2. **Značka konce záznamu**
3. **Indexový soubor** s adresami začátků záznamů

---

## Shrnutí pro zkoušku

🧠 **Principy:**

* Data na disku se zpracovávají **po blocích**.
* Efektivita = **co nejméně čtení bloků** (proto indexy, hash, řazení).

📘 **Typy organizace:**

| Typ souboru   | Hlavní výhoda              | Přístup | Vyhledávání | Použití                          |
| ------------- | -------------------------- | ------- | ----------- | -------------------------------- |
| Sekvenční     | Jednoduchá struktura       | Sekv.   | Pomalé      | Zápisy za sebou                  |
| Přímý přístup | Rychlé podle pozice        | Přímý   | Rychlé      | Záznamy na pevných pozicích      |
| Hashovací     | Nejrychlejší pro hledání   | Přímý   | O(1)        | Statické tabulky, velké množství |
| Utříděný      | Podpora rozsahových dotazů | Sekv.   | O(log n)    | Hledání rozsahu, řazené výpisy   |
| Indexovaný    | Kombinuje výhody           | Přímý   | O(log n)    | Databáze, velké datové systémy   |

✏️ **Co umět:**

* Popsat jednotlivé typy souborů.
* Umět rozdíl mezi sekvenčním a indexovaným souborem.
* Vysvětlit **hashování a kolize**.
* Příklad jednoduché organizace souboru (např. pomocí B⁺ stromu).

