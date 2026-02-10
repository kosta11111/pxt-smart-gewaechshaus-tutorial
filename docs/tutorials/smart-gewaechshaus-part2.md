```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
neopixel=github:microsoft/pxt-neopixel#v0.7.6
```

# Überwachungssystem-Gewächshaus

## Willkommen!
In dem zweiten Teil des Tutorials verbindest du dein Überwachungssystem mit dem dazugehörigen
Dashboard in der Claviscloud.

## Wichtig! @showdialog

**Schließe das Fenster des vorherigen Tutorials**, um beim Herunterladen des Codes Fehler zu vermeiden!

## Schritt 1

* **Ziehe** den ``||basic:beim Start||`` Block ins Programm.

* **Ziehe** den ``||IoTCube:LoRa Netzwerk-Verbindung||`` Block in den ``||basic:beim Start||``
Block **rein**.

* **Ziehe** danach den ``||loops:während||`` Block **darunter** rein 

* **Füge** in das **Falsch-Feld** den ``||logic:nicht||`` Codeblock ein

* In den ``||logic:nicht||`` Codeblock kommt der ``||IoTCube:Gerätstatus-Bit||`` Block

* **Ändere** den ``||IoTCube:Gerätstatus-Bit||`` von **Initialisieren** auf **Verbunden**

* **Ziehe** mit dem ``||basic:zeige Symbol||`` ein X in die **Während-Schleife**

* **Wiederhole** den Schritt unter des **Während-Codeblocks** mit einem Haken



```blocks 
let spaeterSenden = false
let msBeiLetztemSenden = 0
IoTCube.LoRa_Join(
eBool.enable,
eBool.enable,
10,
8
)
while (!(IoTCube.getStatus(eSTATUS_MASK.JOINED))) {
    basic.showIcon(IconNames.No)
}
basic.showIcon(IconNames.Yes)
smartfeldSensoren.initSunlight()
let strip = neopixel.create(DigitalPin.P0, 16, NeoPixelMode.RGB)
```

## Schritt 2

Wir wollen jede 20. Sekunde dem Dashboard die aktuelle Lux (Lichteinheit) schicken, die der
Sonnenlichtsensor misst

* **Ziehe** den ``||loops:alle 500 ms||`` Block in die leere Fläche

* **Ändere** die **500 ms** zu **20000 ms**

* **Ziehe** den ``||functions:Aufruf sendeDaten||`` Codeblock in die ``||loops:alle 20000 ms||`` Schleife

* **Ziehe** den ``||smartfeldSensoren:gib sichtbares Licht||`` Codeblock in die **0** des ``||functions:Aufrufs sendeDaten||``

```blocks
loops.everyInterval(20000, function () {
    sendeDaten(smartfeldSensoren.getHalfWord_Visible())
})
```
## Glückwunsch🤩

Du hast den zweiten Teil des Tutorials erfolgreich absolviert!🙌

Lade den Code auf dein IoT-Cube herunter und leuchte mit deiner Handytaschenlampe
auf den Sonnenlichtsensor. Leuchtet der LED-Strip?

Klicke [Hier]((https://iot.claviscloud.ch/dashboards/all) und verbinde dein Cube mit dem passenden
Dashboard.

```template
basic.forever(function () {
    if (1000 >= smartfeldSensoren.getHalfWord_Visible()) {
        strip.showColor(neopixel.colors(NeoPixelColors.White))
        strip.show()
    } else {
        strip.clear()
        strip.show()
    }
})

function sendeDaten (Lux: number) {
    if (control.millis() > msBeiLetztemSenden + 5000) {
        IoTCube.addUnsignedInteger(eIDs.ID_0, Lux)
        IoTCube.SendBufferSimple()
        spaeterSenden = false
        msBeiLetztemSenden = control.millis()
    } else {
        spaeterSenden = true
    }
}

loops.everyInterval(500, function () {
    if (spaeterSenden) {
        sendeDaten(smartfeldSensoren.getHalfWord_Visible())
    }
})

smartfeldSensoren.initSunlight()
let strip = neopixel.create(DigitalPin.P0, 16, NeoPixelMode.RGB)
```