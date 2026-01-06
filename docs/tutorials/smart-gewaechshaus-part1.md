```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```

# Alarmanlage

## Willkommen!
In dem ersten Teil des Tutorials entwickelst du ein überwachungssystem für ein 
Basilikum-Gewächshaus, welches bei zu wenig Sonnenlicht automatisch die Beleuchtung einschaltet.

## Wichtig! @showdialog
Stecke den **Gassensor und Sonnenlichtsensor** zusammen mit einem **Hub** am **Port J0** an. 

Stecke den **LED-Strip** am Port **J1** an.

![Tutorialbild](https://github.com/kosta11111/pxt-smart-alarm-tutorial/blob/master/docs/imgs/IoTCube.png?raw=true) 

Falls du **Probleme** beim Tutorial hast, kannst du beim Klicken auf der **Glühbirne** sehen,
wie der Code ausschauen soll. 
![Tutorialbild](https://github.com/kosta11111/pxt-smart-alarm-tutorial/blob/master/docs/imgs/Gl%C3%BChbirne.png?raw=true)

Klicke auf die **Codeschnipsel** im Text, um direkt zu den **Codeblöcken** zu kommen!
![Tutorialbild](https://github.com/kosta11111/pxt-smart-alarm-tutorial/blob/master/docs/imgs/Codeschnipsel.png?raw=true)

## Schritt 1

Zuerst müssen wir **Sensoren** und **Aktoren** deklarieren. In anderen Worten
richten wir diese ins Programm ein, um mit den Sensoren und Aktoren arbeiten zu können

* **Ziehe** den ``||smartfeldSensoren:init Sonnenlicht sensor||`` Codeblock in den **Start**

* Mache das gleiche mit den ``||smartfeldSensoren:innit Gassensor||`` und dem  ``||smartfeldSensoren:setze als 400ppm Kalibrationswert||`` Codeblock

* **Ziehe** den ``||neopixel:setze strip||`` ebenfalls in den **Startblock**

* **Ändere** die Pixel auf **16**



```blocks 
smartfeldSensoren.initSunlight()
smartfeldSensoren.gas_init()
smartfeldSensoren.setGasCalibration()
let strip = neopixel.create(DigitalPin.P0, 16, NeoPixelMode.RGB)
```

## Schritt 2

Jetzt teilen wir dem Programm mit einer Wenn-Abfrage

* **Ziehe** den ``||logic:wenn wahr dann...ansonsten||`` Block in den **Dauerhaft-Codeblock**

* **Ziehe** den ``||logic:0 < 0||`` Codeblock ins **Wahr-Feld** und ändere das **<** zu einem **> mit Unterstrich**

* **Schreibe** in die linke null die Zahl **1000**

* **Schreibe** in die linke Null eine **10**

* **Ziehe** in die rechte Null den ``||smartfeldSensoren:gib sichtbares Licht||`` Block rein

```blocks
basic.forever(function () {
    if (1000 >= smartfeldSensoren.getHalfWord_Visible()) {
        
    } else {
        
    }
})
```

## Schritt 3

Jetzt Programmieren wir, dass bei zu wenig Licht (unter 1000 LUX) der LED-Strip
leuchtet, und sonst ausgeschalten ist.

* **Ziehe** in den **Wenn-Block** den ``||neopixel:strip zeige Farbe||`` und den ``||neopixel:strip anzeigen||`` rein
 
* **Versichere**, dass die anzezeigt Farbe **weiß** ist

* **Ziehe** in das **Ansonsten-Feld** den ``||neopixel:strip ausschalten||`` und den ``||neopixel:strip anzeigen||`` rein



```blocks
basic.forever(function () {
    if (1000 >= smartfeldSensoren.getHalfWord_Visible()) {
        strip.showColor(neopixel.colors(NeoPixelColors.White))
        strip.show()
    } else {
        strip.clear()
        strip.show()
    }
})
```

## Glückwunsch🤩

Du hast den ersten Teil des Tutorials erfolgreich absolviert!🙌

Lade den Code auf dein IoT-Cube herunter und leuchte mit deiner Handytaschenlampe
auf den Sonnenlichtsensor. Leuchtet der LED-Strip?

Klicke [Hier](https://makecode.microbit.org/#tutorial:github:kosta11111/pxt-smart-alarm-tutorial/docs/tutorials/smart-alarm-part2),
um den zweiten Part des Tutorials zu starten!