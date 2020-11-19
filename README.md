# SchneeflockenFangen
## ~avatar avatar @unplugged
Du möchtest Schneeflocken fangen aber es schneit gerade nicht? Virtuell mit dem @boardname@ kein Problem!


## ~ @unplugged
In diesem Tutorial lernst du ein Spiel für den @boardname@ mithilfe der Spielbausteine zu programmieren. <br>
Diese Spielbausteine beziehen sich auf sogenannte Sprites, welche Objekte mit verschiedenen Eigenschaften (wie einer Richtung, ein Positionswert uvm.) sind. <br>
Darüber hinaus können diese Sprites auch "miteinander interagieren", so wird bspw. erkannt, wenn sich  Sprites berühren.

## ~ @unplugged
In diesem Spiel sollen Schneeflocke vom oberen Bildschirmrand herab fallen. Diese sollen am unteren Bildschirmrand aufgefangen werden. Wird eine Schneeflocke gefangen erhöt sich der Spielstand um 1. <br>
Wird sie nicht gefangen und erreicht den unteren Bildschirmrand soll eine neue von einer zufälligen Position von oben herab fallen und von deinem Rundenzähler eins abgezogen werden. <br>
Ist der Rundentimer abgelaufen soll das Spiel enden. <br>

![SchneeflockenFangen] (https://github.com/r00b1nh00d/schneeflockenfangen/blob/master/SchneeflockenFangen.gif?raw=true)

## Schritt 1 Erstelle eine Spielfigur zum fangen der Schneeflocken
Beginne damit eine Spielfigur zum fangen der Schneeflocken zu erstellen indem du eine ``||Variables: Variable||`` namens ``||Variables: Fänger||`` festlegst. <br>
Mit dem Block ``||Variables: Setze Fänger auf||`` legst du fest, was auf dieser Variablen gespeichert wird, dies kann beim Start des @boardname@ erfolgen, daher kommt dieser Block in den Block ``||basic: Beim Start||``. <br>
Auf unsere Variable ``||Variables:Fänger||`` Speichern wir einen Sprite den Block ``||game: erzeuge Sprite an Position x y ||`` finest du in der Gruppe ``||game:Spiele||``. <br>
Schiebe diesen Block in den Block ``||Variables: Setze Fänger auf||``. Jetzt sollte im Simulator eine LED in der Mitte der Anzeige des @boardname@ leuchten, eben an der Position x=2 und y=2. Ändere nun den Y-Wert auf 4 um den Sprite am unteren Bildschirmrand erscheinen zu lassen.

```blocks
let Fänger  = game.createSprite(2, 4)
```

## Schritt 2 Den Fänger bewegen
Den Fänger können wir mit den Knöpfen A und B nach links bzw. rechts bewegen. <br>
Dazu benötigst du aus dem Bereich ``||Input: Eingabe||`` den Block ``||Input: Wenn Knopf A gedrückt||``, in diesen kannst du den Block ``||game: Sprite bewege um 1 ||`` hineinschieben. <br>
Wird nun der Knopf A gedrückt sollte sich dein Fänger um eins nach rechts bewegen. Um den Fänger nach links zu bewegen kannst du ihn um -1 bewegen. <br>
Um den Fänger nach rechts zu bewegen nehmen wir den B-Knopf, du kannst dir dafür die Blöcke wieder aus dem Befehlsbereich suchen oder mit einem Rechtsklick auf den schon erstellten Block ``||Input: Wenn Knopf A gedrückt||`` diesen Duplizieren. <br>
Jetzt kannst du im Simulator testen, ob sich der Fänger mit dem A-Knopf nach links und mit dem B-Knopf nach rechts bewegt.


```blocks
let Fänger  = game.createSprite(2, 4)
input.onButtonPressed(Button.A, function () {
    Fänger.move(-1)
})
input.onButtonPressed(Button.B, function () {
    Fänger.move(1)
})
```

## Schritt 3 die Schneflocke erscheinen lassen
Im Prinzip erstellst du die Schneeflocke wie den Fänger.
Um die Schneeflocke beim Start an einer zufälligen Stelle am oberen Bildschirmrand erscheinen zu lassen, kannst du in den Block  ``||game: erzeuge Sprite an Position x y ||`` den Y-Wert auf 0 stellen. Für den X-Wert nimmst du aus dem Bereich ``||math: Matematik||`` den Block ``||math: wähle eine zufällige Zahl||``. <br>
Diese zufällige Zahle kann zwischen 0 und 4 liegen, da dies die Werte der Koordinaten am @boardname@ sind. 

```blocks
let Fänger  = game.createSprite(2, 4)
let Schneeflocke = game.createSprite(randint(0, 4), 0)
```

## Schritt 4 die Schneeflocke bewegen
Die Schneeflocken sollen dauerhaft von oben nach unten fallen, schiebe daher einen Block ``||game: Sprite bewege um 1 ||`` und ändern ``||Variables: Sprite||`` zu ``||Variables: Schneeflocke||`` jetzt wird sich die Schneeflocke sehr schnell bewegen. <br>
Um Diese Bewegung mit den Augen zu sehen musst du den @boardname@ kurz ``||basic:pausieren||`` lassen. Schiebe dazu diesen Block aus dem Bereich ``||basic:Grundlagen||`` ebenfalls in die Dauerhaftschleife. <br>
Du wirst nun feststellen, dass sich deine Schneeflocke von links nach rechts bewegt. Damit sie sich von oben nach unten bewegt musst du ``||basic: Beim Start||`` noch die Richtung mit dem Block ``||game: Sprite drehe rechts um ||`` ändern.

```blocks
let Schneeflocke = game.createSprite(randint(0, 4), 0)
Schneeflocke.turn(Direction.Right, 90)
basic.forever(function () {
    basic.pause(250)
    Schneeflocke.move(1)
})
```

## Schritt 5 Der Spielablauf
Nachdem sich jetzt unsere beiden Elemente bewegen lassen, geht es jetzt darum Die Spielregeln bzw. den Spielablauf zu programmieren. <br>
Dazu nutzen wir jeweils eine ``||logic:wenn dann||`` - Bedingung aus dem Bereich  ``||logic:Logik||``. <br>
So soll unsere erst Regel sein: Wenn die Schneeflocke den Fänger berührt, dann soll der Spielstand um 1 geändert werden. <br>
Zweite Regel: Wenn die Schneeflocke am unteren Bildschirmrand ist also ihre Y-Position gleich 4 ist, dann soll der Runden counter um eins verringert werden und anschließend die Schneeflocke wieder am oberen Bildschirmrand an einer beliebigen Position erscheinen. <br>
Dritte Regel: Wenn der Runden counter kleiner 1 ist, dann soll das Spiel beendet werden. <br>

Hinweis: Um mit den Runden zu arbeiten, muss du ``||basic: Beim Start||`` noch die Variable ``||variables: Runden||`` erstllen und auf einen Wert z.B. 20 setzen.

```blocks

input.onButtonPressed(Button.A, function () {
    Fänger.move(-1)
})
input.onButtonPressed(Button.B, function () {
    Fänger.move(1)
})

let Fänger = game.createSprite(2, 4)
let Schneeflocke = game.createSprite(randint(0, 4), 0)
Schneeflocke.turn(Direction.Right, 90)
let Runden = 20
basic.forever(function () {
    basic.pause(250)
    Schneeflocke.move(1)
    if (Schneeflocke.isTouching(Fänger)) {
        game.addScore(1)
    }
    if (Schneeflocke.get(LedSpriteProperty.Y) == 4) {
        Runden += -1
        Schneeflocke.set(LedSpriteProperty.X, randint(0, 4))
        Schneeflocke.set(LedSpriteProperty.Y, 0)
    }
    if (Runden < 1) {
        game.gameOver()
    }
})

```


## Als Tutorial verwenden

Dieses Repository kann als **Tutorial** für MakeCode verwenden.

öffne dazu den Link: [https://makecode.calliope.cc/#tutorial:https://github.com/r00b1nh00d/schneeflockenfangen]



#### Metadaten (verwendet für Suche, Rendering)

* for PXT/calliopemini
<script src="https://makecode.com/gh-pages-embed.js"></script><script>makeCodeRender("{{ site.makecode.home_url }}", "{{ site.github.owner_name }}/{{ site.github.repository_name }}");</script>
