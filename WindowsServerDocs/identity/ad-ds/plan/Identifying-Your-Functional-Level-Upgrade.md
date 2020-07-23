---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: 識別功能等級升級
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a67c1f5e0c144cecf1c66aaba8b16aa612270ad3
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959850"
---
# <a name="identifying-your-functional-level-upgrade"></a>識別功能等級升級

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您必須先評估目前的環境，並找出最符合您組織需求的功能等級需求，才能提高網域和樹系功能等級。 識別樹系中的網域、位於每個網域中的網域控制站、每個網域控制站執行的作業系統和 service pack，以及您打算升級網域控制站的日期，以評估您目前的環境。 如果您打算淘汰網域控制站，請確定您瞭解這對您的環境所造成的完整影響。

下列情況可能會讓您無法將舊版的 Windows Server 作業系統升級至 Windows Server 2008 或 Windows Server 2008 R2 功能等級：

- 硬體不足

- 執行防毒程式且與 Windows Server 2008 或 Windows Server 2008 R2 不相容的網域控制站

- 使用不是在 Windows Server 2008 或 Windows Server 2008 R2 上執行的特定版本程式

- 需要將程式升級為最新的 Service Pack

記錄此資訊可協助您找出要採取的步驟，以確保您擁有功能完整的 Windows Server 2008 或 Windows Server 2008 R2 環境。

評估目前的環境之後，您必須識別適用于貴組織的功能等級升級。 可用的選項如下：

- Windows 2000 原生模式環境到 Windows Server 2008 或 Windows Server 2008 R2

- Windows Server 2003 樹系到 Windows Server 2008 或 Windows Server 2008 R2

- 新的 Windows Server 2008 樹系

- 新的 Windows Server 2008 R2 樹系

## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>升級原生 Windows 2000 Active Directory 樹系中的功能等級
在只包含 Windows 2000 網域控制站的 Windows 2000 原生環境中，功能等級預設會設定為下列層級，而它們會保留在這些層級，直到您手動加以引發為止：

- Windows 2000 原生網域功能等級

- Windows 2000 樹系功能等級

若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有樹系層級和網域層級功能，您必須將此 Windows 2000 環境升級至 Windows Server 2008 或 Windows Server 2008 R2。 您可以透過下列其中一種方式來執行此升級：

- 將新安裝的 Windows Server 2008 型或 Windows Server 2008 R2 型網域控制站引進樹系中，然後淘汰所有執行 Windows 2000 的網域控制站。

- 執行將樹系中所有執行 Windows 2000 的現有網域控制站就地升級至執行 Windows Server 2003 的網域控制站。 然後，執行這些網域控制站到 Windows Server 2008 或 Windows Server 2008 R2 的就地升級。 如需詳細資訊，請參閱[將 Active Directory 網域升級至 Windows server 2008 和 Windows server 2008 R2 AD DS 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10))。

    > [!IMPORTANT]
    >  Windows Server 2008 R2 是以 x64 為基礎的作業系統。 如果您的伺服器執行的是 x64 版本的 Windows Server 2003，您可以順利地將這部電腦的作業系統就地升級到 Windows Server 2008 R2。 如果您的伺服器執行的是 x86 版本的 Windows Server 2003，就無法將這部電腦升級到 Windows Server 2008 R2。

若要使用 Windows Server 2008 或 Windows Server 2008 R2 網域層級功能，而不將整個 Windows 2000 樹系升級至 Windows Server 2008 或 Windows Server 2008 R2，請只將網域功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。

> [!NOTE]
> 提高網域功能等級之前，您必須將該網域中的所有 Windows 2000 型網域控制站升級至 Windows Server 2008 或 Windows Server 2008 R2。

將樹系中所有 Windows 2000 架構的網域控制站取代為執行 Windows Server 2008 或 Windows Server 2008 R2 的網域控制站之後，您可以將樹系功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。 這麼做會自動將樹系中所有設定為 Windows 2000 原生或更高版本的網域功能等級提升至 Windows Server 2008 或 Windows Server 2008 R2。

如需提高樹系和網域功能等級，以及執行這些工作之程式的詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>升級 Windows Server 2003 Active Directory 樹系中的功能等級
在僅由 Windows Server 2003 型網域控制站組成的 Windows Server 2003 環境中，功能等級預設會設定為下列層級，而且它們會維持在這些層級，直到您手動加以引發為止：

- Windows 2000 原生網域功能等級

- Windows 2000 樹系功能等級

若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有樹系層級和網域層級功能，您必須將此 Windows Server 2003 環境升級至 Windows Server 2008 或 Windows Server 2008 R2。 您可以透過下列其中一種方式來執行此升級：

- 將新安裝的 Windows Server 2008 型或 Windows Server 2008 R2 型網域控制站引進樹系中，然後淘汰執行 Windows Server 2003 的所有網域控制站，或將它們升級至 Windows Server 2008 或 Windows Server 2008 R2。

- 將所有執行 Windows Server 2003 的現有網域控制站就地升級至執行 Windows Server 2008 或 Windows Server 2008 R2 的網域控制站。 如需詳細資訊，請參閱[將 Active Directory 網域升級至 Windows server 2008 和 Windows server 2008 R2 AD DS 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10))。

> [!IMPORTANT]
> Windows Server 2008 R2 是以 x64 為基礎的作業系統。 如果您的伺服器執行的是 x64 版本的 Windows Server 2003，您可以順利地將這部電腦的作業系統就地升級到 Windows Server 2008 R2。 如果您的伺服器執行的是 x86 版本的 Windows Server 2003，您就無法將這部電腦升級為執行 Windows Server 2008 R2。

若要使用所有的 Windows Server 2008 或 Windows Server 2008 R2 網域層級功能，而不將整個 Windows Server 2003 樹系升級至 Windows Server 2008 或 Windows Server 2008 R2，請只將網域功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。

> [!NOTE]
> 提高網域功能等級之前，您必須先將該網域中的所有 Windows Server 2003 網域控制站升級至 Windows Server 2008 或 Windows Server 2008 R2。

將樹系中的所有 Windows Server 2003 網域控制站升級至 Windows Server 2008 或 Windows server 2008 R2 之後，您可以將樹系功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。 這麼做會自動將樹系中所有網域（設定為 Windows Server 2003）的功能等級提升至 Windows Server 2008 或 Windows Server 2008 R2。

如需提高樹系和網域功能等級，以及執行這些工作之程式的詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>升級新的 Windows Server 2008 樹系中的功能等級
當您在新的 Windows Server 2008 樹系中安裝第一個網域控制站時，預設會將功能等級設定為下列層級，而且它們會維持在這些層級，直到您手動加以引發為止：

- Windows 2000 原生網域功能等級

- Windows 2000 樹系功能等級

功能等級是在這些預設層級設定，可讓您選擇將 Windows 2000 或 Windows Server 2003 架構的網域控制站新增至新的 Windows Server 2008 樹系。 建立樹系根域之後，您新增至 Windows Server 2008 樹系的每個網域的網域功能等級都會設定為 Windows 2000 native。 不過，如果您要讓新的 Windows Server 2008 環境中的所有網域控制站執行 Windows Server 2008，請將樹系功能等級和網域功能等級設定為 Windows Server 2008 （當您在樹系中安裝第一個網域控制站時）。 這麼做可以節省時間，並啟用 Windows Server 2008 中的所有樹系層級和網域層級功能。

> [!IMPORTANT]
> 如果樹系操作的是 Windows Server 2008 功能等級，而且您嘗試在 Windows Server 2003 型成員伺服器或 Windows 2000 成員伺服器上安裝 Active Directory，則安裝會失敗。

如需提高樹系和網域功能等級，以及執行這些工作之程式的詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>升級新的 Windows Server 2008 R2 樹系中的功能等級
當您在新的 Windows Server 2008 R2 樹系中安裝第一個網域控制站時，預設會將功能等級設定為下列層級，而且它們會維持在這些層級，直到您手動加以引發為止：

- Windows Server 2003 網域功能等級

- Windows Server 2003 樹系功能等級

功能等級是在這些預設層級設定，可讓您選擇將 Windows Server 2003 網域控制站新增至新的 Windows Server 2008 R2 樹系。 建立樹系根域之後，您新增至 Windows Server 2008 R2 樹系的每個網域的網域功能等級都會設定為 Windows Server 2003。 不過，如果您想要讓新的 Windows Server 2008 R2 環境中的所有網域控制站執行 Windows Server 2008 R2，請將樹系功能等級和網域功能等級設定為 Windows Server 2008 R2 （當您在樹系中安裝第一個網域控制站時）。 這麼做可以節省時間，並啟用 Windows Server 2008 R2 中的所有樹系層級和網域層級功能。

> [!IMPORTANT]
> 如果樹系在 Windows Server 2008 R2 功能等級運作，而您嘗試在 Windows Server 2008 或 Windows Server 2003 型成員伺服器或 Windows 2000 成員伺服器上安裝 Active Directory，則安裝會失敗。

如需提高樹系和網域功能等級，以及執行這些工作之程式的詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

> [!NOTE]
> 雖然 ADMT 3.1 版必須安裝在 Windows Server 2008 上，但是您可以使用 ADMT 3.1 版，將物件遷移到一個或多個 Windows Server 2008 R2 網域控制站所裝載的網域。 如需詳細資訊，請參閱 Microsoft 知識庫中的文章976659，[當您使用 ADMT 3.1 遷移至包含 Windows Server 2008 R2 網域控制站的網域時，可能會發生的已知問題](https://support.microsoft.com/help/976659/)。
