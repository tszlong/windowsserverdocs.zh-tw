---
title: 遠端桌面-允許存取您的電腦
description: 深入了解您的選項，從遠端存取您的電腦
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: af41304e08f19ca155f6fd13c9258e9a8f20c163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817009"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>遠端桌面-允許存取您的電腦

>適用於：Windows 10，Windows 8.1，Windows Server 2012 R2 中，Windows Server 2016

您可以使用遠端桌面連線至並控制您的電腦，從遠端裝置上使用[Microsoft 遠端桌面用戶端](remote-desktop-clients.md)（適用於 Windows、 iOS、 macOS 和 Android）。 當您允許遠端連線至您的電腦時，您可以使用另一個裝置連線到您的電腦，並且具有所有您的應用程式、 檔案和網路資源的存取，如同您坐在辦公桌前。  

> [!NOTE]
> 您可以使用遠端桌面連線到 Windows 10 專業版、 企業、 Windows 8.1 及 8 企業和專業版、 Windows 7 Professional、 Enterprise 和 Ultimate 和 Windows Server 版本比 Windows Server 2008 更新。 您無法連線到執行 （例如 Windows 10 家用版） 的 Home edition 的電腦。 

若要連接至遠端電腦上，必須開啟電腦，它必須有網路連線，必須啟用遠端桌面，您必須具有網路存取權 （可能透過網際網路） 的遠端電腦，和您必須有連接權限。 連接的權限，您必須是使用者清單。 開始連接之前，就是個好主意，若要查詢您正在連接到電腦的名稱，並確保通過其防火牆允許遠端桌面連線進行。

## <a name="how-to-enable-remote-desktop"></a>如何啟用遠端桌面

允許從遠端裝置存取您的電腦最簡單的方式使用 [設定] 下的遠端桌面選項。 由於這項功能已加入 Windows 10 Fall Creators update (1709)，可下載個別應用程式也會提供舊版的 Windows 提供類似的功能。 不過，此方法可提供較少的功能和驗證，您也可以使用 啟用遠端桌面的傳統方式。

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creator Update (1709) 或更新版本

您可以使用幾個簡單的步驟來設定您的電腦進行遠端存取。
1. 在您想要連接到裝置上，選取**開始**，然後按一下**設定**左側的圖示。
2. 選取 **系統**群組後面[**遠端桌面**](ms-settings:remotedesktop)項目。
3. 您可以使用滑桿來啟用遠端桌面。
4. 也建議您保持電腦，甦醒狀態與可探索，以利於進行連線。 按一下 **顯示設定**啟用。
5. 如有需要加入的使用者可以從遠端連線，即可**選取即可從遠端存取此電腦的使用者**。
   1. 系統管理員群組的成員自動擁有存取權。
6. 請記下此電腦的名稱**如何連線到此 PC**。 您將需要此項來設定用戶端。

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 和舊版的 Windows 10

若要設定您的電腦進行遠端存取，請下載並執行[Microsoft 遠端桌面小幫手](https://www.microsoft.com/download/details.aspx?id=50042)。 此助理更新您的系統設定，以啟用遠端存取，請確定您的電腦是醒著的連線，並檢查您的防火牆允許遠端桌面連線。 

### <a name="all-versions-of-windows-legacy-method"></a>所有版本的 Windows （舊版方法）

若要啟用遠端桌面使用舊有系統屬性，請依照下列指示來[連接到另一部電腦使用遠端桌面連線](https://windows.microsoft.com/windows/remote-desktop-connection-faq)。

## <a name="should-i-enable-remote-desktop"></a>應該啟用遠端桌面？

如果您只想要存取您的電腦，當您實際坐在其前面時，您不需要啟用遠端桌面。 啟用遠端桌面開啟您可以看到您的區域網路的電腦上的連接埠。 您只應該在受信任的網路中，啟用遠端桌面，例如您的首頁。 您也不想要嚴格控制存取的任何電腦上啟用遠端桌面。

請注意，當您啟用遠端桌面存取您要授與任何人在系統管理員群組中，以及任何其他使用者您選取，能夠從遠端存取他們的電腦上的帳戶。

您應該確認每個帳戶都可存取您的電腦已設定強式密碼。

## <a name="why-allow-connections-only-with-network-level-authentication"></a>為什麼允許連線的網路層級驗證？ 
 
如果您想要限制誰可以存取您的電腦，請選擇允許存取只含有網路層級驗證 (NLA)。 當您啟用此選項時，使用者必須向其本身網路才能連線至您的電腦。 允許連線只能從電腦執行遠端桌面使用 NLA 是更安全的驗證方法，可協助保護電腦免受惡意的使用者和軟體。 若要深入了解 NLA 和遠端桌面，請參閱[設定為 RDS 連線的 NLA](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx)。 

如果您要從遠端連線至電腦上該網路之外從家用網路上，未選取此選項。
