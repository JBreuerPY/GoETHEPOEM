```python
"""
Die folgenden Zellen enthalten Beispielausgaben von GoETHEPOEM
- Zelle 1: Importieren der Module
- Zelle 2: Beispiel zur Anwendung von GoETHEPOEM
- Zelle 3: Einen "Gedichtband" erstellen (ACHTUNG: wenn Zelle gestartet wird, wird die Beispielausgabe überschrieben)
- Zelle 4: Analyse
- Zelle 5: Poetische und grammatikalische Korrektheit
"""
```


```python
###################################
# Zelle 1: Importieren der Module #
###################################

# GoETHEPOEM importieren
from GoETHEPOEM import *
# Templates importieren
from Templates import *
# choice
from random import choice, sample
# date
from datetime import date
# re
import re
```


```python
#################################################
# Zelle 2: Beispiel zur Anwendung von GoTHEPOEM #
#################################################

# Man kann einen Feed eingeben, auf dessen Basis ein Titel und ein Gedicht generiert werden
print(interactive_poem())
```

    Eingabe: Wissenschaft
    ---------------------------------
    
    Wissenschaft
    
    Oft wie eine Sternwarte so bunt,
    beim ausgluehen wie das heiße Pfund.
    Wieder wie eine Windung so schlau,
    die immer vergroessern muss so grau.
    


```python
##########################################
# Zelle 3: Einen "Gedichtband" erstellen #
##########################################

# Datum
datum = str(date.today()).split("-")
datumformat = datum[2]+"."+datum[1]+"."+datum[0]
# Liste mit Synonymen von Wörtern, die in relationdict enthalten sind
ot_synonyms = list(set([item for sublist in [[synonym for synonym in ot_dict[word] if synonym not in relationdict] for word in [word for word in ot_dict if word in relationdict]] for item in sublist]))
# Liste unbekannter Wörter (Weder in relationdict, noch als Synonym bekannt)
ot_unknowns = [word for word in sample(list(ot_dict.keys()),3000) if word not in ot_synonyms and word not in relationdict]
# Liste mit 30 zufällig gewählten Wörtern aus dem Korpus (10 aus relationdict, 10 aus Synonymliste, 
# 10 aus Liste unbekannter Wörter)
randomtitles = sample(list(relationdict.keys()), 10)+sample(ot_synonyms, 10)+sample(ot_unknowns, 10)
# Datei erstellen
poembook = open("Gedichtband.txt","w",encoding=("UTF-8"))
# Titel des "Buchs"
poembook.write("----------------------------------\nGedichte\n----------------------------------\n\n")
# Zusatztitel und Datum
poembook.write("Eine Zusammenstellung von GoETHEPOEM\n\nerstellt am "+datumformat+"\n\n\n")
# Inhalt
poembook.write("----------------------------------\nInhalt\n----------------------------------\n\n")
for num, title in enumerate(randomtitles):
    poembook.write(str(num+1)+")\t"+title+"\n")
poembook.write("----------------------------------\n\n")
# Gedichte
for num, title in enumerate(randomtitles):
    try:
        poem = goethepoem(title)
        poembook.write("["+str(num+1)+"]\n\n"+poem+"\n\n")
    except:
        print(title)
# Datei schließen
poembook.close()
```


```python
####################
# Zelle 4: Analyse #
####################

# Datei "Gedichtband.txt" öffnen
poembook_open = open("Gedichtband.txt",encoding=("UTF-8"))
# Datei einlesen
poembook = poembook_open.read()
# Datei schließen
poembook_open.close()


# Liste mit einzelnen Gedichten            
poems = re.findall(r"\[\d+\]\n\n.+\n\n.+\n.+\n.+\n.+", poembook)
# Liste mit gegebenen Feeds
feeds = [feed.split()[1] for feed in re.findall(r"\d+\)\t.+", poembook)]
# Liste mit generierten Gedichttiteln
titles = [poem.split("\n")[2] for poem in poems]
# Liste aller Strophen: Jeweils mit Listen der einzelnen Verse
strophes = [poem.split("\n")[-4:] for poem in poems]
# Liste der Reimwörter eines Gedichts
rhymes = [[strip_punctuation(vers.split()[-1]) for vers in verses] for verses in strophes]
# Liste aller generierten Wörter für jedes Gedicht
generated_words = [[strip_punctuation(word) for word in strophe if strip_punctuation(word).lower() not in template_words and strip_punctuation(word).lower() not in list_words and strip_punctuation(word) not in list_words and strip_punctuation(word).lower() not in ["der","die","das","ein","eine","er","sie","es"]] for strophe in [" ".join(poem.split("\n")[4:]).split() for poem in poems]]
# Anzahl der Silben jedes einzelnen Verses für jedes Gedicht
syllables = [[get_syllables(vers) for vers in verses] for verses in strophes]

# Gebe jedes Gedicht, sowie die Analyseergebnisse aus
for index in range(len(poems)):
    print(poems[index]+"\n")
    print("Feed:", feeds[index])
    print("Titel:", titles[index])
    print("Reimpaare:",rhymes[index])
    print("Alle generierten Wörter:",generated_words[index])
    print("Anzahl der Silben:",syllables[index])
    print("\n-----------------------------------------------------\n")
```

    [1]
    
    Kragen
    
    Auch das duenne Leder soll gestalten,
    wie ein Stiefel oder auch das Falten.
    Gelegentlich wie ein Priester so gut,
    wie eine Kraft oder die enge Wut.
    
    Feed: Kragen
    Titel: Kragen
    Reimpaare: ['gestalten', 'Falten', 'gut', 'Wut']
    Alle generierten Wörter: ['duenne', 'Leder', 'gestalten', 'Stiefel', 'Falten', 'Priester', 'gut', 'Kraft', 'enge', 'Wut']
    Anzahl der Silben: [10, 10, 10, 10]
    
    -----------------------------------------------------
    
    [2]
    
    Schwangerschaftsstreifen
    
    Wieder darf ein Ergebener dozieren,
    wenn er zu dick ist um zu decodieren.
    Selten wie eine Wesenheit so bauschig,
    die gelegentlich schreiben soll so lauschig.
    
    Feed: Schwangerschaftsstreifen
    Titel: Schwangerschaftsstreifen
    Reimpaare: ['dozieren', 'decodieren', 'bauschig', 'lauschig']
    Alle generierten Wörter: ['Ergebener', 'dozieren', 'dick', 'decodieren', 'Wesenheit', 'bauschig', 'lauschig']
    Anzahl der Silben: [11, 11, 11, 11]
    
    -----------------------------------------------------
    
    [3]
    
    wichtig
    
    Auch die weiße Ordnung soll suchen,
    wie ein Korn oder auch der Kuchen.
    Immer wie eine Scheibe so fein,
    wie ein Teig oder der graue Stein.
    
    Feed: wichtig
    Titel: wichtig
    Reimpaare: ['suchen', 'Kuchen', 'fein', 'Stein']
    Alle generierten Wörter: ['weiße', 'Ordnung', 'suchen', 'Korn', 'Kuchen', 'Scheibe', 'fein', 'Teig', 'graue', 'Stein']
    Anzahl der Silben: [9, 9, 9, 9]
    
    -----------------------------------------------------
    
    [4]
    
    Ausgiesser
    
    Selten kann eine Sinnesart bruellen,
    wenn sie zu brennend ist um zu fuellen.
    Auch der warme Schuessel möchte trinken,
    wie ein Geschirr oder auch der Zinken.
    
    Feed: Ausgiesser
    Titel: Ausgiesser
    Reimpaare: ['bruellen', 'fuellen', 'trinken', 'Zinken']
    Alle generierten Wörter: ['Sinnesart', 'bruellen', 'brennend', 'fuellen', 'warme', 'Schuessel', 'trinken', 'Geschirr', 'Zinken']
    Anzahl der Silben: [10, 10, 10, 10]
    
    -----------------------------------------------------
    
    [5]
    
    abspringen
    
    Oft will ein Entertainment musizieren,
    wenn es zu oben ist um zu pulsieren.
    Selten so hoch wie die Eulenspiegelei,
    die anschubsen muss wie eine Schwaermerei.
    
    Feed: abspringen
    Titel: abspringen
    Reimpaare: ['musizieren', 'pulsieren', 'Eulenspiegelei', 'Schwaermerei']
    Alle generierten Wörter: ['Entertainment', 'musizieren', 'oben', 'pulsieren', 'hoch', 'Eulenspiegelei', 'anschubsen', 'Schwaermerei']
    Anzahl der Silben: [11, 11, 11, 11]
    
    -----------------------------------------------------
    
    [6]
    
    Saal
    
    Oft egal ob Moebelstueck oder Macht,
    denn ein Saal ist entscheidend wie die Pracht.
    Gelegentlich wie ein Glaube so klein,
    wie ein Palais oder der weite Stein.
    
    Feed: Saal
    Titel: Saal
    Reimpaare: ['Macht', 'Pracht', 'klein', 'Stein']
    Alle generierten Wörter: ['Moebelstueck', 'Macht', 'Saal', 'entscheidend', 'Pracht', 'Glaube', 'klein', 'Palais', 'weite', 'Stein']
    Anzahl der Silben: [10, 10, 10, 10]
    
    -----------------------------------------------------
    
    [7]
    
    Heuboden
    
    Auch die hinaufe Wand muss fallen,
    wie ein Grad oder auch das Krallen.
    Manchmal so pompoes wie die Spritze,
    die klettern darf wie eine Spitze.
    
    Feed: Heuboden
    Titel: Heuboden
    Reimpaare: ['fallen', 'Krallen', 'Spritze', 'Spitze']
    Alle generierten Wörter: ['hinaufe', 'Wand', 'fallen', 'Grad', 'Krallen', 'pompoes', 'Spritze', 'klettern', 'Spitze']
    Anzahl der Silben: [9, 9, 9, 9]
    
    -----------------------------------------------------
    
    [8]
    
    Elektrizitaet
    
    Am Mittag so sueß wie das Foen,
    und am Tag wie ein Staub so schoen.
    Oft wie eine Natur so grell,
    die wieder ziehen darf so hell.
    
    Feed: Elektrizitaet
    Titel: Elektrizitaet
    Reimpaare: ['Foen', 'schoen', 'grell', 'hell']
    Alle generierten Wörter: ['sueß', 'Foen', 'Staub', 'schoen', 'Natur', 'grell', 'ziehen', 'hell']
    Anzahl der Silben: [8, 8, 8, 8]
    
    -----------------------------------------------------
    
    [9]
    
    Wochenende
    
    Selten egal ob Tafelwasser oder Yacht,
    denn ein Wochenende ist modern wie die Jacht.
    Gelegentlich wie ein Badeschaum so keimfrei,
    wie ein Rechner oder die schwere Raserei.
    
    Feed: Wochenende
    Titel: Wochenende
    Reimpaare: ['Yacht', 'Jacht', 'keimfrei', 'Raserei']
    Alle generierten Wörter: ['Tafelwasser', 'Yacht', 'Wochenende', 'modern', 'Jacht', 'Badeschaum', 'keimfrei', 'Rechner', 'schwere', 'Raserei']
    Anzahl der Silben: [12, 12, 12, 12]
    
    -----------------------------------------------------
    
    [10]
    
    Gerippe
    
    Am Mittag so erbittert wie der Knochen,
    und am Abend wie ein Grab so gebrochen.
    Oft wie ein Erdbewohner so verstorben,
    der immer spazieren kann so gestorben.
    
    Feed: Gerippe
    Titel: Gerippe
    Reimpaare: ['Knochen', 'gebrochen', 'verstorben', 'gestorben']
    Alle generierten Wörter: ['erbittert', 'Knochen', 'Grab', 'gebrochen', 'Erdbewohner', 'verstorben', 'spazieren', 'gestorben']
    Anzahl der Silben: [11, 11, 11, 11]
    
    -----------------------------------------------------
    
    [11]
    
    Konditorei und Konfiserie
    
    Selten egal ob Esswaren oder Brei,
    denn eine Konditorei ist sueßlich wie die Kuchenbaeckerei.
    Delikat und zuckerig wie ein Kuchen,
    der erhalten möchte was wir versuchen.
    
    Feed: Konfiserie
    Titel: Konditorei und Konfiserie
    Reimpaare: ['Brei', 'Kuchenbaeckerei', 'Kuchen', 'versuchen']
    Alle generierten Wörter: ['Esswaren', 'Brei', 'Konditorei', 'sueßlich', 'Kuchenbaeckerei', 'Delikat', 'zuckerig', 'Kuchen', 'versuchen']
    Anzahl der Silben: [11, 17, 11, 11]
    
    -----------------------------------------------------
    
    [12]
    
    Transport und Zufuehrung
    
    Wieder wie ein Maultier so klein,
    beim fliegen wie der arme Schein.
    Manchmal wie ein Lappen so blau,
    der selten lagern muss so grau.
    
    Feed: Zufuehrung
    Titel: Transport und Zufuehrung
    Reimpaare: ['klein', 'Schein', 'blau', 'grau']
    Alle generierten Wörter: ['Maultier', 'klein', 'fliegen', 'arme', 'Schein', 'Lappen', 'blau', 'lagern', 'grau']
    Anzahl der Silben: [8, 8, 8, 8]
    
    -----------------------------------------------------
    
    [13]
    
    Welle und Grundsee
    
    Oft wie ein Seepferdchen so schoen,
    beim wachsen wie das blonde Foen.
    Und plötzlich kennt man einen Teich,
    der ist wie ein Friseur so weich.
    
    Feed: Grundsee
    Titel: Welle und Grundsee
    Reimpaare: ['schoen', 'Foen', 'Teich', 'weich']
    Alle generierten Wörter: ['Seepferdchen', 'schoen', 'wachsen', 'blonde', 'Foen', 'einen', 'Teich', 'Friseur', 'weich']
    Anzahl der Silben: [8, 8, 8, 8]
    
    -----------------------------------------------------
    
    [14]
    
    unabhaengig und eigenstaendig
    
    Am Morgen so teuer wie der TV,
    und am Tag wie ein Stueck so lau.
    Manchmal so schoen wie die Mama,
    die lernen darf wie ein Drama.
    
    Feed: eigenstaendig
    Titel: unabhaengig und eigenstaendig
    Reimpaare: ['TV', 'lau', 'Mama', 'Drama']
    Alle generierten Wörter: ['teuer', 'TV', 'Stueck', 'lau', 'schoen', 'Mama', 'lernen', 'Drama']
    Anzahl der Silben: [8, 8, 8, 8]
    
    -----------------------------------------------------
    
    [15]
    
    Absatz und Verkaufszahlen
    
    Praezis und schluessig wie der Magen,
    der bekommen soll was wir tragen.
    Denn contra ist es wenn wir leiten,
    wie das warme Wasser beim reiten.
    
    Feed: Verkaufszahlen
    Titel: Absatz und Verkaufszahlen
    Reimpaare: ['Magen', 'tragen', 'leiten', 'reiten']
    Alle generierten Wörter: ['Praezis', 'schluessig', 'Magen', 'tragen', 'contra', 'leiten', 'warme', 'Wasser', 'reiten']
    Anzahl der Silben: [9, 9, 9, 9]
    
    -----------------------------------------------------
    
    [16]
    
    reißen und mitreißen
    
    Wieder wie eine Pappe so bunt,
    beim fressen wie der sichere Hunt.
    Und plötzlich entfernt man einen Kamm,
    der ist wie eine Socke so stramm.
    
    Feed: mitreißen
    Titel: reißen und mitreißen
    Reimpaare: ['bunt', 'Hunt', 'Kamm', 'stramm']
    Alle generierten Wörter: ['Pappe', 'bunt', 'fressen', 'sichere', 'Hunt', 'einen', 'Kamm', 'Socke', 'stramm']
    Anzahl der Silben: [9, 9, 9, 9]
    
    -----------------------------------------------------
    
    [17]
    
    hell und anstellig
    
    Manchmal wie ein Wesen so grau,
    das oft zuziehen will so blau.
    Und plötzlich versenkt man ein Meer,
    das ist wie ein Moebel so leer.
    
    Feed: anstellig
    Titel: hell und anstellig
    Reimpaare: ['grau', 'blau', 'Meer', 'leer']
    Alle generierten Wörter: ['Wesen', 'grau', 'zuziehen', 'blau', 'Moebel', 'leer']
    Anzahl der Silben: [8, 8, 8, 8]
    
    -----------------------------------------------------
    
    [18]
    
    erfunden und erdichtet
    
    Auch das runde Holz möchte jagen,
    wie ein Berg oder auch der Wagen.
    Oft so kreisrund wie die Schwindelei,
    die kugeln möchte wie ein Windei.
    
    Feed: erdichtet
    Titel: erfunden und erdichtet
    Reimpaare: ['jagen', 'Wagen', 'Schwindelei', 'Windei']
    Alle generierten Wörter: ['runde', 'Holz', 'jagen', 'Berg', 'Wagen', 'kreisrund', 'Schwindelei', 'kugeln', 'Windei']
    Anzahl der Silben: [9, 9, 9, 9]
    
    -----------------------------------------------------
    
    [19]
    
    Altersheim und Seniorenheim
    
    Gelegentlich wie ein Stock so gestorben,
    der immer beriechen soll so verstorben.
    Auch das gemache Krustentier muss sieden,
    wie ein Muster am Mittag so verschieden.
    
    Feed: Seniorenheim
    Titel: Altersheim und Seniorenheim
    Reimpaare: ['gestorben', 'verstorben', 'sieden', 'verschieden']
    Alle generierten Wörter: ['Stock', 'gestorben', 'beriechen', 'verstorben', 'gemache', 'Krustentier', 'sieden', 'Muster', 'verschieden']
    Anzahl der Silben: [11, 11, 11, 11]
    
    -----------------------------------------------------
    
    [20]
    
    Scheide und Schwertscheide
    
    Gelegentlich will ein Tod brechen,
    wenn er zu hart ist um zu stechen.
    Immer wie ein Mann so verbissen,
    der oft rangeln kann so gerissen.
    
    Feed: Schwertscheide
    Titel: Scheide und Schwertscheide
    Reimpaare: ['brechen', 'stechen', 'verbissen', 'gerissen']
    Alle generierten Wörter: ['Tod', 'brechen', 'hart', 'stechen', 'Mann', 'verbissen', 'rangeln', 'gerissen']
    Anzahl der Silben: [9, 9, 9, 9]
    
    -----------------------------------------------------
    
    [21]
    
    Lackaffe oder Reichseinigungskriege
    
    Gelegentlich wie ein Gaertner so schal,
    beim verknuepfen wie das ueppige Tal.
    Auch die liebe Anstellung muss pochen,
    wie ein Loch am Mittag so gebrochen.
    
    Feed: Reichseinigungskriege
    Titel: Lackaffe oder Reichseinigungskriege
    Reimpaare: ['schal', 'Tal', 'pochen', 'gebrochen']
    Alle generierten Wörter: ['Gaertner', 'schal', 'verknuepfen', 'ueppige', 'Tal', 'liebe', 'Anstellung', 'pochen', 'Loch', 'gebrochen']
    Anzahl der Silben: [10, 10, 10, 10]
    
    -----------------------------------------------------
    
    [22]
    
    Duftlampe oder Kernfunktion
    
    Gelegentlich wie eine Waerme so zwei,
    beim aufsetzen wie die rote Schwaermerei.
    Wieder so heiß wie die Gefuehlsduselei,
    die auseinandernehmen will wie ein Hai.
    
    Feed: Kernfunktion
    Titel: Duftlampe oder Kernfunktion
    Reimpaare: ['zwei', 'Schwaermerei', 'Gefuehlsduselei', 'Hai']
    Alle generierten Wörter: ['Waerme', 'zwei', 'aufsetzen', 'rote', 'Schwaermerei', 'heiß', 'Gefuehlsduselei', 'auseinandernehmen', 'Hai']
    Anzahl der Silben: [11, 11, 11, 11]
    
    -----------------------------------------------------
    
    [23]
    
    Holzklopfer oder Sitzbein
    
    Gelegentlich egal ob Pein oder Leid,
    denn ein Holzklopfer ist strikt wie die Zuarbeit.
    Denn hautfarben ist es wenn wir sanieren,
    wie das wilde Weh beim desinfizieren.
    
    Feed: Sitzbein
    Titel: Holzklopfer oder Sitzbein
    Reimpaare: ['Leid', 'Zuarbeit', 'sanieren', 'desinfizieren']
    Alle generierten Wörter: ['Pein', 'Leid', 'Holzklopfer', 'strikt', 'Zuarbeit', 'hautfarben', 'sanieren', 'wilde', 'Weh', 'desinfizieren']
    Anzahl der Silben: [11, 11, 11, 11]
    
    -----------------------------------------------------
    
    [24]
    
    aufruesten oder Serbien
    
    Am Mittag so großzuegig wie der Ger,
    und am Morgen wie ein Soldat so schwer.
    Immer so stoerend wie die Bewaffnung,
    die kaempfen kann wie eine Entwaffnung.
    
    Feed: Serbien
    Titel: aufruesten oder Serbien
    Reimpaare: ['Ger', 'schwer', 'Bewaffnung', 'Entwaffnung']
    Alle generierten Wörter: ['großzuegig', 'Ger', 'Soldat', 'schwer', 'stoerend', 'Bewaffnung', 'kaempfen', 'Entwaffnung']
    Anzahl der Silben: [10, 10, 10, 10]
    
    -----------------------------------------------------
    
    [25]
    
    Felsen oder Schwachpunkt
    
    Wieder wie ein Gestein so steil,
    beim springen wie das weiße Seil.
    Und plötzlich entfernt man ein Fell,
    das ist wie ein Tiger so schnell.
    
    Feed: Schwachpunkt
    Titel: Felsen oder Schwachpunkt
    Reimpaare: ['steil', 'Seil', 'Fell', 'schnell']
    Alle generierten Wörter: ['Gestein', 'steil', 'springen', 'weiße', 'Seil', 'Fell', 'Tiger', 'schnell']
    Anzahl der Silben: [8, 8, 8, 8]
    
    -----------------------------------------------------
    
    [26]
    
    Mountainbike oder Schießbudenfigur
    
    Immer egal ob Ammenmaerchen oder Schwindelei,
    denn ein Mountainbike ist rund wie die Schwaermerei.
    Auch das herzensgute Leibesuebungen darf pfeifen,
    wie eine Komplettbereifung oder auch der Reifen.
    
    Feed: Schießbudenfigur
    Titel: Mountainbike oder Schießbudenfigur
    Reimpaare: ['Schwindelei', 'Schwaermerei', 'pfeifen', 'Reifen']
    Alle generierten Wörter: ['Ammenmaerchen', 'Schwindelei', 'Mountainbike', 'rund', 'Schwaermerei', 'herzensgute', 'Leibesuebungen', 'pfeifen', 'Komplettbereifung', 'Reifen']
    Anzahl der Silben: [14, 14, 14, 14]
    
    -----------------------------------------------------
    
    [27]
    
    kahl oder Zahnriemen
    
    Manchmal möchte ein Outfit kaschieren,
    wenn es zu alt ist um zu ventilieren.
    Gelegentlich wie eine Kappe so rund,
    wie ein Gesicht oder der gegene Mund.
    
    Feed: Zahnriemen
    Titel: kahl oder Zahnriemen
    Reimpaare: ['kaschieren', 'ventilieren', 'rund', 'Mund']
    Alle generierten Wörter: ['Outfit', 'kaschieren', 'alt', 'ventilieren', 'Kappe', 'rund', 'Gesicht', 'gegene', 'Mund']
    Anzahl der Silben: [11, 11, 11, 11]
    
    -----------------------------------------------------
    
    [28]
    
    Großvater oder Webstoff
    
    Oft soll ein Ehepartner rauchen,
    wenn er zu schrill ist um zu schmauchen.
    Gelegentlich wie ein Hut so tot,
    wie ein Qualm oder der liebe Tod.
    
    Feed: Webstoff
    Titel: Großvater oder Webstoff
    Reimpaare: ['rauchen', 'schmauchen', 'tot', 'Tod']
    Alle generierten Wörter: ['Ehepartner', 'rauchen', 'schrill', 'schmauchen', 'Hut', 'tot', 'Qualm', 'liebe', 'Tod']
    Anzahl der Silben: [9, 9, 9, 9]
    
    -----------------------------------------------------
    
    [29]
    
    Drogen oder schlichten
    
    Geschaetzt und belegt wie ein Rauchen,
    das erhalten darf was wir schmauchen.
    Und plötzlich sieht man die Wuestenei,
    die ist wie ein Schmerz so humorfrei.
    
    Feed: schlichten
    Titel: Drogen oder schlichten
    Reimpaare: ['Rauchen', 'schmauchen', 'Wuestenei', 'humorfrei']
    Alle generierten Wörter: ['Geschaetzt', 'belegt', 'Rauchen', 'schmauchen', 'Wuestenei', 'Schmerz', 'humorfrei']
    Anzahl der Silben: [9, 9, 9, 9]
    
    -----------------------------------------------------
    
    [30]
    
    Gier oder Offizialprinzip
    
    Manchmal wie eine Arbeit so praktisch,
    die immer bezahlen darf so faktisch.
    Denn arm ist es wenn wir funktionieren,
    wie der reiche Traum beim finanzieren.
    
    Feed: Offizialprinzip
    Titel: Gier oder Offizialprinzip
    Reimpaare: ['praktisch', 'faktisch', 'funktionieren', 'finanzieren']
    Alle generierten Wörter: ['Arbeit', 'praktisch', 'bezahlen', 'faktisch', 'arm', 'funktionieren', 'reiche', 'Traum', 'finanzieren']
    Anzahl der Silben: [10, 10, 10, 10]
    
    -----------------------------------------------------
    
    


```python
#######################################################
# Zelle 5: Poetische und grammatische Korrektheit #
#######################################################

# Metrum
# In Gedicht [11]: Anzahl der Silben: [11, 17, 11, 11]
# Genauigkeitsrate
meter_accuracy = (30-1)/30*100
# Fehlerrate
meter_error = 1/30*100

# Unreime Reime
# in Gedicht [4]: ('Eulenspiegelei', 'Schwaermerei')
# in Gedicht [8]: ('Yacht', 'Jacht') und ('keimfrei', 'Raserei')
# In Gedicht [13]: ('Mama', 'Drama')
# In Gedicht [17]: ('Schwindelei', 'Windei')
# In Gedicht [27]: ('tot', 'Tod')
# In Gedicht [28]: ('Wuestenei', 'humorfrei')
# 7 unreime Reime, von insgesamt 60 Reimpaaren
# Genauigkeitsrate
rhyme_accuracy = (60-7)/60*100
# Fehlerrate
rhyme_error = 7/60*100

# Falsch klassifiziertes Genus
# in Gedicht [4]: der warme Schüssel
# in Gedicht [5]: die Eulenspiegelei
# in Gedicht [8] und [13]: das Foen
# in Gedicht [29]: die Wuestenei
# Liste aller Artikel (und Pronomen)
all_articles = re.findall(r"\b(er|es|sie|der|die|das|den|ein|eine[nr]?)\b",poembook)
# Genauigkeitsrate
articles_accuracy = (len(all_articles)-5)/len(all_articles)*100
# Fehlerrate
articles_error = 5/len(all_articles)*100

# Falscher POS-Tag
# in Gedicht [22]: Gelegentlich wie eine Waerme so zwei
# in Gedicht [26]: Auch das herzensgute Leibesuebungen darf pfeifen,
# in Gedicht [27]: wie ein Gesicht oder der gegene Mund.
# Liste aller generierten Wörter
all_generated_words = [item for sublist in generated_words for item in sublist]
# Genauigkeitsrate
generated_words_accuracy = (len(all_generated_words)-3)/len(all_generated_words)*100
# Fehlerrate
generated_words_error = 3/len(all_generated_words)*100


# Evaluierung ausgeben:
print("Einheitliches Metrum:",meter_accuracy,"\nNicht einheitliches Metrum:",meter_error,"\n")
print("Reine Reime:",rhyme_accuracy,"\nUnreine Reime:",rhyme_error,"\n")
print("Korrekt klassifizierte Artikel:",articles_accuracy,"\nFalsch klassifizierte Artikel:",articles_error,"\n")
print("Generierte Wörter mit richtigem POS-Tag:",generated_words_accuracy,"\nGenerierte Wörter mit richtigem POS-Tag:",generated_words_error,"\n")
```

    Einheitliches Metrum: 96.66666666666667 
    Nicht einheitliches Metrum: 3.3333333333333335 
    
    Reine Reime: 88.33333333333333 
    Unreine Reime: 11.666666666666666 
    
    Korrekt klassifizierte Artikel: 96.5034965034965 
    Falsch klassifizierte Artikel: 3.4965034965034967 
    
    Generierte Wörter mit richtigem POS-Tag: 98.85931558935361 
    Generierte Wörter mit richtigem POS-Tag: 1.1406844106463878 
    
    


```python

```
