# Dokumentation Projekt Autovermietung

Die Dateien mit V2 im Namen sind die aktuellen Dokumente

##  Beschreibung

Team: Lars Bandikow, Martin Dittmann, Holger Theiler 

Aufgabenstellung (gekürzt):
Formale technische Anforderungen (BPMN) für das Projekt:
-	(min.) 8 Aktivitäten: zwei Decision Tasks, 4 verschiedene Aktivitätstypen, eine Call Activity
-	zwei verschiedene Gateways
-	ein Throw Catch Pattern
-	drei verschiedene Event-Spezifikationen
-	technische Anforderungen an DMN in Summe wie im Seminar

Link zur Projektressource:
https://github.com/larsband/Projekt_Autovermietung_Abgabe

Entscheidungskontext:
Wir haben uns für den Prozess einer Autovermietung entschieden. 
Dabei gibt ein Mitarbeiter der Fahrzeug-Vermietung die Daten bspw. über ein Web-Formular ein. 
Diese Daten beinhalten:
-	Alter des Fahrzeug-Mieters,
-	Führerscheinbesitz,
-	Beginn der Fahrzeug-Anmietung,
-	Mietdauer,
-	Fahrzeugtyp.
 
### Inhalt
1.	Abgrenzung und Beschreibung der Prozesse und Entscheidungen	
2.	Erläuterung fachlicher und technischer Modellierungsentscheidungen	
3.	Reflexion von Schwachstellen und Optionen für Verbesserungen	



 
## 1.	Abgrenzung und Beschreibung der Prozesse und Entscheidungen
Hauptprozess (Hauptprozess_Autovermietung_BPMN_V2):
 
Der Hauptprozess beginnt mit einer Kundenanfrage zur Anmietung eins Wagens. Zuerst werden die Daten durch den Bearbeiter eingegeben und anhand der Mietrichtlinien überprüft. Nach der Prüfung wird je nach Output der DMN, durch ein Exklusives Gateway die Bedigungen geprüft, ob der potentielle Mieter überhaupt berechtigt ist, ein Wagen anzumieten (Überprüfung des Alters, Führerscheinbesitz, Fahrzeugtyp). Sollte dies nicht der Fall sein, wird eine Mietabsage versendet und der Prozess wird beendet. Sollte eine Anmietung möglich sein, wird im nächsten Schritt über eine paralleles Gateway in einem Teilprozess der mögliche Rabatt geprüft, danach der Angebotspreis ermittelt (dabei wird der Fahrzeugtyp, der Preistabelle der Mietfahrzeuge und der Mietdauer der Mietpreis berechnet) und das erstellte Angebot abschließend durch einen Mitarbeiter geprüft sowie die Verfügbarkeit des buchenden Wagens geprüft und das Auto reserviert- Sollte das Auto nicht verfügbar sein wird der Kunde informiert und der Prozess beendet. Nachdem diese Prozesse parallel abgelaufen sind, werden diese wieder zusammengeführt und falls möglich (Auto verfügbar) das Angebot an den Kunden versendet. Der potentielle Kunde prüft jetzt seinerseits das Angebot und kann das Angebot annehmen und eine Auftragsbestätigung senden oder dieses ablehnen und eine Absage senden. Über ein eventbasiertes Gateway wird der Prozess entsprechend der Vorentscheidungen beendet: Angebot angenommen -> das Fahrzeug wird vermietet, Angebot abgelehnt -> das Fahrzeug wird nicht vermietet oder der Kunde antwortet nicht innerhalb einer Woche -> die Angebotsdauer ist abgelaufen und es findet auch keine Vermietung statt.

Teilprozess (Rabatt_Subprozess_BPMN_V2.bpmn):
 
Bei dem Teilprozess wird die aktuelle Temperatur (Standort der Autovermietung geprüft, in unseren Fall 12309 Berlin) ermittelt, da unter anderem von dieser der Rabatt abhängt. Dann wird die Rabattstaffel für die Mietdauer berücksichtigt (siehe Entscheidungstabelle 2 (Rabattabgabe.dmn)).

Entscheidungstabelle 1 (Berechtigung DMN_V2.dmn):
 
Hier wird die grundsätzliche Berechtigung zur Anmietung eines Mietwagens des potentiellen Mieters geprüft. Dabei wird auf Grundlage der „Richtlinien Mietbedingungen“ der Besitz eines Führerscheins berücksichtig, welcher Fahrzeugtyp gebucht werden soll sowie das Alter des potentiellen Kunden.

 
In der Entscheidungstabelle 1 werden die Einflussfaktoren für die Berechnung zur Anmietung dargestellt: Ist der Kunde 18 Jahre oder älter (>=18) und im Besitz eines Führerscheins (Führerschein = true) darf er einen Kleinwagen, Transporter oder Kombi ausleihen; besitzt der Kunde keinen Führerschein (Führerschein = false),  darf er kein Auto ausleihen; ist der Kunde jünger als 18 Jahre (Fahreralter <18) darf er auch kein Auto ausleihen; und ist der Kunde 25 Jahre oder älter (Fahreralter >=25) und im Besitz eines Führerscheins (Führerschein = true) darf er auch einen Sportwagen ausleihen.

Entscheidungstabelle 2 (Rabatt_DMN.dmn):
 
 
In der Entscheidungstabelle 2 werden die Einflussfaktoren für die Berechnung der Mietdauer deutlich (1-7 Tage: 4,99 €/Tag, 8-30 Tage: 9,99 €/Tag und mehr als 30 Tage: 19,99 €/Tag sowie ein Sonderrabatt in der Sommerzeit von Mai-September: 3,99 €/Tag, weiterhin gibt es einen Sonderrabatt von 12,99 €/Tag sollte die Temperatur größer gleich 20 Grad Celsius sein). 
Dabei wird anhand des Attributs „date“ der Zeitraum der Anmietung berechnet, welcher für die Berechnung der Mietkosten benötigt wird. 
 
DMN Modell 3 (Angebotspreis_DMN_V2.dmn)
Mithilfe des DMN Modells wird der Angebotspreis berechnet.Dieser wird am später ausgegeben und dann an den Kunden in Form eines Angebots übermittelt.

Das DMN Modell 3 besteht aus vier verschiedenen Kernelementen:
- Input Data (Fahrzeugtyp, Mietdauer, Rabatt)
- Business Knowledge Model (Mietpreisberechnung)
- Knowledge Source (Preistabelle Mietfahrzeuge)
- Decision (Fahrzeugtyp Auswahl, Mietpreis berechnen)

Hinzu kommt noch eine Literal Expression (Ausgabepreis). 


Entscheidungstabelle 3.1 Fahrzeugtyp Auswahl:
 
Die Entscheidungstabelle 3.1 listet die verschiedenen Fahrzeugtypen auf wobei dort die entsprechenden Tagespreise hinterlegt sind (Kleinwagen: 60,- €/Tag, Kombi: 80,- €/Tag, Transporter: 120,- €/Tag, Sportwagen: 180,- €/Tag) 

 
Entscheidungstabelle 3.2 Mietpreis berechnen:
 
In der Entscheidungstabelle 3.2 wird der Mietpreis durch Addition der Mietdauer und der Kosten pro Tag sowie der Subtraktion des zustehenden Rabatts errechnet.

Literal Expression 3.3 Ausgabepreis:
 
Die Literal Expression 3.3 ist notwendig um mehrere Zwischenergebnisse abrufen zu können. Aus diesem Grunde werden sowohl der Output „KostenProTag“ und „Endpreis“ an die Variable „Ausgabepreis“ übergeben, was ein Array ist.


## 2.	Erläuterung fachlicher und technischer Modellierungsentscheidungen
Nachdem eine Mietanfrage eingegangen ist, wird die Aktivität „Kundendaten ausgeführt“ die als User Task modelliert. Der zuständige Mitarbeiter gibt in dieser User Task die Daten in ein Formular ein. Da wir bei der Implementierung auf den Einsatz von Java und Maven verzichtet haben, haben wir zur Erstellung der Formulare Generated Task Forms genutzt, die direkt im Camunda Modeler umgesetzt werden können. 

 
Die Formularfelder sind Fahrzeugtyp, Mietdauer, Abholdatum, Mailadresse, Name, Alter und Führerschein. 

-	Für das Formularfeld Fahrzeugtyp wurde der Datentyp als ```enum``` gewählt und die Fahrzeugtypen aufgelistet. Durch die Auswahl der verschiedenen Fahrzeugtypen wird verhindert, dass falsche Eingaben getätigt werden.
-	Für die Mietdauer und das Alter wird der Datentyp ```long``` verwendet, wodurch sichergestellt wird, dass nur Zahleneingaben möglich sind. 
-	Das Formularfeld Führerschein hat den Datentyp ```boolean```, dieser nur zwei Werte annehmen kann entweder Wahr oder falsch.
-	Das Abholdatum hat den Datentyp ```date```, womit nur Datumangaben möglich sind.
-	Die Formularfelder Name und Mailadresse haben den Datentyp ```String```.

Die Wahl der Datentypen verhindert zum einen Falscheingaben bzgl. der Datenformate und ist zum anderen nötig für weitere anwendung in den Entscheidungstabellen.

Die DMN Tabelle „Fahrzeugtyp Auswahl“ evaluiert die Berechtigung ob der potentielle Kunde berechtigt ist, ein Fahrzeug anzumieten. Hierbei wurde die Hit Policy ```Unique``` gewählt. Dabei wird eine Inputkombination von genau einer Regel abgedeckt. 
Bild
Für das Versenden der Nachricht per Service Task „Mietabsage versenden“ wird der Connector ```mail-send``` verwendet, über die eine E-Mail versendet wird. Die Service Task könnte auch als Send Task modelliert werden, wir wollten nur die verschiedenen Möglichkeiten austesten und haben uns in diesem Fall für eine Service Task entschieden. Von der Implementierung gibt es jedenfalls keinen Unterschied bei der Verwendung des Connectors ```mail-send```. Die Mail wird direkt an die Emailadresse gesendet, die zu Beginn des Prozesses im Formularfeld „Mailadresse“ eingegeben wurde.

Ausführliche Tutorials, wie der Mail Versand und Empfang ohne Java umgesetzt werden kann, finden sich unter:

https://github.com/camunda/camunda-bpm-mail

https://github.com/MCikus/CamundaBPM-Send-and-Receive-Message

Das Error Event ist an der Script Task „Auto reservieren“ angeheftet. Ist das Fahrzeug verfügbar wird durch die Variable „Fahrzeugverfügbarkeit“ ein ```true``` übergeben und der Prozess läuft normal weiter. Wird ein ```false``` übergeben, löst das Error Event aus und der Kunde wird per Email informiert und der Prozess wird über ein Terminate Event beendet. Das Terminate Event ist notwendig damit alle Prozesse beendet werden, ansonsten wäre durch das parallele Gateway der andere Pfad noch aktiv und der Prozess würde nicht beenden. Der dazugehörige Javascript Code der Script Task „Auto reservieren“ ist in der folgenden Abbildung zu sehen.

```
 var fahrzeugv = execution.getVariable("Fahrzeugverfuegbarkeit");
if ( fahrzeugv == true ) {
    execution.setVariable("FahrzeugReservierung", true);
} else if ( fahrzeugv == false) {
  throw new org.camunda.bpm.engine.delegate.BpmnError("FahrzeugError");
}
```

Eine ausführliche Erklärung, wie das Error Event mit JavaScript verwendet wird, ist auf folgender Webseite nachzulesen:

https://medium.com/@stephenrussett/throwing-bpmn-errors-with-javascript-in-camunda-c678f4b7d9ff

In der Call Activity „Rabatt ermitteln“ wird das BPMN Modell „Rabatt_Supprozess_BPMN_V2.bpmn“ aufgerufen, in diesem der Rabatt berechnet wird. In der Service Task „Temperatur ermitteln“ wird die externe API ```http://api.openweathermap.org/data/2.5/weather?zip=12309,de&appid=3af927f89da1cf2ac68bd304b55c4cf0``` über den ```http-connector``` aufgerufen. Dabei werden aktuellen Wetterdaten für den Postleitzahlbereich 12309 Berlin abgerufen. Im Anschluss wird per JavaScript die Temperatur aus den Informationen herausgefiltert sowie in Grad Celsius umgerechnet und im Anschluss in der Variable ```temperatur``` abgespeichert. 

```
var responseObj = connector.getVariable("response");
var weatherMsg =  S(responseObj).prop("main").prop("temp");
//var weatherMsg = responseObj.main.temp;
var tempMsg = parseInt(Math.round(weatherMsg-273.15));
connector.setVariable("temperatur",tempMsg);
```

In der darauffolgenden Entscheidungstabelle „Rabatt_DMN“ wird über die Hit Policy ```Collect Sum``` genutzt, die die Ausgabewerte der zutreffenden Regeln zusammenrechnet, woraus sich der Rabatt ergibt.

Die Entscheidung ob ein Angebot durch den Kunden angenommen wird oder nicht wird im Hauptprozess durch ein ereignisbasiertes Gateway modelliert. Je nach Rückmeldung des Kunden (eingehende Nachricht) trifft eines der modellierten Events ein und der Prozess wird beendet. 
Eine ausführliche Anleitung zu eingehenden Nachrichten ohne Java befindet sich auf folgenden Seiten:

https://github.com/camunda/camunda-bpm-mail

https://github.com/MCikus/CamundaBPM-Send-and-Receive-Message

Sollte sich der Kunde innerhalb einer Woche nicht melden löst das Timer Event aus
Das Timerevent ist aktuell auf ```1000s``` eingestellt, was jedoch nur exemplarisch zum Testen des Prozesses gewählt wurde.

Nähere Informationen zur Anwendung des Timer Events finden sich hier:

https://docs.camunda.org/manual/7.7/reference/bpmn20/events/timer-events/

Um den Angebotspreis zu berechnen, haben wir die Entscheidungstabelle „MietpreisBerechnung“ genutzt, in der der Endpreis berechnet wird. 
Mit Hilfe einer Literal Expression wird ein Array mit dem Variablennamen ```Ausgabepreis``` angelegt. In diesem befinden sich sowohl die ```KostenProTag``` sowie der ```Endpreis```. Diese können bei Bedarf über den Execution Listener und ein Javascript abgerufen werden, siehe folgender Codeblock.

```
execution.setVariable("Endpreis", Ausgabepreis[1]);
execution.setVariable("KostenProTag", Ausgabepreis[0]);  
```
 
## 3.	Reflexion von Schwachstellen und Optionen für Verbesserungen

Es wäre sicherlich möglich gewesen, das Fachliche Modell von Beginn an wesentlich komplexer zu gestalten. Gleichzeitig war es unser vorrangiges Ziel ein funktional vollständiges implementiertes Modell zu präsentieren.
So haben wir uns entschieden, das erste Modell möglichst einfach zu gestalten und sich nach und nach mit den einzelnen Elementen und Funktionen der Implementierung vertraut zu machen.
Die Entwicklung des Modells der Autovermietung war ein stetiger Lernprozess, gerade durch die fehlende Erfahrung der Implementierung von Prozessen mit Camunda.
Nach und nach haben wir während des Projektes durch viel Recherche neue Elemente und einzelnen technische Möglichkeiten von Camunda kennengelernt. Das Modell ist stetig mit unseren Erfahrungen mitgewachsen, woraus die Version 2 entstanden ist. Mit der Anwendung neuer Elemente, wie die Implementierung von Error Events, konnten komplexere Strukturen modelliert werden. Zusätzlich sind Elemente wie die Service Task hinzugekommen, bei der auf eine externe API (Abruf der Temperatur) zugegriffen wird. Fachlich ist dieser Teil nicht wirklich relevant für eine Autovermietung. Aber gerade da wir die Freiheit eines selbstgewählten Modells hatten, konnten wir uns mit den verschiedenen technischen Möglichkeiten (Connectoren, Zugriff auf externe API’s) von Camunda vertraut machen. 

Das aktuelle Modell hat noch einige Schwachstellen, z.B. werden aktuell die Prozesseingaben nicht automatisch auf Gültigkeit überprüft, so ist es möglich das sowohl der Mietzeitraum als auch das Abholdatum in der Vergangenheit sein können. In einer Zukünftigen Version sollte dies nicht mehr möglich sein und bereits beim Absenden des Formulars eine Fehlermeldung erscheinen.

Unsere gesamte Implementierung fand ohne die Verwendung von Java und Maven statt, wodurch die Möglichkeiten der Implementierung allerdings eingeschränkt wird.
So war es nicht möglich, eigene Formulare (Embedded Task Forms, External Task Forms) einzubinden. Erstellte Formulare ließen sich nicht deployen ohne die Verwendung von Maven. So gibt es zwar eine Anleitung online, die wir leider in der Praxis nicht bei uns nicht umsetzen konnten:

https://medium.com/@stephenrussett/deploying-embedded-forms-with-camunda-rest-api-84cf8010f8c1


In einer zukünftigen Version unseres Modells würde wir von Beginn an Java verwenden, wodurch mehr technische Möglichkeiten zur Verfügung stehen komplexere Prozessmodelle in Camunda zu implementieren.
So wäre eine Möglichkeit das Formular auf einer externen Webseite für den Kunden bereitzustellen. Auf dieser Webseite könnte der Kunde seine Mietanfrage direkt tätigen und die Gültigkeit der Daten könnte direkt geprüft werden. Der Vorteil wäre nach einer Mietanfrage müssten die Daten nicht wiederholt durch den Mitarbeiter in ein Formular eingeben werden, wodurch auch die Integrität der Daten sichergestellt wird.
Insgesamt sind wir mit unserem Projekt sehr zufrieden. Alles, was wir uns zu Beginn des Projektes vorgenommen haben, konnte wir in der Praxis umsetzen und haben vieles Neues dazu gelernt. In der Zukunft werden wir sicherlich mit Camunda weiter experimentieren, was technisch möglich ist.
