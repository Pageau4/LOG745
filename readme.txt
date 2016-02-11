Une vue d'ensemble des fichiers:

.
|-- doNotCompile
|   |-- MultitouchFramework-MOUSE.java        # à utiliser sans écran multitactile, sur Windows 7 à 10
|   `-- MultitouchFramework-JWINPOINTER.java  # à utiliser avec un écran multitactile, sur Windows 10
`-- src
    |-- AlignedRectangle2D.java
    |-- CLASSES.html             # documentation à lire
    |-- Constant.java
    |-- GraphicsWrapper.java
    |-- MultitouchFramework.java # copier par dessus ce fichier une des versions dans doNotCompile
    |-- Point2D.java
    |-- Point2DUtil.java
    |-- SimpleWhiteboard.java    # modifiez surtout (ou même seulement) ce fichier
    `-- Vector2D.java


Étapes pour le setup du projet avec Eclipse :

1- Lancez Eclipse (si vous avez des problèmes au labo multimédia, utilisez la version dans le dossier C:\eclipse-log720)

2- Importez le projet GTI745_Labo03 : File/Import/General/Existing Projects into Workspace puis sélectionnez le dossier du projet

3- Assurez-vous que la version du JRE est la bonne : ce labo fonctionne uniquement avec le JDK 8 x64
	Sur les machines du labo multimédia :
		Allez dans Windows/Preferences/Java/Installed JREs et ajoutez "Standard VM" avec comme chemin C:\Oracle\java\jdk8_65_x64
		L'environnement devrait se mettre à jour
			Sinon, allez dans Project/Properties/Java Build Path/Libraries et éditez le JRE référencé pour utiliser le bon JRE
	Sur une autre machine :
		Obtenez et installez la plus récente version du JDK 8 x64 sur http://www.oracle.com/technetwork/java/javase/downloads/index.html (le JRE seul peut suffir)
		Allez dans Windows/Preferences/Java/Installed JREs et ajoutez "Standard VM" avec le chemin du JDK récemment installé
		Allez dans Project/Properties/Java Build Path/Libraries et éditez le JRE référencé pour utiliser le bon JRE

4- Ouvrez MultitouchFramework.java et lancez le logiciel


Si vous désirez bâtir le projet de toute pièce, tenez compte de ce qui suit :
- L'environnement Java devrait être 1.8 x64
- Il y a 3 JAR à référencer : JWinPointer.jar, gluegen-rt.jar et jogl.jar
- Ces deux derniers JAR ont besoin de DLL et il faudra peut-être préciser l'emplacement de ces librairies


Fonctionnement avec/sans écran tactile :
- Sans écran tactile :
	- Remplacez MultitouchFramework.java par MultitouchFramework-MOUSE.java
	- Vous pouvez simuler un doigt en appuyant sur Ctrl et en faisant un clic-gauche
	- Refaites cette opération sur un marqueur de doigt pour pouvoir le déplacer
	- Simulez le retrait d'un doigt en appuyant sur Ctrl et en faisant un clic-droit sur un marqueur de doigt
- Avec écran tactile :
	- Remplacez MultitouchFramework.java par MultitouchFramework-JWINPOINTER.java
	- Utilisez vos doigts pour les contrôles


Références :
- JWinPointer.jar : http://www.michaelmcguffin.com/code/JWinPointer/
- SimpleWhiteboard original : http://profs.etsmtl.ca/mmcguffin/code/#SimpleWhiteboard


---------------------------------------------------------------------------------------------------------------

Vous devez faire un nombre de modifications au code totalisant un certain nombre de points,
selon le nombre de membres dans votre équipe:

   Équipe de 1 personne: modifs totalisant 4 points
   Équipe de 2 personnes: modifs totalisant 6 points, dont au moins 1 modif à 2 ou 3 points
   Équipe de 3 personnes: modifs totalisant 7 points, dont au moins 2 modifs à 2 ou 3 points
   Équipe de 4 personnes: modifs totalisant 9 points, dont au moins 3 modifs à 2 ou 3 points


Modifications possibles:


- (1 point) Dessiner une ligne qui montre la division entre les espaces
  de travail lorsqu'il y a deux utilisateurs.
  Indice:
    if ( Constant.NUM_USERS == 2 ) {
      Point2D center0 = userContexts[0].palette.getCenter();
      Point2D center1 = userContexts[1].palette.getCenter();
      Vector2D direction = Point2D.diff( center1, center0 ).normalized();
      direction.copy( - direction.y(), direction.x() ); // rotation de 90 degrés
      Point2D centerOfDividingLine = Point2D.average( center0, center1 );
      ... dessiner une ligne passant par centerOfDividingLine dans la direction calculée
    }


- (1 point) Rajouter un bouton dans la palette pour faire un flip
  vertical (c'est-à-dire un flip autour d'un axe horizontal)
  pour complémenter le bouton déjà fourni qui fait un flip horizontal
  (autour d'un axe vertical).


- (1 point) Rajouter un bouton "Frame sel." dans la palette qui
  fait encadrer le rectangle englobant de la sélection actuelle
  de l'utilisateur de la palette.
  Ce bouton complémentera le bouton déjà fourni "Frame all".
  Notez bien que si aucun trait n'est sélectionné par l'utilisateur,
  alors "Frame sel." ne devrait avoir aucun effet.


- (1 point) Empêcher qu'un trait (instance de la classe Stroke)
  soit sélectonné s'il est déjà sélectionné par un autre utilisateur.


  (Variante valant 2 points) Permettre à chaque utilisateur de sélectionner
  seulement les traits (instances de Stroke) qu'il a créés.  Ceci nécessitera
  de stocker dans chaque trait l'identité de l'utilisateur qui l'a créé.


- (1 points) Rajouter un bouton à la palette pour supprimer les traits
  sélectionnés.  Attention de bien enlever les traits des listes de
  traits sélectionnés de chaque UserContext (UserContext.selectedStrokes),
  surtout vu qu'un seul trait peut être sélectionné dans plusieurs
  instances de UserContext.


- (1 point) Rajouter un bouton "Apply" ou "Appliquer" qui change
  la couleur des traits sélectionnés pour la couleur actuelle
  de la palette (noir, rouge, vert).


- (1 point) Empêcher qu'un utilisateur fasse une opération de caméra
  (avec le bouton "Camera" ou "Frame all") lorsqu'un autre utilisateur
  a ses doigts sur l'interface.
  Pour faire, utilisez la booléenne doOtherUserContextsHaveCursors
  qui est passée à processMultitouchInputEvent:
  si cette booléenne est vraie, il faut empêcher les opérations
  de caméra.

  (Variante valant 2 points) En plus de faire ce qui est demandé ci-dessus,
  à chaque fois que vous dessinez la palette d'un utilisateur A,
  s'il y a d'autres utilisateurs qui ont leurs doigts posés sur l'interface,
  dessiner les boutons "Camera" et "Frame all" de A avec un retour
  visuel quelconque qui montre que ces boutons ne sont pas disponibles.
  Des exemples de retour visuels possibles: dessiner le texte de ces
  boutons en gris, ou bien dessiner un X par dessus chacun de ces boutons.
  Lorsque les doigts de tous les autres utilisateurs sont enlevés,
  les boutons "Camera" et "Frame all" devraient être redessinés
  normalement pour montrer qu'ils sont maintenant disponibles.


- (1 point) Rajouter 2 ou 3 boutons dans la palette pour sélectionner
  l'épaisseur des traits qui seront dessinés.
  Ces 2 ou 3 boutons pourraient s'appeler "Thin", "Medium", "Thick"
  ou "Mince", "Moyen", "Épais", par exemple.

  (Variante valant 2 points) Rajouter un glisseur sur la palette
  permettant de régler l'épaisseur des traits qui seront dessinés.




- (2 points) Rajouter un bouton dans la palette qui affiche
  un signe de moins ("-") et, lorsque appuyé, réduit la taille de la palette
  en faisant disparaître la plupart des boutons de façon temporaire.
  Dans cet état plus petit, un autre bouton "+" devrait permettre de
  ré-ouvrir la palette pour montrer tous les boutons originaux.


- (2 points) Remplacer les étiquettes de texte sur les boutons de palettes
  avec des icônes, probablement déssinés avec OpenGL.


- (2 points) Rajouter un bouton "Mult. Sel." aux palettes qui,
  lorsque appuyé, permet de faire des sélections multiples
  (c.-à-d. que les traits sélectionnés par des rectangles ou des lassos
  sont rajoutés à la sélection actuelle, au lieu de la remplacer).
  Attention de ne pas rajouter des copies d'un trait qui est déjà
  sélectionné.


- (2 points) Rajouter un bouton dans la palette qui permet de faire
  une copie des traits sélectionnés.
  La copie devrait apparaître à une position déplacée par rapport
  aux traits originaux.
  Une fois l'opération finie, les traits originaux ne devraient
  plus être sélectionnés, et les nouveaux traits devraient
  être sélectionnés, permettant alors à l'utilisateur d'appuyer le bouton
  pour copier à plusieurs reprises, faisant apparaître plusieurs
  copies, chacune déplacée par rapport à la dernière.


- (2 points) Rajouter un bouton à la palette pour faire un undo
  du dernier trait dessiné.

  (Variante valant 3 points) Rajouteur deux boutons à la palette
  pour faire un undo illimité, ou un redo permettant de revenir
  jusqu'au trait le plus récent.


- (2 points) Dans le code fourni, chaque instance de PaletteButton
  stocke une chaîne
      String tooltip
  qui est initialisée mais pas affichée à l'utilisateur.
  Par exemple, dans le cas du bouton "Move", l'infobulle est
  "Drag on this button to move the palette."
  Pensez à une manière d'afficher ces infobulles et programmez la.
  Chaque bouton devrait afficher son infobulle au moment que le doigt
  appuie le bouton (TOUCH_EVENT_DOWN) et l'infobulle devrait
  disparaître quand le doigt est enlevé.
  (Vous pouvez changes les noms des boutons et des infobulles en français
  si vous voulez.)

  (Variante valant 3 points) Dans le code fourni, les boutons
  "Hor. Flip" et "Frame all" sont activés quand le doigt les appuie
  (TOUCH_EVENT_DOWN).  Changez le code pour que, au moment que
  le doigt appuie "Hor. Flip" ou "Frame all",
  l'infobulle du bouton est affiché, et c'est seulement si l'utilisateur
  relâche (TOUCH_EVENT_UP) *par dessus le même bouton* que la commande
  du bouton est exécutée.
  Si l'utilisateur appuie sur le bouton, voit l'infobulle, ensuite
  glisse sur un autre bouton ou glisse hors de la palette et relâche,
  la commande du bouton ne devrait pas être exécutée,
  permettant à l'utilisateur d'annuler l'action.
  Les autres boutons de la palette peuvent continuer à fonctionner
  tel que fourni, mais doivent tous afficher une infobulle au moment
  de l'appuie.


- (2 points) Rajouter un bouton dans la palette qui affiche un "X" et,
  lorsque appuyé, supprime la palette et l'instance de UserContext
  correspondant.
  Avec ce bouton de "X", l'utilisateur devrait pouvoir enlever toutes
  les palettes sauf la dernière.
  S'il ne reste plus qu'une seule palette, il ne devrait pas être possible
  de la fermer avec le bouton "X".

  Rajouter aussi un bouton aux palettes qui permet d'instancier
  une nouvelle palette, et le UserContext correspondant.
  Avec ce bouton, les utilisateurs devrait pouvoir créer un nombre
  illimité de palettes.

  (Variante valant 3 points) Au lieu d'instancier des palettes
  avec un bouton dans une palette qui existe déjà,
  permettez plutôt à l'utilisateur de dessiner une "queue de cochon"
  n'importe où dans l'interface et instancier une nouvelle
  palettte centrée sur le geste de queue de cochon.
  La queue de cochon ne devrait plus être affichée une fois que la nouvelle
  palette est ouverte.


- (3 points) Rajouter des glisseurs rouge, vert, bleu dans la palette
  permettant de sélectionner n'importe quelle couleur pour les traits.


- (4 points) Rajouter une fonctionnalité qui permettant d'envoyer
  un courriel à un destinataire quelconque avec un fichier .svg en
  pièce jointe contenant le dessin fait par les utilisateurs.
  L'adresse courriel du destinataire peut être hard-codé.
  Resource: http://apike.ca/prog_svg.html


- Nous sommes ouverts aussi aux modifications proposées,
  mais demandez auparavant notre approbation !


- D'autres idées:
  Créer des effets artistiques, comme de l'encre qui coule,
  de l'encre animé ( http://www.flong.com/projects/yellowtail/ ),
  simulation de sable ( http://www.youtube.com/watch?v=NQ9FERXWWsQ ),
  calligraphie ( http://www.graficaobscura.com/dyna/ ),
  etc.







