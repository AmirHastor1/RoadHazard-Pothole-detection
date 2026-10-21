# Pothole Detection in Kanton Sarajevo

**Primjena dubokog učenja za automatsko otkrivanje rupa na kolovozu**

Tim: [naziv tima]

## 1. Definicija problema

Cilj projekta je da se iz fotografije ili video snimka kolovoza automatski **otkriju i lociraju rupe (potholes)** na cestama u Kantonu Sarajevo.

- **Ulaz:** slika (ili pojedinačni kadar videa) kolovoza snimljena iz vozila, npr. kamerom telefona postavljenog na vjetrobran.
- **Izlaz:** za svaku sliku, okvir (bounding box) oko svake otkrivene rupe i procjena pouzdanosti. Radi se dakle o detekciji objekata, a ne samo o odgovoru "ima / nema rupe".
- **Mjera uspjeha:** preciznost (precision), odziv (recall), F1 i mAP, standardne mjere za detekciju objekata. Odziv je posebno važan, jer propuštena rupa je veći problem za sigurnost od lažnog alarma.

Zadatak je težak iz nekoliko razloga. Rupe su često male i daleko od kamere, mijenjaju se svjetlo, sjene i vlažnost kolovoza, a slično izgledaju i zakrpe, šahtovi, pukotine i mrlje. Već je u ranom radu na snimcima s telefona odziv za klasu koja uključuje rupe bio nizak u odnosu na druga oštećenja [1]. Dodatno, modeli trenirani na snimcima iz drugih zemalja ne moraju dobro raditi na našim cestama, što je pitanje koje ovaj projekt provjerava.

## 2. Relevantnost

**Sigurnost i trošak.** Rupe oštećuju vozila i ugrožavaju sigurnost saobraćaja. Za ilustraciju razmjera, američko udruženje AAA procijenilo je da oštećenja od rupa vozače u SAD-u koštaju oko 3 milijarde dolara godišnje, uz prosječno oko 300 dolara po popravci [2]. To su američki podaci i ovdje služe samo kao pokazatelj reda veličine problema.

**Ručni pregled je spor i skup.** Redovno obilaženje cesta zahtijeva ljude i vrijeme. Rad na snimcima s telefona pokazao je da se oštećenja mogu otkrivati jeftinom opremom postavljenom na vozilo [1], a skup podataka RDD2022 je namijenjen upravo jeftinom nadzoru stanja cesta [3]. Pregledni rad o računarskom vidu za otkrivanje rupa navodi da su konvolucijske neuronske mreže danas glavni pristup ovom zadatku [4].

**Lokalni kontekst.** Zakon o uređenju saobraćaja na području Kantona Sarajevo (2017) obavezuje upravljače cesta da kontinuirano prate stanje javnih cesta i vode ažurnu bazu podataka o stanju, te da odmah postupe kad oštećenje kolovoza ozbiljno ugrožava sigurnost saobraćaja [5]. Skupština Kantona je 2023. zaključcima od Vlade zatražila i hitno postupanje po studiji opasnih mjesta i dionica na primarnoj mreži te izradu strategije sigurnosti saobraćaja [6]. Automatsko otkrivanje rupa može pomoći da se stanje cesta prati sistematski i s manje truda.

**Praznina u podacima.** Javni skup RDD2022 sadrži snimke iz Japana, Indije, Češke, Norveške, SAD-a i Kine [3], dakle ne i iz Bosne i Hercegovine. U izvorima koje smo pregledali nismo pronašli javni skup snimaka rupa s cesta u Kantonu Sarajevo, pa je lokalna provjera modela sama po sebi doprinos projekta.

## 3. Podaci i planirani pristup

**Podaci.** Javni skup RDD2022 (47.420 slika, preko 55.000 označenih oštećenja, među klasama su i rupe) [3] za pretreniranje i poređenje, te vlastiti snimci cesta u Kantonu Sarajevo, snimljeni telefonom iz vozila kao u radu [1]. Vlastite snimke ćemo ručno označiti i pri objavi zamutiti lica i registarske oznake.

**Pristup.** Detekcija objekata modelima iz porodice YOLO [7] uz transfer learning: polazimo od već pretreniranih težina, fine-tuniramo ih na RDD2022 podacima, a zatim na lokalnim snimcima.

**Provjera na lokalnim cestama.** Model testiramo na našim snimcima da procijenimo koliko su tačnost i primjenjivost očuvani kad se snimci razlikuju od trening podataka (kamera, svjetlo, vrsta kolovoza, stanje cesta).

## 4. Izvori i literatura

[1] Maeda, H., Sekimoto, Y., Seto, T., Kashiyama, T., Omata, H. (2018). *Road damage detection using deep neural networks with images captured through a smartphone.* arXiv:1801.09454. https://arxiv.org/abs/1801.09454

[2] AAA (2016). *Pothole damage costs U.S. drivers $3 billion annually.* AAA Oregon/Idaho, 16. februar 2016. https://info.oregon.aaa.com/pothole-damage-costs-u-s-drivers-3-billion-annually

[3] Arya, D., Maeda, H., Ghosh, S. K., Toshniwal, D., Sekimoto, Y. (2022). *RDD2022: A multi-national image dataset for automatic road damage detection.* arXiv:2209.08538. https://arxiv.org/abs/2209.08538

[4] Ma, N., Fan, J., Wang, W., Wu, J., Jiang, Y., Xie, L., Fan, R. (2022). *Computer vision for road imaging and pothole detection: a state-of-the-art review of systems and algorithms.* **Transportation Safety and Environment**. arXiv:2204.13590. https://arxiv.org/abs/2204.13590

[5] Skupština Kantona Sarajevo (2017). *Zakon o uređenju saobraćaja na području Kantona Sarajevo.* Usvojen 5. jula 2017. https://www.centar.ba/upload/modules/documents/2023/bfcf9b49c1044ad0c442a4dc1ddc3099.pdf

[6] Skupština Kantona Sarajevo (2023). *Zaključci Skupštine Kantona Sarajevo s Prve hitne sjednice, 9. juni 2023.*, broj 01-04-27085/23. https://ks105.ks.gov.ba/sites/default/files/2024-08/zakljucci_sigurnost_saobracaja.pdf

[7] Redmon, J., Divvala, S., Girshick, R., Farhadi, A. (2016). *You Only Look Once: Unified, real-time object detection.* **Proc. IEEE CVPR 2016**, 779-788. https://doi.org/10.1109/CVPR.2016.91
