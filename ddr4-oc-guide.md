# Table des matières
1. [Préparation](#setup)
   1. [Utilitaires de test de mémoire](#memory-testing-software)
      1. [A éviter](#avoid)
      2. [Recommandés](#recommended)
      3. [Alternatives](#alternatives)
      4. [Comparaison](#comparison)
   2. [Logiciel pour voir les timings](#timings-software)
   3. [Benchmarks](#benchmarks)
2. [Informations générales sur la RAM](#general-ram-info)
   1. [Relation entre fréquence et timings](#frequency-and-timings-relation)
   2. [Timings primaires, secondaires et tertiaires](#primary-secondary-and-tertiary-timings)
3. [Attentes/Limites](#expectationslimitations)
   1. [Carte mère](#motherboard)
   2. [Circuits intégrés (ICs)](#integrated-circuits-ics)
      1. [Rapport Taiphoon burner](#thaiphoon-report)
      2. [Etiquettes sur les barettes](#label-on-sticks)
      3. [Note sur les rangs logiques et la densité](#a-note-on-logical-ranks-and-density)
      4. [Evolution en fonction de la tension](#voltage-scaling)
      5. [Fréquence maximale attendue](#expected-max-frequency)
      6. [Tri sur le volet (Binning)](#binning)
      7. [Tension maximale recommandée pour une utilisation quotidienne](#maximum-recommended-daily-voltage)
      8. [Classement](#ranking)
      9. [La température et son effet sur la stabilité](#temperatures-and-its-effect-on-stability)
   3. [Controlleur de mémoire intégré (IMC)](#integrated-memory-controller-imc)
      1. [Intel - LGA1151](#intel---lga1151)
      2. [AMD - AM4](#amd---am4)
4. [Overclocking](#overclocking)
   1. [Trouver un point de départ](#finding-a-baseline)
   2. [Essayer des fréquences plus hautes](#trying-higher-frequencies)
   3. [Resserrer les timings](#tightening-timings)
   4. [Conseils divers](#miscellaneous-tips)
      1. [Intel](#intel)
      2. [AMD](#amd)
5. [Liens utiles](#useful-links)
   1. [Benchmarks](#benchmarks-1)
   2. [Information](#information)

# Préparation
* Assurez-vous que vos barettes de RAM sont dans les emplacements recommandés (Souvent emplacements 2 et 4).
* Assurez-vous également que votre CPU est entèrement stable avant de commencer à overclocker la RAM car un CPU instable peut causer des erreurs sur la mémoire. Lorsque que l'on essaie des fréquences plus élévées, le stress induit sur le CPU peut le rendre instable.
* Assurez-vous que votre BIOS/UEFI est à la dernière version.

## Utilitaires de test de mémoire
Il est très important de toujours tester avec plusieurs tests variés pour être sûr que votre overclock est stable.
### A éviter
* Je ne recommande pas l'utilisation du test de mémoire AIDA64 ainsi que [Memtest64](https://forums.anandtech.com/threads/techpowerups-memtest-64-is-it-better-than-hci-memtest-for-determining-stability.2532209/) car tous deux ne sont pas très bons pour trouver les erreurs.
### Recommandés
* [TM5](http://testmem.tz.ru/tm5.rar) Avec n'importe laquelle des configurations listées:
  * [Configuration Extreme par anta777](https://drive.google.com/file/d/1uegPn9ZuUoWxOssCP4PjMjGW9eC_1VJA) (Recommandée). Faire attention à bien charger le fichier de configuration. Il devrait y avoir un message disant 'Customize: Extreme1 @anta777' une fois chargé.
  Credits: [u/nucl3arlion](https://www.reddit.com/r/overclocking/comments/dlghvs/micron_reve_high_training_voltage_requirement/f4zcs04/).
  * [Ici](https://www.overclock.net/threads/memory-testing-with-testmem5-tm5-with-custom-configs.1751608/) se trouve un lien vers TM5 compilé avec plusieurs fichiers de configuration.
  * [LMHz configuration Universelle 2](https://www.hardwareluxx.de/community/threads/ryzen-ram-oc-m%C3%B6gliche-limitierungen.1216557/page-159#post-27506598)
  * Si vous rencontrez un problème avec tous les threads qui plantent au lancement avec la configuration Extreme, une potentielle solution est de changer la ligne "Testing Window Size (Mb)=1408" du fichier. Remplacez la valeur par votre quantité totale de RAM en Mo divisée par le nombre de threads de votre processeur (exemple : 16384/16 = 1024 Mo par thread).
* [OCCT](https://www.ocbase.com/) avec le test de mémoire dédié, en utilisation les instructions SSE ou AVX.
  * Note: La vitesse de détection des erreurs peut varier entre les instructions AVX ou SSE. Sur les systèmes basés sur un processeur Intel, il semblerait que SSE soit meilleur pour tester les tensions du controlleur de mémoire tandis que AVX semble mieux pour les tensions de la RAM.
  * Le test CPU AVX2 Large est un très bon test de stabilité pour le processeur et la RAM en même temps. Plus vous réglerez votre RAM, plus il sera difficile de la rendre stable dans ce test.
### Alternatives
* [GSAT](https://github.com/stressapptest/stressapptest).
  1. [Installer WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) et [Ubuntu](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab).
  2. Ouvrir une invite de commandes bash et taper `sudo apt update`.
  3. Taper `sudo apt-get install stressapptest`.
  4. Pour commencer à tester, taper `stressapptest -M 13000 -s 3600 -W --pause_delay 3600`.
     * `-M` est la quantité de mémoire à tester (MB).
     * `-s` est la durée du test (secondes).
     * `--pause_delay` est le délais entre les pics de puissance. Il doit être identique à l'argument `-s` pour ignorer le test des pics de puissance.
* [Karhu RAM Test](https://www.karhusoftware.com/ramtest/) (payant).
* [y-cruncher](http://www.numberworld.org/y-cruncher/) avec [cette configuration](https://pastebin.com/dJQgFtDH).
  * Coller la configuration dans un nouveau fichier `memtest.cfg` dans le même dossier que `y-cruncher.exe`.
  * Créer un raccourci vers `y-cruncher.exe` et ajouter `pause:1 config memtest.cfg` dans le champ Cible (clic droit > Propriétés).
    Votre champ cible devrait ressembler à quelque chose comme ça: `"chemin\vers\y-cruncher\y-cruncher.exe" pause:1 config memtest.cfg`
  * Credits: [u/Nerdsinc](https://www.reddit.com/r/overclocking/comments/iyp1n7/ycruncher_is_a_really_effective_tool_for_testing/)
* [Prime95](https://www.mersenne.org/download/) avec FFTs larges est également pas mal pour trouver les erreurs de mémoire.
  * J'utilise une fourchette de FFT personnalisée de 800k - 800k, mais je pense que n'importe quelle valeur dans la fourchette FFTs larges devrait faire l'affaire.
    * Bien vérifier que 'Run FFTs in place' n'est pas coché.
    * Dans `prime.txt`, ajouter `TortureAlternateInPlace=0` en dessous de `TortureWeak` pour empêcher P95 de tester sur-place. Tester sur-place d'utilisera qu'un seul bit de RAM, ce que l'on ne veut pas.
  * Il est possible de créer un raccourci vers `prime95.exe` et d'ajouter `-t` au champ Cible pour lancer immédiatement le test avec les paramètres dans `prime.txt`.
    Votre champ Cible devrait alors ressembler à ca: `"chemin\vers\prime95\prime95.exe" -t`.
  * Il est également possible de changer le répertoire courant des fichiers de configuration de Prime95, ce qui permet d'avoir une configuration pour tester le CPU et une autre pour la RAM.
    1. Dans le dossier contenant `prime95.exe`, créer un nouveau dossier. Pour cet exemple, on l'appellera 'RAM' (sans les guillemets).
    2. Copier `prime.txt` et `local.txt` dans le dossier que vous venez de créer.
    3. Modifier les paramètres dans `prime.txt` à votre convenance.
    4. Créer un nouveau raccourci vers `prime95.exe` et dans le champ cible, ajouter `-t -W<nom_du_dossier>`.
       Votre champ cible devrait alors ressembler à ça: `"chemin\vers\prime95\prime95.exe" -t -WRAM`
    5. Vous pouvez maintenant utiliser le raccourci pour instantanément lancer Prime95 avec les paramètres définits.
* [randomx-stress](https://github.com/00-matt/randomx-stress/releases) - Peut être utilisé pour tester la stabilité du FCLK pour les processeurs AMD.
### Comparaison
[Comparaison](https://imgur.com/a/jhrFGhg) entre Karhu RAMTest, TM avec la configuration Extreme et GSAT.
  * TM5 est le plus rapide et le plus intensif par une assez grande marge, Dans certains cas je pouvais passer 30 minutes de TM5 et avoir une erreur au bout de 10 minutes dans Karhu. Un autre utilisateur a eu la même expérience. Votre propre expérience peut varier. L'achat de Karhu n'est pas recommandé si vous ne comptez pas overclocker régulièrement
    
## Logiciel pour voir les timings
* Pour consulter les timings dans Windows:
  * Intel: 
    * Z370(?)/Z390: [Asrock Timing Configurator v4.0.4](https://www.asrock.com/MB/Intel/X299%20OC%20Formula/index.asp#Download) (Ne requiert pas une carte mère Asrock).
    * Cartes mère EVGA et Z170/Z270(?)/Z490: [Asrock Timing Configurator v4.0.3](https://www.asrock.com/mb/Intel/Z170%20OC%20Formula/#Download).
    * Pour Rocket Lake: [ASRock Timing Configurator v4.0.10](http://picx.xfastest.com/nickshih/asrock/AsrTCSetup(v4.0.10).rar)
  * AMD: 
    * [ZenTimings](https://zentimings.protonrom.com/).

## Benchmarks
* [AIDA64](https://www.aida64.com/downloads) - période d'essai de 30 jours gratuite.Nous utiliserons le test Performance caches et mémoire (situé sous Outils) pour voir comment performe notre RAM. Vous pouvez faire un clic droit sur Start Benchmark puis cliquer sur Start Memory Tests Only pour ignorer les tests de cache. Attention, ce test est connu pour avoir de grosses variations dans les résultats.
* [Intel Memory Latency Checker](https://software.intel.com/content/www/us/en/develop/articles/intelr-memory-latency-checker.html) - contient tout un tas de tests utiles pour mesurer les performances de la RAM. Il donne des informations plus détaillées que AIDA64 et la valeur de bande passante fluctue moins entre les tests. Notez qu'il faut lancer le logiciel en tant qu'administrateur pour désactiver le prefecthing. Sur les systèmes avec un processeur AMD, il se peut que le prefetching soit à désactiver dans le BIOS.
* [Intel MLC GUI](https://github.com/FarisR99/IMLCGui) - Interface graphique pour "Intel Memory Latency Checker" crée par Faris. Cela évite de passer par l'invite de commandes pour ceux que ça repousserait.
* [xmrig](https://github.com/xmrig/xmrig) est très sensible à la mémoire ce qui le rends très utile pour tester l'effet de timings spécifiques. A lancer en tant qu'administrateur via l'invite de commande avec l'argument `--bench=1M`. Utiliser le temps d'exécution pour comparer.
* [MaxxMEM2](https://www.softpedia.com/get/System/Benchmarks/MaxxMEM2.shtml) - alternative gratuite à AIDA64 mais les tests de bande passante donnent des valeurs beaucoup plus basses qu'AIDA64 donc il ne faut pas comparer les valeurs entre les deux logiciels.
* [Super Pi Mod v1.5 XS](https://www.techpowerup.com/download/super-pi/) - Un autre benchmark sensible à la RAM, mais je ne l'ai pas utilisé autant que AIDA64. 1M - 8M devrait être suffisant pour un test rapide. Il faut uniquement regarder le dernier temps (total), où un score plus bas est meilleur.
* [HWBOT x265 Benchmark](https://hwbot.org/benchmark/hwbot_x265_benchmark_-_1080p/) - J'ai entendu dire que ce benchmark était également sensible à la RAM mais je ne l'ai jamais vraiment testé moi-même.
* [PYPrime 2.x](https://github.com/monabuntur/PYPrime-2.x) - Ce benchmark est rapide et apprécie la fréquence processeur/cache/FCLK ainsi que la fréquence et les timings de RAM

# Informations générales sur la RAM
## Relation entre fréquence et timing
* La fréquence de la RAM est mesurée en mégahertz (MHz) ou millions de cycles par seconde. Une fréquence plus élevée signifie plus de cycles par seconde et donc de meilleures performances.
* Note ésotérique: Les gens pensent souvent que DDR4-3200 signifie 3200**MHz**, pourtant en réalité le fréquence de la RAM est seulement de 1600MHz. Vu que le transfert de données s'effectue sur le front montant et sur le front descendant de l'horloge en DDR (Double Data Rate), la fréquence réelle de la RAM est égale à la moitié du nombre de transferts réalisés par seconde. En DDR4-3200, la mémoire transfère 3200 millions de bits par seconde, il s'agit donc de 3200**MT/s** (MegaTransfers Per Second) ou de 3200**Mbps** (MegaBits Per Second) à une fréquence de 1600**MHz**.
* Les timings de RAM se mesurent en cycles d'horloge ou ticks. Des timings plus bas signifient moins de cycles requis pour faire une opération, donc de meilleures performances.
  * La seule exception est le timing tREFI, l'intervalle de rafraichissment. Comme son nom l'indique, tREFI corresponds au temps entre chaque rafraichissement. Lors de cette opération, la RAM ne peut rien faire d'autre donc on veut qu'elle le fasse le moins souvent possible. Pour atteindre ce but, on veut que le temps entre les rafraichissements soit maximisé. Cela signifie que l'on va définir tREFI le plus haut possible.
* Même si des timings plus bas sont mieux, cela dépends également de la fréquence à laquelle la RAM opère. Par exemple : DDR4-3000 CL15 et DDR4-3200CL16 ont la même latence malgré que DDR4-3000 ait un tCL plus faible. Cela est du au fait que la fréquence plus élevée contre-balance l'augmentation de tCL.
* Pour calculer le temps réel en nano-secondes (ns) d'un timing donné : `2000 * timing / freq_ddr`.
  * Par exemple, CL15 à DDR4-3000 corresponds à `2000 * 15 / 3000 = 10ns`.
  * De la même manière, CL16 à DDR4-3200 corresponds à `2000 * 16 / 3200 = 10ns`.

## Timings primaires, secondaires et tertiaires
* [Intel](https://i.imgur.com/hcKDkCc.png)
* [AMD](https://i.imgur.com/Ie4LVtI.png)
* Les timings de RAM sont séparés en 3 catégories: primaires, secondaires et tertaires. Ils sont indiqués par 'P', 'S' et 'T' respectivement.
  * Les timings primaires et secondaires vont affecter la latence et la bande passante.
  * Les timings tertiaires affectent uniquement la bande passante.
    * Il y a une exception : tREFI/tREF qui affecte la latence et la bande passante, par contre il n'est pas modifiable sur AMD.

# Attentes/Limites
* Cette section aborde les 3 composants pouvant influencer votre expérience d'overclocking : la carte mère, les ICs et l'IMC.

## Carte mère
* Les cartes mères avec seulement 2 slots de RAM seront généralement capables d'atteindre les fréquences les plus élevées. Surtout vrai sur les cartes mères haut-de-gamme optimisées pour l'overclocking RAM. Les cartes mères petit-budget avec uniquement 2 slots de RAM ne verrons peut-être pas d'amélioration notable mais cela dépend des modèles.
* Pour les cartes mères à 4 slots, le nombre de barrettes installées influe sur votre fréquence de RAM maximale.
  * Sur les cartes mère utilisant une topologie "daisy-chain" [agencement des traces sur le circuit imprimé](https://www.youtube.com/watch?v=3vQwGGbW1AE), on préférera 2 barrettes. L'utilisation de 4 barrettes impactera grandement votre fréquence RAM maximale.
  * D'un autre côté, les carte mères utilisant une topologie en T overclockeront mieux avec 4 barrettes. N'utiliser que 2 barrettes n'impactera pas autant la fréquence maximale que 4 sticks sur une carte mère daisy-chain (?).
  * La plupart des vendeurs ne mettent pas en avant la topologie utilisée, mais il est possible de se faire une idée en consultant la liste de compatibilité de RAM. Par exemple, la [Z390 Aorus Master](http://download.gigabyte.asia/FileList/Memory/mb_memory_z390-aorus-master_20190214.pdf) utilise une topologie en T car sa plus haute fréquence validée est avec 4 barrettes. Si la plus haute fréquence validée était avec 2 barrettes, il s'agirait *probablement* d'une topologie daisy-chain.
  * D'après Buildzoid, le débat daisy-chain contre topologie en T n'est vraiment important qu'au dessus de DDR4-4000. En suivant sa logique, si vous êtes sur Ryzen 3000, cela n'a pas vraiment d'importance car DDR4-3800 est typiquement votre fréquence maximale de RAM en conservant un ratio MCLK:FLCK de 1:1.
* Les cartes mères d'entrée de gamme ont souvent plus de mal en overclock, possiblement à cause de la qualité du circuit imprimé et du nombre de couches (?).
  
## Circuits intégrés (ICs)
* Connaitre quel ICs (*Integrated Circuits* également appelés "dies", prononcé en anglais) sont sur vos barrettes vous donnera une idée des performances attendues. En revanche, même sans disposer de cette information, il est toujours possible d'overclocker votre RAM. Les ICs sont les modules de mémoires qui sont soudés sur le PCB de la barette.

### Rapport Taiphoon burner
* Note: Taiphoon burner est connu pour essayer de deviner les ICs sans en être sûr, il n'est donc pas conseillé de lui faire confiance aveuglément mais il peut aider à faire une supposition réflechie. Il est fortement recommandé de regarder l'étiquette sur la RAM si possible.
  * Voir [here](https://www.reddit.com/r/overclocking/comments/ig9d76/thaiphoon_burner_cluelessly_guessing_memory_ics/) pour plus d'info.
* [Single rank 8Gb Hynix CJR](https://i.imgur.com/hbFyKB2.jpg).
* [Single rank 8Gb Micron Revision E](https://i.imgur.com/3pQjQIG.jpg) (source: Coleh#4297).
  * [SpecTek](https://www.micron.com/support/spectek-support) ICs are lower binned Micron ICs.
  * Note ésotérique : Beaucoup de gens ont commencé à dire Micron E-die voire même juste E-die. Le premier ne pose pas soucis mais dire uniquement E-die peut porter à confusion car l'apellation Lettre-die est généralement utilisée en parlant des ICs Samsung, i.e. 4Gbit Samsung E-die. Samsung est implicite lorsque l'on dit E-die, mais comme certainess personnes appellent les Micron Rev.E *E-die*, c'est sûrement une bonne idée d'indiquer le fabricant.
* [Dual rank 8Gb Samsung B-die](https://i.imgur.com/Nqn8s76.jpg).

### Etiquettes sur les barrettes

Parfois le rapport de Taiphoon burner ne vous dira pas l'IC ou se trompera dans sa supposition. Pour confirmer/réfuter ceci, vous pouvez regarder sur l'étiquette de vos barrettes. Actuellement, seul Corsair, G.Skill et Kingston ont une étiquette permettant d'indentifier l'IC utilisé.

#### Corsair Version Number
* Corsair possède un numéro de version à 3 chiffres sur l'étquette qui indique quel ICs sont sur la barrette.
* Le premier chiffre est le fabricant.
  * 3 = Micron
  * 4 = Samsung
  * 5 = Hynix
  * 8 = Nanya
* Le second chiffre est la densité.
  * 1 = 2Gb
  * 2 = 4Gb
  * 3 = 8Gb
  * 4 = 16Gb
* Le dernier chiffre est la revision.
* Regardez le [wiki r/overclocking](https://www.reddit.com/r/overclocking/wiki/ram/ddr4#wiki_corsair) pour une liste complète.
#### Le code 042 de G.Skill
* De la même manière que corsair, G.Skill utilise un code en 042 pour indiquer l'IC.
* Exemple: 04213X**8**8**1**0B
  * Le premier caractère en gras est la densité. 4 pour 4Gb, 8 pour 8Gb et S pour 16Gb.
  * Le second caractère en gras est le fabricant. 1 pour Samsung, 2 pour Hynix, 3 pour Micron, 4 pour PSC (powerchip), 5 pour Nanya et 9 pour JHICC.
  * Le dernier caractère est la révision.
  * Le code si dessus correspond à des ICs Samsung 8Gb B-die.
* Voir le [wiki r/overclocking](https://www.reddit.com/r/overclocking/wiki/ram/ddr4#wiki_new_markings_-_.22042_code.22_table) pour une liste complète.
#### Code Kingston
* Exemple: DPM**M**16A1823
  * La lettre en gras indique le fabricant. H pour Hynix, M pour Micron et S pour Samsung.
  * Les deux chiffres suivants indiquent les rangs. 08 = simple rang et 16 = double rang.
  * La lettre qui suit indique le mois de production. 1-9, A, B, C.
  * Les deux chiffres suivants indiquent l'année de production.
  * This is the code for dual rank Micron produced in October 2018.
  * Le code si dessus correspond à des ICs en double rang par Micron, produits en Octobre 2018.
* [Source](http://www.xtremesystems.org/forums/showthread.php?285750-Interesting-memory-deals-thread&p=5230258&viewfull=1#post5230258)

### Note sur les rangs logiques et la densité
* Single rank sticks usually clock higher than dual rank sticks, but depending on the benchmark the performance gain from rank interleaving<sup>1</sup> can be significant enough to outperform faster single rank sticks. [This can be observed in both synthetics and games](https://kingfaris.co.uk/ram).
* Les barrettes simple rang atteignent généralement des fréquences plus élevée que les barrettes double rangs
   * On recent platforms (Comet Lake and Zen3), BIOS and memory controller support for dual rank has improved greatly. On many Z490 boards, dual rank Samsung 8Gb B-die (2x16GB) will clock just as high as single-rank B-die, meaning you have all the performance gains of rank interleaving with little to no downsides.
   * <sup>1</sup>Rank interleaving allows the memory controller to parallelize memory requests, for example writing on one rank while the other is refreshing. The impact of this is easily observed in AIDA64 copy bandwidth. From the eyes of the memory controller, it doesn't matter whether the second rank is on the same DIMM (two ranks on one DIMM) or a different DIMM (two DIMM in one channel). It does however matter from an overclocking perspective when you consider memory trace layouts and BIOS support.
   * Having a second rank of the same IC also means that there are twice as many bank groups available. That means that short timings - such as RRD_S rather than RRD_L - can be used more often, as it's more likely for there to be a fresh bank group available. The long timing (L) is required when operating on the same bank group twice in a row, and when there are 7 other bank groups instead of 3 you have a lot more choice to avoid doing that.
   * It also means that there are twice as many banks, and thus twice as many memory rows can be open at any given time. It's more likely that the row that you need will be open.
You won't have to close row A, open row B and then close B to open A again as often.
You're held up by operations like RAS/RC/RCD (when waiting for a row to open because it was closed) and RP (when waiting for a row to close so that you can open another one) less often.
   * x16 configurations will have half as many banks and bank groups compared to the traditional x8 configurations which means less performance. See [buildzoid's video](https://www.youtube.com/watch?v=k6SIdxq2yxE) for more information.
* Density matters when determining how far your ICs can go. For example, 4Gb AFR and 8Gb AFR will not overclock the same despite sharing the same name. The same can be said for Micron Rev. B which exists as both 8Gb and 16Gb. The 16Gb ICs overclock better and are sold as both in 16GB and 8GB capacities despite the DIMMs using 8 chips. The 8GB sticks have their SPD modified and can be found in higher-end Crucial kits (BLM2K8G51C19U4B).
* As the count of ranks total in a system increases, so does the load on the memory controller. This usually means that more memory ranks will a require higher voltage, especially VCCSA on Intel and SOC voltage on AMD.

### Evolution en fonction de la tension (Voltage scaling)
* Le scaling veut simplement dire la réponse de la RAM face à une augmentation de tension. Est-ce qu'elle va pouvoir tenir une fréquence plus élevée ou des timings plus serrés.
* Sur beaucoup d'ICs, tCL scale avec la tension, ce qui signifie qu'augmenter la tension peut permettre de diminuer tCL. Au contraire, tRCP et/ou tRP ont plutôt tendance à ne pas être influencés par une augmentation de tension, si grande soit-elle.
De ce que je sais, tCL, tRCD, tRP et possiblement tRFC peuvent (ou non) scale avec la tension.
De la même manière, si un timing scale avec la tension, cela signifie que vous pouvez augmenter la tension pour utiliser la même valeur sur le timing mais à une fréquence plus élevée.
![CL11 Voltage Scaling](https://i.imgur.com/66GrCz3.png)
  * On peut voir que tCL scale de manière presque linéaire jusqu'à DDR4-2533 avec la tension sur de ICs Hynix 8Gb CJR.
  * Avec des ICs Samsung 8Gb B-die, tCL scale avec la tension de manière presque parfaitement linéaire.
  * tCL sur des ICs Micron Rev.E possède également un comportement similaire.
  * Ces données ont été adaptées en un [calculateur](https://www.desmos.com/calculator/psisrpx3oh). Changez les valeurs de *f* et *v* à la fréquence et tension que vous voulez et il vous donnera les fréquences et tensions atteignables pour un tCL donné (en supposant que tCL scale de manière linéaire jusqu'à 1.50V). Par exemple, DDR4-3200 CL à 1.35V devrait être capable de faire ~DDR4-3333 CL14 à 1.40V, ~DDR4-3533 CL14 à 1.45V et DDR4-3733 CL14 à 1.50V.

* Le scaling avec la tension de tRFC sur Samsung 8Gb B-die
![B-die tRFC Voltage Scaling](https://i.imgur.com/Wngug1M.png)
  * Ici vous pouvez voir que tRFC scale plutôt bien sur Samsung B-die

* Certains ICs anciens fabriqués par Micron (avant 8Gb Rev. E), sont connus pour scale de manière négative avec la tension. C'est-à-dire qu'ils deviennent instable à la même fréquence et timings simplement en augmentant la tension (Généralement au dessus de 1.35V).
* Voici un tableau des ICs que j'ai testé pour voir si les timings scalent avec la tension:

  | IC                 | tCL | tRCD | tRP | tRFC |
  | :-:                | :-: | :--: | :-: | :--: |
  | Hynix 8Gb AFR      | O   | N    | N   | ?    |
  | Hynix 8Gb CJR      | O   | N    | N   | O    |
  | Hynix 8Gb DJR      | O   | N    | N   | O    |
  | Micron 8Gb Rev. B  | O   | N    | N   | N    |
  | Micron 8Gb Rev. E  | O   | N    | N   | N    |
  | Micron 16Gb Rev. B  | O   | N    | N   | N    |
  | Nanya 8Gb B-die    | O   | N    | N   | N    |
  | Samsung 4Gb E-die  | O   | N    | N   | N    |
  | Samsung 8Gb B-die  | O   | O    | O   | O    |
  | Samsung 8Gb D-die  | O   | N    | N   | N    |
  * Les timings qui ne scalent pas avec la tension auront généralement besoin d'être augmentés en même temps que vous augmentez la fréquence pour maintenir la stabilité.
  
### Fréquence maximale attendue
* Ci-dessous se trouvent les fréquences maximales attendues pour certains de ICs les plus courants:

  | IC | Vitesse Maximale Attendue (MT/s) |
  | :-: | :------------: |
  | Hynix 8Gb AFR | 3600 |
  | Hynix 8Gb CJR | 4133<sup>1</sup> |
  | Hynix 8Gb DJR | 5000+ |
  | Nanya 8Gb B-die | 4000+ |
  | Micron 8Gb Rev. B | 3600 |
  | Micron 8Gb Rev. E | 5000+ |
  | Micron 16Gb Rev. B | 5000+ |
  | Samsung 4Gb E-die | 4200+ |
  | Samsung 8Gb B-die | 5000+ |
  | Samsung 8Gb D-die | 4200+ |
  * <sup>1</sup> Les ICs Hynix 8Gb CJR se sont montrés un peu irréguliers dans mes tests. J'ai testé 3 kit de RipJaws V 3600CL19 de 2x8Go. Un des kits n'a jamais réussi à dépasser DDR4-3600, un autre DDR4-3800 mais le dernier a tenu DDR4-4000 avec un tCL=16 et une tension de 1.45V.
  * Il ne faut pas s'attendre à ce que les ICs moins bien sélectionnés s'overclockent aussi bien que les ICs minutieusement choisis. Ceci est particulièrement vrai pour les [Samsung B-die](https://www.youtube.com/watch?v=rmrap-Jrfww).
  * Ces valeurs correspondent simplement aux capacités moyenne des ICs, en revanche certains autre facteurs, comme la carte mère ou le CPU, ont un impact considérable qui détermine si oui ou non les-dites valeurs sont atteignables.
  
### Tri sur le volet (Binning)
* Le binning signifie le trie des composants en fonction de performances
  Les fabricants séparent les ICs dans différents bacs (*bins* en anglais) en fonction de leur fréquence. D'où le terme binning
* G.Skill est un des fabricants bien connu pour faire beaucoup de binning. Souvent, différentes références de mémoire G.Skill vont correspondre au même bin (i.e. DDR4-3600 16-16-16-36 1.35V est le même bin que DDR4-3200 14-14-14-34 1.35V B-Die).
* Un kit de Samsung B-die bin pour 2400 15-15-15 sera considérablement moins bon qu'un kit bin pour 3200 14-14-14 ou 3000 14-14-14. Il ne faut pas s'attendre à ce qu'il scale de la même manière qu'un bon kit de B-die.
* Pour trouver quelle fréquence et timings correspondent à un meilleur bin pour des ICs identiques à la même tension, trouvez quel timing ne scale pas avec la tension.
  Ensuite divisez le fréquence par ce timing, plus la valeur est haute, plus le bin sera bon.
  * Par exemple, des kits Crucial Ballistix DDR4-3000 15-16-16 et DDR4-3200 16-18-18 utilisent tous des ICs Micron Rev.E. Diviser la fréquence par tCL nous donne la même valeur (200), donc ça veut dire que c'est le même bin ?
  Non car tCL scale avec la tension.
  tRCD ne scale pas, ce qui signifie qu'il doit être augmenté lorsque l'on augmente la fréquence.
  `3000 / 16 = 187.5` mais `3200 / 18 = 177.78`.
  Comme on peut le voir, DDR4-3000 15-16-16 est un bin plus serré que DDR4-3200 16-18-18. Cela signifie que le kit vendu pour DDR4-3000 15-16-16 sera probablement capable d'atteindre DDR4-3200 16-18-18 mais pas l'inverse. Dans cet exemple, la différence de fréquence et de timings est petite donc les kits s'overclockeront sûrement de manière similaire.
  
### Tension maximale recommandée pour une utilisation quotidienne
* [JEDEC JESD79-4B (p.174)](http://www.softnology.biz/pdf/JESD79-4B.pdf) spécifie que le maximum absolu est 1.50V.
  > Stresses greater than those listed under “Absolute Maximum Ratings” may cause permanent damage to the device. This is a stress rating only and functional operation of the device at these or any other conditions above those indicated in the operational sections of this specification is not implied. Exposure to absolute maximum rating conditions for extended periods may affect reliability.
* Cette valeur est officiellement le maximum des spécification pour la DDR4, que toute DDR4 doit suivre. En revanche, de nombreux ICs sont incapable de soutenir une telle tension. [Samsung 8Gb C-die](https://www.hardwareluxx.de/community/f13/samsung-8gbit-ddr4-c-die-k4a8g045wc-overclocking-ergebnisse-im-startbeitrag-1198323.html) peut se dégrader avec des tensions aussi basses que 1.35V dans certaines conditions de température de d'alimentation. D'un autre côté, il existe des ICs comme les 8Gb Hynix DJR ou encore 8Gb Samsung B-die qui ont été observés en utilisation quotienne au dessus de 1.55V. Je vous encourage à faire vos recherches pour déterminer quelles tensions sont sans danger pour vos ICs, ou bien restez à 1.35V ou similaire si cette valeur est inconnue. A cause de variations aléatoires, votre experience peut différer de celle des autres, donc restez prudents.
* Un facteur commun limitant pour la tension maximale sans danger est l'architecture de votre processeur. D'après l'organisme [JEDEC](https://www.jedec.org/standards-documents/dictionary/terms/output-stage-drain-power-voltage-vddq), VDDQ, la tension de données en sortie, est liée au VDD, également appelé VDIMM ou DRAM Voltage (Tension de la RAM). Cette tension interragit avec le PHY ou Couche Physique présente sur le CPU et peut causer des dérgadations à l'IMC sur long terme si trop élevé. De ce fait, l'utilisation quotidienne d'une tension RAM supérieure à 1.6V sur Ryzen 3000/5000 et 1.65V sur les processeurs Intel Lake-series n'est pas recommandée. Faite bien attention car la dégradation du PHY est difficile à mesurer et on ne s'en rend compte qu'une fois qu'il est trop tard.
* Il se peut que 1.6V soit sans danger pour une utilisation quotidienne car on trouve dans la [QVL de la B550 Unify-X](https://www.msi.com/Motherboard/support/MEG-B550-UNIFY-X#support-mem-20) des kits conçus pour 1.6V. Les ICs B-die, 8Gb Rev.E, DJR et 16Gb Rev.B *devraient* n'avoir aucun problème avec 1.6V quotidiennement, mais il est recommandé d'avoir un flux d'air approprié sur les barettes. Une tension plus élevée va induire des températures plus hautes et une température plus haute peut diminuer le seul de sûreté de la tension.
  
### Classement
* Ci-dessous la liste des puces les plus courantes classées par capacités de fréquence et de timings
  | Tier | ICs | Description |
  | :-:  | :-: | :--:        |
  | S | Samsung 8Gb B-Die | Meilleur IC DDR4 en général |
  | A | Hynix 8Gb DJR, Micron 8Gb Rev. E<sup>1</sup>, Micron 16Gb Rev. B | Très bons ICs. Connues pour ne pas avoir de limite de fréquences et pour avoir un bon scaling.|
  | B | Hynix 8Gb CJR, Samsung 4Gb E-Die, Nanya 8Gb B-Die | ICs haut-de-gamme avec possibilité de fonctionner à haute fréquence avec des bons timings.|
  | C | Hynix 8Gb JJR, Hynix 16Gb MJR, Hynix 16Gb CJR, Micron 16Gb Rev. E, Samsung 8Gb D-Die | ICs décents avec de bonnes performances et scaling correct.|
  | D | Hynix 8Gb AFR, Micron 8Gb Rev. B, Samsung 8Gb C-Die, Samsung 4Gb D-Die | ICs bad-de-gamme communéments trouvés dans les kits bons-marché. La plupart sont en fin de vie.|
  | F | Hynix 8Gb MFR, Micron 4Gb Rev. A, Samsung 4Gb S-Die, Nanya 8Gb C-Die | ICs très mauvais, incapable d'attendre même les standarts JEDEC les plus hauts.|
  * Partiellement basé sur [l'ancien classement de Buildzoid](https://www.reddit.com/r/overclocking/comments/8cjla5/the_best_manufacturerdie_of_ddr_ram_in_order/dxfgd4x/). Certains ICs ne sont pas inclus dans cette liste car le post se fait vieux.
  * <sup>1</sup>Les différentes revision de 8Gb Rev.E se distinguent principalement par leur tRCD minimum et leur fréquence max atteignable sans modification de VTT (Termination Voltage ou Tension des terminaisons) en restant stable. De manière générale, les revisions plus récentes de 8Gb Rev. E (C9BKV, C9BLL, etc.) fonctionneront avec un tRCD plus serré et accepterons des fréquences plus élevées sans toucher au VTT.
 
### La température et son effet sur la stabilité
* En général, plus la température de la RAM est élévée, plus elle sera instable à haute fréquences ou avec des timings serrés.
* Les timings tRFC sont très dépendants de la temperature car ils sont liés à la fuite de courante des condensateurs qui est affectée par la température. Des températures plus élevées demanderons un tRFC plus élevé. tRFC2 et tRFC4 sont des timings qui s'activent lorsque la RAM dépasse les 85°C, ce qui ne devrait pas arriver en conditions normales. En dessous de cette température, ces timings ne font rien.
* Les puces B-die sont sensibles à la température, la température idéale d'environ ~30-40°C. Certains sont capables de supporter des températures plus hautes.
* 8Gb Rev. E d'un autre côté, ne semble pas être aussi sensible à la température comme démontré par [buildzoid](https://www.youtube.com/watch?v=OeHEtULQg3Q).
* Vous pouvez vous trouver dans une situation où votre RAM est stable durant le test mais crash pendant que vous jouez. La raison de ceci est que votre CPU et/ou GPU font monter la température en jeu, ce qui augmente également la température de la RAM. C'est pourquoi il peut être une bonne idée de tester la RAM en stressant également la carte graphique pour simuler une utilisation en jeu.
 
## Contrôleur de mémoire intégré (IMC)
### Intel - LGA1151
* Le contrôleur de mémoire des processeurs Skylake d'Intel est plutôt bon donc il ne devrait pas être votre facteur limitant lors de l'overclocking.
* The Rocket Lake IMC, aside from the limitations regarding Gear 1 and Gear 2 memory support, has the strongest memory controller of all Intel consumer CPUs, and by a fair margin.
* Le contrôleur de mémoire des CPUs Rocket Lake, à part les limitations liées aux modes Gear1 et Gear2, est le meilleur IMC de tous les processeurs Intel et de loin.
* There are 2 voltages you need to change if overclocking RAM: system agent (VCCSA) and IO (VCCIO).  
  **DO NOT** leave these on auto, as they can pump dangerous levels of voltage into your IMC, potentially degrading or even killing it. Most of the time you can keep VCCSA and VCCIO the same, but [sometimes too much can harm stability](https://i.imgur.com/Bv8617y.png) (credits: Silent_Scone). I wouldn't recommend going above 1.25V on each.  
  Below are my suggested VCCSA and VCCIO for 2 single rank DIMMs:

  | Effective Speed (MT/s) | VCCSA/VCCIO (V) |
  | :-------------: | :-------------: |
  | 3000 - 3600 | 1.10 - 1.15 |
  | 3600 - 4000 | 1.15 - 1.20 |
  | 4000 - 4200 | 1.20 - 1.25 |
  | 4200 - 4400 | 1.25 - 1.30 |
  * With more DIMMs and/or dual rank DIMMs, you may need higher VCCSA and VCCIO than suggested.
* tRCD and tRP are linked, meaning if you set tRCD 16 but tRP 17, both will run at the higher timing (17). This limitation is why many ICs don't do as well on Intel and why B-die is a good match for Intel.
  * On Asrock and EVGA UEFIs, they're combined into tRCDtRP. On ASUS UEFIs, tRP is hidden. On MSI and Gigabyte UEFIs, tRCD and tRP are visible but setting them to different values just sets both of them to the higher value.
* Expected memory latency range: 40ns - 50ns.
   * Expected memory latency range for Samsung B-Die: 35ns - 45ns.
   * Overall, latency varies between generations due to a difference in die size (ringbus). As a result, a 9900K will have slightly lower latency than a 10700K at the same settings since the 10700K has the same die as a 10900K.
   * Latency is affected by the RTLs and IOLs. Generally speaking, higher quality boards and overclocking oriented boards will be more direct in their routing of the memory traces and will likely have lower RTLs and IOLs. On some motherboards, changing RTLs and IOLs have no effect.
  
### AMD - AM4
Some terminology:
* MCLK: Real memory clock (half of the effective RAM speed). For example, for DDR4-3200 the MCLK is 1600MHz.
* FCLK: Infinity Fabric clock.
* UCLK: Unified memory controller clock. Half of MCLK when MCLK and FCLK are not equal (desynchronised, 2:1 mode).
* On Zen and Zen+, MCLK == FCLK == UCLK. However on Zen2 and Zen3, you can specify FCLK. If MCLK is 1600MHz (DDR4-3200) and you set FCLK to 1600MHz, UCLK will also be 1600MHz unless you set MCLK:UCLK ratio to 2:1 (also known as UCLK DIV mode, etc.). However, if you set FCLK to 1800MHz, UCLK will run at 800MHz (desynchronised).

* Ryzen 1000 and 2000's IMC can be a bit finnicky when overclocking and can't hit as high frequencies as Intel can. Ryzen 3000 and 5000's IMCs are much better and are more or less on par with Intel's newer Skylake based CPUs ie. 9th gen and 10th gen.
* SOC voltage is the voltage to the IMC and like with Intel, it's not recommended to leave it on auto. Typical ranges for this value range around 1.00V and 1.10V. Higher values are generally acceptable, and may be necessary in stabilizing higher capacity memory and may aid in attaining FCLK stability.
* By contrast, when SOC voltage is set too high, memory instability can occur. This negative scaling typically occurs between 1.15V and 1.25V on most Ryzen CPUs.
  > There are clear differences in how the memory controller behaves on the different CPU specimens. The majority of the CPUs will do DDR4-3466 or higher at 1.050V SoC voltage, however the difference lies in how the different specimens react to the voltage. Some of the specimens seem scale with the increased SoC voltage, while the others simply refuse to scale at all or in some cases even illustrate negative scaling. All of the tested samples illustrated negative scaling (i.e. more errors or failures to train) when higher than 1.150V SoC was used. In all cases the maximum memory frequency was achieved at =< 1.100V SoC voltage.  
  [~ The Stilt](https://forums.anandtech.com/threads/ryzen-strictly-technical.2500572/page-72#post-39391302)
* On Ryzen 3000, there's also CLDO_VDDG (commonly abbreviated to VDDG, not to be confused with CLDO_VDD**P**), which is the voltage to the Infinity Fabric. SOC voltage should be at least 40mV above CLDO_VDDG as CLDO_VDDG is derived from SOC voltage.
  > Most cLDO voltages are regulated from the two main power rails of the CPU. In case of cLDO_VDDG and cLDO_VDDP, they are regulated from the VDDCR_SoC plane.
Because of this, there are couple rules. For example, if you set the VDDG to 1.100V, while your actual SoC voltage under load is 1.05V the VDDG will stay roughly at 1.01V max.
Likewise if you have VDDG set to 1.100V and start increasing the SoC voltage, your VDDG will raise as well. I don't have the exact figure, but you can assume that the minimum drop-out voltage (Vin-Vout) is around 40mV.
Meaning you ACTUAL SoC voltage has to be at least by this much higher, than the requested VDDG for it to take effect as it is requested.  
Adjusting the SoC voltage alone, unlike on previous gen. parts doesn't do much if anything at all.
The default value is fixed 1.100V and AMD recommends keeping it at that level. Increasing the VDDG helps with the fabric overclocking in certain scenarios, but not always.
1800MHz FCLK should be doable at the default 0.9500V value and for pushing the limits it might be beneficial to increase it to =< 1.05V (1.100 - 1.125V SoC, depending on the load-line).  
  [~ The Stilt](https://www.overclock.net/forum/28031966-post35.html)  
  * On AGESA 1.0.0.4 or newer VDDG is separated into VDDG IOD and VDDG CCD for the I/O die and the chiplets parts, respectively.

* Below are the expected memory frequency ranges for 2 single rank DIMMs, provided your motherboard and ICs are capable:

  | Ryzen | Expected Effective Speed (MT/s) |
  | :---: | :----------------------: |
  | 1000 | 3000 - 3600 |
  | 2000 | 3400 - 3800<sup>1</sup> |
  | 3000 | 3600 - 3800 (1:1 MCLK:FCLK) <br/> 3800+ (2:1 MCLK:FCLK) |
  * With more DIMMs and/or dual rank DIMMs, the expected frequency can be lower.
  * <sup>1</sup>3600+ is typically achieved on a 1 DIMM per channel (DPC)/2 DIMM slot motherboard and with a very good IMC.
    * See [here](https://docs.google.com/spreadsheets/d/1dsu9K1Nt_7apHBdiy0MWVPcYjf6nOlr9CtkkfN78tSo/edit#gid=1814864213).
  * <sup>1</sup>DDR4-3400 - DDR4-3533 is what most, if not all, Ryzen 2000 IMCs should be able to hit.
    > On the tested samples, the distribution of the maximum achievable memory frequency was following:  
    > DDR4-3400 – 12.5% of the samples   
    > DDR4-3466 – 25.0% of the samples  
    > DDR4-3533 – 62.5% of the samples  
    [~ The Stilt](https://forums.anandtech.com/threads/ryzen-strictly-technical.2500572/page-72#post-39391302)
  * 2 CCD Ryzen 3000 CPUs (3900X and 3950X) seem to prefer 4 single rank sticks over 2 dual rank sticks.
    > For 2 CCD SKUs, 2 DPC SR configuration seems to be the way to go.
    > Both the 3600 and 3700X did 1800MHz UCLK on 1 DPC DR config, but most likely due to the discrepancy of the two CCDs in 3900X, it barely does 1733MHz on those DIMMs.
    > Meanwhile with 2 DPC SR config there is no issue in reaching 1866MHz FCLK/UCLK.  
[~ The Stilt](https://www.overclock.net/forum/10-amd-cpus/1728758-strictly-technical-matisse-not-really-26.html#post28052342)
* tRCD is split into tRCDRD (read) and tRCDWR (write). Usually, tRCDWR can go lower than tRCDRD, but I haven't noticed any performance improvements from lowering tRCDWR. It's best to keep them the same.
* Geardown mode (GDM) is automatically enabled above DDR4-2666, which forces even tCL, even tCWL, even tRTP, even tWR and CR 1T. If you want to run odd tCL, disable GDM. If you're unstable try running CR 2T, but that may negate the performance gain from dropping tCL, and may even be less stable than GDM enabled.
  * For example, if you try to run DDR4-3000 CL15 with GDM enabled, CL will be rounded up to 16.
  * In terms of performance: GDM disabled CR 1T > GDM enabled CR 1T > GDM disabled CR 2T.
* On single CCD Ryzen 3000 CPUs (CPUs below 3900X), write bandwidth is halved.
  > In memory bandwidth, we see something odd, the write speed of AMD's 3700X, and that's because of the CDD to IOD connection, where the writes are 16B/cycle on the 3700X, but it's double that on the 3900X. AMD said this let them conserve power, which accounts for part of the lower TDP AMD aimed for. AMD says applications rarely do pure writes, but it did hurt the 3700X's performance in one of our benchmarks on the next page.  
  [~ TweakTown](https://www.tweaktown.com/reviews/9051/amd-ryzen-3900x-3700x-zen2-review/index3.html)
* Expected memory latency range:

  | Ryzen | Latency (ns) |
  | :---: | :----------: |
  | 1000 | 65 - 75 |
  | 2000 | 60 - 70 |
  | 3000 | 65 - 75 (1:1 MCLK:FCLK) <br/> 75+ (2:1 MCLK:FCLK) |
* On Ryzen 3000 and 5000, high enough FCLK can overcome the penalties from desynchronising MCLK and FCLK, provided that you can lock your UCLK to MCLK.
  
  ![Chart](https://i.imgur.com/F9HpkO2.png) 
  * (Credits: [Buildzoid](https://www.youtube.com/watch?v=10pYf9wqFFY))
  
# Overclocking
* **Disclaimer**: The silicon lottery will affect your overclocking potential so there may be some deviation from my suggestions.
* **Warning**: Data corruption is possible when overclocking RAM. It's advised to run `sfc /scannow` every so often to ensure any corrupted system files are fixed.
* The overclocking process is pretty simple and boils down to 3 steps:
  * Set very loose (high) timings.
  * Increase DRAM frequency until unstable.
  * Tighten (lower) timings.

## Trouver un point de départ
1. On Intel, start off with 1.15V VCCSA and VCCIO.  
   On AMD, start off with 1.10V SOC.
   * SOC voltage might be named differently depending on the manufacturer.
     * Asrock: CPU VDDCR_SOC Voltage. If you can't find that, you can use SOC Overclock VID hidden in the AMD CBS menu.
       * [VID values](https://www.reddit.com/r/Amd/comments/842ehb/asrock_ab350_pro4_guide_bios_overclocking_raven/).
     * Asus: VDDCR SOC.
     * Gigabyte: (Dynamic) Vcore SOC.
       * Note that dynamic Vcore SOC is an offset voltage. The base voltage can change automatically when increasing DRAM frequency. +0.100V at DDR4-3000 might result in 1.10V actual, but +0.100V at DDR4-3400 might result in 1.20v actual.
     * MSI: CPU NB/SOC.
2. Set DRAM voltage to 1.40V. If you're using ICs that roll over above 1.35V, set 1.35V.
   * "Roll over" means that the IC becomes more unstable as you increase the voltage, sometimes to the point of not even POSTing.
   * ICs that are known to roll over above 1.35V include but is not limited to: 8Gb Samsung C-die, older Micron/SpecTek ICs (before 8Gb Rev. E).
3. Set primary timings to 16-20-20-40 (tCL-tRCD-tRP-tRAS) and tCWL to 16.
   * Most ICs need loose tRCD and/or tRP which is why I recommend 20.
   * See [this post](https://redd.it/ahs5a2) for more information on these timings.
4. Increase the DRAM frequency until it doesn't boot into Windows any more. Keep in mind the expectations detailed above.
   * If you're on Intel, a quick way of knowing if you're unstable is to examine the RTLs and IOLs. Each group of RTLs and IOLs correspond to a channel. Within each group, there are 2 values which correspond to each DIMM.  
   [Asrock Timing Configurator](https://i.imgur.com/EQBl2wd.jpg)  
   As I have my sticks installed in channel A slot 2 and channel B slot 2, I need to look at D1 within each group of RTLs and IOLs.  
   RTLs should be no more than 2 apart and IOLs should be no more than 1 apart.  
   In my case, RTLs are 53 and 55 which are exactly 2 apart and IOLs are both 7.
   Note that having RTLs and IOLs within those ranges doesn't mean you're stable.
   * If you're on Ryzen 3000 or 5000, make sure that the Infinity Fabric frequency (FCLK) is set to half your effective DRAM frequency.
5. Run a memory tester of your choice.  
   * Windows will use ~2000MB so make sure to account for that when entering the amount of RAM to test, if the test has manual input. I have 16GB of RAM and usually test 14000MB.
   * Minimum recommended coverage/runtime:
     * MemTestHelper (HCI MemTest): 200% per thread.
     * Karhu RAMTest: 5000%.
       * In the advanced tab, make sure CPU cache is set to enabled. This will speed up testing by ~20%.
       * Testing for 6400% coverage and a 1 hour duration has an error cover rate of 99,41% and 98,43%, respectively ([Source - FAQ section](https://www.karhusoftware.com/ramtest/)).
     * TM5 anta777 Extreme: 3 cycles.
       * Runtime varies with density. For 16GB RAM, it usually takes between 1.5-2 hours. If you run 32GB RAM you can set the 12th row of the config (Time (%)) to half and you'll get roughly the same runtime as 16GB.
     * OCCT Memory: 30 minutes each for SSE and AVX.
6. If you crash/freeze/BSOD or get an error, drop the DRAM frequency by a notch and test again.
7. Save your overclock profile in your UEFI.
8. From this point on you can either: try to go for a higher frequency or work on tightening the timings.
   * Keep in mind the expectations detailed above. If you're at the limit of your ICs and/or IMC it's best just to tighten the timings.
   
## Essayer des fréquences plus hautes
* This section is applicable if you're not at the limit of your motherboard, ICs and IMC.  
  This section is not for those who are having trouble stabilising frequencies within the expected range.
     * Note that some boards have auto rules that can stifle your progress, an example being tCWL = tCL - 1 which can lead to uneven values of tCWL. Reading the [Miscellaneous Tips](#miscellaneous-tips) might give you insight into your particular platform and your motherboards functionality.
1. Intel:
   * Increase VCCSA and VCCIO to 1.25V.
   * Set command rate (CR) to 2T if it isn't already.
   * Set tCCDL to 8. Asus UEFIs don't expose this timing.
   
   Ryzen 3000:
   * Desynchronising MCLK and FCLK can incur a massive latency penalty, so you're better off tightening timings to keep your MCLK:FCLK 1:1. See [AMD - AM4](#amd---am4) for more information.
   * Otherwise, set FCLK to whatever is stable (1600MHz if you're unsure).
2. Loosen primary timings to 18-22-22-42 and set tCWL to 18.
3. Increase DRAM voltage to 1.45v if it is safe for your IC.
5. Follow steps 4-7 from [Finding a Baseline](#finding-a-baseline).
6. Proceed to [Tightening Timings](#tightening-timings).
   
## Resserrer les timings
* Make sure to run a memory test and benchmark after each change to ensure performance is improving.
  * I would recommend to benchmark 3 to 5 times and average the results, as memory benchmarks can have a bit of variance.
  * Theoretical maximum bandwidth (MB/s) = `Transfers per clock * Actual Clock * Channel Count * Bus Width * Bit to Byte ratio`.
       * Transfers per clock refers to the number of data transfers that can occur in one full memory clock cycle. This occurs twice per cycle on DDR RAM, on the rising and falling clock edges.
       * Actual Clock is the real clock of the memory, simply measured in MHz. This is generally shown as the real memory frequency by programs such as CPU-Z.
       * Channel Count is the number of memory channels active on your CPU.
       * Bus Width is the width of each memory channel, measured in bits. Since DDR1, this is 64 bits.
       * Bit to Byte ratio is a constant 1/8, or 0.125
 
    | Effective Memory Speed (MT/s) | Max Dual Channel Bandwidth (MB/s) |
    | :-------------: | :------------------------: |
    | 3000 | 48000 |
    | 3200 | 51200 |
    | 3400 | 54440 |
    | 3466 | 55456 |
    | 3600 | 57600 |
    | 3733 | 59728 |
    | 3800 | 60800 |
    | 4000 | 64000 |
    
    * Your read and write bandwidth should be 90% - 98% of the theoretical maximum bandwidth.
      * On single CCD Ryzen 3000-5000 CPUs, write bandwidth should be 90% - 98% of half of the theoretical maximum bandwidth.  
        It is possible to hit half of the theoretical maximum write bandwidth. See [here](https://redd.it/cgc9bh).
      * Percentage of theoretically max bandwidth is inversely proportional to most memory timings. Generally speaking, as RAM timings are tightened, this value will increase.

1. I would recommend to tighten some of the secondary timings first, as they can speed up memory testing.  
   My suggestions:
   
   | Timing | Safe | Tight | Extreme |
   | ------ | ---- | ----- | ------- |
   | tRRDS tRRDL tFAW | 6, 6, 24 | 4, 6, 16 | 4, 4, 16 |
   | tWR | 16 | 12 | 10 |
   * The minimum value for which lowering tFAW will have an effect on the performance of RAM is `tRRDS * 4` or `tRRDL * 4`, whichever is lower.
   * You don't have to run all of the timings at one preset. You might only be able to run tRRDS tRRDL tFAW at the tight preset, but you may be able to run tWR at the extreme preset.
   * On some Intel motherboards, tWR in the UEFI does nothing and instead needs to be controlled through tWRPRE (sometimes tWRPDEN). Dropping tWRPRE by 1 will drop tWR by 1, following the rule tWR = tWRPRE - tCWL - 4.
     
2. Next is tRFC. Default for 8Gb ICs is 350**ns** (note the units).
   * Note: Tightening tRFC too much can result in system freezes/lock ups.
   * tRFC is the number of cycles for which the DRAM capacitors are "recharged" or refreshed. Because capacitor charge loss is proportional to temperature, RAM operating at higher temperatures may need substantially higher tRFC values.
   * To convert to ns: `2000 * timing / ddr_freq`.  
   For example, tRFC 250 at DDR4-3200 is `2000 * 250 / 3200 = 156.25ns`.
   * To convert from ns (this is what you would type in your UEFI): `ns * ddr_freq / 2000`.  
   For example, 180ns at DDR4-3600 is `180 * 3600 / 2000 = 324`, so you would type 324 in your UEFI.
   * Below are the typical tRFC in ns for the common ICs:
   
     | IC | tRFC (ns) |
     | :-: | :-------: |
     | Hynix 8Gb AFR | 260 - 280 |
     | Hynix 8Gb CJR | 260 - 280 |
     | 8Gb DJR | 260 - 280 |
     | Micron 8Gb Rev. E | 280 - 310 |
     | Micron 16Gb Rev. B | 290 - 310 |
     | Samsung 8Gb B-Die | 120 - 180 |
     | Samsung 8Gb C-Die | 300 - 340 |
     
   * For all other ICs, I would recommend doing a binary search to find the lowest stable tRFC.  
   For example, say your tRFC is 630. The next tRFC you should try is half of that (315). If that is unstable, you know that your lowest tRFC is somewhere between 315 and 630, so you try the midpoint (`(315 + 630) / 2 = 472.5, round down to 472`). If that is stable, you know that your lowest tRFC is between 315 and 472, so you try the midpoint and so on.
   * [tRFC table by Reous](https://www.hardwareluxx.de/community/threads/hynix-8gbit-ddr4-cjr-c-die-h5an8g8ncjr-djr-2020-update.1206340/)(bottom of page).
3. Here are my suggestions for the rest of the secondaries:

   | Timing | Safe | Tight | Extreme |
   | :----: | :--: | :---: | :-----: |
   | tWTRS tWTRL | 4 12 | 4 10 | 4 8 |
   | tRTP | 12 | 10 | 8 |
   | tCWL<sup>1</sup> | tCL | tCL - 1 | tCL - 2 |
   * On Intel, tWTRS/L should be left on auto and controlled with tWRRD_dg/sg respectively. Dropping tWRRD_dg by 1 will drop tWTRS by 1. Likewise with tWRRD_sg. Once they're as low as you can go, manually set tWTRS/L.
   * On Intel, changing tCWL will affect tWRRD_dg/sg and thus tWTR_S/L. If you lower tCWL by 1 you need to lower tWRRD_dg/sg by 1 to keep the same tWTR values. Note that this might also affect tWR per the relationship described earlier.
   * <sup>1</sup>Some motherboards don't play nice with odd tCWL. For example, I'm stable at 4000 15-19-19 tCWL 14, yet tCWL 15 doesn't even POST. Another user has had similar experiences. Some motherboards may seem fine but have issues with it at higher frequencies (Asus). Manually setting tCWL equal to tCL if tCL is even or one below if tCL is uneven should alleviate this (eg. if tCL = 18 try tCWL = 18 or 16, if tCL = 17 try tCWL = 16).
   * The extreme preset is not the minimum floor in this case. tRTP can go as low as 5 (6 with Gear Down Mode on), while tWTRS/L can go as low as 1/6. Some boards are fine doing tCWL as low as tCL - 6. Keep in mind that this *will* increase the load on your memory controller.
   * On AMD, tCWL can often be set to tCL - 2 but is known to require higher tWRRD.
   
4. Now for the tertiaries:
    * If you're on AMD, refer to [this post](https://redd.it/ahs5a2).  
      My suggestion:
  
       | Timing | Safe | Tight | Extreme |
       | ------ | ---- | ----- | ------- |
       | tRDRDSCL tWRWRSCL | 4 4 | 3 3 | 2 2 |
     
        * A lot of ICs are known to have issues with low SCLs. Values such as 2 are extremely difficult for all but ICs such as Samsung 8Gb B-Die. These values are not necessarily linked, and values such as 5 are acceptable. Mixing and matching is possible, and more often than not tRDRDSCL will be the one that needs to be run 1 or even 2 values higher. Values above 5 greatly hurt bandwidth and so their use is not advised.
     
    * If you're on Intel, tune the tertiaries one group at a time.  
      My suggestions:
      
      | Timing | Safe | Tight | Extreme |
      | ------ | ---- | ----- | ------- |
      | tRDRD_sg/dg/dr/dd | 8/4/8/8 | 7/4/7/7 | 6/4/6/6 |
      | tWRWR_sg/dg/dr/dd | 8/4/8/8 | 7/4/7/7 | 6/4/6/6 |
      * For tWRRD_sg/dg, see step 3. For tWRRD_dr/dd, drop them both by 1 until you get instability or performance degrades.
      * For tRDWR_sg/dg/dr/dd, drop them all by 1 until you get instability or performance degrades. You can usually run them all the same e.g. 9/9/9/9.
        * Setting these too tight can cause system freezes.
      * Note that dr only affects dual rank sticks, so if you have single rank sticks you can ignore this timing. In the same way, dd only needs to be considered when you run two DIMMs per channel. You can also set them to 0 or 1 if you really wanted to.  
        [These](https://i.imgur.com/61ZtPpR.jpg) are my timings on B-die, for reference.
      * For dual rank setups (see [notes on ranks](#a-note-on-ranks-and-density)):
         * tRDRD_dr/dd can be lowered a step further to 5 for a large bump in read bandwidth.
         * tWRWR_sg 6 can cause write bandwidth regression over tWRWR_sg 7, despite being stable.
    
5. Drop tCL by 1 until it's unstable.
   * On AMD, if GDM is enabled drop tCL by 2.   
 
6. On Intel, drop tRCD and tRP by 1 until unstable.  

   On AMD, drop tRCD by 1 until unstable. Repeat with tRP.
   * Note: More IMC voltage may be necessary to stabilise tighter tRCD.
   
7. Set `tRAS = tRCD(RD) + tRTP`. Increase if unstable.
   * This is the absolute minimum tRAS can be.  
   ![tRAS](https://user-images.githubusercontent.com/16512539/118769121-298a6000-b8c3-11eb-8793-7d90e885ca67.png)
   Here you can see that tRAS is the time between ACT and PRE commands.
     * ACT to READ = tRCD
     * READ to PRE = tRTP
     * Hence, tRAS = tRCD + tRTP.


8. Set `tRC = tRP + tRAS`. Increase if unstable.
   * tRC is only available on AMD and some Intel UEFIs.
   * On Intel UEFIs, tRC does seem to be affected by tRP and tRAS, even if it is hidden.
     * (1) [tRP 19 tRAS 42](https://i.imgur.com/gz1YDcO.png) - fully stable.
     * (2) [tRP 19 tRAS 36](https://i.imgur.com/lHjbLjC.png) - instant error.
     * (3) [tRP 25 tRAS 36](https://i.imgur.com/7c46Qes.png) - stable up to 500%.
     * In (1) and (3), tRC is 61 and isn't completely unstable. However, in (2) tRC is 55 and RAMTest finds an error instantly. This indicates that my RAM can do low tRAS, but not low tRC. Since tRC is hidden, I need higher tRAS to get higher tRC to ensure stability.

9. Increase tREFI until it's unstable. The binary search method explained in finding the lowest tRFC can also be applied here.  
   Otherwise, here are my suggestions:
   | Timing | Safe | Tight | Extreme |
   | ------ | ---- | ----- | ------- |
   | tREFI | 32768 | 40000 | Max (65535 or 65534) |
   * It's typically not a good idea to increase tREFI too much as ambient temperature changes (e.g. winter to summer) can be enough to cause instability.
   * Keep in mind that running max tREFI can corrupt files so tread with caution.

10. Finally onto command rate.

    AMD:
    * Getting GDM disabled and CR 1 stable can be pretty difficult but if you've come this far down the rabbit hole it's worth a shot.
    * If you can get GDM disabled and CR 1 stable without touching anything then you can skip this section.
    * CR 1 becomes significantly harder to run as the frequency increases. Oftentimes, running CR 2 can help with achieving higher frequencies.
    * On AMD, Gear Down Mode will override Command Rate. For this reason, disabling Gear Down Mode in order to set CR 2 may be beneficial to overall stability.
    
    1. One possibility is to set the drive strengths to 60-20-20-24 and setup times to 63-63-63.
       * Drive strengths are ClkDrvStr, AddrCmdDrvStr, CsOdtDrvStr and CkeDrvStr.
       * Setup times are AddrCmdSetup, CsOdtSetup and CkeSetup.
    2. If you can't POST, adjust the setup times until you can (you should adjust them all together).
    3. Run a memory test.
    4. Adjust setup times then drive strengths if unstable.
    * [My stable GDM off CR 1 settings](https://i.imgur.com/z547RLa.jpg)
    5. Oftentimes, a drive strength above 24 ohms may hurt stability. Furthermore, running non-zero setup times is rarely needed, however may aid in the stabilization of CR 1.
   
    Intel:
    * If below DDR4-4400, try setting CR to 1T. If that doesn't work, leave CR on 2T.
    * On Asus Maximus boards enabling Trace Centering can help greatly with pushing CR 1T to higher frequencies.

11. You can also increase DRAM voltage to drop timings even more. Keep in mind the [voltage scaling characteristics of your ICs](#voltage-scaling) and the [maximum recommended daily voltage](#maximum-recommended-daily-voltage).
    
## Conseils divers
* Usually a 200MHz increase in effective DRAM frequency negates the latency penalty of loosening tCL, tRCD and tRP by 1, but has the benefit of higher bandwidth.  
  For example, DDR4-3000 15-17-17 has the same latency as DDR4-3200 16-18-18, but DDR4-3200 16-18-18 has higher bandwidth. This is typically after initial tuning has been completed, and not at XMP.
* Generally speaking, frequency should be prioritized over tighter timings, so long as performance is not negatively impacted by FCLK sync, Command Rate or Memory Gear mode.
* Secondary and tertiary timings (except for tRFC) don't really change much, if at all, across the frequency range. If you have stable secondary and tertiary timings at DDR4-3200, you could probably run them at DDR4-3600, even DDR4-4000, provided your ICs, IMC and motherboard are capable.

### Intel
* Loosening tCCDL to 8 may help with stability, especially above DDR4-3600. This does not bring a significant penalty to latency but may affect memory read and write bandwidth considerably.
* Higher cache (aka uncore, ring) frequency can increase bandwidth and reduce latency.
* After you've finished tightening the timings, you can increase IOL offsets to reduce IOLs. Make sure to run a memory test after. More info [here](https://hwbot.org/newsflash/3058_advanced_skylake_overclocking_tune_ddr4_memory_rtlio_on_maximus_viii_with_alexaros_guide).
  * In general, RTL and IOL values impact memory performance. Lowering them will increase bandwidth and decrease latency [quite significantly](https://i.imgur.com/wS2ZqUx.png). Lower values will in some cases also help with stability and lower memory controller voltage requirements. Some boards train them very well on their own. Some boards allow for easy tuning while other boards simply ignore any user input.
  * If all else fails, you can try manually decreasing the RTL and IOL pair.
* For Asus Maximus boards:
   * Play around with the Maximus Tweak Modes, sometimes one will post where the other does not.
   * You can enable Round Trip Latency under Memory Training Algorithms to let the board attempt to train RTL and IOL values.
   * If you can't boot, you can try tweaking the skew control values.  
     More info [here](https://rog.asus.com/forum/showthread.php?47670-Maximus-7-Gene-The-road-to-overclocking-memory-without-increasing-voltage).
* tXP (and subsequently PPD) has a major impact on AIDA64 memory latency.
* RTT Wr, Park and Nom can have a massive impact on overclocking. The ideal values may depend on your board, your memory IC and density. The "optimal" values will let you clock higher with less memory controller voltage. Some boards reveal the auto values (MSI) while others don't (Asus). Finding the perfect combination is time-consuming but very helpful for advanced tuning.
* On some motherboards, enabling XMP can allow for better overclocking.
  * Thanks to Bored and Muren for finding and verifying this on their Asrock motherboards.

### AMD
* Try playing around with ProcODT if you can't boot. This setting determines the processor on-die termination impedance. According to [Micron](https://www.micron.com/support/~/media/D546161C2C6140BCB0BAEE954AA53433.pdf), higher settings of ProcODT can lead to more stable RAM, but trade off potentially needing higher voltages. On Ryzen 1000 and 2000, you should try values between 40Ω and 68.6Ω due to the considerably weaker memory controller. 
On Ryzen 3000 and 5000, [1usmus](https://www.overclock.net/forum/13-amd-general/1640919-new-dram-calculator-ryzena-1-5-1-overclocking-dram-am4-membench-0-7-dram-bench-480.html#post28049664) suggests 28Ω - 40Ω. Lower settings may be harder to run but potentially helps with voltage requirements. Higher values may aid with stability, though according to [Micron](https://media-www.micron.com/-/media/client/global/documents/products/technical-note/dram/tn4040_ddr4_point_to_point_design_guide.pdf?la=en&rev=d58bc222192d411aae066b2577a12677), values of ODT above 60Ω are only suitable for extremely weak memory controllers and lower power solutions.
This seems to line up with [The Stilt's](https://www.overclock.net/forum/10-amd-cpus/1728758-strictly-technical-matisse-not-really-26.html) settings.
  > Phy at AGESA defaults, except ProcODT of 40.0Ohm, which is an ASUS auto-rule for Optimem III.
* Lower SOC voltage and/or VDDG IOD may help with stability.
* On Ryzen 3000 and 5000, higher CLDO_VDDP can help with stability above DDR4-3600.
  > Increasing cLDO_VDDP seems beneficial > 3600MHz MEMCLKs, as increasing it seems to improve the margins and hence help with potential training issues. Source: [The Stilt](https://www.overclock.net/forum/10-amd-cpus/1728758-strictly-technical-matisse-not-really-26.html).
 
  > This value is not to exceed 1.10V on Ryzen 3000 and 5000, and should always be restricted to at least 0.10V less than DRAM Voltage. Source: [AMD](https://community.amd.com/t5/blogs/community-update-4-let-s-talk-dram/ba-p/415902)
* When pushing FCLK around 1800MHz intermittent RAM training errors may be alleviated or completely eliminated by increasing VDDG CCD.

# Liens utiles
## Benchmarks
* [Impact of RAM on Intel's Skylake desktop architecture by KingFaris](https://kingfaris.co.uk/ram)
* [RAM timings and their influence on games and applications (AMD) by Reous](https://www.hardwareluxx.de/community/threads/ram-timings-und-deren-einfluss-auf-spiele-und-anwendungen-amd-update-23-05-2020.1269156/)
## Information
* [r/overclocking Wiki - DDR4](https://www.reddit.com/r/overclocking/wiki/ram/ddr4)
* [Demystifying Memory Overclocking on Ryzen: OC Guidelines and Explaining Subtimings, Resistances, Voltages, and More! by varexos717](https://redd.it/ahs5a2)
* [HardwareLUXX Ryzen RAM OC Thread](https://www.hardwareluxx.de/community/f13/ryzen-ram-oc-thread-moegliche-limitierungen-1216557.html)
* [Ryzen 3000 Memory / Fabric (X370/X470/X570) by elmor](https://www.overclock.net/forum/13-amd-general/1728878-ryzen-3000-memory-fabric-x370-x470-x570.html)
* [Intel Memory Overclocking Quick Reference by sdch](https://www.overclock.net/forum/27784556-post7836.html)
* [The road to overclocking memory without increasing voltage by Raja@ASUS](https://rog.asus.com/forum/showthread.php?47670-Maximus-7-Gene-The-road-to-overclocking-memory-without-increasing-voltage)
* [Advanced Skylake Overclocking: Tune DDR4 Memory RTL/IO on Maximus VIII with Alex@ro's Guide](https://hwbot.org/newsflash/3058_advanced_skylake_overclocking_tune_ddr4_memory_rtlio_on_maximus_viii_with_alexaros_guide)
* [BSOD codes when OC'ing and possible actions](https://www.reddit.com/r/overclocking/comments/atwtt5/psa_bsod_codes_when_ocing_and_possible_actions/)
