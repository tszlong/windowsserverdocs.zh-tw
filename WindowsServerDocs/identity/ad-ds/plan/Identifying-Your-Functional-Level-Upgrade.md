---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: 識別功能等級升級
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3a3422b49c54894084d8a3442d1f805003076f8d
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068410"
---
# <a name="identifying-your-functional-level-upgrade"></a>識別功能等級升級

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在您可以提高網域和樹系功能等級之前，您必須先評估目前的環境，並找出最符合您組織需求的功能等級需求。 藉由識別樹系中的網域、每個網域中的網域控制站、每個網域控制站正在執行的作業系統和 service pack，以及您打算升級網域控制站的日期，來評估您目前的環境。 如果您打算淘汰網域控制站，請確定您瞭解在您的環境中將會有的完整影響。

下列情況可能會讓您無法將舊版 Windows Server 作業系統升級至 Windows Server 2008 或 Windows Server 2008 R2 功能等級：

- 硬體不足

- 執行防毒程式且與 Windows Server 2008 或 Windows Server 2008 R2 不相容的網域控制站

- 使用不在 Windows Server 2008 或 Windows Server 2008 R2 上執行的版本特定程式

- 需要以最新的 service pack 升級程式

記錄此資訊可協助您找出要採取的步驟，以確保您具備功能完整的 Windows Server 2008 或 Windows Server 2008 R2 環境。

在您評估目前的環境之後，您必須識別適用于您組織的功能等級升級。 可用的選項如下：

- Windows 2000 原生模式環境到 Windows Server 2008 或 Windows Server 2008 R2

- Windows Server 2003 樹系至 Windows Server 2008 或 Windows Server 2008 R2

- 新的 Windows Server 2008 樹系

- 新的 Windows Server 2008 R2 樹系

## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>升級原生 Windows 2000 Active Directory 樹系中的功能等級
在僅包含 Windows 2000 網域控制站的 Windows 2000 原生環境中，功能等級預設為下列層級，而且它們會維持在這些層級，直到您以手動方式引發它們為止：

- Windows 2000 原生網域功能等級

- Windows 2000 樹系功能等級

若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有樹系層級和網域層級功能，您必須將此 Windows 2000 環境升級至 Windows Server 2008 或 Windows Server 2008 R2。 您可以透過下列其中一種方式來執行這項升級：

- 將新安裝的 Windows Server 2008 型或 Windows Server 2008 R2 型網域控制站引進樹系，然後淘汰所有執行 Windows 2000 的網域控制站。

- 針對執行 Windows Server 2003 的網域控制站，執行樹系中所有執行 Windows 2000 的現有網域控制站的就地升級。 然後，執行這些網域控制站的就地升級至 Windows Server 2008 或 Windows Server 2008 R2。 如需詳細資訊，請參閱 [將 Active Directory 網域升級至 Windows server 2008 和 Windows server 2008 R2 AD DS 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10))。

    > [!IMPORTANT]
    >  Windows Server 2008 R2 是 x64 架構的作業系統。 如果您的伺服器執行 x64 型版本的 Windows Server 2003，您可以成功地將此電腦的作業系統就地升級到 Windows Server 2008 R2。 如果您的伺服器執行的是 x86 版本的 Windows Server 2003，則無法將這部電腦升級為 Windows Server 2008 R2。

若要使用 Windows Server 2008 或 Windows Server 2008 R2 網域層級功能，而不將整個 Windows 2000 樹系升級到 Windows Server 2008 或 Windows Server 2008 R2，請只將網域功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。

> [!NOTE]
> 提高網域功能等級之前，您必須將該網域中的所有 Windows 2000 型網域控制站升級到 Windows Server 2008 或 Windows Server 2008 R2。

以執行 Windows Server 2008 或 Windows Server 2008 R2 的網域控制站取代樹系中所有的 Windows 2000 網域控制站之後，您可以將樹系功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。 這樣做會自動將樹系中所有網域的功能層級，從設定為 Windows 2000 native 或更高版本設為 Windows Server 2008 或 Windows Server 2008 R2。

如需提高樹系和網域功能等級的詳細資訊，以及執行這些工作之程式的詳細資訊，請參閱 [部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>升級 Windows Server 2003 Active Directory 樹系中的功能等級
在僅包含 Windows Server 2003 型網域控制站的 Windows Server 2003 環境中，功能等級預設會設定為下列層級，而且它們會維持在這些等級，直到您以手動方式引發它們為止：

- Windows 2000 原生網域功能等級

- Windows 2000 樹系功能等級

若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有樹系層級和網域層級功能，您必須將此 Windows Server 2003 環境升級至 Windows Server 2008 或 Windows Server 2008 R2。 您可以透過下列其中一種方式來執行這項升級：

- 將新安裝的 Windows Server 2008 型或 Windows Server 2008 R2 型網域控制站引進樹系，然後淘汰所有執行 Windows Server 2003 的網域控制站，或將其升級為 Windows Server 2008 或 Windows Server 2008 R2。

- 將執行 Windows Server 2003 的所有現有網域控制站就地升級到執行 Windows Server 2008 或 Windows Server 2008 R2 的網域控制站。 如需詳細資訊，請參閱 [將 Active Directory 網域升級至 Windows server 2008 和 Windows server 2008 R2 AD DS 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10))。

> [!IMPORTANT]
> Windows Server 2008 R2 是 x64 架構的作業系統。 如果您的伺服器執行 x64 型版本的 Windows Server 2003，您可以成功地將此電腦的作業系統就地升級到 Windows Server 2008 R2。 如果您的伺服器執行的是 x86 版本的 Windows Server 2003，您就無法將這部電腦升級為執行 Windows Server 2008 R2。

若要使用所有 Windows Server 2008 或 Windows Server 2008 R2 網域層級功能，而不將整個 Windows Server 2003 樹系升級到 Windows Server 2008 或 Windows Server 2008 R2，請只將網域功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。

> [!NOTE]
> 提高網域功能等級之前，您必須先將該網域中的所有 Windows Server 2003 型網域控制站升級為 Windows Server 2008 或 Windows Server 2008 R2。

將樹系中所有的 Windows Server 2003 型網域控制站升級為 Windows Server 2008 或 Windows Server 2008 R2 之後，您可以將樹系功能等級提高至 Windows Server 2008 或 Windows Server 2008 R2。 這樣做會自動將樹系中所有網域的功能層級，從設定為 Windows server 2003 設定為 windows server 2008 或 Windows Server 2008 R2。

如需提高樹系和網域功能等級的詳細資訊，以及執行這些工作之程式的詳細資訊，請參閱 [部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>升級新 Windows Server 2008 樹系中的功能等級
當您在新的 Windows Server 2008 樹系中安裝第一個網域控制站時，預設會將功能等級設定為下列層級，而這些等級會維持在這些等級，直到您以手動方式引發它們為止：

- Windows 2000 原生網域功能等級

- Windows 2000 樹系功能等級

功能等級設定為這些預設層級，讓您可以選擇將 Windows 2000 或 Windows Server 2003 型網域控制站新增至新的 Windows Server 2008 樹系。 建立樹系根域之後，您新增至 Windows Server 2008 樹系的每個網域的網域功能等級都會設定為 Windows 2000 native。 但是，如果您要讓新的 Windows Server 2008 環境中的所有網域控制站執行 Windows Server 2008，請在樹系中安裝第一個網域控制站時，將樹系功能等級和網域功能等級設定為 [Windows Server 2008]。 這麼做可以節省時間，並啟用 Windows Server 2008 中所有的樹系層級和網域層級功能。

> [!IMPORTANT]
> 如果樹系是在 Windows Server 2008 功能等級運作，而您嘗試在 Windows Server 2003 型成員伺服器或 Windows 2000 成員伺服器上安裝 Active Directory，則安裝會失敗。

如需提高樹系和網域功能等級的詳細資訊，以及執行這些工作之程式的詳細資訊，請參閱 [部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>升級新 Windows Server 2008 R2 樹系中的功能等級
當您在新的 Windows Server 2008 R2 樹系中安裝第一個網域控制站時，預設會將功能等級設定為下列層級，而且它們會維持在這些等級，直到您以手動方式引發它們為止：

- Windows Server 2003 網域功能等級

- Windows Server 2003 樹系功能等級

功能等級設定為這些預設層級，讓您可以選擇將 Windows Server 2003 型網域控制站新增至新的 Windows Server 2008 R2 樹系。 建立樹系根域之後，您新增至 Windows Server 2008 R2 樹系的每個網域的網域功能等級會設定為 Windows Server 2003。 但是，如果您要讓新的 Windows Server 2008 R2 環境中的所有網域控制站執行 Windows Server 2008 R2，請在樹系中安裝第一個網域控制站時，將樹系功能等級和網域功能等級設定為 Windows Server 2008 R2。 這麼做可以節省時間，並啟用 Windows Server 2008 R2 中的所有樹系層級和網域層級功能。

> [!IMPORTANT]
> 如果樹系是在 Windows Server 2008 R2 功能等級運作，而您嘗試將 Active Directory 安裝在 Windows Server 2008 或 Windows Server 2003 型的成員伺服器上，或在 Windows 2000 成員伺服器上，則安裝會失敗。

如需提高樹系和網域功能等級的詳細資訊，以及執行這些工作之程式的詳細資訊，請參閱 [部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

> [!NOTE]
> 雖然 ADMT 3.1 版必須安裝在 Windows Server 2008 上，但您可以使用 ADMT 3.1 來將物件遷移至一個或多個 Windows Server 2008 R2 網域控制站所裝載的網域。 如需詳細資訊，請參閱 Microsoft 知識庫中的文章976659， [當您使用 ADMT 3.1 遷移至包含 Windows Server 2008 R2 網域控制站的網域時，可能發生的已知問題](https://support.microsoft.com/help/976659/)。
