# 11/04/2024
Dobrý den, komentuji v textu..

Díky a zdravím!  
Pavel Hazdra  

> Dobrý den,  
>   
> rád bych se Vám nejprve omluvil za sníženou aktivitu ze své strany. Taiwanem se teď v týdnu prohnal ukrutný tajfun a do toho si za mnou udělali dovolenou rodiče, tak občas nevím, kam dříve skočit. Samozřejmě se do toho ale snažím občas pokračovat v práci, a rozhodl jsem se Vám proto poslat alespoň nějaké mezivýsledky.

Jojo, rozumím, informaci o tajfunu jsem zaznamenal a vzpomněl jsem na Vás. Dobře už bylo, jak říkal náš dřívější šef.. Takže vše OK??

> S dříve diskutovaným napojením na polarizér jsem se již lépe seznámil a nyní se pokouším o nějaké ladění toho přechodu. Zprvu jsem strukturu ladil naivně rovnou celou, ale brzy jsem došel k závěru, že duální napájení jako celek obsahuje příliš mnoho optimalizačních proměnných pro efektivní ladění, a tak jsem se pustil do postupného ladění po krocích počínaje jednoduchým samostatným přechodem. V archivu outputs.zip tak naleznete:
>   
>   1. Prvotní ladění samostatného napájení pro jeden vlnovodný mód, kde jsem se pokusil vyladit geometrii přechodu koax-čtverec, tj. délku sondy a vzdálenost od zkracující zdi. Domnívám se totiž, že tyto parametry budou hodnotami pro oba přechody velmi podobné.

- [x] ANO  

>   2. Ladění vzdálenosti polarizační mřížky od prvního přechodu. Opět se domnívám, že tento parametr bych mohl být schopen takto do určité míry eliminovat z finální optimalizace, neboť by měl mít vliv ideálně pouze na vlnu vedenou z prvního portu.  
>  
> Ačkoli jsem začal takto "z gruntu" od jednoduché struktury, nedaří se mi po přidání mřížky dosáhnout akceptovatelných hodnot odrazu, neboť přidáním mřížky vyvstává v pásmu resonanční špička.  
>   
> Při návrhu postupuji hlavně podle teoretických vodítek popsaných v článku o duálním napájením sondy, který jste mi před určitou dobou doporučil. V případě návrhu jednoho přechodu se teoretické vztahy pro hodnoty parametrů jako zlomků vlnové délky shodují s mými závěry, ale doporučená vzdálenost mřížky od prvního portu se mi nedaří vyladit. Autoři jí uvádí jako $3\lambda/4$, přičemž předpokládám, že se jedná o vlnovou délku ve vlnovodu, ale zatím jsem se se zdarem nesetkal.

- [ ] Tady je obecný problém, že autoři článku můžou jednoduše kecat.. Nebylo by to poprvé. Struktura je evidentně rezonanční - dal jsem si monitory E na pozici peaků, kdy je přizpůsobení špatné a krásně je vidět, jak dutina (zadní zkrat - mřížka rezonuje). Toho se asi principiálně nejde zbavit. Bude to tedy asi úzkopásmové a možná je čas nasadit optimalizátor. Parametry budou:  
	- délka napájecího pino  
	- jeho vzdálenost od zadního zkratu  
	- pozice mřížky od zadního zkratu  
	- počet drátů v mřížce  
	- jejich průměr  

> Ještě se během zítřka pokusím doplnit svoje návrhové poznámky do  
> sdíleného wordu, protože těch s nabitou zkušeností ohledně  
> přechodů znatelně přibylo :)
>   
> Mimo to se dále budu pokoušet strukturu ladit. Například mi až teď  
> došlo, že jsem to měl asi celou dobu ladit s tím vlnovodným portem na  
> výstupu :)

OK :)  
  
ono to takhle nějak funguje, ale jistě je prostor na zlepšení  

> Zatím zdravím a přeji vše dobré.  
> Martin Šimák   

Ještě jednou děkuji moc a opatrujte se!  
  
PH

# 10/21/2024
Dobrý den!  
  
Komentuji v textu....  

> Dobrý den,  
>   
> nastudoval jsem si něco málo k CST Design Studio, což je, pokud dobře chápu, v nových verzích zkrátka Schematic v modulu Circuits & Systems, a rád bych se hned zkraje doptal.  
>   
> Vy jste v předchozí korespondenci uvedl, že by se hodilo vyzkoušet ruzná řešení výstupního vlnovodu z hlediska boundaries, a že bychom to mohli porovnávat již napojené feed + polarizér. Chápu to tedy dobře, že onou boundary je myšleno řešení až na úplně výstupním, tj. vyzařujícím, vlnovodu za polarizérem? Pro možnost napojení feedu na polarizér totiž potřebuji feed zakončit aktivním portem, aby importovaný blok měl i výstup, tj. druhý port pro výpočet přenosu.

- [ ] Aha, tohle jsem myslel z hlediska samotné optimalizace přechodu na odrazy a přeslech, čili na přechodu. Z důvodu, aby se hned zkraje nemusela simulovat celá struktura včetně vyzařovací části. Asi největši smysl dává zakončit přechod vlnovodným portem, protože to je vlastně ekvivalentní, jako kdyby za ním byl připojen polarizér. Současně se to bude hodit pro Design Studio.  

> Po náhledu do Design Studia jsem také stanul před rozhodnutím, v jaké podobě budu jednotlivé projekty importovat a následně za sebe napojovat. Máme totiž možnosti hned 3, přičemž první dvě budou, tuším, funkcionálně identické:
>   (a) Touchstone blok S-parametrů staticky exportovaných z projektu,
>   (b) CST Studio Suite file block (v sekci Field Simulators), který vezme statickou kopii (snapshot) projektu v moment importu a provádí simulace v Design Studiu skrz tyto spočítané výsledky,
>   (c) CST Studio Suite block (opět v sekci Field Simulators), který vytvoří blok jako dynamický link na importovaný projektový soubor bez toho, aby si vytvářel svoji statickou kopii.

- [x] Tak v tomto jste mě předběhl :) Jak tak o tom přemýšlím, všechny postupy by měly dávat stejný výsledek. (c) si myslím bude identické s (a), (b) s tím, že pokud se hne s parametry imprortovaného projektu, přepočte se 3D simulátorem.  

> Mně osobně se mi nejvíce zamlouvá varianta (c), neboť umožňuje dynamickou práci s oběma projekty v tom smyslu, že jakékoli strukturální, materiální či jiné změny v importovaném projektu se projeví v Design Studiu rovnou, neboť tam se pracuje pouze s referencí na projekt. Také zachovává parametrizaci, tj. parametry v interních projektů jsou prolinkovány do Design Studia a lze ladit přimo z pohledu schématu. Je otázkou, jestli tato varianta nepřináší nějaký výpočetní overhead, ale domnívám se, že ne, neboť CST potřebuje k výpočtům prakticky pouze S-parametry, které si z projektu vytáhne, popř. si je nejprve automaticky spočítá spuštěním simulace interního projektu. Proto se také domnívám, že varianty (a) a (b) budou z pohledu CST Design Studia identické. Bohužel však v případě, že bychom používali tyto metody a následně jednotlivé projekty ladili, tak se pokaždé musí znovu importovat do Design Studia, aby se vytvořil opět aktuální snapshot projektu.

- [x] Zkuste si s tím nějak poradit, protože opravdu detaily tohoto neznám... Určitě srovnejte přístup z Design Studia se simulací kompletní struktury, mělo by to navzájem sedět.  

> Dejte mi prosím vědět, zda-li dobře rozumím Vámi představené ideji a zda souhlasíte s mou volbou práce s Design Studiem. Já se v mezičase pokusím získat nějaké výsledky s duálním feedem zakončeným aktivním portem a importem pomocí dynamického bloku :)  
>   
> Zatím děkuji a přeji vše dobré!  
> Martin Šimák  

Moc díky za super práci!!! Mějte se a zdravím  
  
Pavel Hazdra  

> P.S. Právě jsem si opožděně všiml, že Touchstone import S-parametrů je také možný buď pomocí dynamického bloku (block), nebo statického snapshotu (file block), takže s toutu "block vs. file block hantýrkou" budete pravděpodobně obeznámen :)  

Nejsem :))))
# 10/14/2024
Dobrý den,  
  
super :) Díky za soubory, ten dual feed vypadá opravdu nadějně!  
  
Ohledně jeho ladění je však otázka, do jakého prostoru by měl koukat výstupní vlnovod (a jaká má být jeho délka v režimu ladění). Jsou 3 možnosti, které by stálo za to porovnat už s polarizérem (který by se po porovnání zase odpojil):  
  
Čili feed koukající do:  
  
- volného prostoru (jako máte teď)  
- bezodrazné stěny (Z max = open)  
- aktivního (ale nebuzeného portu)  
- pasivního portu (v parametrech portu "monitor only")  
  
Jde totiž i o to, aby amplitudy pole pro obě buzení byly na výstupu pokud možno stejné. Fáze určitě nebudou, ale to nemá díky různé délce řešení a nastaví se externě pro CP. V případě výstupního vlnovodného portu bude třeba nastavit počet modů 2, aby se postihly obě polarizace.

- [x] Všechny 4 uvedené možnosti jsem vyzkoušel na samostatném přechodu a moc se neshodují.
  
A samozřejmě minimalizovat S21.
  
Ještě jednou díky a budu se těšit na další výsledky ladění!  
  
mějte se pěkně, zdraví  
PH

P.S. Dále by šlo ladění zrychlit tím, že by se spočetly S parametry polarizéru a z nich se udělal krabička pro Design studio.. Ale s tím moc zkušeností nemám. Na 100% to přes Design studio možné je.. Pokud byste se s tím trápil déle než je rozumné, zeptáme se na podpoře.

- [x] Přemodelováno tak, aby seděl Port{1,2} přechodu na Mode{1,2} polarizéru.
- [ ] Vyzkoušet, že schematic dává stejné vysledky jako temp projekt, ve kterém vykopíruji oba projekty za sebe a napřímo je spojím pro jednotnou simulaci.

> Dobrý den,  
>   
> děkuji za starost. Měl jsem v plánu dát Vám o své odmlce vědet, ale bohužel jsem to ve spěchu kolem odletu nestihl :)  
>   
> Prozatím Vám zasílám onu prezentaci, kde jsem prof. Linovi předvedl náš postup, co se týče jak vyzařovací části, tak i části napájecí. V napájecí části jsem ukázal výsledky napájení jedním koaxem a předestřel ideu duálního napájení. Jako demo jsem také ukázal, že idea funguje, za použití stejných hodnot pro vdálenosti a rozměry druhé koaxiální sondy jako u single feed řešení. Počet drátů v mřížce a její rozestup jsem zatím jenom nastřelil, ale i tato prvotní konfigurace naznačuje, že postup má slibnou budoucnost - samozřejmě s tím, že bude ještě třeba hodně ladit.  
>   
> Na závěr jsem mu představil komplikace spojené s výrobou prototypu a Váš návrh ohledně modulární výroby "per partes", načež profesor uznal, že bude nejjednodušší, když to ještě pořádně odladíme a nakonec se to pokusíme vyrobit až ve finální podobě.  
>   
> Takže zatím takto a brzy se pustím do dalšího ladění duálního napájení. V příloze také zasílám svůj project file, kdybyste chtěl nahlédnout na nabídnout radu či dvě ohledně implementace.  
>   
> S pozdravem a přáním pěkného týdne  
> Martin Šimák
# 09/24/2024
Dobrý den, v textu...  
Díky moc, super práce!  
Zdraví  
PH  

> Dobrý den,  
>   
> po načtení článků jsem podnikl první kroky jak v návrhu přechodu (zatím jeden koaxiál do čtvercového vlnovodu), tak vyzařovací části. V obou částech jsem dospěl k následujících závěrům a rád bych se s Vámi poradil, zda-li odpovídají očekáváním, než se pustím dále.  
>   
> Přechod jsem tedy oddělil do samostatného projektu, kde jsem postupoval podle hezky sepsané příručky na webu Microwaves101 ([https://www.microwaves101.com/encyclopedias/waveguide-to-coax-transitions?jtpl=12](https://www.microwaves101.com/encyclopedias/waveguide-to-coax-transitions?jtpl=12)). Spočítal jsem si tedy guide wavelength na frekvenci 5..5 GHz (střed pásma), neboť ideální vzdálenost koaxiální sondy od zkratující stěny na konci vlnovodu Microwaves101 uvádí jako čtvrtinu této hodnoty, což dle mých výpočtů činí zhruba 16.26 mm.. Dále pro délku sondy je doporučeno brát za referenci 3/16 vlnové délky ve vakuu na střední frekvenci. Tento údaj mi z kontextu uvedeného na stránce přijde silně spjat s obdělníkovou geometrií, kterou článek bere v potaz, ale lepší referenci jsem neměl a nastavil jsem proto tuto hodnotu, což je zhruba 10.22 mm. Dále jsem provedl několik parsweepů těchto proměnných a na závěr jsem provedl optimalizaci, která měla za cíl minimalizovat odraz (S11) v pásmu od 5 do 6 GHz. Výsledné hodnoty jsou: vzdálenost od zkratu 13.08 mm a délka sondy 12.16 mm. Hodnoty mi přijdou vcelku rozumné a výsledné S-parametry a vyzařováky jsou obsaženy v příloze "coax-to-waveguide-transition-sweep.zip" k nahlédnutí.

- [x] Bezva, ano, máte naprosto pravdu, že "klasické" návrhy vychází z obdélníkového vlnovodu. Výsledky vypadají OK!  

> Pro zlepšení vyzařovací části polarizéru jsem za jeho výstup přidal čtvercový vlnovod a jeho vlastnosti se dle mého názoru zlepšily. Dále jsem provedl parsweep délky přidaného vývodu, ale neshledal jsem výrazný dopad. Výsledky v podobě exportovaných AR a vyzařováků naleznete v příloze "polarizer_outlet-simulation.zip".  

- [x] Super, to pomohlo dost! V tuto chvíli by bylo možné optimalizovat přechod na kruhový trychtýř za prodloužením..  

> Mimo jiné bych rád zmínil určitý update spojený s naším sdíleným Google Docs dokumentem. Již nějakou dobu si vedu jakési "Design notes" během své práce, abych nezapomněl klíčové kroky a lépe se mi vracelo v postupu, kdyby bylo třeba něco vrátit do dřívější fáze návrhu. Uvědomil jsem si tedy, že bude nejlepší, když tyto poznámky budu následně kopírovat do tohoto dokumentu, abyste případně mohl nahlédnout, jak během návrhu postupuji. Já se Vám tedy snažím psát rozsáhlejší emaily, ale vím, že mnohdy mohou být tyto kondenzované zprávy trochu nepřehledné.  

- [x] To je výborné, projdu, dost toho přibylo :) Já se omlouvám za stručnost, jenže ono není moc co komentovat, protože výsledky vypadají bezvadně..  

> Zatím budu předpokládat, že výsledky jsou uspokojivé a pokusím se podniknout následující kroky:  
> 1. V návrhu přechodu bych se dále vydal tím směrem, že vlnovod prodloužím směřem "dozadu" a zkratovací stěnu nahradím dráty. Uvidím, jak se bude tento virtuální zkrat chovat na simulacích, ale předpokládám, že vzdálenost stávající sondy od něj by se neměla příliš lišit od momentální hodnoty vzdálenosti od reálné stěny. Dále přikreslím zbytek duálního napájení a budu se pokoušet strukturu ladit tak, jak postupovali autoři článku.  

- [x] No já bych možná postupoval obráceně - ponechal současný napaječ, jak je, strukturu prodloužil, přidal "zkratovací dráty" a za ně otočený feed. ? Bude se to lépe kreslit. Nejprve přidejte ty dráty a koukněte, jak se změní současné přizpůsobení, případně dooptimalizovat a pak dokreslit druhý feed (ten by první neměl příliš ovlivňovat, řekl bych vůbec)  

> 2. Ve vyzařovací části se pokusím navrhnout přechod na kruhový vlnovod přímo z čtvercového polarizéru. Pokud to nebude vykazovat dobrý výkon, zkusím ponechat čtverec a z něj pak přejít na kruh.. Skrytě se však domnívám, že přimý přechod z polarizéru na kruhový vlnovod by mohl vykazovat lepší vlastnosti, protože generovaná vlna je v obou polarizacích lehce natočená kolem boresight osy a rotační invariance kruhového vlnovodu by mohla poskytovat v tomto případě nižší míru nespojitostí. Zatím je to jenom domněnka a hlavně si musím držet palce, aby ten kruh měl podobné frekvenční pásmo jako polarizér :)  

- [ ] Tady bych asi postupoval tak, že bych ponechal jen samostatný úsek čtvercového vlnovodu (bez polarizéru) a na něj optimalizoval přímo kruhový trychtýř (pro jednu polarizaci). Tipl bych si, že to pak bude fungovat dobře i s polarizérem a ušetří se buňky..

# 09/13/2024
Dobrý den, komentuji v mailu...  

> Dobrý den,  
>   
> děkuji za odpověď a omlouvám se za svou netrpělivost :)

Nene, dobře, že se připomínáte, mě to tu snadno propadne mimo dohled a pak zapomenu.. :)  

> OK, na přechod si tedy založím oddělený projekt. Pokud se nepletu, tak CST pak nějak umí napojit více projektů za sebe pomocí portů, že? Dále potvrzuji přijem navazujícího mailu a děkuji za náčrty.

- [x] To propojení by mělo jít, bohužel s tím nemám žádnou zkušenost. Každopádně přechod řešte separátně..  

> Ano, vyzařovák se mi také moc nelíbí. Zkusím se podívat na to lepší zakončení. Myslíte, že bude lepší ten přechod na kruhovou strukturu? Já jsem nějak předpokládal, že se bude lepší držet čtverce, abychom se vyhnuli nespojitostem, a zakončit to čtvercových hornem, ale možná je to zbytečná obava.

- [x] Naopak se domnívám (i z hlediska výroby), že kruhová struktura bude lepší. Ten "přechod" jsme kdysi řešili, pošlu projekt. Nicméně bude to chtít ještě kousek čtvercového vlnovodu za polarizérem a až pak trychtýř..  
- [x] Ale mrkněte nejprve, co se stane s vyzařovákem, když se za trojuhelniky přidá úsek čtvercového vlnovodu (bude asi třeba optimalizovat jeho délku)

> Ještě bych rád vyzdvihnul jednu věc, a to že místní profesor mě již před pár týdny pobízel, abych přišel s nějakým minimálním návrhem, který bychom mohli vyrobit a vyzkoušet si na něm měření v místních podmínkách. To mi přijde jako skvelá nabídka, ale zároveň zůstává otázka, co bychom považovali za takový minimální výrobek. Jako první možnost mě napadá vynechat druhé napájení, ale to bychom přišli o půlku měření včetně měření přeslechů. Zároveň ale návrh obou napájení určitě ještě chvíli zabere, neboť to vnímám jako nejnáročnější část. Obracím se tedy na Vás, jestli Vás něco nenapadá, popř. jestli vlastně považujete tuto možnost za užitečnou.

- [x] :) no to není úplně snadné... Vyzařovací část v podstatě máte, ta by šla vyzkoušet separátně, pokud by ovšem končila standardním vlnovodem - což není náš případ. Vzhledem k tomu, že je na práci ještě dost času mi toto nepřijde jako vhodný nápad.  
- [x] Co by snad šlo, je mít strukturu blokově: Trychtýř, polarizér a přechod (zatím s jedním portem - ten by šel přes příruby mechanicky otočit). Dále by se vyrobil dvouportový přechod.  

> Takhle na papíře mi to přijde srozumitelné a uvidíme, na co narazím během implementace. Případně se ozvu. Ještě jednou děkuji za cenné rady a přeji, aby se Vám i rodině vyhnula nebezpečí spojená s hlášenou povodňovou situací v ČR.

Děkuji, jsem na příjmu... Ať se Vám daří, dešťem přímo ohrožen nejsem, jediné co hrozí je, že mi vítr shodí anténu na KV...  
  
Mějte se príma, zdraví  
Pavel Hazdra

### Follow-up email #1 (with attachment)
Zde jiný projekt, jde nám o to, jak je uděláno napojení čtvercového vlnovodu na kruhový trychtýř.  
  
Jde o tuto práci, kde jsou i nějaké detaily k přechodu.  
[https://dspace.cvut.cz/handle/10467/100817](https://dspace.cvut.cz/handle/10467/100817)  
### Follow-up email #2
ad přechod:  
- je také možno řešit planárně
  [https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8076919](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8076919)  
- klasické řešení je
  [https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9933815](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9933815)  
  
Tohoto bych se asi držel. Ta "mřížka" slouží a) jako odrazná plocha pro Port 2 a b) zlepšení izolace mezi porty  
Zkusil bych přeškálovat rozměry na naše pásmo a doladit. Pozor, občas autoři záměrně uvádí špatné rozměry :)  

P.S. Myslím, že místo hranaté mřížky by obdobně fungovaly i kulaté - s ohledem na výrobu asi lepší - vyvrtat díry a nasunout dráty..
# 09/12/2024
Dobrý den,  
  
ještě jednou se omlouvám za zpoždění. Velmi děkuji za skvělé zprávy jak se zápisem, ale hlavně s možností výroby.  
  
Ohledně přechodu (a budeme posléze potřebovat dva kolmé pro vybuzení LHC/RHC polarizace) bude nejjednodušší, když Vám v dalším mailu načrtnu, jak to asi může vypadat. A ano, ten přechod řešte samostatně, s kusem čtvercového vlnovodu, který zakončíte ideálně portem (na konci bude tím pádem stejná impedance, jako budoucí navazující vlnovod). Tohle je víceméně formalita, horší budou ty dva kolmé porty a optimalizace přeslechů.  
  
Ještě jsem se díval na vyzařovák. Přidejte si broadband farfield monitor a do postprocesingového makra LHC, RHC a AR v hlavním směru (theta=phi=0). To jsem udělal a osový poměr je dobrý někde mezi 5-6 GHz. Nicméně tvar vyzařováku není úplně pěkný - možná tím, že se vyzařuje přímo ze struktury s trojuhelníky. Tj. by bylo dobré na konec přidat ještě kus čtvercového vlnovodu, případně přímo nasadit kruhový vlnovod s trychtýřem. Přechod mezi čtvercem a kruhem se dá celkem jednoduše optimalizovat, jde to připojit přímo na sebe bez nějakého pozvolného přechodu.  
  
Tudíž se práce rozpadá do dvou víceméně samostatných částí:  
- návrh přechodu, tj. koaxiál do čtvercového vlnovodu. Nejdřív jeden a pak druhý kolmý  
- návrh vyzařovací části  
Snad je to srozumitelné, případně si můžeme někdy zavolat přes Teams apod..  
  
Díky moc!  
  
Zdraví  
Pavel Hazdra

> Dobrý den,  
>   
> po určité odmlce jsem zkontroloval KOS a mohu potvrdit, že jsem se shledal býti zapsán k připravenému tématu a že mi bylo systémem umožněno zapsat si předmět DP :)  
>   
> Co se samotné práce týče, místnímu profesorovi jsem podal v uplynulém týdnu report v podobě prezentace, kterou přikládám k této zprávě, neboť dobře shrnuje můj dosavadní postup při návrhu. Aktuální verzi projektového souboru též naleznete v příloze. Kromě samotné práce jsme také hovořili o možnostech výroby, načež mi profesor sdělil, že se osobně zná s víceprezidentem jedné taiwanské firmy, která vyrábí vlnovody v milimetrovém pásmu. Došel tak k názoru, že by nám tam měl být schopen domluvit výrobu za rozumnou cenou, kterou pokryje jeho budget, a že se také určitě nemusíme bát o přesnost strojní výroby :)  
>   
> Momentálně se tedy nacházím ve fázi, kdy jsem k polarizéru přikreslil úsek čtvercového vlnovodu, který nejprve posloužil k tomu, abych si ověřil, že se oba vybuzené módy hezky polarizují tak, jak jsem zamýšlel. Dále jsem otevřený konec zkratoval zdí a nyní se snažím vymyslet způsob, jak vybudit daný mód vlnovodu koaxiálním napájením. Postupoval jsem podle stránky [https://www.microwaves101.com/encyclopedias/waveguide-to-coax-transitions](https://www.microwaves101.com/encyclopedias/waveguide-to-coax-transitions) , ale v simulaci se mi nedaří dosáhnout kýženého výsledku. Prozatím Vám ale posílám svůj dosavadní postup s tím, že se dále budu pokoušet dosáhnout lepších výsledků laděním primárně délky "anténní sondy", neboli prodlouženého vnitřního vodiče napájecího koaxu do dutiny vlnovodu, a vzdálenosti této sondy od zkratovací zdi. Pokud se mi nebude dařit, tak si možná udělám oddělený CST projekt, kde budu pouze trénovat přechod koax - čtvercový vlnovod...  
>   
> S pozdravem  
> Martin Šimák
# 08/12/2024
Dobrý den,  
  
ok, dobře, začal bych se pomalu zabývat návrhem přechodu. Říkali jsme, že bude dvoupolarizační, tj. 2 navzájem kolmé piny, bude třeba řešit izolaci mezi porty...  

Tu výrobu budeme řešit kde prosím? Tady by to šlo v RFSpinu, ale otázka, kdo to zaplatí :) Možná bych kolegu ukecal...

Ano, ten návrh projektu jsme v tomto duchu psali, podívám se na to a případně upravím a pak to probereme...

Díky za práci!  
Zdraví  
PH

> Dobrý den,  
>   
> děkuji Vám za připomínky. Bohužel to mám teď také trochu rozlítané, tak jsem se k zapracování dostal až s prodlevou. Definice ostatních metrik tedy prozatím ponechám, ale návrh budu optimalizovat primárně podle výsledků z E-field probe. Dále jsem upravil vyhodnocení fáze pomocí funkce UPhD(-,180), která provádí "unwrapping" a poskytuje výslednému grafu mnohem lepší rozlišení.  
>   
> V uplynulém týdnu jsem také prezentoval svou práci na vyhodnocování metrik profesoru Linovi. Ten byl s postupem spokojen a následně mě pobídl, abych se pomalu porozhlédl po možnostech výroby a měl tak možnost si vyzkoušet měření na polarizéru, až budeme mít odladěný polarizér.  
>   
> Kromě dalšího postupu v návrhu bych se s Vámi chtěl poradit také o specifikaci tématu. Domnívám se totiž, že jak na taiwanské straně, tak i na straně ČVUT bychom měli do zacátku semestru mít finalizované zadání, tj. název a cíle práce, popř. bibliografické zdroje. Předpokládám, že to bude něco obdobného první stránce našeho sdíleného dokumentu.  
>   
> S pozdravem  
> Martin Šimák
# 07/31/2024
Dobrý den,  
  
konečně jsem se dostal k odpovědi..  
Díky moc, komentuji prozatím stručně v textu a zdravím! Zítra budu mít víc času se na projekt podívat.  
Pavel Hazdra

p.s. ono to takhle vlastně nepolarizuje...

- [x] Možná by bylo fajn předřadit kus čtvercového vlnovodu (s proměnnou délkou) a na něj dát port. Poté by na monitorovacím portu měl být vidět rozklad do dvou polarizací. A také vyzařovák.. update, teď jsem to zkusil, přidal jsem 20mm obd. vlnovodu a ono to funguje dost pěkně!
- [x] Jen je třeba opravdu (aspoň v mém případě) u portu na obdélníkovém WG dát ten polarizační uhel 0, jinak to chytí nějaký šikmý moc..
- [x] Zkusil jsem parsweep na delku vstupniho vlnovodu a efekt na S parametry je malý, ale docela zásadní na RHC, LHC a AR..

> Dobry den,  
>   
> omlouvam se za prodlevu. Nepodarilo se mi to stihnout zapracovat do sveho odjezdu, nebot jsem se take v mezicase odebral na dovolenou. Nicmene dekuji za pripominky, prikladam projektovy soubor spolecne s archivem obsahujicim obrazky grafu a nize se pokusim reprodukovat Vas styl odpovedi, ktery se mi velmi zamlouva. :)  
> 
> > Port mode solver umožňuje nastavit frekvenci modálního výpočtu - někde v T solver a Specials, zkoušel jste toto?
> 
> Diky! Toto nastaveni jsem jiz v minulosti hledal a nepodarilo se mi. Bohuzel vsak toto nastaveni umoznuje pouze specifikovat jiny frekvencni bod, ktery nemuze byt parametrem, takze stale nejsem schopen pracovat pres pasmo.

- [x] Aha, no to je bohužel pravda.. zparametrizovat to nejde.  

> > Domnívám se, že pokud by se dal na výstup polaizéru port a nastavily 2 správné (na sebe kolmé) mody, mělo by to dát požadovanou informaci. Tj. na vstupu port s jedním modem a na výstupu port se 2mi, S21 do těch dvou modů bude odpovídat amplitudám.
> 
> Pole v portech vlnovodu jsou snad orientovana spravne. Kdybych vsak nebyla, tak vlastne nevim, jak je zmenit. Vlnovodne porty vzdy definuji pomoci picknuti steny v usti vlnovodu, coz si orientaci portu vyresi samo, prestoze mohu mit treba i orotovany WCS. To je ovsem asi spravne, protoze se takto nemusim starat o to, ktery mod je ktery.

- [x] Nastavit by to jít mělo, v parametrech portu je Polarization angle, to by mělo zafixovat orientaci prvního modu.  

> Kdyz dam na vstup port s jednim modem, tak mi zmizi S-parametry s 1(2) a z dvojice parametru S2(1),1(1) a S2(2),1(1), tj. prenos z prvniho modu buzeneho na vstupnim portu do obou modu monitorovanych na vystupu, mnoho informaci nejsem schopen vycist, nebot prvni z nich je ocekavatelne vysoky (kolem -0.0035 dB) a druhy komplementarne k nemu extremne nizky (kolem -180 dB).
> 
> Jestli ten pomer amplitud modu v usti polarizeru vubec budeme schopni vycist takhle jednoduse z S-parametru, tak to podle me bude neco jako |Mod2|/|Mod1| = S2(2),1(2)/S2(1),1(1). V priloze jsou vysledky v obrazcich "s_parameters-phase.png" a "s_parameters-amplitude.png".

- [x] Ano, to je pravda, pardon, s jednim modem to fungovat nebude, tech 180 dB je numericka "chyba", prostě 0.  

> > Druhá možnost je dát na výstup polarizéru (nebo někam kousek dovnitř) nikoli řez polem, ale E field probes. Ty rovnou dávají složky Ex Ey Ez v daném místě..  
> 
> Vysledky teto metody ve slozce Ex se dobre shoduji ve fazi i amplitude s metodou pomoci S-parametru (obrazky "probe-x-phase.png" a "probe-x-amplitude.png"). Nicmene jako nejduveryhodnejsi referenci povazuji vysledky na stredove frekvenci ve slozce Port Information (obrazky "port_information-phase.png" a "port_information-amplitude.png"). Jak slo asi ocekavat, zatimco fazovy rozdil lze z S-parametru extrahovat dobre (dobra shoda s Port Information), pomer amplitud dava nejake nesmysly. Co ale E-field probe take pocita, je amplituda pole (slozka Abs) v danem bode, coz lze jednoduse pouzit pro zjisteni amplitudy, ktere se dokonce velmi dobre shoduje na stredove frekvenci s vysledkem v Port Information.
> 
> Bolestivym aspektem tohoto pristupu je, ze "1D Results/Probes/E-field/E-Field (0 0 39)" obsahuje hodnotu parametru "natvrdo" interpretovanou behem vypoctu, tj. po kazde zmene parametru "WaveguideLength" proto clovek musi menit post-processing vypocet a mozna kvuli tomu nepujde sweepovat/optimalizovat pres tuto promennou.
> 
> Paklize by se nam podarilo nejak rozumne pouzit tuto metodu, tj. odstranit tu neprijemnost s neparametrizovatelnosti, tak se muzeme zbavit 2. portu, ktery je v projektu definovan kvuli definici prenosovych S-parametru. Navic se mi zda, ze touto metodou budeme schopni ziskavat solidni vysledky jak pro fazi, tak i pro amplitudu.

- [x] Tak to je super zpráva!  

> > A třetí možnost nechat polarizér vyzařovat a koukat na AR v ose, případně poměr LHC/RHC (makro též umožňuje)  
> 
> Vzhledem k tomu, ze polarizer momentalne neni vyladen, tak jsem vypocet AR sice take definoval, ale zatim pouze pro uplnost.  
>   
> Rad bych Vas tedy v tuto chvili poprosil, zda-li byste se podival na projekt a na vysledky, nebot si nejsem 100% jist, ze se mi podarilo vse implementovat spravne. Jednou veci, kterou si napriklad nejsem zcela jist je to, zda-li muzeme povazovat fazovy rozdil mezi mody v Ex za celkovy fazovy rozdil. Zatimco pomoci slozky Abs jsem byl schopen ziskat informaci ucelenou, pro fazi se mi to nepodarilo - secist vsechny tri slozky a vzit z nich fazi mi vyhodilo nesmysl.

- [x] Kouknu na to..

# 07/06/2024
Dobrý den,  
  
díky moc, omlouvám se za zpoždění, já jsem akorát před odjezdem na dovolenou, budu zpět příští neděli - komentuji v textu a zdravím!  
  
Pavel Hazdra

> Dobrý den,  
>   
> po určité odmlce jsem se opět pustil do práce a pokusil jsem se zapracovat Vaše komentáře. Po přikrmení meshe jsem se také pustil do extrakce kvalitativních parametrů kruhové polarizace (fázové zpoždění a poměr amplitud módů v ústí polarizéru) v širším pásmu než pouze na střední frekvenci. To se však ukázalo jako výrazně náročnější, neboť konstantu šíření (Gamma) CST počítá pouze v rámci Port Information, tj. pouze na oné středové frekvenci nastaveného pásma, a mé pokusy o oklamání softwaru, aby mi to počítal i na jiných frekvencích, se setkaly s neúspěchem. Dospěl jsem tak následně k následujícím možnostem:

- [x] Port mode solver umožňuje nastavit frekvenci modálního výpočtu - někde v T solver a Specials, zkoušel jste toto?
	- Bohuzel mohu specifikovat stale jenom jeden frekvencni bod.
	- Good to know though! 

> Lze předpokládat, že vybuzenou monochromatickou vlnu v polarizéru lze popsat jako klasickou vedenou vlnu šířící se ve směru osy z, tj. $$\boldsymbol{E}(x,y,z,t) = \boldsymbol{E}_0(x,y)*\exp(-\mathrm{i}(kz-\omega t),$$kde $$k = \beta-\mathrm{i}\alpha$$je komplexní vlnové číslo, kde $\beta$ je fázová konstanta a $\alpha$ je konstanta útlumu. Proto bychom z definice S-parametrů měli být schopni psát $$S_{21} = \exp(-\mathrm{i}kL),$$kde $L$ je délka polarizéru/vlnovodu. Po rozepsání vlnového čísla v exponenciále by tak mělo platit, že rozdíl fází $S_{21}$ prvního a druhého módu (v CST zápis `S2(1),1(1)` a `S2(2),1(2))` dává rozdíl jejich fázových konstant násobený délkou polarizéru, tj. přímo fázové zpoždění módů.

- [x] Ano, to dává smysl. Alfa bych s klidem považoval = 0.  

> Další zajímavou možností je diskuze na [fóru ResearchGate](https://www.researchgate.net/post/how_to_plot_propagation_constant_vs_frequency_in_cst_microwave_studio), kde popisují možnost využití Eigenmode Solveru, kdy člověk nastaví periodic boundary s fázovým posuvem a nechává si počítat disperzní charakteristiky módů pomocí VBA makra. Jakkoli se zdál tento postup slibný, nenašel jsem toto VBA makro ve své verzi CST. Stále však tuto diskuzi považuji za zajímavý zdroj infromací, neboť s Eigenmode Solverem nemám mnoho zkušeností, a tak si nemohu být jist, že neskýtá jiné možnosti dosažení tohoto cíle.

- [x] Tohle řešil kolega doktorand v jiném projektu a co vzpomínám, tak si musel napsat skript v Matlabu na extrakci disperzní charakteristiky. Můžeme zkusit později, zatím se mi to zdá jako overkill.  

> Hned druhým - ne-li snad ještě vetším - problémem je potom extrakce poměru amplitud. Tam se obávám, že si S-parametry a konstantou šíření nepomohu, a tak jediné, co mě napadá, je navzorkování frekvenčního pásma E-field monitory a extrapolovat data z jejich řezů, což mi ale přijde jako šílená mašinerie.

- [x] Domnívám se, že pokud by se dal na výstup polaizéru port a nastavily 2 správné (na sebe kolmé) mody, mělo by to dát požadovanou informaci. Tj. na vstupu port s jedním modem a na výstupu port se 2mi, S21 do těch dvou modů bude odpovídat amplitudám.
	- Domivam se, ze pole v portech vlnovodu mam orientovana spravne. Kdybych ale nemel, tak vlastne nevim, jak je zmenit. Vlnovodne porty vzdy definuji pomoci picknuti steny v usti vlnovodu, coz si orientaci portu vyresi samo, prestoze mohu mit treba i orotovany WCS. To je ovsem asi spravne, protoze se takto nemusim starat o to, ktery mod je ktery.
	- Kdyz dam na vstup port s jednim modem, tak mi zmizi S-parametry s 1(2) a z dvojice parametru S2(1),1(1) a S2(2),1(1), tj. prenos z prvniho modu buzeneho na vstupnim portu do obou modu monitorovanych na vystupu, mnoho informaci nejsem schopen vycist, nebot prvni z nich je celkem uniformne vysoky (kolem -0.0035 dB) a druhy komplementarne k nemu extremne nizky (kolem -180 dB).
		- Jestli ten pomer amplitud modu v usti polarizeru vubec budeme schopni vycist takhle jednoduse z S-parametru, tak to podle me bude neco jako |Mod2|/|Mod1| = S2(2),1(2)/S2(1),1(1).
- [x] Druhá možnost je dát na výstup polarizéru (nebo někam kousek dovnitř) nikoli řez polem, ale $E$-field probes. Ty rovnou dávají složky $E_x$ $E_y$ $E_z$ v daném místě..
      Vysledky teto metody ve slozce $E_x$ se dobre shoduji ve fazi i amplitude s metodou pomoci S-parametru.
      Kandidat na idealni reseni, protoze ani nepotrebuje druhy port na vystupu.
      Bolestiva realita toho, ze *1D Results/Probes/E-field/E-Field (0 0 39)* obsahuje hodnotu parametru "natvrdo" interpretovanou behem vypoctu, tj. po kazde zmene parametru `WaveguideLength` proto clovek musi menit post-processing vypocet a mozna kvuli tomu nepujde sweepovat/optimalizovat pres tuto promennou.
- [x] A třetí možnost nechat polarizér vyzařovat a koukat na AR v ose, případně poměr LHC/RHC (makro též umožňuje)
      Vzhledem k tomu, ze polarizer momentalne neni vyladen, tak tento vypocet jsem sice take definoval, ale zatim pouze pro uplnost.

> Rád bych se s Vámi tedy poradil, zda-li Vás nenapadnou elegantnější řešení. Obávám se, že se dále nepohneme bez tohoto kroku, neboť se jedná o širokopásmové vyhodnocení kvality produkované polarizace, což dozajista potřebujeme. V příloze naleznete opět můj model, kde došlo pouze k malým změnám (mesh, frekvence a post processing).

- [x] Na model se snad stihnu ještě kouknout...
# 06/25/2024
Dobrý den,  
  
ok príma, díky za zprávy!  
  
Ohledně CST je to na naší straně, novou licenci již máme, admin ji jen musí nahodit na licenční server.  
Připomenu mu :)  
  
Zdraví  
Pavel Hazdra

> Dobrý den,  
>   
> děkuji za další poznámky, které se postupně snažím zapracovat. Stejně tak pracuji na již lehce uceleném úvodním povídání, kde shromažďuji své dosavadní poznatky. Ještě jsem nepostoupil příliš daleko, abych se vyptával na další kroky, ale dnes mi CST vyhodilo zprávu, že mi za 4 dny končí moje stávající akademická licence. Vím, že počet těchto licencí má ČVUT omezený, a tak bych se rád zeptal, zda-li bude možné mou licenci obnovit. Pakliže by to byla potíž, mohu se pokusit získat licenci zde na univerzitě, která také určitým počtem disponuje.  
>   
> S pozdravem  
> Martin Šimák
# 06/10/2024
Dobrý den,  
  
príma, díky, pěkný víkend!  
  
Pavel Hazdra

p.s. ještě jsem koukal na ten model..  
Cutoff těch dvou modů je někde kolem 9 GHz a simulace probíhá jen do 10, to pro vyhodnocení nějakého širokopásmového stavu nebude stačit.  
  
Dále bych trochu přikrmil mesh, alespoň na 20 cells/lambda a ve Specials Edge refinement na 3 a víc, aby to mělo celkem minimálně tak 500 tis buněk.

... a stejně tak nemá cenu budit strukturu pod cutoffem, díky odrazům to zpomaluje simulaci..

> Dobrý den,  
>   
> děkuji za odpověď. Souhlasím jak Vašimi komentáři, tak i s poznámkou, že bude nejlepší dosavadní poznatky sepsat. O něco se v blízké době pokusím, a to nejspíše do toho sdíleného Google Docs dokumentu. Opět se Vám ozvu, až budu mít něco nového, nebo pokud byste na něco narazil Vy :)  
>   
> Děkuji Vám a také přeji, at se daří!  
> Martin Šimák
# 06/06/2024
Dobrý den,  

přeji úspěch u zkoušek, komentuji v textu.
> Dobrý den,  
>   
> po určité prodlevě vynucené závěřečnými zkouškami jsem se opět pustil s chutí do práce, a tak bych Vám rád prezentoval výsledky svého snažení.  
>   
> Jak jste navrhoval, pustil jsem se neprve do přípravy parametrizovaných modelů čtvercového a kruhového vlnovodu s trojúhelníky. Zatímco v případě čtvercového jsem si byl ohledně geometrie řezu jistý, u kruhového jsem musel lehce improvizovat a pokusil jsem se o jakousi adaptaci stejného postupu. V příloze naleznete moje projektové soubory k nahlédnutí.

- [x] Díval jsem se na soubory, vypadají OK

> Během konkrétní realizace jsem se rovnou soustředil na dosažení kruhové polarizace pomocí dané struktury, konkrétně na ladění parametrů geometrie průžezu a délky polarizéru tak, aby vybuzené ortogonální módy dosahovaly (a) poměru amplitud elektrického pole co nejbližší 1 (= 0 dB) a (b) fázových konstant takových, že mezi módy na dané délce vlnovodu vznikne fázové zpoždění 90 stupňů. Jen pro ujištění bych rád uvedl, že tyto metriky sleduji pomocí post-processingu (a) jako poměr maxima absolutní hodnoty elektrického pole jednoho módu s druhým v dB a (b) jako rozdíl fázových konstant módů násobený délkou polarizéru.

- [x] Príma, to je dobrý nápad! Ale jen začátek, protože bude v dalším kroku třeba přikreslit napájecí část, tj. čtvercový/kruhový úsek vlnovodu. Ten se bude již budit již oběma kolmými mody postupně (tím se bude generovat LHC/RHC). Na výstupu polarizeru by šel dát vlnovodný port v režimu "monitor" a tam sledovat amplitudy a fáze. Pak by se pokračovalo vyzařovací částí a přechodem na koaxiál a pak dva kolmé koaxiály. Čili se pak bude řešit i izolace portů atd..

> Brzy jsem zjistil, že mezi fázovým zpožděním módů na jednotku délky a rovnosti amplitud elektrického pole je nutné udělat kompromis. Abych tento spor upřesnil, pokud chci dosáhnout zpoždění 90 stupňů na kratším polarizéru, musím zvětšit trojúhelníky, což ale zároveň snižuje poměr amplitud, a tedy opět kazí čistotu kruhové polarizace. Již tedy v této fázi návrhu se mi zdá, že čtvercový vlnovod bude vhodnější volbou, neboť kompromisu pro tuto stukturu se mi podařilo dosáhnout mnohem lepšího: poměr amplitud -0.17 dB za fázového rozdílu 90 deg na 39 mm dlouhém polarizéru. U struktury na kruhovém vlnovodu se mi podařilo dosáhnout pouze -1.31 dB za fázového rozdílu 90 deg, a to ještě za cenu toho, že polarizér musí mít 55 mm. Výsledné hodnoty jsou v podobě obrázků opět v příloze.

- [x] Rozumím a také si myslím, že čtvercový bude lepší.

> Jako poslední krok jsem ještě hledal práce od autorů Vámi odkazovaného článku, což bohužel mnoho plodů nepřineslo. Co se mi ale vyplatilo, bylo prohledávání citací onoho článku, neboť jsem se dopídil jedné publikace, kde autoři pracují se stejným principem jako my, ale s jinou geometrií v podobě "butterfly-shaped" řezu. Jedná se o článek obsáhlejší, byť velice propracovaný a kvalitně diskutovaný z mnoha pohledů. Zároveň se zdá, že prezentovaná struktura je schopna produkovat o něco kvalitnější výsledky, a tak by mohla sloužit jako slibná alternativa. Odkaz na článek zde: [https://ieeexplore.ieee.org/document/8852811](https://ieeexplore.ieee.org/document/8852811)  

- [x] Díky, koukal jsem.. Pěkný článek! Na druhou stranu je struktura výrazně komplikovanější.  
- [x] Doporučil bych stávající výsledky hrnout do nějakého dokumentu s krátkým popisem, ono se brzy zapomene, co se dělalo.  

Tak zatím asi takto, děkuji za dosavadní super práci a mějte se príma!  
Pavel Hazdra
# 05/13/2024
Dobrý den,  
  
děkuji za dobré zprávy! :)  
  
Ohledně těch článků, nic se mi k tomuto konkrétnímu polarizeru nepodařilo dohledat, zkuste taky, třeba budete úspěšnejší.. Jisté je, že funguje :)  
  
update: prave jsem objevil tohle  
[https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8364165](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8364165)  
  
mozna, kdyz prohledate clanky tech autoru, najde se jeste neco.  
  
V tuhle chvili bych si pripravil zparametrizovane modely ctvercoveho a kruhoveho WG s temi trojuhelniky (po cele delce) a podival se "Calculate modes only", jaké mody se tam generují, včetně vlnových délek apod..  
  
Zatím takto, třeba mě ještě něco napadne..  
  
Díky, zdravím  
PH

> Dobrý den,  
>   
> s radostí Vám mohu oznámit, že prof. Lin po mé rozsáhlejší prezentaci projektu neměl námitek a na leccos se doptával. Mám tedy dobrý pocit ohledně toho, že mu téma vyhovuje.  
>   
> Zároveň jsem si načetl všechny články, na které jste odkazoval v našem sdíleném dokumentu, a nabyl jsem tak většího porozumění principu polarizátorů. Musím však uznat, že jsem nenalezl shodu mezi články a Vámi popisovanou struktuou polarizéru s trojbokými hranoly utvářející onen seříznutý patch.  
>   
> Co se další práce týče, mám se tedy nyní pustit do modelování některých ze struktur z článků? Předpokládám, že jich budeme simulovat více, minimálně pro srovnání čtvercového a kruhového vlnovodu.  
>   
> S pozdravem  
> Martin Šimák
# 05/02/2024
Dobrý den,

aby anténa šlo rozumně vyrobit, ani moc malá ani velká. Viděl bych to někde mezi 5 a 10 GHz.

Zdravim
PH

> Dobrý den,
> 
> děkuji za potvrzení a obracím se na Vás hned s dalším dotazem :)
> 
> Pustil jsem se do rešerše Vámi doporučených článků, které nabízí pěkná řešení, a přestože je ještě nemám prostudované do konce, rád bych s Vámi otevřel otázku frekvence. Domnívám se totiž, že místní profesor se na frekvenční pásmo bude ptát, až mu budu projekt v úterý prezentovat, a tak bych rád měl připravenou alespoň nějakou odpověď. Máte zatím nějakou zevrubnou představu, na jaké frekvenci by mohl návrh fungovat?
> 
> S pozdravem
> Martin Šimák
# 04/29/2024
Dobrý den,

1. ano :)  
2. též ano, spočteno modálním analyzátorem CST (když dáte na strukturu port a necháte spočítat více modů). Je z toho možno odečíst cutoff a vlnovou délku daného modu - což se může hodit.  
  
Zdraví  
PH

> Dobrý den,  
>   
> děkuji za obrázek. Nicméně bych se rád doptal na funkcionalitu:  
>   
> 1. Rozumím tomu správně, že se jedná o čtvercový vlnovod, přičemž  
> polarizátor tvoří jeho sekce, kde jsou v protilehlých rozích vloženy  
> kovové trojboké hranoly, což v kolmém průřezu vytváří onu  
> strukturu patchové antény se seříznutými rohy?   
> 2. Vizualizace polí v průřezu polarizátoru zobrazuje dva ortogonální  
> módy elektrického pole?  
>   
> Opětuji pozdravy do Čech.  
> Martin Šimák
# 04/26/2024
Dobrý den,  
  
tak paráda :)  
  
Našel jsem obrázek modelu, s kterým jsme si kdysi hráli pro představu. Co si vzpomínám, nevycházelo to zcela optimálně, je tam již dost parametrů k optimalizaci. Taky bude třeba vyřešit to dvojité napájení.  
  
Zdravím do TW,  
Pavel Hazdra

> Dobrý den,  
>   
> draft se mi zatím velice zamlouvá! Profesor Lin po mě ostatně chtěl, abych mu na příštím meetingu (úterý 7. 5.) projekt blíže představil, k čemuž mi toto poslouží jako dobrá osnova. V následujících dnech bych se tedy v rámci bodu 1 pustil do rešerše, abych se seznámil s popisem polarizátoru.  
>   
> S pozdravem  
> Martin Šimák
# 04/25/2024
Dobrý den,  
  
tak to máme hromadu času :) Bezva!  
  
Nahodil jsem draft zadání, koukněte na to a uvidíme, co dál...  
[https://docs.google.com/document/d/1LQH2nck_XH05uqXsesR1cPJuTL99_3WOsDPxCpodvuY/edit?usp=sharing](https://docs.google.com/document/d/1LQH2nck_XH05uqXsesR1cPJuTL99_3WOsDPxCpodvuY/edit?usp=sharing)  
  
Zdravím!  
Pavel Hazdra

> Dobrý den,  
>   
> děkuji za iniciativu. Rád bych končil v zimě příštího akademického roku. Předmět projekt jsem již splnil před odjezdem na Taiwan :)  
>   
> S pozdravem  
> Martin Šimák
# 04/24/2024
Dobrý den,  
  
super! Nic se neděje..  
Měli bychom se tedy pokusit zformulovat zadání, něco připravím do konce týdne.  
Ještě mi prosím připomeňte, kolik na to máme dohromady času..  
  
Zdravím Vás,  
  
Pavel Hazdra

> Dobrý den,  
>   
> s radostí Vám mohu oznámit, že se mi konečně podařilo odchytit místního profesora. Zároveň mě mrzí, že jsem se musel takto na delší dobu odmlčet, ale prof. Lin byl zrovna na konferenci v Japonsku a vzdálená komunikace s ním vázla :) Nicméně s projektem souhlasí, neboť anténní a ostatní RF technika jsou jeho hlavními zájmy výzkumu.  
>   
> Rád bych se tak brzy pustil do práce, neboť zde musím každé 4 týdny reportovat svůj progres. Příští meeting mám za 2 týdny, kde bych rád projekt podrobněji uvedl a specifikoval důležité návrhové parametry, jako například frekvenční pásmo, zvolenou architekturu apod.  
>   
> Ješte jednou Vám děkuji za Vaši přízeň a za doposud příjemnou a proaktivní komunikaci.  
>   
> S pozdravem  
> Martin Simak
# 04/08/2024
Dobrý den,  
  
v podstatě ano, shrnul bych to následnovně:

- Analýza modů (příčných rezonancí) toho čtverce s trojuhelníky - v podstatě je to ala mikropásková anténa se seříznutými rohy, jen "otočená" do vlnovodu. Zjištění potřebné délky polarizátoru (mělo by jít částečně analyticky)  
- Optimalizace čtvercového vlnovodu s polarizérem na odraz a polarizační vlastnosti (zatím jako vlnovodný transformátor měnící lineární polarizaci na kruhovou)  
- Návrh napájení (přechodu z koaxu), ideálně pro obě polarizace (LHC/RHC)  
  
No a kdyby se to podařilo vyrobit, jen dobře :)  
  
Dejte vědět! Zdraví  
Pavel Hazdra

> Dobrý den,  
>   
> děkuji za navazující zprávu. Chápu tedy správně, že hlavní náplní mé práce by bylo nalézt vhodné řešení napájení struktury a následný návrh antény, tj. primárně simulační práce?  
>   
> Celkově se mi návrh líbí, ale musím se ještě ujistit, že se zamlouvá zdejšímu profesorovi. Napíši mu tedy e-mail a uvidíme, co řekne :)  
>   
> S pozdravem  
> Martin Šimák
# 04/04/2024
Dobrý den,  
  
napadlo mě dotáhnout jeden projekt, co jsme kdysi začali a nedokončili. Jednalo by se o vlnovodný polarizér, který by se dal dále doplnit anténou. V tuhle chvíli tato struktura generuje kruhovou polarizaci a pokud by se dřešilo napájení (2 piny) včetně izolace portů, měli bychom k dispozici LHC/RHC. Ty 5G antény jsou dost komplikované a po pravdě jsem takové téma vypsal jen pro nalákání :)))  
  
Co myslíte, líbilo by se Vám toto?  
Zdraví  
PH

> Dobrý den,
> 
> moc Vám děkuji za rychlou a pozitivní odpověď. Těším se na budoucí spolupráci :)
> 
> S pozdravem
> Martin Šimák
# 04/03/2024
Dobrý den,  
  
děkuji za Váš zájem! Určitě něco vymyslíme, zatím narychlo potvrzuji příjem mailu s tím, že se ozvu zítra, až bude více času. Budu s Vámi počítat, zvládneme to jistě i distančně.  
  
Zdravím na dálný východ :)  
  
Pavel Hazdra

> Vážený pane docente,  
>   
> rád bych Vás oslovil se zájmem o Vaše vedení mé diplomové práce. Mé jméno je Martin Šimák a pravděpodobně si na mě vzpomenete z předmětu Antény nebo ze seminářů Pravidelného setkání zájemců o mikrovlnnou techniku, které jsem v uplynulém roce dvakrát navštívil.  
>   
> Jsem studentem programu Elektronika a komunikace se specializací Rádiové komunikace a systémy a Vámi přednášeným předmětem Antény jsem prošel minulý rok (známkován A). Návrh antén mě velmi zaujal a zapsal jsem si proto navazující předmět Návrh a konstrukce antén, který jsem taktéž úspěšně zakončil (známkován A). Dále mě však studium zaneslo do dalekých krajin – trávím totiž tento a následující semestr, které by měly být mými závěrečnými, na Taiwanu v rámci Double degree spolupráce s tchajwanskou univerzitou NTUST, tzv. Taiwan Tech. Zde dále studuji další polní předměty jako je elektromagnetická kompatibilita a návrh vysokofrekvenčních PCB. Zároveň bych měl postupně pracovat na své diplomové práci, která bude současně vedena místním profesorem Ding-Bing Linem, jehož specializace zahrnuje návrh antén, mikrovlnných obvodů a jiná vysokofrekvenční témata, a zároveň potenciálně Vámi ze strany ČVUT.  
>   
> Pro výběr tématu jsem navšívil Vámi publikovaná témata závěřečných prací na stránce katedry elektromagnetického pole ([https://elmag.fel.cvut.cz/zaverecne-prace/](https://elmag.fel.cvut.cz/zaverecne-prace/)), kde mě zaujaly zejména témata "Antény pro systémy 5G" a "Polarizačně-diverzitní antény". Rád bych se Vás tak zeptal, zda-li byste měl zájem o vedení mé diplomové práce v jednom ze zmíněných témat.  
>   
> Uvědomuji si, že spolupráce, o níž Vás žádám, je vcelku nestandardní. Již snad jen proto, že celá by probíhala distačně. Proto jsem také výbíral taková témata, která mi přišla zatížená zejména na analytickou a simulační práci. Rád bych Vás ujistil, že tento nedostatek jsem schopen také vynahradit velkou a pravidelnou pílí z mé strany. Na zdejší univerzitě je zvykem na diplomové rešerši pracovat usilovně a často – každé 4 týdny reportuji prof. Linovi svůj progres ve skupině studentů. Bylo by tedy z mé strany ideální udržovat po celou dobu spolupráce pravidelný kontakt až již v podobě distančních schůzek (Teams apod.), anebo v podobě e-mailové korespondence. Co se mých dalších dispozic týče, zdejší univerzita údajně disponuje akademickou licencí pro CST a HFSS, takže přístup k simulačnímu softwaru bych měl mít zajištěn i zapomoci lokálních licenčních serverů.  
>   
> Kdybyste měl jakékoli dotazy, neváhejte se mě prosím na cokoli doptat. Budu se těšit na Vaši odpověď.  
>   
> S pozdravem a přáním pěkného zbytku týdne  
> Martin Šimák