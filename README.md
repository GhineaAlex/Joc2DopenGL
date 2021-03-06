# Joc2DopenGL


Clasa Engine.h


Am pornit crearea proiectului cu o clasa de baza Engine care va reprezenta fereastra de baza 
si setarile pentru refresh si render.
Aceasta clasa are rolul de a seta timpul delta, este instantata o singura data, iar unul dintre 
campurile importante este delta care reprezinta un camp static privat care are rolul de a seta 
timpul de update, astfel incat cadrele sa fie randate uniform. De asemenea acest delta are 
utilitate in setarea gravitatiei si frecarii, adica o translatie de incetire a miscarii pe 
parcursul a cateva secunde pentru fiecare frame randat.
De asemenea, metoda InitEngine are rolul de a initializa GLFW si de a afisa erorile 
corespunzatoare in cazul in care exista la apelarea librariei respective sau la crearea ferestrei.
Am folosit doua buffere pentru a evita fenomenul de flickering care poate aparea cand 
sunt randate scenele. De asemenea in engine se regasesc setururile de tip callback 
pentru fiecare input. Jocul va avea posibilitatea de input atat de la mouse, cat si de la tastatura.
In clasa Engine mai gasim o variabila const denumita mode care are rolul de a prelua tipul 
ferestrei deshise. Vom folosi acest mode pentru a centra coordonatele ferestrei.
Ulterior setam tipul de matrice folosit, viewport-ul, coordonatele ortogonale. 
xista enable GL_Alpha_Test care are rolul de a seta transparenta in cazul texturilor 
background transparent incarcate deoarece nu dorim sa avem un poligon pentru fiecare 
element incarcat de o textura sau o textura cu un background transparent, dar care ar trebui umplut cu o culoare.



Clasa Texture


Ulterior am creat clasa Texture care are rolul de incarca texturile. Clasa Texture se va 
folosi de libraria SOIL care este o librarie OpenGL destul de simpla care are rolul de a
incarca texturile. Clasa Texture este destul de simpla, am realizat cateva mesaje de eroare 
in cazul in care este o problema cu id-ul imaginii incarcate sau cu locul in care este 
gasita imaginea pentru textura respectiva.
Am folosit pentru id o functie de la SOIL care are rolul de a prelua acea textura. 
Am setat o compresie 0 in momentul in care facem get-ul pentru textura respectiva 
deoarece nu avem in vedere o compresie cat mai buna la incarcarea texturilor, ci 
ne intereseaza mai mult calitatea imaginii. O problema pe care am avut-o aici a fost 
la SOIL_load_OGL_texture care reprezinta functia din SOIL care are rolul de a incarca 
textura, a trebuit sa folosesc operatorul pe biti |, altfel textura era inversata gresit pe axa Oy.


Clasa Sprite


Dupa crearea acestei clase am realizat clasa Sprite care este una dintre cele mai importante 
clase din acest engine deoarece ea practic plaseaza textura in sine ca un obiect in fereastra 
respectiva. Pe parcursul programului am realizat anumite schimbari la acestea, printre 
cele mai importante fiind folosirea unei clase aditionale Vector cu rolul de a usura o 
parte din operatiile realizate in clasa Sprite, dar voi explica pe parcurs care a fost motivatia.
Clasa Sprite realizeaza miscarea pe orizontala si pe verticala, realizeaza de asemenea 
rotatiile, dar si scalarea. In aceasta clasa operatiile de tip translate sunt cele mai vizbile. 
Clasa sprite are mai multi constructori cu diferiti parametri de intrare, adica fara parametri, 
cu un parametru – path care trimite catre textura si cu 2 parametri – path si 
vector3p v care are rolul de a face pozitionarea in functie de acel vector v 
introdus ca parametru. Vector3p face parte din clasa Vector amintita mai anterior 
si ajuta la usurarea procesului deoarece se poate face atribuirea directa pos = v; deoarece clasa Vector3p 
are constructorul de copiere supraincarcata.
Metoda render are rolul de a realiza translatiile in functie de input-ul 
respectiv dat de pos.x si pos.y fiind in plan 2D, iar ulterior sunt realizate rotatiile si scalarile.
GL_Quads va realiza practic o margine invizbila cu rolul de a 
pozitiona textura in planul ortogonal din acel joc.
Pe langa acestea, Sprite mai are metode pentru Translatie cu o 
anumita valoare, Translatie continua cu un v inmultit cu delta 
(o variabila constanta care are rolul explicat anterior de a uniformiza procesul de randare).
Am folosit de asemenea retur prin referinta pentru getRotatie, 
getScalare si getSize deoarece va fi nevoie de acele valori ulterior si rescrise pentru fiecare iteratie al unui cadru randat.


Clase Input – Mouse. h si Tastatura.h


Pe parcursul crearii clasei Sprite, am realizat si clasele pentru input, 
clasa Tastatura si clasa Mouse. Clasele in sine sunt asemanatoare avand aceeasi utilitate. 
In implementare am decis sa fac o diferenta intre o tasta apasata, o tasta care tocmai a 
fost eliberata si o tasta in mod normal (neapasata). Diferentele acestea sunt relevante 
deoarece o tasta abia eliberata va avea un anumit efect asupra fortele care actioneaza 
asupra acelui obiect, mai exact daca un obiect merge inainte cu tasta D, iar aceasta este 
eliberata, el se poate deplasa in continuare. Sau daca acesta merge in sus, iar tasta W este 
eliberata, gravitatia nu va actiona imediat, ci mai lent decat daca tasta W nu ar fi fost actionata.


Clasa Rigid.h


Ulterior am decis sa realizez si clasa Rigid care are rolul de a crea un 
corp rigid care va avea gravitatie si frecare, practic este obiectul din planul 
respectiv care se va misca. Acesta se foloseste de clasa Vector3p pentru a 
usura calculele respective fiind in principal motivul pentru care am decis sa 
modific ulterior si sa creez o clasa auxiliara Vector3p. Initial m-am gandit 
ca aceasta clasa Rigid sa mosteneasca clasa Sprite avand in principal aceeasi 
utilitate, dar am renuntat deoarece era mai greu sa redefinesc metodele 
respective si ar fi trebuit sa le creez din prima faza ca virtual. 
Astfel ca am implementat metoda de init care are ca variabile de intrare frecare, 
gravitatie, position in plana, rotatia obiectului, “scalarea” si respectiv dimensiunea obiectului.


Clasa auxiliara Vector3p


Clasa Vector3p s-a dovedit din nou a fi utila deoarece am folosit campuri de tip Vector 
in care nu a mai trebuit sa introduc acele coordonate. Clasa Vector3p este compusa din 3 campuri publice 
care au rolul de a determina coordonatele in spatiu. Avem constructori de initiliazare 
fara parametri de intrare, cu 1 parametru de intrare, respectiv cu 3, dar si un 
constructor de copiere cu un parametru de intrare de tip const pe care il vom folosi la supraincarcarea operatorilor.

De asemenea, am supraincarcata operatorii =, +, -, de doua ori * avand ca diferente 
parametrii de intrare, adica de tip Vector sau de tip float. De asemenea, avem supraincarcate 
2 boolene == si != . Implementarile acestora metode sunt destul de tipice pentru POO.

	Clasa Rigid - Continuare
	
Refresh si Render au aceleasi functii ca in cazul Sprite, dar pe axa O.y am adaugat gravitatia, 
respectiv frecarea pe O.x. De asemenea, aceste variabile au rolul de a simula aceste forte folosind variabila delta din Engine implementata ca o constanta, astfel incat randarea sa fie realizata corect.
Metoda render este asemenatoare cu cea din Sprite, dar difera un anumit lucru, am decis sa realizez un frame in jurul texturii respective, acel frame va reprezenta domeniile de coliziune cu un alt obiect din planul respectiv. 

Clasa Joc.h

In final mai avem clasa Joc care reprezinta o clasa care include date membre din clasa Rigid si 
Sprite si anume sprite si rb, dar cu metode de Refresh si Render reimplementate.
Aceasta clasa are rolul de a usura apelul unui obiect de tip Sprite care are gravitatie si frecare. 
Practic se poate face apelul in Main atat a unor de tip Sprite, cat si Rigid.
Mai jos este relatia dintre clase si librarii, pe langa acestea am mai folosit clasa vector si math.
