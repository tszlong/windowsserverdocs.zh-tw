---
title: 變更接聽的連接埠，在 [遠端桌面
description: 了解如何變更為 [遠端桌面用戶端接聽的連接埠。
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
ms.openlocfilehash: b70479b644f4984c93136d6493483c372703244d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297317"
---
# 變更您電腦上接聽的連接埠遠端桌面

>適用於： Windows 10、 Windows 8.1、 Windows 8、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2008 R2

當您連線到透過遠端桌面用戶端電腦 （Windows 用戶端或 Windows Server） 時，您的電腦上的遠端桌面功能 」 可以聽見 」 透過定義接聽連接埠連線要求 (預設為 3389)。 您可以藉由修改登錄變更該接聽的連接埠，在 Windows 電腦上。

1. 啟動 [登錄編輯程式。 （在搜尋方塊中輸入 regedit）。
2. 瀏覽至下列登錄子機碼： 結束 Server\WinStations\RDP Tcp\PortNumber
3. 按一下 [**編輯 > 修改**，然後按一下 [**十進位**。
4. 輸入新的連接埠號碼，，然後按一下 **[確定]**。 
5. 關閉登錄編輯程式，並重新啟動您的電腦。

下一次連線到此電腦使用遠端桌面連線，您必須輸入新的連接埠。 如果您使用防火牆，請務必設定防火牆，以允許連線至新的連接埠號碼。
