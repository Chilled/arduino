# Impulsz�hler mit MQTT / HTTP Schnittstelle

Dies ist ein ganz simpler Sketch f�r einen Impulsz�hler basierend auf einem Wemos D1 Mini Mikrocontroller.

Der Impulsz�hler besitzt 6 digitale Z�hlereing�nge und kann diese gleichzeitig unabh�ngig voneiander verarbeiten. Das hei�t, es k�nnen bis zu 6 Z�hler, die Impulssignale ausgeben, gleichzeitig angeschlossen werden.

Urspr�nglich konzipiert ist diese L�sung zum Auslesen eines Gasz�hlers, dessen Z�hlwerk einen integrierten Dauermagneten besitzt. Auf diese Weise liefert dieser Z�hler pro 0.01 m� Gas einen magnetischen Impuls. Dieser Impuls l�sst sich �ber einen Reedkontakt, der bei Anliegen eines magnetischen Feldes einen Stromkreis schlie�t, abgreifen und �ber einen digitalen Eingang des Mikrocontrollers abgreifen.

Dar�ber hinaus k�nnen aber auch weitere Z�hler (Wasseruhren, Durchflusssensoren, Drehstromz�hler mit Impuls-LED, Ferraris-Z�hler) angeschlossen werden, lediglich der Reedkontakt m�sste hier gegen jeweils geeignete andere Peripherie ausgetauscht werden, um zum Beispiel optische in elektrische Impulse umzuwandeln.

## Features
- Besitzt 6 unabh�ngige Z�hlereing�nge (PINs D1 - D6)
-- Alle 6 Eing�nge werden in voneinander unabh�ngigen Routinen �ber Interrupts gez�hlt
-- Entprellzeit pro Eingang frei konfigurierbar (in Millisekunden)
- MQTT-Schnittstelle
-- Versendet die gez�hlten Impulse regelm��ig (alle 30 Sekunden) an einen MQTT-Broker
-- Entprellzeit pro Eingang kann per MQTT konfiguriert werden, indem zum Beispiel eine Nachricht auf den Topic /SmartHome/Sensor/Haustechnikraum/Impulszaehler/Zaehler_1/Entprellzeit mit dem Inhalt 1000 gesendet wird
- HTTP-Schnittstelle
-- �ber einen bereitgestellten Webserver k�nnen die gez�hlten Impulse per http abgefragt werden (http://<ipAdresseDesWMOS>)


## Ben�tigte Teile

- Wemos D1 Mini (ESP8266)
- USB-Netzteil
- Reedkontakt (f�r Gasz�hler)
- Dauermagnet (zum Testen)
- Kabel

## Schaltplan

<img src="fritzing/Fritzing.jpg" width=1024 /><br/>


## Umsetzung

- In der src/PulseCounter.ino sind die folgenden Konstanten auf die tats�chlichen Werte anzupassen:

	// Zugangsdaten zum WLAN:
	const char* ssid = "meineWLAN-SSID";
	const char* password = "meinWLANPasswort";

	// Zugangsdaten zum MQTT-Broker:
	const char* mqtt_server = "HostnameMQTT-Broker";
	const char* mqtt_user = "meinMQTTUserName";
	const char* mqtt_password = "meinMQTTPasswort";
	
- Falls gew�nscht k�nnen die Namen der MQTT-Topics noch angepasst werden, diese sind per Default

	// Topic, auf das vom WMOS die gez�hlten Impulse geschrieben werden
	"/SmartHome/Sensor/Haustechnikraum/Impulszaehler/Zaehler_" + String(i) + "/Impulse"
	
	// Topic, �ber das die Entprellzeit der einzelnen Z�hlereing�nge konfiguriert werden k�nnen
	"/SmartHome/Sensor/Haustechnikraum/Impulszaehler/Zaehler_" + String(i) + "/Entprellzeit";

- Folgende Library muss dem Projekt hinzugef�gt werden: 
--  https://github.com/knolleary/pubsubclient

- Anschlie�end den Sketch auf den Mikrocontroller flashen