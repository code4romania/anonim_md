# Anonim.md

[Anonim.md](https://anonim.md) este o instanță [GlobaLeaks](https://github.com/globaleaks/GlobaLeaks) care oferă o comunicare sigură și anonimă între denunțătorii și jurnaliștii din Republica Moldova.

Acest repository oferă instrucțiunile și fișierele de activare necesare pentru setarea unei pagini de pornire personalizate pentru o instanță GlobaLeaks.

### Setarea unei pagini de pornire personalizate în Globaleaks 3.x
Globaleaks versiunea 2.x oferea un mijloc pentru a seta o pagină de pornire personalizată (sau conținut personalizat) direct din panoul de administrare. Începând cu versiunea 3.x, această funcționalitate s-a modificat pentru a oferi mai multă flexibilitate; cele mai recente versiuni ale platformei permit personalizarea suplimentară prin CSS și scripturi JavaScript furnizate de utilizator, fișierele fiind încărcate automat în fiecare instanță GlobaLeaks.

În principal, există două abordări pentru adăugarea de conținut nou sau setarea propriei pagini de start în GlobaLeaks 3.x:

1. Modificați pagina implicită manipulând DOM-ul prin scriptul personalizat.
2. Construiți o pagină HTML separată, încărcați-o pe platformă, apoi utilizați redirecționări folosind scriptul personalizat pentru a o seta ca pagină de destinație. Aceasta este metoda folosită în prezent în Anonim. Pentru mai multe detalii, te uiți la codul comentat al [scriptului personalizat al lui Anonim](assets/custom.js).

În timp ce prima metodă permite integrarea propriului dvs. conținut cu formularele de trimitere GlobaLeaks în cadrul aceleiași pagini, este predispusă la întreruperi de funcționalitate dacă elementele/atributele sunt schimbate în versiunile viitoare ale Globaleaks, astfel că a doua abordare este întotdeauna preferată.

### Aspecte critice de securitate care trebuie avute în vedere
Întrucât anonimatul este prioritatea maximă pentru instanțele GlobaLeaks, fiecare activ folosit de o implementare **trebuie** să fie difuzat din sistemul gazdă al instanței. Aceasta înseamnă că în nici o condiție resursele solicitate automat de browser (cum ar fi fișiere CSS/JS, imagini, fonturi și așa mai departe) nu trebuie servite de la alte domenii/adresă IP/CDN.

Modul corect de a face este să descărcați resursa local, să o încărcați pe instanță, apoi să o legați în paginile dvs. folosind un href relativ (de ex. `/s/header_image.png`).

### Pași pentru a reproduce implementarea anonim.md
Într-o distribuție Ubuntu nouă (versiunea 18.04 preferată) faceți următoarele:


1. Instalați o nouă instanță de Globaleaks folosind [documentația oficială](https://docs.globaleaks.org/en/latest/setup/InstallationGuide.html); ceea ce constă în principal în emiterea următoarelor comenzi cu privilegii „root”:

```
root # wget https://deb.globaleaks.org/install-globaleaks.sh
root # chmod +x install-globaleaks.sh
root # ./install-globaleaks.sh
```

2. Configurați platforma și utilizatorul administrator accesând adresa `http://[ip_address]:8082/` urmați pașii furnizați (așa-numitul [wizard](https://docs.globaleaks.org/en/latest/setup/PlatformWizard.html));

3. Conectați-vă la panoul de administrare (la adresa `http://[ip_address]:80/admin`) și încărcați următoarele [assets](assets/), după cum urmează:
    - fișiere sub [assets/files](assets/files) la `Site settings > Files`
    - `custom.js`,` favicon.ico` la `Site settings > Theme customization`
    - `logo.png` la `Site settings > Main configuration > Logo`

4. Întrucât pagina de pornire (situată la `/s/index.html`) este setată folosind o redirecționare, atunci când accesați platforma, pagina de destinație GlobaLeaks implicită va fi afișată pentru o secundă. Pentru a ascunde această inconsistență vizuală, este necesar să editați direct fișierul `/usr/share/globaleaks/client/index.html` de la linia de comandă și să adăugați o etichetă `hidden` la elementul `body`, astfel:
```
root# cd /usr/share/globaleaks/client/
root# vim index.html
# în interiorul editorului schimba <body> în <body hidden>
(salvează și ieși)
# gzip fișierul index pentru a aplica modificări
root# gzip -k -f index.html
```
Al patrulea pas trebuie aplicat după fiecare actualizare a platformei folosind comanda `apt` din linia de comandă.