Une vue d'ensemble des fichiers:

.
|-- doNotCompile
|   |-- MultitouchFramework-MOUSE.java        # � utiliser sans �cran multitactile, sur Windows 7 � 10
|   `-- MultitouchFramework-JWINPOINTER.java  # � utiliser avec un �cran multitactile, sur Windows 10
`-- src
    |-- AlignedRectangle2D.java
    |-- CLASSES.html             # documentation � lire
    |-- Constant.java
    |-- GraphicsWrapper.java
    |-- MultitouchFramework.java # copier par dessus ce fichier une des versions dans doNotCompile
    |-- Point2D.java
    |-- Point2DUtil.java
    |-- SimpleWhiteboard.java    # modifiez surtout (ou m�me seulement) ce fichier
    `-- Vector2D.java


�tapes pour le setup du projet avec Eclipse :

1- Lancez Eclipse (si vous avez des probl�mes au labo multim�dia, utilisez la version dans le dossier C:\eclipse-log720)

2- Importez le projet GTI745_Labo03 : File/Import/General/Existing Projects into Workspace puis s�lectionnez le dossier du projet

3- Assurez-vous que la version du JRE est la bonne : ce labo fonctionne uniquement avec le JDK 8 x64
	Sur les machines du labo multim�dia :
		Allez dans Windows/Preferences/Java/Installed JREs et ajoutez "Standard VM" avec comme chemin C:\Oracle\java\jdk8_65_x64
		L'environnement devrait se mettre � jour
			Sinon, allez dans Project/Properties/Java Build Path/Libraries et �ditez le JRE r�f�renc� pour utiliser le bon JRE
	Sur une autre machine :
		Obtenez et installez la plus r�cente version du JDK 8 x64 sur http://www.oracle.com/technetwork/java/javase/downloads/index.html (le JRE seul peut suffir)
		Allez dans Windows/Preferences/Java/Installed JREs et ajoutez "Standard VM" avec le chemin du JDK r�cemment install�
		Allez dans Project/Properties/Java Build Path/Libraries et �ditez le JRE r�f�renc� pour utiliser le bon JRE

4- Ouvrez MultitouchFramework.java et lancez le logiciel


Si vous d�sirez b�tir le projet de toute pi�ce, tenez compte de ce qui suit :
- L'environnement Java devrait �tre 1.8 x64
- Il y a 3 JAR � r�f�rencer : JWinPointer.jar, gluegen-rt.jar et jogl.jar
- Ces deux derniers JAR ont besoin de DLL et il faudra peut-�tre pr�ciser l'emplacement de ces librairies


Fonctionnement avec/sans �cran tactile :
- Sans �cran tactile :
	- Remplacez MultitouchFramework.java par MultitouchFramework-MOUSE.java
	- Vous pouvez simuler un doigt en appuyant sur Ctrl et en faisant un clic-gauche
	- Refaites cette op�ration sur un marqueur de doigt pour pouvoir le d�placer
	- Simulez le retrait d'un doigt en appuyant sur Ctrl et en faisant un clic-droit sur un marqueur de doigt
- Avec �cran tactile :
	- Remplacez MultitouchFramework.java par MultitouchFramework-JWINPOINTER.java
	- Utilisez vos doigts pour les contr�les


R�f�rences :
- JWinPointer.jar : http://www.michaelmcguffin.com/code/JWinPointer/
- SimpleWhiteboard original : http://profs.etsmtl.ca/mmcguffin/code/#SimpleWhiteboard


---------------------------------------------------------------------------------------------------------------

Vous devez faire un nombre de modifications au code totalisant un certain nombre de points,
selon le nombre de membres dans votre �quipe:

   �quipe de 1 personne: modifs totalisant 4 points
   �quipe de 2 personnes: modifs totalisant 6 points, dont au moins 1 modif � 2 ou 3 points
   �quipe de 3 personnes: modifs totalisant 7 points, dont au moins 2 modifs � 2 ou 3 points
   �quipe de 4 personnes: modifs totalisant 9 points, dont au moins 3 modifs � 2 ou 3 points


Modifications possibles:


- (1 point) Dessiner une ligne qui montre la division entre les espaces
  de travail lorsqu'il y a deux utilisateurs.
  Indice:
    if ( Constant.NUM_USERS == 2 ) {
      Point2D center0 = userContexts[0].palette.getCenter();
      Point2D center1 = userContexts[1].palette.getCenter();
      Vector2D direction = Point2D.diff( center1, center0 ).normalized();
      direction.copy( - direction.y(), direction.x() ); // rotation de 90 degr�s
      Point2D centerOfDividingLine = Point2D.average( center0, center1 );
      ... dessiner une ligne passant par centerOfDividingLine dans la direction calcul�e
    }


- (1 point) Rajouter un bouton dans la palette pour faire un flip
  vertical (c'est-�-dire un flip autour d'un axe horizontal)
  pour compl�menter le bouton d�j� fourni qui fait un flip horizontal
  (autour d'un axe vertical).


- (1 point) Rajouter un bouton "Frame sel." dans la palette qui
  fait encadrer le rectangle englobant de la s�lection actuelle
  de l'utilisateur de la palette.
  Ce bouton compl�mentera le bouton d�j� fourni "Frame all".
  Notez bien que si aucun trait n'est s�lectionn� par l'utilisateur,
  alors "Frame sel." ne devrait avoir aucun effet.


- (1 point) Emp�cher qu'un trait (instance de la classe Stroke)
  soit s�lectonn� s'il est d�j� s�lectionn� par un autre utilisateur.


  (Variante valant 2 points) Permettre � chaque utilisateur de s�lectionner
  seulement les traits (instances de Stroke) qu'il a cr��s.  Ceci n�cessitera
  de stocker dans chaque trait l'identit� de l'utilisateur qui l'a cr��.


- (1 points) Rajouter un bouton � la palette pour supprimer les traits
  s�lectionn�s.  Attention de bien enlever les traits des listes de
  traits s�lectionn�s de chaque UserContext (UserContext.selectedStrokes),
  surtout vu qu'un seul trait peut �tre s�lectionn� dans plusieurs
  instances de UserContext.


- (1 point) Rajouter un bouton "Apply" ou "Appliquer" qui change
  la couleur des traits s�lectionn�s pour la couleur actuelle
  de la palette (noir, rouge, vert).


- (1 point) Emp�cher qu'un utilisateur fasse une op�ration de cam�ra
  (avec le bouton "Camera" ou "Frame all") lorsqu'un autre utilisateur
  a ses doigts sur l'interface.
  Pour faire, utilisez la bool�enne doOtherUserContextsHaveCursors
  qui est pass�e � processMultitouchInputEvent:
  si cette bool�enne est vraie, il faut emp�cher les op�rations
  de cam�ra.

  (Variante valant 2 points) En plus de faire ce qui est demand� ci-dessus,
  � chaque fois que vous dessinez la palette d'un utilisateur A,
  s'il y a d'autres utilisateurs qui ont leurs doigts pos�s sur l'interface,
  dessiner les boutons "Camera" et "Frame all" de A avec un retour
  visuel quelconque qui montre que ces boutons ne sont pas disponibles.
  Des exemples de retour visuels possibles: dessiner le texte de ces
  boutons en gris, ou bien dessiner un X par dessus chacun de ces boutons.
  Lorsque les doigts de tous les autres utilisateurs sont enlev�s,
  les boutons "Camera" et "Frame all" devraient �tre redessin�s
  normalement pour montrer qu'ils sont maintenant disponibles.


- (1 point) Rajouter 2 ou 3 boutons dans la palette pour s�lectionner
  l'�paisseur des traits qui seront dessin�s.
  Ces 2 ou 3 boutons pourraient s'appeler "Thin", "Medium", "Thick"
  ou "Mince", "Moyen", "�pais", par exemple.

  (Variante valant 2 points) Rajouter un glisseur sur la palette
  permettant de r�gler l'�paisseur des traits qui seront dessin�s.




- (2 points) Rajouter un bouton dans la palette qui affiche
  un signe de moins ("-") et, lorsque appuy�, r�duit la taille de la palette
  en faisant dispara�tre la plupart des boutons de fa�on temporaire.
  Dans cet �tat plus petit, un autre bouton "+" devrait permettre de
  r�-ouvrir la palette pour montrer tous les boutons originaux.


- (2 points) Remplacer les �tiquettes de texte sur les boutons de palettes
  avec des ic�nes, probablement d�ssin�s avec OpenGL.


- (2 points) Rajouter un bouton "Mult. Sel." aux palettes qui,
  lorsque appuy�, permet de faire des s�lections multiples
  (c.-�-d. que les traits s�lectionn�s par des rectangles ou des lassos
  sont rajout�s � la s�lection actuelle, au lieu de la remplacer).
  Attention de ne pas rajouter des copies d'un trait qui est d�j�
  s�lectionn�.


- (2 points) Rajouter un bouton dans la palette qui permet de faire
  une copie des traits s�lectionn�s.
  La copie devrait appara�tre � une position d�plac�e par rapport
  aux traits originaux.
  Une fois l'op�ration finie, les traits originaux ne devraient
  plus �tre s�lectionn�s, et les nouveaux traits devraient
  �tre s�lectionn�s, permettant alors � l'utilisateur d'appuyer le bouton
  pour copier � plusieurs reprises, faisant appara�tre plusieurs
  copies, chacune d�plac�e par rapport � la derni�re.


- (2 points) Rajouter un bouton � la palette pour faire un undo
  du dernier trait dessin�.

  (Variante valant 3 points) Rajouteur deux boutons � la palette
  pour faire un undo illimit�, ou un redo permettant de revenir
  jusqu'au trait le plus r�cent.


- (2 points) Dans le code fourni, chaque instance de PaletteButton
  stocke une cha�ne
      String tooltip
  qui est initialis�e mais pas affich�e � l'utilisateur.
  Par exemple, dans le cas du bouton "Move", l'infobulle est
  "Drag on this button to move the palette."
  Pensez � une mani�re d'afficher ces infobulles et programmez la.
  Chaque bouton devrait afficher son infobulle au moment que le doigt
  appuie le bouton (TOUCH_EVENT_DOWN) et l'infobulle devrait
  dispara�tre quand le doigt est enlev�.
  (Vous pouvez changes les noms des boutons et des infobulles en fran�ais
  si vous voulez.)

  (Variante valant 3 points) Dans le code fourni, les boutons
  "Hor. Flip" et "Frame all" sont activ�s quand le doigt les appuie
  (TOUCH_EVENT_DOWN).  Changez le code pour que, au moment que
  le doigt appuie "Hor. Flip" ou "Frame all",
  l'infobulle du bouton est affich�, et c'est seulement si l'utilisateur
  rel�che (TOUCH_EVENT_UP) *par dessus le m�me bouton* que la commande
  du bouton est ex�cut�e.
  Si l'utilisateur appuie sur le bouton, voit l'infobulle, ensuite
  glisse sur un autre bouton ou glisse hors de la palette et rel�che,
  la commande du bouton ne devrait pas �tre ex�cut�e,
  permettant � l'utilisateur d'annuler l'action.
  Les autres boutons de la palette peuvent continuer � fonctionner
  tel que fourni, mais doivent tous afficher une infobulle au moment
  de l'appuie.


- (2 points) Rajouter un bouton dans la palette qui affiche un "X" et,
  lorsque appuy�, supprime la palette et l'instance de UserContext
  correspondant.
  Avec ce bouton de "X", l'utilisateur devrait pouvoir enlever toutes
  les palettes sauf la derni�re.
  S'il ne reste plus qu'une seule palette, il ne devrait pas �tre possible
  de la fermer avec le bouton "X".

  Rajouter aussi un bouton aux palettes qui permet d'instancier
  une nouvelle palette, et le UserContext correspondant.
  Avec ce bouton, les utilisateurs devrait pouvoir cr�er un nombre
  illimit� de palettes.

  (Variante valant 3 points) Au lieu d'instancier des palettes
  avec un bouton dans une palette qui existe d�j�,
  permettez plut�t � l'utilisateur de dessiner une "queue de cochon"
  n'importe o� dans l'interface et instancier une nouvelle
  palettte centr�e sur le geste de queue de cochon.
  La queue de cochon ne devrait plus �tre affich�e une fois que la nouvelle
  palette est ouverte.


- (3 points) Rajouter des glisseurs rouge, vert, bleu dans la palette
  permettant de s�lectionner n'importe quelle couleur pour les traits.


- (4 points) Rajouter une fonctionnalit� qui permettant d'envoyer
  un courriel � un destinataire quelconque avec un fichier .svg en
  pi�ce jointe contenant le dessin fait par les utilisateurs.
  L'adresse courriel du destinataire peut �tre hard-cod�.
  Resource: http://apike.ca/prog_svg.html


- Nous sommes ouverts aussi aux modifications propos�es,
  mais demandez auparavant notre approbation !


- D'autres id�es:
  Cr�er des effets artistiques, comme de l'encre qui coule,
  de l'encre anim� ( http://www.flong.com/projects/yellowtail/ ),
  simulation de sable ( http://www.youtube.com/watch?v=NQ9FERXWWsQ ),
  calligraphie ( http://www.graficaobscura.com/dyna/ ),
  etc.







