---
title: 變更遠端桌面中的接聽連接埠
description: 了解如何變更遠端桌面用戶端的接聽連接埠。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 818ae5217d0144b2a4ec6e45f3a7757455cfabf1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854661"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>變更電腦上的遠端桌面接聽連接埠

>適用於：Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2008 R2

當您透過遠端桌面用戶端連線至電腦 (Windows 用戶端或 Windows Server) 時，電腦上遠端桌面功能會透過定義的接聽連接埠 (預設為 3389) 「聽到」連線要求。 您可以修改登錄來變更 Windows 電腦上的接聽連接埠。

1. 啟動登錄編輯程式。 (在搜尋方塊中鍵入 regedit。)
2. 巡覽至下列登錄子機碼：HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. 依序按一下 [編輯] > [修改]  和 [十進位]  。
4. 鍵入新的連接埠號碼，然後按一下 [確認]  。 
5. 關閉登錄編輯程式，並重新啟動您的電腦。

下次您使用遠端桌面連線連線到此電腦時，必須鍵入新的連接埠。 如果您使用防火牆，請務必設定防火牆以允許連線至新的連接埠號碼。
