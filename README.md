# DevGenia Studio

Een lokale 3D-game-editor in **Java**, gebouwd op de bestaande Eclipse-projectstructuur. De engine en gamescripts blijven Java. Je hebt geen Maven, externe bibliotheken of internetverbinding nodig om te bouwen en te spelen.

## Starten

Installeer een **JDK 17 of nieuwer** (een losse JRE is niet genoeg voor scriptcompilatie). Dubbelklik op `run.bat`. Het script compileert de bronnen en opent de studio.

In Eclipse: importeer deze map als *Existing Project*, selecteer een JDK als runtime en voer `src/devgenia/Studio.java` uit als Java Application. Vernieuw het project met F5 als de nieuwe bestanden nog niet zichtbaar zijn.

Handmatig vanuit deze map:

```powershell
javac -encoding UTF-8 -d bin src\devgenia\*.java
java -cp bin devgenia.Studio
```

Begin met **File → New Beacon Run example**, of open `examples/Beacon Run.dgs`. Deze speelbare voorbeeldwereld combineert triggers, een score-UI, geluid, een geanimeerde wereld, een bewegende NPC en een deur met een tween. `examples/Moving Platforms.dgs` blijft beschikbaar als eenvoudiger voorbeeld. Zie [de handleiding voor de nieuwe systemen](ENGINE_GUIDE.md).

## Wat werkt

- Perspectiefviewport met softwarematige 3D-rendering, dieptebuffer, near-plane clipping en selectie van zichtbare objecten.
- Blokken, bollen en cilinders toevoegen, selecteren, dupliceren en verwijderen.
- Verplaatsen, schalen en roteren via X/Y/Z-handles; exacte waarden via Properties; bewerkbare snapping die **tijdens het slepen** wordt toegepast. Met Move kun je ook de part zelf over het X/Z-vlak slepen.
- Explorer met type-iconen, naamfilter, geneste folders en instances. Rechtermuisknop → Insert Object, Rename, Duplicate of Delete. Sleep instances naar een andere geldige parent; de hele subtree gaat mee en de verplaatsing kan ongedaan worden gemaakt.
- Inklapbare Properties-secties met naam, positie, afmetingen, rotatie, kleur, Transparency, Visible, Anchored en CanCollide.
- Meerdere Java-script-tabs met sluitknoppen en een indicator voor niet-opgeslagen wijzigingen; syntax highlighting, autocomplete, current-line highlight, bracket matching, regelnummers, auto-indent, automatische sluittekens, zoeken en eigen tekst-undo/redo.
- Instelbare Lighting-, Camera-, Physics- en Players-services, met directe preview en Java-scripttoegang.
- Textures op blokken, bollen en cilinders: vier ingebouwde patronen, PNG/JPEG-import, herhaling en optionele kleurtint. TextureLibrary bewaart de geïmporteerde afbeeldingen in het project.
- Afzonderlijke server- en clientwerelden voor de lokale speeltest. Gewijzigde servereigenschappen worden naar de client gekopieerd; lokale visuele wijzigingen blijven lokaal.
- Play, Pause/Resume en Stop; bestuurbare speler met zwaartekracht, springen en eenvoudige botsingen.
- De speeltest gebruikt kopieën. Stop herstelt de ongewijzigde editorwereld en camerastand.
- Undo/redo van scènebewerkingen, opslaan/laden, waarschuwing voor onopgeslagen wijzigingen en export van een draagbaar `.dgs`-project.
- Centraal donker thema met vectoriconen, hover/selection states en versleepbare paneelscheidingen. Play Mode heeft een eigen indicator; Pause/Stop zijn daarbuiten uitgeschakeld.
- Output onderscheidt informatie, waarschuwingen, compilerfouten en runtimefouten. Dubbelklik op een scriptdiagnose om het script op de betreffende regel te openen; **Next error** springt naar de volgende foutmelding.

## Bediening

| Actie | Bediening |
| --- | --- |
| Camera draaien | Rechtermuisknop slepen |
| Camera verschuiven | Shift + rechtermuisknop, of middelste muisknop slepen |
| Zoomen | Muiswiel |
| Focus op selectie | F, met focus in de viewport |
| Transformeren | Kies Move/Scale/Rotate en sleep een gekleurd uiteinde |
| Move / Rotate / Scale kiezen | T / R / X; niet tijdens tekstinvoer of Play |
| Instance toevoegen | Rechtermuisknop op parent → Insert Object |
| Instance hernoemen | F2 in Explorer |
| Instance verplaatsen in de hiërarchie | Sleep in Explorer op de nieuwe parent |
| Dupliceren | Ctrl+D |
| Verwijderen | Delete in viewport, of Edit → Delete selection |
| Undo / redo | Ctrl+Z / Ctrl+Y |
| Opslaan / opslaan als | Ctrl+S / Ctrl+Shift+S |
| Openen / nieuwe wereld | Ctrl+O / Ctrl+N |
| Play / Stop | F5 |
| Lopen / springen | WASD / Spatie tijdens Play; klik eerst in de viewport |
| Script openen | Dubbelklik of Enter op het script in Explorer |
| Zoeken in script | Ctrl+F, Enter volgende, Shift+Enter vorige, Escape sluiten |
| Code inspringen | Tab / Shift+Tab, ook met meerdere geselecteerde regels |
| Autocomplete openen | Automatisch tijdens typen, of Ctrl+Space |
| Suggestie kiezen / invoegen | Pijltjes / Tab of Enter; Escape sluit de lijst |

Posities en afmetingen gebruiken studs; rotaties gebruiken graden. De drie Properties-velden zijn gelabeld X, Y, Z. Scale / size verandert de afmetingen van de part. De bestaande rotatiehandles veranderen Eulerhoeken.

**Snap** accepteert eindige positieve waarden van `0.000001` tot `100000`, met een punt of komma als decimaalteken. Enter of focusverlies bevestigt invoer. Ongeldige waarden worden gemarkeerd en niet toegepast; bij focusverlies wordt de vorige waarde hersteld. Het selectievakje schakelt snapping uit zonder het ingestelde getal te verliezen. Move en Scale snappen live; rotatie wordt in graden aangepast. Kleine muisbewegingen worden opgeteld zodat ook een grote snapafstand geleidelijk kan worden bereikt. Properties volgt de transform tijdens het slepen.

**Transparency** loopt van `0` (ondoorzichtig) tot `1` (volledig onzichtbaar). Tussenwaarden mengen zichtbaar met de achtergrond en andere parts. Transparantie verandert de collider niet; gebruik CanCollide voor botsingen. Volledig transparante of onzichtbare parts blijven via Explorer selecteerbaar.

Folders organiseren de scène. Parts en Models geven hun positie en rotatie door aan children. Models geven daarnaast een uniforme schaal door. Properties toont lokale coördinaten ten opzichte van de parent; gizmos werken in de wereld. Explorer-reparenting bewaart de wereldtransform. Oudere projecten worden bij het laden gemigreerd, zodat bestaande objecten niet verschuiven. Duplicate kopieert de hele geselecteerde subtree met nieuwe IDs; Delete verwijdert ook de children. Services kunnen niet worden hernoemd, gedupliceerd, verwijderd of versleept. Explorer toont tijdens slepen de drop-parent. Verplaatsen naar jezelf, een eigen child, een script of een ongeschikte service wordt geweigerd. Een folder met Parts kan bijvoorbeeld niet in ServerScriptService. Tijdens Play staat slepen uit.

## Services en textures

Selecteer een service in Explorer om de instellingen in Properties te bewerken. Ze horen bij het project en ondersteunen undo/redo en opslaan/laden.

| Service | Werkende instellingen |
| --- | --- |
| Lighting | Ambient, helderheid, zonrichting en zonkleur; kleuren van de hemelachtergrond |
| Camera | Follow / Fixed, field of view, afstand, yaw/pitch en target / offset |
| Physics | Zwaartekracht en kill plane; gevallen objecten verschijnen weer boven Spawn |
| Players | Loopsnelheid, sprongsnelheid en de kleur van het speeltestpersonage |
| TextureLibrary | Afbeeldingen importeren, vervangen en in folders ordenen |

**Camera:** Follow volgt de speler; Target / offset is daarbij een verschuiving ten opzichte van de speler. Fixed kijkt naar een vaste wereldpositie. **Preview camera** toont de ingestelde camerastand. **Use scene view as fixed camera** neemt de huidige editorcamera over, binnen de ondersteunde afstanden en hoeken. Tijdens Play kun je een Follow-camera nog draaien, verschuiven en zoomen; een Fixed-camera blijft vast. Stop herstelt de editorcamera.

**Textures:** selecteer een Part en open de sectie **Texture**. Kies Checker, Brick, Wood of Grid, of gebruik **Import & apply…** voor een eigen afbeelding. **Repeat** bepaalt het aantal herhalingen per oppervlak; **Tint with Color** vermenigvuldigt de texturekleur met de Part-kleur. Transparante pixels in een PNG tonen de onderliggende Part-kleur. Gebruik de afzonderlijke eigenschap Transparency om de hele Part doorzichtig te maken. Vervangen van een TextureLibrary-afbeelding werkt alle verwijzende Parts bij; verwijderen zet hun texture op None.

PNG/JPEG-bestanden mogen maximaal 8 MB, 16 megapixels en 8192 pixels per zijde zijn. De import verkleint ze tot maximaal 512 pixels per zijde en sluit de pixels in als PNG. Het bronbestand is daarna niet meer nodig. Het `.dgs`-project mag maximaal 96 MB groot zijn. Lighting ondersteunt omgevingslicht, zonlicht en Point Lights; er zijn nog geen geworpen schaduwen. De optionele GPU-backend staat beschreven in ENGINE_GUIDE.md.

## Java-gamescripts

Klik in Explorer met de rechtermuisknop op **ServerScriptService** of **StarterPlayerScripts**, kies **Insert Object → Server Script / Local Script**. In Workspace, folders en parts zijn de daar geldige types eveneens beschikbaar. Schrijf methodes in de editor; de studio voegt zelf de klasse en imports toe. Bronwijzigingen worden direct in het project verwerkt; opnieuw op Play drukken compileert de nieuwste versie.

Elk script heeft zijn eigen tab, cursorpositie en tekstgeschiedenis. De stip in een tab betekent dat de broncode of scriptinstellingen afwijken van het opgeslagen project. **Een tab sluiten bewaart de wijzigingen in het project**; Ctrl+S slaat alle scripts samen met de scène op. Hernoemen in Explorer werkt de tab direct bij. Tijdens Play zijn scripts alleen-lezen, maar diagnoses blijven aanklikbaar.

De kleurindeling onderscheidt Java-keywords, control flow, strings, comments, getallen, typen, methodes en properties. Dit is een lichtgewicht lexicale highlighter: type-/methodeherkenning gebruikt de syntax en naamgeving, geen volledige semantische compileranalyse. Haakjes in strings en comments worden niet meegenomen bij bracket matching. Zoeken is hoofdlettergevoelig en loopt bij het einde door vanaf het begin.

Autocomplete verschijnt na een korte typpauze en na een punt, bijvoorbeeld bij `game.`, `game.world.lighting.` of een bekende `Scene.Part`-variabele. Het biedt Java-keywords, bekende API-typen, velden, methodes, lokale identifiers en snippets voor `start`, `update`, `stop`, `if` en `for`. In `game.find("...")` verschijnen Part-namen uit de huidige scène. Gebruik Ctrl+Space om de lijst handmatig te openen, pijltjes om te kiezen en Tab/Enter om in te voegen. Zonder suggestielijst behouden Tab en Enter hun gewone inspringgedrag. Dit is lichte codehulp, zonder volledige Java-scopeanalyse, generieke type-inferentie of externe library-index.

```java
private double time;

public void start(GameContext game) {
    game.log("Game gestart!");
}

public void update(GameContext game, double dt) {
    time += dt;
    Scene.Part part = game.find("Platform 1");
    if (part != null) {
        part.y = 2 + Math.sin(time);
        part.ry += 30 * dt;
    }
}

public void stop(GameContext game) {
    game.log("Game gestopt");
}
```

Beschikbare API:

| API | Gebruik |
| --- | --- |
| `game.world` | De scène van deze uitvoercontext |
| `game.side` | `"Server"` of `"Local"` |
| `game.find("naam")` | Eerste part met deze naam, of `null` |
| `game.createPart("naam", x, y, z)` | Nieuwe part maken |
| `game.destroy(part)` | Part verwijderen |
| `game.log(value)` | Naar Output schrijven met contextlabel |
| `game.keyDown(KeyEvent.VK_E)` | Toetsstatus in een Local-script |
| `game.world.lighting` | Ambient, zonlicht en hemelkleuren |
| `game.world.camera` | Cameramodus, FOV, afstand, richting en target |
| `game.world.physics` | Zwaartekracht en kill plane |
| `game.world.players` | Instellingen van de speeltestspeler |

`Scene.Part` heeft openbare Java-velden: `x/y/z`, `sx/sy/sz`, `rx/ry/rz`, `color`, `anchored`, `collision`, `visible`, `transparency`, `textureId`, `textureRepeat`, `textureTint`, `shape`, `name`, `parentId`. Gebruik unieke namen om parts op naam te vinden. `Color` en `KeyEvent` worden automatisch geïmporteerd. Namen en IDs zijn geen beveiligingsgrens.

Bijvoorbeeld in `start`:

```java
game.world.lighting.ambient = 0.4;
game.world.lighting.sunColor = new Color(255, 224, 190);
game.world.camera.fieldOfView = 70;
game.world.physics.gravity = 18;
game.world.players.walkSpeed = 14;
Scene.Part platform = game.find("Platform 1");
if (platform != null) {
    platform.textureId = "builtin:Brick";
    platform.textureRepeat = 2;
}
```

Service-instellingen en texture-eigenschappen van Parts worden vanaf de server naar de lokale wereld gerepliceerd als hun waarde verandert. Een Local Script kan bijvoorbeeld eigen Lighting- of Camera-effecten instellen; ongewijzigde servervelden overschrijven die niet elke frame. Physics en de spelercontroller draaien op de server, dus pas die instellingen in een Server Script aan. De avatar-kleur wordt bij het maken van de speler toegepast.

**Scriptuitvoering is voor vertrouwde lokale code.** Java-scripts hebben de rechten van jouw gebruikersaccount en zijn niet gesandboxed. Ze kunnen bestanden en het netwerk benaderen. Compileer of speel geen onbekend project. De gewone Studio-speeltest draait hooks op de Swing-eventthread; de standalone player heeft een eigen simulatiewerker. Voor beide geldt: gebruik geen `Thread.sleep`, eindeloze loops of blokkerende taken; gebruik `dt` voor tijdsafhankelijke beweging. Een normale exception in `update` schakelt dat script uit en verschijnt in Output. Foutieve scripts worden overgeslagen, andere scripts kunnen blijven draaien.

## Projectformaat en architectuur

`.dgs` is leesbare XML. Formaatversie `4` bewaart lokale transforms, Models, Parts, Java-broncode, services, meshes, textures, WAV-audio, game-UI, animatiekeys en scene-links. Projecten uit versie `1`, `2` en `3` blijven leesbaar en worden gemigreerd. Oudere Studio-versies kunnen versie `4` niet laden. Opslaan schrijft eerst naar een tijdelijk bestand naast het doelbestand en vervangt daarna het doel. Laden controleert versie, IDs, parent-relaties, cycles, typen, numerieke waarden en textureverwijzingen en weigert XML external entities. Alleen openen voert geen scripts uit.

| Bestand | Verantwoordelijkheid |
| --- | --- |
| `Studio.java` | Desktopeditor, opdrachten, geschiedenis, projectbeheer en speeltest |
| `Scene.java` | Scènemodel en geïsoleerde kopieën |
| `WorldSettings.java`, `TextureData.java` | Service-instellingen, replicatie en begrensde afbeeldingopslag |
| `Viewport.java` | Camera, meshes, dieptebuffer, picking en transformhandles |
| `Physics.java` | OBB-botsingen, raycasts, triggers, zwaartekracht en bewegende platforms |
| `ScriptRuntime.java` | Java-compilatie, classloaders, hooks en foutmeldingen |
| `GameScript.java`, `GameContext.java` | Java-scriptcontract en wereld-API |
| `ProjectIO.java` | Geversioneerde opslag |
| `DemoProjects.java` | Voorbeeldwereld met server- en localscripts |
| `DevGeniaTheme.java`, `StudioIcons.java`, `EditorUI.java` | Centrale stijl, vectoriconen en herbruikbare controls |
| `InstanceType.java` | Type-register en beschikbare objectsoorten |
| `ExplorerPanel.java`, `PropertiesPanel.java`, `NumericField.java` | Hiërarchie, contextmenu's, inspector en invoervalidatie |
| `ScriptEditor.java`, `JavaSyntax.java`, `CompletionEngine.java`, `CompletionPopup.java` | Script-tabs, highlighting, autocomplete en editorfuncties |
| `OutputPanel.java`, `LogEntry.java` | Getypeerde meldingen en navigatie naar scriptregels |

**Export project** schrijft een bewerkbaar `.dgs`-bestand. **Export playable game** maakt een nieuwe exportmap met een zelfstandig player-JAR, startbestanden en alle ingebedde assets en gekoppelde levels. De ontvanger heeft een JDK 17+ nodig; Studio is niet nodig. `java -jar DevGeniaPlayer.jar --check` test de startup zonder zichtbaar venster.

## Grenzen van deze versie

Dit is een uitgebreid werkend fundament, geen vervanging voor een complete commerciële engine of een publiek gameplatform. De huidige mogelijkheden, concrete workflow en resterende beperkingen staan in [ENGINE_GUIDE.md](ENGINE_GUIDE.md). Java-code in de editor en gewone player blijft vertrouwde lokale code. Een apart Java-proces op zichzelf is geen sandbox.

## Verificatie

Voer `test.bat` uit. De tests controleren projectroundtrips, geweigerde XML-entiteiten en formaatversies, diepe kopieën, botsingen bij hoge valsnelheid, afzonderlijke scriptcontexten, compileerfouten en runtime-exceptions. De editorintegratietests controleren toevoegen, dupliceren, undo/redo, script editing, syntaxkleuren, zoeken, tabs, snapping, foutnavigatie, Play, replicatie en herstel na Stop.

`CompletionTests` controleert suggesties, contexten, invoegen en behoud van normale editortoetsen. `ExplorerDragDropTests` controleert transfers, selectie, ongeldige subtrees/cycles, oude transfers en Play-blokkering. `WorldServicesTests` controleert instellingen, Physics, texture-import, projectopslag, rendering, camerastanden en de echte Studio-opdrachten voor verplaatsen en importeren met undo/redo. `TextureAlphaTests` controleert PNG-alpha, kleurtint en Part-transparantie. `ServiceWorkflowTests` controleert texture-replicatie, lokale overrides, behoud van onbevestigde Properties-invoer en camerabediening tijdens Play. Swing-components worden zonder zichtbaar venster opgebouwd, bediend via acties en synthetische events en naar `artifacts/*-preview.png` gerenderd voor visuele controle. Dit vervangt geen handmatige controle van native muis-drag-and-drop op iedere Windows-configuratie.

De uitgebreide suites testen Models/prefabs, UI, animatie en scene-wissels (`AdvancedEditorTests`), gameplayservices, georiënteerde physics, standalone export, echte JDI-debugging, Beacon Run, TCP-multiplayer, het lokale platform, glTF/GLB-import en renderer-/containergrenzen. De optionele GPU- en containerfases worden expliciet als niet geverifieerd/overgeslagen gemeld zolang hun dependencies ontbreken; zie [ENGINE_GUIDE.md](ENGINE_GUIDE.md).
