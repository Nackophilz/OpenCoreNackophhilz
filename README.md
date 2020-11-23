<span align="center">
<h1>AMD Vanilla OpenCore</h1>
</span>

### Langues: Français (actuelle) [English](languages/README.md), [Ukrainian](languages/README_UA.md), [Russian](languages/README_RUS.md), [Traditional Chinese](languages/README_CHT.md), [Spanish](languages/README_ES.md), [Simplified Chinese](languages/README_CHS.md), [Vietnamese](languages/README_VI.md)
Patch binaire de Kernel afin d’activer le support natif de macOS sur les CPU AMD.

###Options:
- Offrir la possibilité de lancer macOS sur des CPU AMD.
- Activer iMessage, Siri, FaceTime, Continuity etc.
- Stable comparer au kernel XNU modifier.

### Désavantage:
- Pas de support pour le 32 bit (OPEMU) en 10.14 et en dessous.

### CPU AMD SUPPORTE : 
| Famille | Nom de code| Exemples |
|--------|---------|----------|
|   [15h](https://github.com/AMD-OSX/AMD_Vanilla/tree/opencore/15h_16h)  | Bulldozer | FX Series|
|   [16h](https://github.com/AMD-OSX/AMD_Vanilla/tree/opencore/15h_16h)  | Jaguar | A Series (including AM4 A-Series) |
|   [17h and 19h](https://github.com/AMD-OSX/AMD_Vanilla/tree/opencore/17h_19h) | Zen | Ryzen, 1st, 2nd + 3rd Gen Threadripper, Athlon 2xxGE |<br />

### Information sur le patch « PAT »
Le patch activé par défaut est le « Algrey’s » (patch original). Il fonctionne pour tout les GPUs et n’affecte pas l’audio, mais n’améliore pas non plus les performances.
L’autre choix est le patch de « Shaneee’s » il augmentera les performances du GPU sur un GPU AMD, mais peut faire cesser de fonctionner les GPUs Nvidia. Il cause aussi des problèmes sur la sortie audio en HDMI et en DP (DisplayPort)
Faites votre choix concernant le patch à utiliser. Mais surtout ne les utilisez pas en même temps.

### Note sur les systèmes TRX40 : 
Le fait de désactiver le patch « mtrr_update_action » à démontrer une amélioration des performances GPU sur certains systèmes que nous avons testés. Si vous voulez essayer, nous vous recommandons de le faire sur une clef USB avec OpenCore avant de le mettre sur votre EFI de disque pour vous assurer que tout est ok.Il peut il y avoir certains problème selon les système que nous n’avons pas tester. Cela relève de votre responsabilitée.

### Version de  macOS supportées : 
- High Sierra 10.13.x
- Mojave 10.14.x
- Catalina 10.15.x
- Big Sur 11.00.X

### Instructions : 
- Téléchargez macOS High Sierra, Mojave ou Catalina depuis le App Store.
- Insérez une clef USB.
- Exécuter l’une des commandes ci-dessous dans l’application Terminal pour rendre la clef USB bootable sur macOS.
```
NOTE: Faites attention à remplacer 'MyVolumeName' avec le nom de votre clef USB dans les commandes du dessous.

## High Sierra
sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolumeName

## Mojave
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolumeName

## Catalina
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolumeName

## Big Sur
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolumeName
```
- Installez OpenCore sur votre clef USB. (Les release ce trouve ici : https://github.com/acidanthera/OpenCorePkg/releases)
- Lisez la documentation OpenCore ici : https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf) pour configurer votre « config.plist ». Nous ne fournissons pas de configuration par défaut.
- Appliquez les patches que nous vous proposons, selon votre famille de CPU (ex: `15h_16h` ou `17h`) dans votre « config.plist » OpenCore, et éditez le selon vos besoins.

### Notes spéciales :
- Les patch requiert OpenCore 0.6.1 et ultérieurs.
- Pour avoir de l’aide concernant la configuration OpenCore lisez la documentation et aller voir [ici](https://dortania.github.io/OpenCore-Install-Guide/) ou rejoignez notre serveur Discord.
- Pour les CPU 15_16h sur le macOS Mojave :
  - Quand vous booter pour la première fois sur macOS Mojave, le système va rebooter après l’écran « Données et vie privée ». Pour résoudre ce problème suivez [cette](https://www.insanelymac.com/forum/topic/335877-amd-mojave-kernel-development-and-testing/?do=findComment&comment=2658085) procédure, en dessous de l’entête « UPDATE-2 ».
  - Sur macOS Mohave certaines pages web peuvent crasher lorsque qu’elles charges (ex: brew.sh, facebook.com). Pour résoudre ce problème suivez [cette](https://www.insanelymac.com/forum/topic/335877-amd-mojave-kernel-development-and-testing/?do=findComment&comment=2661857) prcoédure, en dessous de l’entête « UPDATE-5 ».
- Pour booter sur la version 10.15 vous devez faire attention à deux choses :
  - Si vous avez un périphérique EC avec l’ID « PNP0C09 » dans votre DSDT alors macOS va bloquer dans sa première phase de démarrage. Pour résoudre ce problème, vous devez vous assure que votre périphérique EC est désactiver en mettant sont statut à « Zero ». Vous pouvez utiliser un DSDT modifié [SSDT-EC0.aml](./Extra/SSDT-EC0.aml) pour faire cela et si vous voulez comprendre comment cela marche vous pouvez vous référer à ce [document](https://github.com/acidanthera/OpenCorePkg/blob/5e020bb06b33f12fa8b404cc3d1effaa5fbc00ea/Docs/AcpiSamples/SSDT-EC.dsl#L33). <br> -ou- <br> Vous pouvez changer l’ID du périphérique EC, en utilisant ce patch ACPI : 
    ```
        Comment             Find        Replace
    PNP0C09 to PNPFFFF    41D00C09     41D0FFFF
    ```
  - Quand vous utilisez le SMBIOSes « MacPro6,1 », « MacPro7,1 », or « iMacPro1,1 », « AppleIntelMCEReporter.kext » macOS peut faire un kernel panic. Pour éviter ce problème vous devez utiliser un SMBIOS différent OU désactiver le Kext venant d’[ici](./Extra/).

### Crédits
- [AlGrey](https://github.com/AlGreyy) pour son idée et la création des patchs.
- [XLNC](https://github.com/XLNCs) pour maintenir les patchs et les rendre compatibles avec de nombreuses version de macOS.
- Sinetek, Andy Vandijck, spakk, Bronya, Tora Chi Yo, Shaneee et beaucoup d’autres pour partager leurs connaissances sur les kernel AMD/XNU.
- [0xD81CF](https://github.com/0xD81CF), [doesprintfwork](https://github.com/doesprintfwork), [erikjara](https://github.com/erikjara) et [Nackophilz](https://github.com/Nackophilz)pour la traduction du Lisez-moi.
