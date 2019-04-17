---
title: 遠端桌面-允許存取您的電腦
description: 深入了解您的選項從遠端存取您的電腦
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
ms.openlocfilehash: d3c85c1c765583eb26cef60ecd245708e0e02772
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297217"
---
# 遠端桌面-允許存取您的電腦

>適用於： Windows 10、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

您可以使用遠端桌面連線，並使用[Microsoft 遠端桌面用戶端](remote-desktop-clients.md)（適用於 Windows、 iOS、 macOS 和 Android） 來控制您的電腦，從遠端裝置。 當您允許遠端連線到您的電腦時，您可以使用另一部裝置連線至您的電腦，並可以存取您的應用程式、 檔案及網路資源的所有像您坐在您的辦公桌前面一樣。  

> [!NOTE]
> 您可以使用遠端桌面連線至 Windows 10 專業版和企業、 Windows 8.1 和 8 企業版和專業版、 Windows 7 專業版、 企業版，以及 Ultimate 和 Windows Server 版本比 Windows Server 2008。 您無法連線到執行家用版 （例如 Windows 10 家用版） 的電腦。 

若要連線到遠端電腦，電腦必須開啟，它必須具備網路連線，必須啟用遠端桌面，您必須有網路存取權 （這可能是透過網際網路） 的遠端電腦，而且必須連線的權限。 適用於連線的權限，您必須是在使用者的清單。 啟動連線之前，它會是不錯的想法來尋找您要連線的電腦名稱，並請確定遠端桌面連線允許透過其防火牆。

## 如何啟用遠端桌面

允許您的電腦從遠端裝置存取最簡單方式使用遠端桌面選項，在設定下。 由於這項功能已新增 Windows 10 Fall Creators update (1709)、 可下載個別應用程式也會提供較舊版本的 Windows 可提供類似的功能。 不過，這個方法提供較少的功能和驗證，您也可以使用啟用遠端桌面的傳統方式。

### Windows 10 Fall Creators Update (1709) 或更新版本

您可以透過幾個簡單步驟來設定您的電腦的遠端存取。
1. 在裝置上您想要連線到，選取 **[開始]** ，然後按一下左側的 [**設定**] 圖示。
2. 選取的**系統**群組，後面接著[**遠端桌面**](ms-settings:remotedesktop)項目。
3. 若要啟用遠端桌面使用滑桿。
4. 也建議您保留清醒並搜尋以促進連線的電腦。 按一下 [**顯示設定**以啟用。
5. 如有需要新增使用者可以按一下 [**選取的使用者，可以從遠端存取此電腦**的遠端連線。
   1. 系統管理員群組的成員會自動有存取權。
6. 請記下**如何連線到此電腦**的此電腦的名稱。 您將需要此呼叫以用戶端設定。

### Windows 7 與早期版本的 Windows 10

若要設定為 [遠端存取您的電腦，下載並執行[Microsoft 遠端桌面小幫手](https://www.microsoft.com/download/details.aspx?id=50042)。 這個小幫手更新您的系統設定，以啟用遠端存取權，可確保您的電腦是清醒連線，並檢查您的防火牆允許遠端桌面連線。 

### 所有版本的 Windows （傳統方法）

若要啟用遠端桌面使用舊版的系統內容，請依照指示來[連線到另一部電腦使用遠端桌面連線](https://windows.microsoft.com/windows/remote-desktop-connection-faq)。

## 應該啟用遠端桌面？

如果您只想要存取您的電腦，您實際坐最前面它時，您不需要啟用遠端桌面。 啟用遠端桌面會開啟您顯示給您的區域網路的電腦上的連接埠。 您只應該在受信任的網路中啟用遠端桌面，例如家。 您也不想要啟用遠端桌面緊密控制存取的任何電腦上。

請注意，當您啟用遠端桌面存取您授與任何人在系統管理員群組中，以及任何其他使用者選取，能夠從遠端存取其電腦上的帳戶。

您應該確定每個帳戶可存取您的電腦已設定強式密碼。

## 為什麼要允許只使用網路層級驗證的連線？ 
 
如果您想要限制誰可以存取您的電腦，選擇要允許存取只與網路層級驗證 (NLA)。 當您啟用此選項時，讓使用者驗證自己至網路，才能讓它們連接到您的電腦。 允許只從具有 NLA 執行遠端桌面的電腦連線是更安全的驗證方法，可協助保護電腦免於惡意的使用者及軟體。 若要深入了解 NLA 和遠端桌面，請參閱[設定 NLA RDS 連線](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx)。 

如果您從遠端連線到電腦從該網路以外的地方家用網路上，請勿選取此選項。
