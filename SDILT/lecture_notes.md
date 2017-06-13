# Software Development in Large Teams

> [Lecture notes](https://learnit.itu.dk/pluginfile.php/191144/mod_resource/content/16/lecture%20notes.pdf)

These are notes from a video conference we had. They are written in dangerous. Some of this might be plain wrong, and I might have misinterpreted other's comments. So take this document as pure noise.

## Basic Ray Tracing

### Orthonormal Space

Vigtigt
Vi bruger det når vi sætter kameraet op. Hvordan vores kamera vender

Ligesom texture coordinates med u og v mellem 0 og 1.

For hver eneste pixel fyrer vi en ray igennem et view plane (f.eks. [0.5, 0.5]).
Så translater vi fra et 2D-space til et 3D-space.

## Implicit Surfaces

Du har en ligning, f.eks. x+y+2+1=0

3 skridt:

1. Tæl rødder mellem a og b.
	- Gøres med Sturm Sequences.
2. Find interval hvor der er 1 rod OG det er den mindste positive (fordi vi vil have den, der er tættest på os).
3. Inden for det interval, approximer.

Så vi har nu et interval hvor vi ved at der ligger den mindste positive rod et eller andet sted. Så vi bruger dette interval til at prøve at finde (approximere) den mindste rod.

### Sturm Sequences

Tager en funktion, f(x).
p0 = f(x)
p1 f'(x)
pn = (pn-2 mod Pn-1)

Til sidst har vi en konkret værdi.

Vi sætter:
a=0
b=100

Så beregner vi nogle bud:
S1 = P0(a) ... P1(a)
Så får vi måske:
S1 = [2, -3, 4, 5, -1, -10]

Så kigger vi efter negative værdier i den. Her er der f.eks. 3. ("Sign changes")

Så udregner vi S2, f.eks. [2, -3, -4, 6, 8, 10]

Her er der 2 sign changes (-3 og -4)

Så beregner vi s1 - s2 som betyder rødder mellem a og b.

Vi formindsker og formindsker rekursivt indtil vi havner på én konkret værdi.

Her er vores fejl:

I stedet for at checke om det venstre interval er større end det højre, skulle vi have checket om det venstre interval er større end 0. Hvis det er større end 0, brug venstre interval, ellers brug højre.

### Newton Raphson

Får intervallet, f.eks. [0, 100]. Der er en rod imellem dem. Newton Raphson skal gætte hvor den er.

Den får også et gæt. Det er typisk bare midten.

Den tager funktionsværdien og bestemmer tangens hældning rekursivt, men maks 20 gange. Hvis vi ender på over 20, så returnerer vi hvor langt vi er nået.

Hvis vi bevæger os meget lidt per iteration terminerer vi og siger at vi er tæt "nok" på.

#### Problemer med Newton Raphson

Den er ikke designet til at finde den nærmeste rod, den er designet til at finde *en eller anden* rod.

**Jo mere vi kan snævnre roden ind før vi fodrer Newton Raphson med et gæt, jo mere sikker kan vi være på at Newton Raphson ikke fucker op**.

Det med at den giver op efter 20 iterationer kan også være farligt. Så "tror" vi det er gået godt, selvom det egentlig ikke er tilfældet.

TODO: Læs op til Newton Raphson problemer.
TODO: Se videoer på nettet.

## Texturing

## Hvad

Texture er en funktion der tager to texture koordinater, typisk kaldet u og w og som returnerer et Material.

## Hvorfor

Det handler om at ændre 3D koordinater til 2D koordinater.

Koordinaterne forventes til at være normaliseret til intervallet [0, 1] for alle shapes undtagen Infinite Shape.
Normaliseret betyder at lower left hjørnet has koordinaterne u = 0, v = 0 og upper right corner har koordinaterne u = 1, v = 1.

Hvordan vi beregner disse koordinater er forskelligt fra shape til shape. Vi er jo blot interesserede i at vide hvilken farve der skal tegnes for hvert hitpoint.

(u,v) = (u(w −1),v(h −1))

TODO: Læs op på hvordan texture koordinater udregnes for alle shapes.

Farven bestemmes ud fra teksturens pixel farve + shading, altså shadow og reflections.

## Krav

Det vi skal vide om ALT:

1. HVAD er det/hvad er pitfalls
2. HVORDAN bruger vi det.
3. HVORFOR bruger vi det.

## KD-tree

KD-træet er en shape ligesom alt andet.

### Hvad er det

En spacial partitioning algorithm.
Det er en rekursiv måde at opdele bounding boxes i mindre dele.

En måde at inddele volumen i mindre dele.

Vi bruger det for at kunne lave pre-processing i stedet for hver gang en ray rammer.

### Hvad hvis IKKE vi gjorde det

Så skulle vi for hvert hitpoint checke *alle* meshes for hits i hele scenen. Det ville tage uendelig lang tid.

### Hvad tager KD-træet

Det tager en liste af bounding boxes.
Så bestemmer vi en stor bounding box rundt om alle bounding boxesne som nu er KD-træets bounding box.

Så skal vi i gang med at splitte det op i mindre dele.

Vi kunne bare halvere hver gang skiftende mellem x og y.
Vi bruger nogle heuristikker i stedet:

### Hvordan

Når vi fyrer en ray ind i den, checker vi for hits på bounding boxes på alle de bounding boxes den rammer i den rækkefølge, rayen rammer dem.

Et KD-træ for en kanin kan ligge inde i en bounding box for en anden kanin.

Vi har ét KD-træ for scene og ét KD-træ for mesh.

Havde vi 100 kaniner, havde vi et KD-træ for scene der indeholder 100 KD-træer, et for hver kanin.

Du bygger et nyt KD-træ når du parser en PLY-fil.

## Heuristikker

Der er 5. Disse 5 i prioriteret rækkefølge bestemmer *hvor* vi splitter.

### Empty space

**Denne er den vigtigste heuristik**.

Det er et conditional. Det er et *if*-statement. Alle andre heuristikker køres sideløbende med hinanden og det bedste bud vælges.

Først checker vi: Hvor meget empty space er der på hver side af items, og hvor meget udgør det på den akse?

Så bruger vi det til at bestemme vores split punkter.

**Det vigtige er**, når vi bygger det, så vi vil vi gerne have at den bounding box der ligger rundt om vores items er så **fitted** som muligt. Vi vil minimere empty space mest muligt.

Så Empty space handler om at lave den bounding box, der indeholder mindst mulig whitespace.

I første opdeling vil vi aldrig kunne have empty-space, men det vil kunne opstå under yderligere opdelinger.

#### TL;DR

Man får en masse tomme bounding boxes, men har stor impact da vi checker langt mindre får hits.

## Longest dimension

Start med den bedste (længste) akse først og gå videre derfra.

## Equal Distribution

Handler om at vi gerne vil have at der er nogenlunde lige mange elementer på hver side.

Find den værdi mellem min og max der ligger tættest på gennemsnittet.

Vi vil gerne så vidt som muligt undgå at dele op på begge sider.

FORDI: Vi gerne vil have et **balanceret KD-træ**.

## Overlap

Overlap må ikke være over 50%.
Altså, mere end 50% af shapes må ikke ligge i begge subtræer.

Hvis overlap > 60% så siger vi "okay, x-aksen gik ikke, vi prøver på y-aksen i stedet".

### Overordnet koncept

Vi har vores KD-træ.
Så fyres der en ray ind i træet.
Der ligger jo en bounding box rundt om træet, så vi vil have en *t* værdi for enter og en *t* værdi for exit på den yderste bounding box.

Så finder vi ud af: Hvad er *t<sup>hit></sup>* når vi rammer splitpunktet. Vi ved den ligger mellem *t-enter* og *t-exit*.

Hver akse har en *t<sup>hit></sup>*. Måden vi finder den: Hver akse har en *Split* værdi, et konkret koordinat.

Split = OriginY + hit * direction</sub>y</sub>
tHit = (OriginY - Split) / directionY

### Pitfalls

Vi skulle have taget tEnter og tExit. Vi troede at bounding boxen var større end den i virkeligheden var.

Overlap er også et problem. Hvis ikke overlap breaker, så vil vi blive ved med at lave nye nodes. Så der kan opstå inifinite recursion.

## Affine Transformations

Baseret på Matrix-udregninger.

F.eks.

[1 0 0 0]
[0 1 0 0]
[0 0 1 0]
[0 0 0 1]

Hvis du havde et 0 i nederste højre hjørne ville du ikke kunne forskyde punkter.

Vi arbejder med 4x4 matricer fordi vi arbejder i 3 dimensioner.
Den højre række bruges til at *store information*.

Den nederste række bliver ikke anvendt.
Indeholder 0'er og 1'er. Hvis det er et 1, så er det et Point. Hvis det er et 0, så er det en Vektor.

Vi har 3 operationer vi ønsker at udføre:

- Translate
- Scale
- Rotate

Så har vi en Skew som er en blanding af de 3 ovenstående.

En transpose operation tager den højre kolonne og "lader den falde ned til venstre".

### Inverse

Hvis man laver en *Inverse* kan man gå tilbage igen, non-destructive.

### Det vi skal forstå

Det er hele ray'en vi transformer. Det er ikke kun vektoren vi transformer - det er hele ray'en inkl. ray origin vi transformer. Det vi hiver ud af det er afstand + material og flytter ind i den oprindelige ray

## Parallelization

## CSG

Vi har:

- Union
- Subtract
- Intersect

Fælles for dem er, at de alle tager 2 shapes: s1, s2 og returnerer en ny shape:
`(s1, s2) => s`.

Man får en træstruktur.

### Union

Alle hit-functions på en Union-shape redirecterer bare hit til dens 2 shapes:
`Hit -> Hit s1 && Hit s2`.

### Mesh

Mesh er en samling af trekanter som deler edges og vertices.
Disse defineres af PLY-filer. De indeholder informationer om hvad det er vi er ved at parse. Hvilke properties der er, hvis der er u og v værdier, så beskrives de her.

Det vigtigste der sker i headeren er at finde ud af hvor mange vertices der er samt formatet.

Formatet kan enten være binary eller ASCI.

Vi har en type der hedder PLYData. Som har w lister: Faces og Vertices.
Hvert element i Faces, som har a,b og c værdier peger på et index i vertices, som bl.a. har u og v.

Hvis noget data ikke er givet, så lapper vi den. F.eks. normalvektor.

Vi gemmer det hele i en kompakt repræsentation.

2 typer shading:

#### Flat shading

Uanset hvor vi rammer på trekanten vil vi returnere den samme normalvektor.

#### Smooth shading

Vi laver et gennemsnit af de omkringliggende normalvektorer og returnerer den.

#### Største pitfall

Der er en bestemt datatype der hedder vertex indicies. Det er en array.

## Bounding box

En Bounding Box er en rektangulær boks rundt om en shape som repreæsenterer dens borders.

Her er min svarende til venstre nederste hjørne og max svarer til øverste højre hjørne.

### Udregning af en Bounding Box

Tag alle mulige værdier - lavest mulige x og y, højest mulige x og y og generer en Bounding Box.

For nogle shapes er det svært, f.eks. Shape. Der er -radius tilsvarende nederste venstre hjørne og +radius tilsvarende øverste højre hjørne.

Det du vil vide er - er bounding boxen ramt? Ja/Nej.
Hvis ja, kan du gå ind i den og lave det, du nu engang skal lave inde i den

For at finde tEnter og TExit på en bounding box udregner du tEnterX og tEnterY og tExitX og tExitY, altså t for hver akse i hver ende.

Der hvor vi kommer ind i boksen vil altid være dér hvor tEnter[x|y] er højest, og der hvor vi kommer ud af boksen vil altid være dér hvor tExit[x|y] er lavest.

Så - hvis tEnter < tExit har vi ramt boksen.

### Udregning a Bounding Box fra meshes

Alle vertices for PLY-fil. Tag højeste x,y og laveste x,y koordinat fra vertices. Så har vi bounding boxen.

### Transformation af Bounding Box

Du transformer hver enkelt vertex på kaninen og så udregner du bounding boxen igen.