---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: "檢測軍人層級功能升級"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9527a186cb20c470e0b5644fff58f90786520f97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-your-functional-level-upgrade"></a>檢測軍人層級功能升級

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以提高網域及森林功能等級之前，您必須評估您目前的環境並找出功能層級需求最符合您的組織的需求。 檢測軍人網域中樹系位於每個網域、 作業系統 service pack 可執行的每個網域控制站，並想要升級網域控制站日期的網域控制站的評估您目前的環境。 如果您的網域控制站的淘汰計劃，請確定您知道完整的影響，如此一來，就可以在您的環境。  
  
下列下可能會使您從較舊版本的 Windows Server 作業系統升級到 Windows Server 2008 或 Windows Server 2008 R2 功能層級：  
  
-   不足的硬體  
  
-   網域控制站執行的是 Windows Server 2008，或 Windows Server 2008 R2 相容的防毒程式   
  
-   不會執行 Windows Server 2008 或 Windows Server 2008 R2 的特定版本程式之使用   
  
-   需要升級程式的最新的 service pack  
  
文件此資訊可協助您找出的步驟進行以確保您擁有完全正常運作的 Windows Server 2008 或 Windows Server 2008 R2 環境。  
  
您評估您目前的環境之後，您必須以找出適用於您組織的功能層級升級。 可以使用這些選項：  
  
-   Windows Server 2008，或 Windows Server 2008 R2 的 Windows 2000 原生模式環境   
  
-   Windows Server 2008，或 Windows Server 2008 R2 的 Windows Server 2003 森林   
  
-   新的 Windows Server 2008 樹系  
  
-   新的 Windows Server 2008 R2 樹系  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>升級原生 Windows 2000 Active Directory 森林中的功能層級  
在僅包含 Windows 2000 的網域控制站的 Windows 2000 原生環境，下列層級，預設設定功能的層級，以手動方式提高之前維持在這些層級：  
  
-   Windows 2000 原生網域功能層級  
  
-   Windows 2000 的樹系功能層級  
  
若要使用 Windows Server 2008，或 Windows Server 2008 R2 的所有森林層級和網域層級功能，您需要此 Windows 2000 的環境升級到 Windows Server 2008 或 Windows Server 2008 R2。 您可以執行此升級其中一項下列方法：  
  
-   將最近安裝的 Windows Server 2008-基礎 Windows Server 2008 R2 或-入樹系，根據網域控制站，然後淘汰所有執行 Windows 2000 的網域控制站。  
  
-   執行所有現有執行網域控制站 Windows 2000 森林中執行 Windows Server 2003 的網域控制站就地的升級。 接著，執行的 Windows Server 2008，或 Windows Server 2008 R2 網域控制站的就地升級。 如需詳細資訊，請查看[以 Windows Server 2008 AD DS 網域 \[LH\ 升級 Active Directory 網域]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 是 x64 型作業系統。 如果您的伺服器執行的 Windows Server 2003 x64 版本，您可以順利執行就地升級的 Windows Server 2008 R2 這台電腦的作業系統。 如果您的伺服器執行的 Windows Server 2003 x86 型版本，您無法以 Windows Server 2008 R2 升級此電腦。  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 網域層級功能不是整個 Windows 2000 樹系升級到 Windows Server 2008 或 Windows Server 2008 R2、 提高只網域功能等級 Windows Server 2008，或 Windows Server 2008 R2。  
  
> [!NOTE]  
> 您提高網域功能等級之前，您必須先升級到 Windows Server 2008 或 Windows Server 2008 R2 網域中的所有 Windows 2000 的網域控制站。  
  
森林中的所有 Windows 2000 的網域控制站都取代執行 Windows Server 2008 或 Windows Server 2008 R2 網域控制站之後，您可以提高 Windows Server 2008，或 Windows Server 2008 R2 的樹系功能層級。 自動擔任功能森林中的所有網域原生或更高到 Windows Server 2008 或 Windows Server 2008 R2 到 Windows 2000 設定的層級。  
  
如需有關引發樹系和網域功能層級，以及程序，才能執行這些工作，查看[部署 Windows Server 2008 森林根網域 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>升級 Windows Server 2003 Active Directory 森林中的功能層級  
在組成僅為基礎 Windows Server 2003 的網域控制站的 Windows Server 2003 環境，下列層級，預設設定功能的層級，以手動方式提高之前維持在這些層級：  
  
-   Windows 2000 原生網域功能層級  
  
-   Windows 2000 的樹系功能層級  
  
若要使用 Windows Server 2008，或 Windows Server 2008 R2 的所有森林層級和網域層級功能，您需要此 Windows Server 2003 的環境升級到 Windows Server 2008 或 Windows Server 2008 R2。 您可以執行此升級其中一項下列方法：  
  
-   將新安裝的 Windows Server 2008-基礎 Windows Server 2008 R2 或-入樹系，根據網域控制站然後淘汰所有執行 Windows Server 2003 的網域控制站或升級到 Windows Server 2008 或 Windows Server 2008 R2。  
  
-   執行所有現有網域控制站執行到執行 Windows Server 2008 或 Windows Server 2008 R2 網域控制站的 Windows Server 2003 就地的升級。 如需詳細資訊，請查看[以 Windows Server 2008 AD DS 網域 \[LH\ 升級 Active Directory 網域]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 是 x64 型作業系統。 如果您的伺服器執行的 Windows Server 2003 x64 版本，您可以順利執行就地升級的 Windows Server 2008 R2 這台電腦的作業系統。 如果您的伺服器執行的 Windows Server 2003 x86 型版本，您無法升級執行 Windows Server 2008 R2 這台電腦。  
  
若要使用所有的 Windows Server 2008 或 Windows Server 2008 R2 網域層級功能不是整個 Windows Server 2003 樹系升級到 Windows Server 2008 或 Windows Server 2008 R2、 提高只網域功能等級 Windows Server 2008，或 Windows Server 2008 R2。  
  
> [!NOTE]  
> 您提高網域功能等級之前，您必須先升級到 Windows Server 2008 或 Windows Server 2008 R2 網域中的所有 Windows Server 2003 根據網域控制站。  
  
森林中的所有 Windows Server 2003 根據網域控制站都升級到 Windows Server 2008 或 Windows Server 2008 R2 之後，您可以提高 Windows Server 2008，或 Windows Server 2008 R2 的樹系功能層級。 自動擔任功能所有網域森林中的 Windows Server 2008，或 Windows Server 2008 R2 的 Windows Server 2003 已設定的層級。  
  
如需有關引發樹系和網域功能層級，以及程序，才能執行這些工作，查看[部署 Windows Server 2008 森林根網域 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>新的 Windows Server 2008 森林中的升級功能層級  
當您安裝新的 Windows Server 2008 森林中的第一個網域控制站時，功能層級下列層級，預設設定，並手動提高之前維持在這些層級：  
  
-   Windows 2000 原生網域功能層級  
  
-   Windows 2000 的樹系功能層級  
  
功能層級，讓您選擇加入新的 Windows Server 2008 樹系的 Windows 2000 或 Windows Server 2003 根據網域控制站的設定，這些預設的層級。 建立網域森林根之後，網域功能層級的每個您新增到 Windows Server 2008 的樹系網域是設定為 Windows 2000 原生。 不過，如果您想在執行 Windows Server 2008 您新的 Windows Server 2008 環境中的所有網域控制站，為的樹系功能的層級，然後網域功能層級，當您安裝您森林中的第一個網域控制站的 Windows Server 2008。 如此一來節省時間，並讓所有的樹系層級和網域層級功能在 Windows Server 2008。  
  
> [!IMPORTANT]  
> 如果您在 Windows Server 2008 的功能運作的樹系層級，而您嘗試安裝 Windows Server 2003 成員的伺服器上的 Active Directory 或 Windows 2000 成員伺服器，安裝失敗。  
  
如需有關引發樹系和網域功能層級，以及程序，才能執行這些工作，查看[部署 Windows Server 2008 森林根網域 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>新的 Windows Server 2008 R2 森林中的升級功能層級  
當您安裝新的 Windows Server 2008 R2 森林中的第一個網域控制站時，功能層級下列層級，預設設定，並手動提高之前維持在這些層級：  
  
-   Windows Server 2003 網域功能層級  
  
-   Windows Server 2003 森林功能層級  
  
功能層級，讓您選擇加入新的 Windows Server 2008 R2 樹系的 Windows Server 2003 根據網域控制站的設定，這些預設的層級。 建立網域森林根之後，您新增到 Windows Server 2008 R2 的樹系的每個網域網域功能層級是以 Windows Server 2003 設定。 不過，若要所有網域控制站在您執行 Windows Server 2008 R2 的新 Windows Server 2008 R2 環境中，為的樹系功能的層級，然後網域功能層級，當您安裝的第一個網域控制站在您的樹系 Windows Server 2008 R2。 如此一來節省時間，並讓所有的樹系層級和網域層級功能在 Windows Server 2008 R2。  
  
> [!IMPORTANT]  
> 如果在 Windows Server 2008 R2 功能層級的樹系運作，且您嘗試安裝在 Windows Server 2008 的 Active Directory-根據或 Windows Server 2003 成員 server 或 Windows 2000 的成員，伺服器上安裝失敗。  
  
如需有關引發樹系和網域功能層級，以及程序，才能執行這些工作，查看[部署 Windows Server 2008 森林根網域 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
> [!NOTE]  
> 雖然 ADMT v3.1 必須安裝在 Windows Server 2008，您可以使用 ADMT v3.1 網域一或多個 Windows Server 2008 R2 網域控制站主控移轉物件。 如需詳細資訊，請查看[文章 976659](https://go.microsoft.com/fwlink/?LinkId=180398)中「Microsoft 知識庫 (https://go.microsoft.com/fwlink/?LinkId=180398)。  
  


