---
title: 變更遠端桌面中的接聽連接埠
description: 了解如何變更遠端桌面用戶端的接聽連接埠。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5bf90010143e742f7a0c9b5c262be01e6ccf8c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882719"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>變更遠端桌面的電腦上的接聽連接埠

>適用於：Windows 10，Windows 8.1、 Windows 8、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2008 R2

當您連接到電腦 （Windows 用戶端或 Windows Server） 透過遠端桌面用戶端時，在您的電腦上的遠端桌面功能"聽到 「 連線要求，透過定義的接聽連接埠 (預設值為 3389)。 您可以藉由修改登錄來變更在 Windows 電腦上的接聽連接埠。

1. 啟動登錄編輯程式。 （在搜尋方塊中輸入 regedit）。
2. 瀏覽至下列登錄子機碼：HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. 按一下 **編輯 > 修改**，然後按一下**十進位**。
4. 輸入新的連接埠號碼，然後按一下**確定**。 
5. 關閉登錄編輯程式，並重新啟動電腦。

下次您連線到此電腦使用遠端桌面連線 中，您必須輸入新的連接埠。 如果您使用防火牆，請務必設定防火牆以允許連接至新的連接埠號碼。
