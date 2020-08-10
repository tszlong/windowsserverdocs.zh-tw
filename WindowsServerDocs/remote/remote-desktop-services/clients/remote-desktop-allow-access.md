---
title: 遠端桌面 - 允許存取您的電腦
description: 深入了解您從遠端存取電腦的選項
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f581aeee099c906c970fdc320150fd1d6c102e17
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946521"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>遠端桌面 - 允許存取您的電腦

>適用於：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

您可以使用 [Microsoft 遠端桌面用戶端](remote-desktop-clients.md) (適用於 Windows、iOS、macOS 和 Android)，從遠端裝置使用遠端桌面連線至您的電腦，並加以控制。 當您允許從遠端連線至電腦時，您可以使用其他裝置連線至您的電腦並存取所有應用程式、檔案和網路資源，就像是坐在辦公桌前面一樣。

> [!NOTE]
> 您可以使用遠端桌面連線至 Windows 10 專業版和企業版、Windows 8.1 與 8 企業版與專案版、Windows 7 專業版、企業版和旗艦版，以及比 Windows Server 2008 更新的 Windows Server 版本。 您無法連線至執行家用版 (例如 Windows 10 家用版) 的電腦。

若要連線至遠端電腦，該電腦必須開啟且必須有網路連線、遠端桌面必須啟用、您必須具有遠端電腦的網路存取權 (可透過網際網路)，且您必須有連線的權限。 若要有連線的權限，您必須列於使用者清單中。 開始連線之前，建議您先查閱您要連接的電腦名稱，並確定遠端桌面連線可通過該電腦的防火牆。

## <a name="how-to-enable-remote-desktop"></a>如何啟用遠端桌面

要允許從遠端裝置存取您的電腦，最簡單的方式是使用 [設定] 下的 [遠端桌面] 選項。 在這項功能新增至 Windows 10 Fall Creators Update (1709) 後，也有個別的應用程式可供下載，為您提供與舊版 Windows 類似的功能。 您也可以使用舊有方式來啟用遠端桌面，但此方法可提供較少的功能和驗證。

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creator Update (1709) 或更新版本

只需幾個簡單的步驟，您即可完成電腦的遠端存取設定。
1. 在您要連接的裝置上選取 [開始]  ，然後按一下左側的 [設定]  圖示。
2. 選取 [系統]  群組，然後選取[**遠端桌面**](ms-settings:remotedesktop)項目。
3. 使用滑桿來啟用遠端桌面。
4. 此外也建議您將電腦保持在警醒且可探索的狀態，以利進行連線。 按一下 [顯示設定]  加以啟用。
5. 如有需要，請按一下 [選取可從遠端存取此電腦的使用者]  ，以新增可從遠端連線的使用者。
   1. 系統管理員群組的成員會自動取得存取權。
6. 在 [如何連線至此電腦]  下方記下此電腦的名稱。 您將需要用這項資訊來設定用戶端。

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 和舊版的 Windows 10

若要為電腦進行遠端存取設定，請下載並執行 [Microsoft 遠端桌面小幫手](https://www.microsoft.com/download/details.aspx?id=50042)。 這個小幫手會更新您的系統設定以啟用遠端存取、確保您的電腦可辨識連線，並確認您的防火牆允許遠端桌面連線。

### <a name="all-versions-of-windows-legacy-method"></a>所有版本的 Windows (舊有方法)

若要使用舊有系統內容來啟用遠端桌面，請依照指示[使用「遠端桌面連線」連線至另一部電腦](https://windows.microsoft.com/windows/remote-desktop-connection-faq)。

## <a name="should-i-enable-remote-desktop"></a>是否應啟用遠端桌面？

如果您只想要在本機位置存取電腦，則不需要啟用遠端桌面。 啟用遠端桌面會在您的電腦上開啟區域網路可看到的連接埠。 您只應在信任的網路中啟用遠端桌面，例如在家裡。 您也不應在存取受到嚴格控制的電腦上啟用遠端桌面。

請注意，當您啟用對遠端桌面的存取時，您即會為系統管理員群組中的任何人以及您選取的任何其他使用者，授與在電腦上從遠端存取其帳戶的能力。

您應確定可存取您的電腦的每個帳戶都已設定了強式密碼。

## <a name="why-allow-connections-only-with-network-level-authentication"></a>為何僅允許使用網路層級驗證的連線？

如果您想要限制可存取電腦的人員，請選擇僅允許使用網路層級驗證 (NLA) 的存取。 此選項啟用時，使用者必須向網路進行身分驗證，才能連線至您的電腦。 僅允許執行遠端桌面的電腦使用 NLA 進行連線是較安全的驗證方法，有助於保護電腦不受惡意使用者和軟體的侵害。 若要深入了解 NLA 和遠端桌面，請參閱[為 RDS 連線設定 NLA](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732713(v=ws.11))。

如果您要從家用網路外部遠端連線至該網路上的電腦，請不要選取此選項。
