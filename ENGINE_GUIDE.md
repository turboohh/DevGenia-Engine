# DevGenia — uitgebreidere enginefuncties

Start `run.bat`. Probeer eerst **File → New Beacon Run example** en druk op **F5**. Verzamel drie gouden beacons; de score verandert en de poort opent. De Controls-knop toont de bediening. Met **C** wissel je de camera-afstand. De projectbron staat ook in `examples/Beacon Run.dgs`.

## Models, hiërarchie en prefabs

Voeg een **Model** toe via Explorer → Insert Object, of selecteer een object en kies **Edit → Group selection as Model**. Sleep Parts en andere Models onder dit Model. Ze volgen de parentpositie, rotatie en uniforme schaal. Parts geven positie en rotatie door, maar hun afmetingen schalen children niet. Folders zijn organisatorisch.

Properties toont lokale coördinaten. De viewportgizmos werken in wereldcoördinaten. Explorer-reparenting bewaart de wereldtransform. Scripts kunnen `game.world.worldPart(part)` gebruiken om een wereldtransform te lezen en `game.world.setWorldPosition(part, x, y, z)` om die te wijzigen.

**File → Save selection as prefab** schrijft een `.dgprefab` met de geselecteerde subtree en ingebedde assets. **Import prefab** voegt een onafhankelijke kopie met nieuwe IDs toe onder de gekozen parent. Wijzigingen aan één kopie veranderen andere kopieën niet. Live gekoppelde prefab-overrides zijn nog niet ingebouwd.

## Meshes en rendering

**File → Import 3D model** importeert statische glTF 2.0 / GLB-geometrie als een Mesh Part. De geïmporteerde geometrie blijft in het project bewaard. De assetbrowser onder **View → Asset browser** toont meshes, afbeeldingen, audio, animaties en scene-links.

De software-renderer blijft beschikbaar zonder externe dependencies. De GPU-schakelaar in de viewport vraagt de optionele OpenGL-backend aan; als die niet beschikbaar is valt de viewport terug naar software en meldt dat in de tooltip. `setup-gpu.ps1` en `run-gpu.bat` verzorgen de optionele graphicsbibliotheken en het starten met GPU-rendering.

Voeg **Point Light** toe onder Workspace, een Model of Part. Positie, kleur, intensiteit en bereik staan in Properties. Lighting regelt daarnaast zonlicht, omgevingslicht en hemelkleuren. Er is nog geen PBR-materiaalpipeline of systeem voor geworpen schaduwen. Meshes zijn statisch: skins en morph targets zijn geen onderdeel van deze importeur.

## Physics en gameplayservices

Parts hebben **Mass**, **Friction**, **Restitution** en **Trigger**, naast Anchored en CanCollide. De botsingsvorm is een georiënteerde box die de rotatie volgt, ook bij een Sphere, Cylinder of Mesh. Dynamische bodies hebben lineaire snelheid; volledige rigid-body-hoekdynamica is nog niet ingebouwd. De speler kan op hellingen en bewegende platforms staan. Raycasts gebruiken dezelfde boxcolliders.

| API | Gebruik |
| --- | --- |
| `game.input.bind("Jump", KeyEvent.VK_SPACE)` | Toetsen aan een actie koppelen |
| `game.input.down/pressed/released("Jump")` | Vasthouden, indrukken en loslaten per frame |
| `InputService.MOUSE_LEFT/MOUSE_MIDDLE/MOUSE_RIGHT` | Muisknoppen als inputbinding |
| `game.events.onEnter(zone, other -> { ... })` | Trigger betreden |
| `game.events.onExit(zone, other -> { ... })` | Trigger verlaten |
| `game.events.onTouch(part, other -> { ... })` | Nieuw contact met een andere Part |
| `game.tween.to(part, "y", 10, 2)` | Numerieke eigenschap over twee seconden aanpassen |
| `game.navigation.moveTo(part, x, z, speed)` | Via een begrensd obstakelvermijdend pad bewegen |
| `game.audio.play("Collect")` | Een Sound uit AudioService afspelen |
| `game.animation.play("Door Animation")` | Een property-animatie afspelen |
| `game.ui.setText("Score", "Score: 5")` | Tekst in de game-interface veranderen |
| `game.ui.onClick("Play Button", () -> { ... })` | Een UI-knop afhandelen |
| `game.scenes.load("Next Level")` | Een Scene-link laden |

Inputactions zijn lokaal. Gewone Part-beweging, physics en gedeelde game-state horen in serverscripts. De lokale speeltest behoudt lokale visuele overrides zolang de server die eigenschap niet verandert. Services zijn gedeeld tussen scripts binnen dezelfde uitvoercontext.

Navigatie gebruikt een tweedimensionaal X/Z-grid met begrensde A*. Het is bedoeld voor eenvoudige NPC-routes; geen navmesh met springen en meerdere verdiepingen. Tweens en animaties veranderen eigenschappen, geen fysische krachten.

## Audio

Voeg een **Sound** toe onder AudioService of importeer een WAV via de assetbrowser. Volume, bereik, looping, autoplay en mixgroep staan in Properties. Spatial audio gebruikt afstand tot de speler en de positie van een gekozen emitter. WAV-data wordt ingebed; de import heeft limieten op bestandsgrootte, lengte, kanalen en samplefrequentie. Afspelen gebeurt buiten de editor-eventthread. Zonder beschikbaar audioapparaat blijft de game werken en verschijnt een melding.

## Game-UI maken

Open **View → UI designer**. Maak Frames, Text Labels, Buttons en Images. Sleep een element om het te positioneren; sleep het hoekje rechtsonder om te schalen. De waarden voor X/Y/Width/Height zijn relatief aan de parent: `0.5` betekent de helft van de parentafmeting. Een Frame kan children bevatten. Properties bevat tekst, kleuren, lettergrootte, afbeelding, opacity, zichtbaarheid en draw order.

Dezelfde layout wordt gebruikt in Play Mode en de standalone player. UI-events worden tijdens een update afgehandeld: in de gewone Studio-speeltest op de Swing-eventthread en in de standalone player op de simulatiewerker. De voorbeeldgame laat een buttoncallback en score-replicatie zien.

## Animatie

Voeg een **Animation** toe onder Animations en open de timeline. Kies een target en eigenschap, voer tijd en waarde in en klik **Add key**. **Capture** neemt de huidige eigenschapswaarde over. Keys worden lineair geïnterpoleerd. Stel Duration, Loop en Autoplay in via Properties. De slider scrubt door de clip; Preview speelt een afzonderlijke kopie van de scène af.

Dit is property-animatie van Parts, Models, Lights en UI. Skeletanimatie, curve-tangenten, blending en state machines zijn nog niet ingebouwd. Animatiebewerkingen zijn tijdens Play vergrendeld.

## Profiler en debugger

**View → Script profiler** toont het aantal updates, laatste/gemiddelde/maximale looptijd en fouten per script. Het meet verstreken tijd rond de updatehook; het is geen GPU-profiler of meting van uitsluitend CPU-instructies.

**View → Script debugger** opent een echte Java/JDI-debugger. Kies script en regel, voeg een breakpoint toe en klik **Launch debug player**. De debugger start een apart JVM-proces met een snapshot van het project. Bij een breakpoint kun je de call stack en lokale variabelen lezen, **Step over**, **Continue** of **Stop** gebruiken. Klikken op een scriptframe navigeert naar de regel in Studio. Nieuwe code uit de editor komt bij een volgende debuglaunch mee.

De debuglaunch neemt gekoppelde levels binnen de projectmap mee, tot 64 levels en 128 MB. De breakpointselector in het paneel toont de huidige editorscène. Voor navigatie naar broncode van een ander level open je dat level zelf in Studio. Conflicterende definities van dezelfde script-ID in verschillende levels worden geweigerd, zodat breakpoints niet aan de verkeerde code worden gekoppeld.

Een apart JVM-proces voorkomt dat het pauzeren van een breakpoint de editor zelf pauzeert. Het is geen veiligheidsgrens voor onbekende Java-code. De gewone Studio-speeltest vereist nog steeds niet-blokkerende hooks; de losse player heeft een eigen simulatiewerker.

## Projecten, herstel en meerdere scenes

Een gewijzigd project krijgt elke circa 45 seconden een herstelsnapshot onder `.devgenia/recovery`. Normaal afsluiten verwijdert de eigen tijdelijke snapshot. Na een crash biedt een volgende start de achtergebleven snapshot aan; **File → Recover autosave** opent dezelfde herstelkeuze. Een hersteld project moet opnieuw worden opgeslagen. Undo-geschiedenis wordt niet opgeslagen.

Maak onder SceneService een **Scene**-link naar een `.dgs` binnen de map van het hoofdproject. Sla je hoofdproject eerst op. Alle scene-links, ook in gekoppelde levels, worden relatief aan deze hoofdmap geïnterpreteerd. `game.scenes.load("Next Level")` wisselt tijdens het spelen van level. De editorwereld blijft intact bij Stop. Padtraversal buiten de projectmap wordt geweigerd.

Projectformaat 4 bewaart deze nieuwe gegevens; oudere projecten blijven leesbaar. Het formaat gebruikt begrensde XML en expliciet gevalideerde typen, geen Java-objectdeserialisatie.

## Losse player en export

**File → Run standalone player** speelt een kopie van het huidige project in een eigen venster. **Export playable game** maakt een lege/nieuwe exportmap met een player-JAR en `play.bat` / `play.sh`. Project, assets en recursief gekoppelde levels worden in het JAR ingebed. De speler heeft een JDK 17+ nodig om de Java-gamescripts te compileren. Er wordt geen complete JDK meegeleverd.

```powershell
java -jar DevGeniaPlayer.jar
java -jar DevGeniaPlayer.jar --check
```

`--check` voert echte scriptstartup en de eerste frames uit zonder venster. Ook deze controle voert Java-code uit; gebruik hem lokaal uitsluitend voor vertrouwde projecten.

## Multiplayer testen

Open **View → Local / LAN multiplayer**. Host gebruikt standaard `127.0.0.1` en poort `0`, waarna de toegekende poort verschijnt. Open twee lokale clients om replicatie te testen. Een andere computer kan met een expliciet LAN-adres en poort verbinden wanneer de host daarop luistert.

De server verwerkt input, spelersbeweging, scripts en physics. Clients sturen begrensde inputintenties; zij kunnen geen posities dicteren. Serverbronnen en privé-services gaan niet over de verbinding. Gedownloade scripts worden op clients niet uitgevoerd. Daardoor zijn gewone Local Scripts voorlopig niet actief in deze multiplayerplayer. Knoppen kunnen op de server worden afgehandeld.

Deze eerste TCP-implementatie heeft maximaal acht spelers, 2048 Parts en snapshots van 4 MiB, met tien volledige snapshots per seconde. Er zijn nog geen prediction, interpolatie, encryptie, matchmaking of Internet-relays. Het is een lokale/LAN-testbasis, geen publieke hostingdienst.

## Lokaal platformprototype

**View → Local platform** opent een lokale catalogus. Maak een account of log in, publiceer de huidige scène, voeg een game toe aan je bibliotheek en open een preview. Wachtwoorden worden met een afzonderlijk salt en PBKDF2 opgeslagen. Alleen de eigenaar mag een nieuwe versie publiceren; eerdere versies blijven bewaard. Een bibliotheekitem verwijst naar een gekozen versie. Opnieuw toevoegen haalt de nieuwste versie binnen.

Deze eerste publicatieworkflow ondersteunt één scène met ingebedde assets. Externe scene-links worden geweigerd, zodat een publicatie niet afhankelijk is van bestanden buiten de catalogus. De gewone game-export ondersteunt wel meerdere gekoppelde scenes.

Een **safe preview** is een bewerkbare kopie met alle scripts en autoplay uitgeschakeld. Alleen het openen van een download voert geen gamescripts uit. Wie scripts daarna bewust inschakelt en lokaal op Play drukt, voert ze met de rechten van zijn gebruikersaccount uit.

Het paneel kan per account en game begrensde key/value-voortgang bewaren en teruglezen. Dit is een opslag- en inspectie-API; automatische accountkoppeling aan de multiplayerplayer is nog niet ingebouwd. Gegevens staan onder `.devgenia-platform` op deze computer. Er is geen publieke website, cloudaccount, betaling, moderatie of Internet-hosting aan gekoppeld.

## Geïsoleerde startupcontrole

**View → Isolated startup check** kan een geëxporteerde game kort in een Linux-container starten. De runner vereist een al geïnstalleerde lokale Docker-daemon en een vooraf gebouwde image. Hij installeert of downloadt niets automatisch en voert zonder Docker geen gamescripts uit. Zie [sandbox/README.md](sandbox/README.md) voor de vereisten en expliciete buildopdracht.

De controle heeft onder andere een tijdlimiet, geheugen-/CPU-/proceslimieten, uitgeschakeld netwerk en alleen-lezen bestanden. Dit is een begrensde startupcontrole, geen interactieve gesandboxte gameplayer of certificering dat een gepubliceerd spel veilig is. De native containerfase is op de huidige ontwikkelcomputer **niet uitgevoerd**, omdat Docker ontbreekt.

## Verificatie

`test.bat` compileert alle bronnen en voert unit-, integratie-, subprocess- en Swing-tests uit. De tests dekken ook echte scriptcompilatie, JDI-breakpoints, exporterstartup, twee TCP-clients, world/local transforms, prefabs, UI, animatie, audio zonder apparaat, padvalidatie en herstelbestanden. Previewafbeeldingen staan onder `artifacts`.

De optionele native GPU-fase vereist LWJGL/native libraries. Zonder die bestanden test de suite de mesh-rendering en softwarefallback en meldt **GPU NOT VERIFIED**. De Docker-tests melden een native **SKIP** als de daemon of image ontbreekt; controles op argumenten, padbegrenzing, cleanup en het weigeren van hostuitvoering blijven dan wel actief. Een groene standaardtest betekent dus niet dat deze twee native backends zijn bevestigd.
