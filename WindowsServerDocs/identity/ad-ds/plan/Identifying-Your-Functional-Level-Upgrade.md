---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: 識別功能等級升級
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8aee6a5560edfae656582b3c2812ca69c6b90f57
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812039"
---
# <a name="identifying-your-functional-level-upgrade"></a>識別功能等級升級

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以提高網域和樹系功能等級之前，您必須評估您目前的環境，並找出最符合您組織的需求的功能等級需求。 藉由識別網域中您樹系中，網域控制站位於中每個網域、 作業系統和 service pack，每個網域控制站正在執行和您想要升級網域的日期評估目前的環境控制站。 如果您計劃要淘汰的網域控制站，請確定您了解這種方式會對您的環境的完整影響。  
  
在下列情況下可能會讓您從舊版的 Windows 伺服器作業系統升級為 Windows Server 2008 或 Windows Server 2008 R2 的功能等級：  
  
-   硬體不足  
  
-   執行與 Windows Server 2008 或 Windows Server 2008 R2 不相容的防毒程式的網域控制站   
  
-   使用不會在 Windows Server 2008 或 Windows Server 2008 R2 執行特定版本的程式   
  
-   您不必升級最新的 service pack 的程式  
  
記錄這項資訊可協助您識別要以確定您有功能完整的 Windows Server 2008 或 Windows Server 2008 R2 環境所採取的步驟。  
  
目前環境評估之後，您必須識別您組織適用的功能等級升級。 這些選項可用：  
  
-   Windows Server 2008 或 Windows Server 2008 R2 的 Windows 2000 原生模式環境   
  
-   Windows Server 2008 或 Windows Server 2008 R2 的 Windows Server 2003 樹系   
  
-   新的 Windows Server 2008 樹系  
  
-   新的 Windows Server 2008 R2 樹系  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>升級原生的 Windows 2000 Active Directory 樹系中的功能等級  
在 Windows 2000 原生環境中，只包含 Windows 2000 型網域控制站，功能等級設定預設會將下列層級，而它們是停留在這些層級直到您手動提高：  
  
-   Windows 2000 原生網域功能等級  
  
-   Windows 2000 樹系功能等級  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有樹系等級與網域等級功能，您必須將這個 Windows 2000 環境升級至 Windows Server 2008 或 Windows Server 2008 R2。 您可以執行這項升級，下列兩種方法之一：  
  
-   將新安裝的 Windows Server 2008 為基礎或 Windows Server 2008 R2-基礎到樹系的網域控制站，然後淘汰所有執行 Windows 2000 的網域控制站。  
  
-   執行就地升級的所有現有網域控制站執行 Windows 2000 樹系中，執行 Windows Server 2003 的網域控制站。 然後，執行就地升級至 Windows Server 2008 或 Windows Server 2008 R2 的網域控制站。 如需詳細資訊，請參閱 <<c0> [ 至 Windows Server 2008 AD DS 網域升級 Active Directory 網域\[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 是 x64 型作業系統。 如果您的伺服器執行 Windows Server 2003 的 x64 型版本，您可以成功地執行這台電腦的作業系統為 Windows Server 2008 R2 就地升級。 如果您的伺服器執行 Windows Server 2003 x86 版本，您無法升級到 Windows Server 2008 R2 的這台電腦。  
  
若要使用的 Windows Server 2008 或 Windows Server 2008 R2 的網域層級功能，而不需 Windows Server 2008 或 Windows Server 2008 R2 升級您的整個 Windows 2000 樹系，只有網域功能等級提高至 Windows Server 2008 或 Windows Server 2008R2。  
  
> [!NOTE]  
> 您提高網域功能等級之前，您必須升級至 Windows Server 2008 或 Windows Server 2008 R2 的該網域中的所有 Windows 2000 網域控制站。  
  
您將樹系中的所有 Windows 2000 網域控制站都取代執行 Windows Server 2008 或 Windows Server 2008 R2 網域控制站之後，您可以樹系功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。 自動，就會引發所有樹系中的網域設定為 Windows 2000 原生或更高版本以 Windows Server 2008 或 Windows Server 2008 R2 的功能等級。  
  
如需有關提升樹系和網域功能等級，以及執行這些工作的程序，請參閱[部署 Windows Server 2008 樹系根網域\[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>升級在 Windows Server 2003 Active Directory 樹系功能等級  
在 Windows Server 2003 環境中，只有 Windows Server 2003 為基礎的網域控制站所組成，功能等級設定預設會將下列層級，而它們是停留在這些層級直到您手動提高：  
  
-   Windows 2000 原生網域功能等級  
  
-   Windows 2000 樹系功能等級  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有樹系等級與網域等級功能，您必須將這個 Windows Server 2003 環境升級至 Windows Server 2008 或 Windows Server 2008 R2。 您可以執行這項升級，下列兩種方法之一：  
  
-   將新安裝的 Windows Server 2008 為基礎或 Windows Server 2008 R2-基礎到樹系的網域控制站然後淘汰所有執行 Windows Server 2003 的網域控制站或將它們升級到 Windows Server 2008 或 Windows Server 2008 R2。  
  
-   執行所有現有網域控制站執行 Windows Server 2003 網域控制站執行 Windows Server 2008 或 Windows Server 2008 R2 就地的升級。 如需詳細資訊，請參閱 <<c0> [ 至 Windows Server 2008 AD DS 網域升級 Active Directory 網域\[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 是 x64 型作業系統。 如果您的伺服器執行 Windows Server 2003 的 x64 型版本，您可以成功地執行這台電腦的作業系統為 Windows Server 2008 R2 就地升級。 如果您的伺服器執行 Windows Server 2003 x86 版本，您無法升級此電腦來執行 Windows Server 2008 R2。  
  
若要使用的所有 Windows Server 2008 或 Windows Server 2008 R2 的網域層級功能，而不需要將整個 Windows Server 2003 樹系升級至 Windows Server 2008 或 Windows Server 2008 R2，只有網域功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。  
  
> [!NOTE]  
> 您提高網域功能等級之前，您必須升級至 Windows Server 2008 或 Windows Server 2008 R2 的該網域中的所有 Windows Server 2003 網域控制站。  
  
您升級到 Windows Server 2008 或 Windows Server 2008 R2 的樹系中的所有 Windows Server 2003 網域控制站之後，您可以樹系功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。 自動，就會引發樹系中所有設定為 Windows Server 2008 或 Windows Server 2008 R2 的 Windows Server 2003 的網域功能等級。  
  
如需有關提升樹系和網域功能等級，以及執行這些工作的程序，請參閱[部署 Windows Server 2008 樹系根網域\[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>升級中新的 Windows Server 2008 樹系功能等級  
當您在新的 Windows Server 2008 樹系中安裝第一個網域控制站時，預設會將下列層級中，已設定的功能等級，直到您手動提高保持這些層級：  
  
-   Windows 2000 原生網域功能等級  
  
-   Windows 2000 樹系功能等級  
  
功能等級設定為這些預設等級，讓您加入新的 Windows Server 2008 樹系中的 Windows 2000 或 Windows Server 2003 網域控制站中的選項。 建立樹系根網域之後，您新增至 Windows Server 2008 樹系的每個網域的網域功能等級是設為 Windows 2000 原生。 不過，如果您想在您執行 Windows Server 2008 的新 Windows Server 2008 環境中的所有網域控制站，設定樹系功能等級，然後按一下 網域功能等級，為您 fores 中安裝第一個網域控制站時，Windows Server 2008t。 如此一來節省時間，並可讓 Windows Server 2008 中的所有樹系等級與網域等級功能。  
  
> [!IMPORTANT]  
> 如果在 Windows Server 2008 功能運作的樹系層級，而您嘗試在 Windows Server 2003 為基礎的成員伺服器上安裝 Active Directory 或 Windows 2000 的成員伺服器，安裝會失敗。  
  
如需有關提升樹系和網域功能等級，以及執行這些工作的程序，請參閱[部署 Windows Server 2008 樹系根網域\[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>升級中新的 Windows Server 2008 R2 樹系功能等級  
當您在新的 Windows Server 2008 R2 樹系中安裝第一個網域控制站時，預設會將下列層級中，已設定的功能等級，直到您手動提高保持這些層級：  
  
-   Windows Server 2003 網域功能等級  
  
-   Windows Server 2003 樹系功能等級  
  
讓您選擇將 Windows Server 2003 網域控制站新增至新的 Windows Server 2008 R2 樹系功能等級設定為這些預設等級。 建立樹系根網域之後，您新增至 Windows Server 2008 R2 樹系的每個網域的網域功能等級設為 Windows Server 2003。 不過，如果您想在您新的 Windows Server 2008 R2 環境，以執行 Windows Server 2008 R2 中的所有網域控制站，設定樹系功能等級，然後按一下 網域功能等級為 Windows Server 2008 R2 安裝時的第一個網域控制站 yo您的樹系中。 如此一來節省時間，並可讓 Windows Server 2008 R2 中的所有樹系等級與網域等級功能。  
  
> [!IMPORTANT]  
> 如果在 Windows Server 2008 R2 的功能等級的樹系運作，而且您嘗試在 Windows Server 2008 上安裝 Active Directory 為基礎或以 Windows Server 2003 為基礎的成員伺服器，或在 Windows 2000 為基礎的成員伺服器上，安裝將會失敗。  
  
如需有關提升樹系和網域功能等級，以及執行這些工作的程序，請參閱[部署 Windows Server 2008 樹系根網域\[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
> [!NOTE]  
> 雖然 ADMT v3.1 必須安裝 Windows Server 2008 上，您可以使用 ADMT v3.1 移轉物件由一或多個 Windows Server 2008 R2 網域控制站的網域。 如需詳細資訊，請參閱 <<c0> [ 文章 976659](https://go.microsoft.com/fwlink/?LinkId=180398)在 Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=180398)。  
  


