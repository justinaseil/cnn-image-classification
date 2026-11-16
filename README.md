# Vaizdų klasifikavimas naudojant konvoliucinius neuroninius tinklus (CNN)

## Aprašymas

Šiame projekte kuriami ir tiriami konvoliuciniai neuroniniai tinklai (CNN) skirti vaizdų klasifikavimui. Naudojant „Rock-Paper-Scissors“ duomenų rinkinį, modelis mokomas atpažinti tris rankų gestų klases: akmenį (*rock*), popierių (*paper*) ir žirkles (*scissors*).

Projekto metu kuriamos ir lyginamos skirtingos neuroninių tinklų architektūros, atliekami hiperparametrų eksperimentai bei vertinamas galutinio modelio klasifikavimo tikslumas.

## Projekto tikslas

Pagrindinis projekto tikslas – apmokyti konvoliucinius neuroninius tinklus vaizdų klasifikavimo užduočiai ir ištirti, kaip tinklo architektūra bei skirtingi hiperparametrai veikia klasifikavimo tikslumą ir nuostolio funkcijos reikšmes.

## Naudotos technologijos

- **Python** – programavimo kalba.
- **TensorFlow** – neuroninių tinklų kūrimas ir mokymas.
- **Keras** – konvoliucinių neuroninių tinklų architektūrų sudarymas.
- **NumPy** – duomenų apdorojimas ir skaičiavimai.
- **Matplotlib** – rezultatų vizualizavimas.
- **Scikit-learn** – duomenų padalijimas ir modelio vertinimas.

## Duomenų rinkinys

Naudojamas [Rock-Paper-Scissors duomenų rinkinys iš Kaggle](https://www.kaggle.com/datasets/drgfreeman/rockpaperscissors).

Duomenų rinkinį sudaro daugiau nei 2000 vaizdų, suskirstytų į tris klases:

- `rock` – akmuo;
- `paper` – popierius;
- `scissors` – žirklės.

Originalių vaizdų dydis – 200 × 300 pikselių. Prieš mokymą visi vaizdai pakeičiami į 128 × 128 pikselių dydį, o pikselių reikšmės normalizuojamos į intervalą nuo 0 iki 1.

Duomenys suskirstomi į tris aibes:

| Duomenų aibė | Dalis |
| ------------ | ----: |
| Mokymo       |  80 % |
| Validavimo   |  10 % |
| Testavimo    |  10 % |

Duomenys padalijami naudojant stratifikavimą, kad visose aibėse būtų išlaikytas proporcingas klasių pasiskirstymas.

Taip pat mokymo metu taikomas duomenų padidinimas (*Data Augmentation*), naudojant horizontalius apvertimus ir nedidelius vaizdų pasukimus.

## Projekto įgyvendinimas

### 1. Duomenų paruošimas

- Nuskaitomi vaizdai ir jų klasių žymos.
- Vaizdai pakeičiami į 128 × 128 pikselių dydį.
- Pikselių reikšmės normalizuojamos.
- Duomenys padalijami į mokymo, validavimo ir testavimo aibes.
- Mokymo duomenims taikomas duomenų padidinimas.

### 2. Modelio kūrimas

Kuriamos konvoliucinių neuroninių tinklų architektūros, kurias sudaro:

- Konvoliuciniai sluoksniai (`Conv2D`);
- Aktyvacijos funkcijos (`ReLU`, `Tanh`);
- Maksimalaus sujungimo sluoksniai (`MaxPooling2D`);
- Išmetimo sluoksniai (`Dropout`);
- Paketų normalizavimo sluoksniai (`BatchNormalization`);
- Pilnai sujungti sluoksniai (`Dense`);
- Galutinis klasifikavimo sluoksnis su `Softmax` aktyvacijos funkcija.

### 3. Eksperimentiniai tyrimai

Atliekami eksperimentai, siekiant nustatyti skirtingų parametrų įtaką modelio veikimui.

Tiriama:

**Neuroninių tinklų architektūra**

- Palyginamos trys skirtingos CNN architektūros.
- Vertinamas konvoliucinių blokų ir filtrų skaičiaus poveikis rezultatams.

**Išmetimo sluoksnio tikimybė (Dropout)**

- Lyginamos 0 ir 0,25 reikšmės.

**Paketų normalizavimas (Batch Normalization)**

- Vertinamas modelių veikimas su paketų normalizavimu ir be jo.

**Aktyvacijos funkcijos**

- Lyginamos `ReLU` ir `Tanh` aktyvacijos funkcijos.

**Optimizavimo algoritmai**

- Lyginami `Adam` ir `SGD` optimizavimo algoritmai.
- Išbandomi 0,001 ir 0,01 mokymosi greičiai.

Eksperimentų metu stebimas mokymo ir validavimo tikslumas bei nuostolio funkcijos reikšmės. Rezultatai pateikiami grafikuose.

### 4. Galutinio modelio testavimas

Remiantis atliktų eksperimentų rezultatais, parenkama antroji CNN architektūra ir sudaromas galutinis modelis.

Modelis testuojamas naudojant atskirą testavimo duomenų aibę. Jo veikimas vertinamas pagal:

- Testavimo tikslumą;
- Nuostolio funkcijos reikšmę;
- Klasifikavimo matricą (*Confusion Matrix*);
- Tikrųjų ir prognozuotų klasių palyginimą.

## Galutinio modelio parametrai

| Parametras                   | Reikšmė                  |
| ---------------------------- | ------------------------ |
| Vaizdo dydis                 | 128 × 128                |
| Paketų dydis (*Batch Size*)  | 32                       |
| Epochų skaičius              | 10                       |
| Aktyvacijos funkcija         | ReLU                     |
| Galutinio Dropout tikimybė   | 0,25                     |
| Batch Normalization          | Nenaudojamas             |
| Optimizavimo algoritmas      | Adam                     |
| Mokymosi greitis             | 0,001                    |
| Nuostolio funkcija           | Categorical Crossentropy |
| Išėjimo aktyvacijos funkcija | Softmax                  |

## Rezultatai

\<details> \<summary>\<strong>Galutinio modelio klasifikavimo rezultatai\</strong>\</summary>

Galutinis modelis pasiekė **93 % tikslumą testavimo duomenų aibėje**.

Klasifikavimo matrica:

| Tikroji / Prognozuota klasė | Rock | Paper | Scissors |
| --------------------------- | ---: | ----: | -------: |
| Rock                        |   58 |    11 |        3 |
| Paper                       |    1 |    71 |        0 |
| Scissors                    |    0 |     0 |       75 |

Geriausiai modelis klasifikavo „paper“ ir „scissors“ klases. Daugiausia klaidų pasitaikė klasifikuojant „rock“ ir „paper“ vaizdus.

\</details>

## Eksperimentų išvados

Atlikus eksperimentus nustatyta:

- Geriausius bendrus rezultatus iš trijų išbandytų architektūrų pasiekė antroji architektūra.
- `ReLU` aktyvacijos funkcija veikė efektyviau nei `Tanh`.
- 0,25 išmetimo tikimybė padėjo pasiekti geresnius validavimo rezultatus.
- Pasirinktoje galutinėje konfigūracijoje geresni rezultatai pasiekti nenaudojant paketų normalizavimo.
- `SGD` su 0,01 mokymosi greičiu pasiekė apie 90 % validavimo tikslumą.
- Galutiniam modeliui pasirinktas `Adam` optimizavimo algoritmas su 0,001 mokymosi greičiu.
- Galutinis modelis pasiekė 93 % testavimo tikslumą.

## Autorius

**Justina Seiliūnaitė**

Vilniaus universitetas
Matematikos ir informatikos fakultetas
Duomenų mokslo studijų programa
