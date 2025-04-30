---
title: Wir haben unsere SaaS-Anwendung auf fly.io deployed (und dabei richtig gute Erfahrungen gemacht).
subtitle: Wie wir unsere Anwendung in einem Bruchteil der Zeit bereitgestellt und dabei 100 % der Kosten eingespart haben.
date: 2024-10-23
toc: false
---

Unser Team, bestehend aus einer Gruppe erfahrener Software-Entwickler ohne Cloud Vorkenntnisse, wollte unseren OCPP-konformen EV-Ladesäulen-Simulator (*Electric Vehicle Charging Station Simulator, EVCSS*) öffentlich bereitstellen, damit Erstanwender und andere Interessensgruppen ihn problemlos testen können.
Klingt einfach genug, oder? Das haben wir uns auch gedacht. Und wir haben das Ticket „Cloud-Deployment“ optimistisch in unseren aktuellen Sprint gezogen, weil wir ein paar zusätzliche Tage hatten, um daran zu arbeiten.

## Größe M sollte passen, oder?

Bei unserer ersten Einführung in AWS wurden wir mit einem Admin-Panel und einer beträchtlichen Auswahl an Diensten begrüßt und befanden uns mitten in der Paralyse durch Analyse. Was brauchen wir eigentlich? Es war schwierig, die Beste oder überhaupt eine Möglichkeit zu finden, unsere einfache Client-Server-Anwendung bereitzustellen. Also haben wir einen Spezialisten engagiert, jemanden mit AWS-Erfahrung, der uns möglicherweise dabei helfen sollte, etwas Licht in den Tunnel voller Akronyme und ähnlich klingender Dienste (*similar sounding services*, steht S3 etwa dafür?) zu bringen.

## Bei Risiken und Nebenwirkungen, wenden Sie sich an den Cloud-Spezialisten.

Wir haben ein paar Stunden damit verbracht, unsere Anforderungen zusammenzufassen und die verschiedenen Kompromisse zwischen einem Service und dem anderen besprochen. Wir haben Kästchen und Pfeile gezeichnet, um zu visualisieren, wie unser künftiges Deployment aussehen und sich verhalten sollte. Die ganze Zeit jedoch hatte ich einen Gedanken im Hinterkopf: Muss das wirklich alles so kompliziert sein?

Wir haben keine hohen Anforderungen. Wir möchten lediglich, dass unsere Web Anwendung für unsere Stakeholder erreichbar ist. Bonus: Ein öffentlicher Zugang, um weitere Interessenten dafür zu gewinnen. Warum sind wir also am Ende bei rund zehn Diensten gelandet, die auf nichttriviale Weise miteinander verbunden sind? Ist AWS für uns hier wirklich die beste Wahl, wenn das Ergebnis eine hochkomplexe und wahrscheinlich teure Infrastruktur ist, nur um ein paar Leuten unsere Anwendung zu zeigen?

## fly.io zur Rettung

Ich bin fest davon überzeugt, dass gute Produkte und Dienstleistungen ihre Nutzer dort abholen sollten, wo sie sind. Als Cloud-Neuling hatte ich nicht das Gefühl, dass AWS mich dort abholt, wo ich aktuell stehe. Ich habe mich also nach etwas anderem umgesehen. In [einem Podcast](https://podcasts.apple.com/de/podcast/xe-iaso-on-fly-io/id120906714) habe ich das erste Mal von fly.io gehört. Deren Slogan:


> A Public Cloud Built For Developers Who Ship
>
> …
>
> Deploy your App in 5 minutes.


Hey, das bin ich! Und genau das hatte ich im Sinn, als wir unsere Cloud-Deployment Story geschätzt haben. Wie kann ich loslegen? Nun, natürlich mit dem Getting Started Kapitel in der Dokumentation! Und da ich auf der Suche nach einem schnellen Einstieg war, um herauszufinden, ob wir statt AWS doch etwas einfacheres verwenden konnten, habe ich mich an den [Quickstart](https://fly.io/docs/getting-started/launch/) Guide gehalten.

## Nur 4 Schritte bis zum Abflug

1. Installiere das `flyctl` Befehlszeilentool
2. Erstelle ein Konto mit `fly auth signup`
3. Konfiguriere das Deployment mit `fly launch` aus dem aktuellen Projekt heraus
4. Stelle die Anwendung mit `fly deploy` bereit

Das war alles, was zur Bereitstellung unserer Anwendung auf fly.io erforderlich war. Ich war begeistert. Konnte es wirklich so einfach sein?

## Kannst du mich hören?

Es stellt sich heraus, dass `flyctl` unsere Client-Server-Anwendung zwar erkannt und den Docker-Container gefunden hat, jedoch keine WebSocket Verbindung zwischen Client und Server konfiguriert hat. Außerdem haben wir noch keinen API-Endpunkt, mit dem sich überprüfen lässt, ob unser Server tatsächlich betriebsbereit ist. Nach kurzer Recherche in der Dokumentation (was eine äußerst befriedigende Erfahrung war) und der Implementierung des notwendigen Endpunkts zur Überprüfung der korrekten Bereitstellung unseres Servers funktionierte alles wie es sollte. Und das Beste daran: Es war noch nicht einmal Mittag.

## Endlich Glückseligkeit.

Die Bereitstellung dauerte weniger als 4 Stunden und das Arbeiten mit der Dokumentation hat wirklich Spaß gemacht. Ich hatte immer das Gefühl, auf dem richtigen Weg zu sein, was ich nach der kurzen Begegnung mit dem Angebot von AWS nicht sagen konnte. Meine Kollegen haben währenddessen in den sauren Apfel gebissen und die Bereitstellung auf AWS weiter verfolgt. Wir haben jetzt also zwei Instanzen unserer Anwendung. Eine davon läuft netterweise kostenlos (da fly.io keine Rechnungen für Deployments stellt, die Kosten von weniger als 5 Euro pro Monat verursachen). Für die andere Instanz mussten wir uns eine Genehmigung einholen, da die anfallenden monatlichen Kosten bereits am Anfang über 100 Euro lagen, ohne dass nur ein Benutzer die Anwendung je gesehen hatte.

Ich bin mir sicher, dass unsere AWS-Instanz noch nicht kostenoptimal ist. Außerdem bin ich mir bewusst, dass AWS die reifere und flexiblere Plattform ist. Es gibt schließlich Gründe, warum sie der Marktführer in diesem Segment sind (und das schon seit geraumer Zeit). *Aber wenn ich meine nächste Anwendung entwickle und sie potenziellen Nutzern so früh wie möglich im Entwicklungs-Lebenszyklus zeigen möchte, werde ich auf jeden Fall wieder auf fly.io zurückgreifen.*
