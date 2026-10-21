# OrnithoAI

**Tema: Primjena dubokog učenja za klasifikaciju vrsta ptica na osnovu audio snimaka**

## 1. Definicija problema

Cilj projekta je da se iz audio snimka automatski odredi **koje vrste ptica se u njemu čuju**.

- **Ulaz:** audio snimak podijeljen na segmente fiksne dužine (5 sekundi).
- **Izlaz:** za svaki segment, vjerovatnoća prisustva svake ciljne vrste. Isti segment može sadržavati više vrsta istovremeno, pa se radi o višeoznačnoj (multi-label) klasifikaciji.
- **Mjera uspjeha:** makro-usrednjeni ROC-AUC preko svih vrsta, što je i metrika korištena na BirdCLEF+ 2026 takmičenju [4]. Makro-usrednjavanje svakoj vrsti daje jednak značaj, pa model ne može biti dobar samo na čestim vrstama.

Zadatak je težak iz nekoliko razloga: terenski snimci sadrže šum (vjetar, kiša, saobraćaj), vrste se preklapaju u istom trenutku, za mnoge vrste postoji vrlo malo označenih primjera, a modeli trenirani u jednoj regiji slabije rade u drugoj (pomak domene) [4, 7].

## 2. Relevantnost

**Zaštita prirode.** Brojnost ptica opada: procjena za Sjevernu Ameriku pokazuje neto gubitak od oko 2,9 milijardi ptica (oko 29 % brojnosti iz 1970.) [1]. Da bi se takvi trendovi pratili, potrebne su metode koje rade na velikoj skali.

**Pasivni akustički monitoring.** Autonomni snimači mogu danima i mjesecima neprekidno bilježiti zvuk staništa, ali nastaje više podataka nego što ih ljudi mogu preslušati [3]. Automatsko prepoznavanje vrsta čini takve snimke upotrebljivim za praćenje biodiverziteta.

**Zrelost metoda.** Duboko učenje je danas standardni pristup u bioakustici [2]. Gotovi sistemi i modeli, poput BirdNET-a [5] i Perch 2.0 [8], dostupni su za ponovnu upotrebu, a transfer learning se pokazao posebno korisnim kad je označenih podataka malo [6, 7].

**Lokalna primjena.** Postojeći alati i skupovi podataka većinom dolaze iz drugih regija, a za Bosnu i Hercegovinu nema provjere koliko dobro rade. Ovaj projekt to testira na snimcima iz naše okoline.

## 3. Podaci i planirani pristup

**Dataset:** [BirdCLEF+ 2026 (Kaggle)](https://www.kaggle.com/c/birdclef-2026). Prema pregledu takmičenja [4], skup obuhvata brazilski Pantanal i 234 ciljne vrste, od kojih su 162 ptice (ostalo su vodozemci, insekti, sisari i jedan gmaz). Sadrži oko 35 hiljada kratkih snimaka pojedinačnih vrsta (iz Xeno-canto i iNaturalist baza [9]) i označene terenske snimke s pasivnih snimača. Za potrebe ovog projekta fokusiramo se na ptice.

**Pristup:** transfer learning s postojećih modela umjesto treniranja od nule. Razmatramo BirdNET [5], Perch 2.0 [8], PANNs [10] i Audio Spectrogram Transformer [11], uz fine-tuning na BirdCLEF+ podacima [6, 7].

**Provjera na pticama iz BiH:** snimit ćemo nekoliko ptičjih glasova iz svoje okoline i provjeriti tačnost i primjenjivost modela u lokalnim uslovima (snimci s telefona, šum okoline, vrste koje žive kod nas).

**Ograničenje koje treba imati na umu:** Pantanal je neotropsko stanište, pa se većina njegovih vrsta ne preklapa s vrstama u BiH. Model fine-tuniran samo na BirdCLEF+ 2026 neće znati imenovati naše vrste. Zato će snimci iz BiH služiti kao test robusnosti i primjenjivosti (pomak domene), a za prepoznavanje domaćih vrsta koristit ćemo i modele koji već pokrivaju evropske vrste, poput BirdNET-a i Perch-a.

## 4. Izvori i literatura

[1] Rosenberg, K. V., Dokter, A. M., Blancher, P. J., et al. (2019). *Decline of the North American avifauna.* **Science**, 366(6461), 120-124. https://doi.org/10.1126/science.aaw1313

[2] Stowell, D. (2022). *Computational bioacoustics with deep learning: a review and roadmap.* **PeerJ**, 10, e13152. https://doi.org/10.7717/peerj.13152

[3] Kahl, S., Denton, T., Klinck, H., et al. (2023). *Overview of BirdCLEF 2023: Automated bird species identification in Eastern Africa.* CEUR Workshop Proceedings. https://hal.inrae.fr/hal-04345437/document

[4] Kahl, S., Denton, T., Sugai, L. S. M., et al. (2026). *Overview of BirdCLEF+ 2026: Acoustic species identification in the Pantanal, South America.* CEUR Workshop Proceedings, Vol. 4283. https://ceur-ws.org/Vol-4283/paper317.pdf

[5] Kahl, S., Wood, C. M., Eibl, M., Klinck, H. (2021). *BirdNET: A deep learning solution for avian diversity monitoring.* **Ecological Informatics**, 61, 101236. https://doi.org/10.1016/j.ecoinf.2021.101236

[6] Ghani, B., Denton, T., Kahl, S., Klinck, H. (2023). *Global birdsong embeddings enable superior transfer learning for bioacoustic classification.* **Scientific Reports**, 13, 22876. https://doi.org/10.1038/s41598-023-49989-z

[7] Ghani, B., Kalkman, V., Planqué, B., Vellinga, W.-P., Gill, L., Stowell, D. (2025). *Impact of transfer learning methods and dataset characteristics on generalization in birdsong classification.* **Scientific Reports**, 15, 16273. https://doi.org/10.1038/s41598-025-00996-2

[8] van Merriënboer, B., Dumoulin, V., Hamer, J., Harrell, L., Burns, A., Denton, T. (2025). *Perch 2.0: The Bittern Lesson for Bioacoustics.* arXiv:2508.04665. https://arxiv.org/abs/2508.04665

[9] Vellinga, W.-P., Planqué, R. (2015). *The Xeno-canto collection and its relation to sound recognition and classification.* CEUR Workshop Proceedings, Vol. 1391. https://ceur-ws.org/Vol-1391/166-CR.pdf

[10] Kong, Q., Cao, Y., Iqbal, T., Wang, Y., Wang, W., Plumbley, M. D. (2020). *PANNs: Large-scale pretrained audio neural networks for audio pattern recognition.* **IEEE/ACM Transactions on Audio, Speech, and Language Processing**, 28, 2880-2894. https://doi.org/10.1109/TASLP.2020.3030497

[11] Gong, Y., Chung, Y.-A., Glass, J. (2021). *AST: Audio Spectrogram Transformer.* **Proc. Interspeech 2021**, 571-575. https://doi.org/10.21437/Interspeech.2021-698
