---
title: 在 MultiPoint 服務中設定 RDP over LAN 的連線站
description: 瞭解如何在 MultiPoint 服務中設定 RDP over LAN 系統
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9c43f207ad968c76fef027f09c2a13f1b36f1998
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971725"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>在 MultiPoint 服務中設定 RDP over LAN 的連線站
RDP over LAN 連接的工作站是一種瘦用戶端、傳統桌上型電腦或膝上型電腦，會使用遠端桌面通訊協定 (RDP) 連接到區域網路上的 MultiPoint 服務 (LAN) 。 如需此和其他工作站類型的詳細資訊，請參閱[MultiPoint 電臺](MultiPoint-services-Stations.md)。

## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>在 LAN 上使用電腦或瘦用戶端設定 MultiPoint 工作站

1.  開啟正在執行 MultiPoint 服務的電腦。

2.  確定 MultiPoint 伺服器電腦已透過交換器、路由器或其他網路裝置連線到 LAN，並具有適當的 IP 位址。  (以169.254 開頭的 IP 位址 (APIPA 位址) 可能表示 LAN 連線有問題，或 DHCP 伺服器無法連線或無法正常運作。 ) 

3.  將用戶端電腦或瘦用戶端連線到 LAN。

4.  開啟用戶端電腦或瘦用戶端。

5.  在用戶端電腦或瘦用戶端上，啟動遠端桌面連線或對等應用程式，然後輸入執行 MultiPoint 服務之電腦的名稱或 IP 位址。

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>使用連接器服務設定 Windows 10 裝置以進行遠端系統管理
任何執行 Windows 10 的電腦或膝上型電腦都可以從遠端系統管理，只要：
- 連接器服務已啟用
- 電腦已新增至 MultiPoint 伺服器上的受管理電腦。

在執行 Windows 10 的電腦上，遵循下列步驟來啟用 MultiPoint Connector：

1. 在搜尋方塊中，輸入「開啟或關閉 Windows 功能」，然後選取適當的搜尋結果。

2. 在功能清單中，啟用**MultiPoint 連接器**。 這會啟用管理裝置所需的 MultiPoint 連接器服務。

在 MultiPoint 伺服器上：
1. 開啟 MultiPoint 管理員，然後選取 [**新增或移除個人電腦**] 或 [**新增或移除 MultiPoint 服務**]。

2. 選取您要管理的遠端電腦，然後按一下 **[確定]**。  系統會提示您輸入遠端電腦上的管理員認證。  完成後，您會在 [MultiPoint 管理員] 的 [首頁] 索引標籤上看到遠端電腦。

當成功設定儀表板管理員時，可以監視使用受管理裝置的使用者。

> [!IMPORTANT]
> 監視受管理的 Windows 10 裝置時，無法監視 administratrive 的使用者，但伺服器設定也會隨之變更。 請參閱[編輯服務器設定](Edit-Server-Settings.md)