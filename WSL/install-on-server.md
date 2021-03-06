---
title: Installer le sous-système Linux sur Windows Server
description: Instructions d’installation du sous-système Linux sur Windows Server.
keywords: BashOnWindows, bash, wsl, windows, sous-système windows pour linux, sous-système windows, ubuntu, windows server
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: high
ms.openlocfilehash: 86fd7de0ef45af760f46bb2a18932f513b813609
ms.sourcegitcommit: 1b6191351bbf9e95f3c28fc67abe4bf1bcfd3336
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83270883"
---
# <a name="windows-server-installation-guide"></a>Guide d’installation sur Windows Server

Le sous-système Windows pour Linux peut être installé sur Windows Server 2019 (version 1709) et ultérieur. Ce guide vous accompagne tout au long des étapes d’activation de WSL sur votre ordinateur.

## <a name="enable-the-windows-subsystem-for-linux"></a>Activer le sous-système Windows pour Linux

Avant de pouvoir exécuter les distributions Linux sur Windows, vous devez activer la fonctionnalité facultative « Sous-système Windows pour Linux » et redémarrer.

Ouvrez PowerShell en tant qu’administrateur et exécutez :

```powershell
    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

```

**Si vous recherchez une compatibilité à 100 % des appels système et des performances d’E/S plus rapides, lisez ce qui suit pour installer WSL 2.**

WSL 2 est disponible uniquement dans Windows 10, version 2004, build 19041 ou ultérieure. Vous devez [mettre à jour votre version de Windows](ms-settings:windowsupdate) et [rejoindre le programme Windows Insider](https://insider.windows.com/insidersigninboth/) sur l’anneau « Release Preview » jusqu’à la publication de la version publique fin mai.

**Si vous continuez avec WSL 1, redémarrez votre ordinateur lorsque vous y êtes invité et poursuivez l’installation [ici](./install-on-server.md#download-a-linux-distribution)**

## <a name="enable-the-virtual-machine-platform-optional-component"></a>Activer le composant facultatif Plateforme de machine virtuelle

Veillez à ce que le composant facultatif « Plateforme de machine virtuelle » soit installé. Pour ce faire, exécutez la commande suivante dans PowerShell :

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## <a name="download-a-linux-distribution"></a>Télécharger une distribution Linux

Suivez [ces instructions](install-manual.md) pour télécharger votre distribution Linux préférée.

## <a name="extract-and-install-a-linux-distribution"></a>Extraire et installer une distribution Linux

Maintenant que vous avez téléchargé une distribution Linux, afin d’extraire son contenu et de l’installer manuellement, procédez comme suit :

1. Extrayez le contenu du package `<distro>.appx` à l’aide de PowerShell :

    ```powershell
    Rename-Item .\Ubuntu.appx .\Ubuntu.zip
    Expand-Archive .\Ubuntu.zip .\Ubuntu
    ```

2. Exécutez l’application de lancement de distribution dans le dossier cible. Le lanceur est généralement nommé `<distro>.exe` (par exemple, `ubuntu.exe`).

    ![Distribution Ubuntu développée sur Windows Server](media/server-appx-expand.png)

> [!CAUTION]
> **Échec de l’installation avec l’erreur 0x8007007e** : Si vous recevez cette erreur, votre système ne prend pas en charge WSL. Veillez à exécuter la build 16215 ou ultérieure de Windows. [Vérifiez votre build](troubleshooting.md#check-your-build-number). [Vérifiez également que WSL est activé](troubleshooting.md#confirm-wsl-is-enabled) et que votre ordinateur a été redémarré après l’activation de la fonctionnalité.  

3. Ajoutez votre chemin de distribution à la variable PATH de chemin d’environnement Windows (`C:\Users\Administrator\Ubuntu` dans cet exemple), à l’aide de PowerShell :

```powershell
$userenv = [System.Environment]::GetEnvironmentVariable("Path", "User")
[System.Environment]::SetEnvironmentVariable("PATH", $userenv + ";C:\Users\Administrator\Ubuntu", "User")
```

Vous pouvez maintenant lancer votre distribution à partir de n’importe quel chemin en tapant `<distro>.exe`. Par exemple : `ubuntu.exe`.

Maintenant qu’elle est installée, vous devez [initialiser votre nouvelle instance de distribution](initialize-distro.md) avant de l’utiliser.
