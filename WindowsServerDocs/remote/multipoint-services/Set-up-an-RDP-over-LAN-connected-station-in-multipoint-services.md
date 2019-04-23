---
title: 設定要在 MultiPoint 服務中的 RDP 移轉網路連接站台
description: 了解如何設定 MultiPoint 服務中的 RDP 移轉網路系統
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 8d2f1644918f1a581c1bcab181cd084e12c6b576
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843699"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>設定要在 MultiPoint 服務中的 RDP 移轉網路連接站台
RDP 移轉網路連接站台是精簡型用戶端、 傳統的桌面或使用遠端桌面通訊協定 (RDP) 連接到 MultiPoint 服務在區域網路 (LAN) 的膝上型電腦。 如需有關這個主題以及其他站台類型的詳細資訊，請參閱[MultiPoint 站台](MultiPoint-services-Stations.md)。  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>若要設定 MultiPoint 站台使用 在 LAN 上的 電腦或精簡型用戶端  
  
1.  開啟正在執行 MultiPoint 服務電腦。  
  
2.  請確定 MultiPoint Server 的電腦連線到區域網路交換器、 路由器或其他網路裝置，並具有適當的 IP 位址。 （啟動具有 169.254 （APIPA 位址） 的 IP 位址可能表示與區域網路連線發生問題，或 DHCP 伺服器無法連線，或無法正常運作）。  
  
3.  用戶端電腦或精簡型用戶端連線到 LAN。  
  
4.  開啟用戶端電腦或精簡型用戶端。  
  
5.  在用戶端電腦或精簡型用戶端上，啟動遠端桌面連線或對等的應用程式，並輸入名稱或執行 MultiPoint 服務電腦的 IP 位址。

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>使用連接器服務，設定 Windows 10 裝置以進行遠端管理
可以從遠端管理任何電腦或膝上型電腦執行 Windows 10，只要：
- 已啟用連接器服務  
- 已新增電腦至 MultiPoint 伺服器上的受管理電腦。  

在執行 Windows 10 電腦上遵循下列步驟來啟用 MultiPoint 連接器：

1. 在 [搜尋] 方塊中，輸入 「 關閉 Windows 功能開啟或關閉 」，然後選取適當的搜尋結果。 

2. 在功能清單，讓**MultiPoint 連接器**。 這可讓 MultiPoint 連接器服務時所需管理裝置。 

在 MultiPoint server:
1. 開啟 MultiPoint 管理員，並選取**新增或移除個人電腦**或是**新增或移除 MultiPoint 服務**。

2. 選取您想要管理，並按一下 遠端電腦**確定**。  將提示您在遠端電腦上的系統管理員認證。  完成之後，您會看到 MultiPoint 管理員的首頁索引標籤上的遠端電腦。

當成功組啟動儀表板管理員可以監視使用者在受管理的裝置上工作。

> [!IMPORTANT]  
> 當監視受管理 Windows 10 裝置 administratrive 使用者無法監視除了已經據以變更設定的伺服器。 請參閱[編輯伺服器設定](Edit-Server-Settings.md)