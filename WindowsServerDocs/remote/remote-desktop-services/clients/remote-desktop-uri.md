---
title: 遠端桌面用戶端 URI 配置
description: 深入了解統一資源識別項配置適用於遠端桌面用戶端
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/11/2018
ms.localizationpriority: medium
ms.openlocfilehash: f2934fed43c8f4feec2f321d684cc3593933eb5d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297467"
---
# 遠端桌面用戶端通用資源識別項 (URI) 配置支援

>適用於： Windows Server 版本 1803 起，Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

啟用統一資源識別項 (URI) 配置可以讓 IT 專業人員和開發跨平台整合遠端桌面用戶端的功能，並藉由允許可增強使用者體驗： 

- 協力廠商應用程式啟動 Microsoft 遠端桌面和啟動遠端工作階段使用預先定義的設定 （提供做為 URI 字串的一部分）。
- 若要開始使用預先設定的 Url 的遠端連線的使用者。

>[!NOTE]
> 適用於 Windows 作業系統不支援使用 URI 來連線到 RD 用戶端-MacOS、 iOS 和 Android 裝置搭配使用 URI。

Microsoft 遠端桌面用來儲存時，啟動用戶端會使用的預先設定的屬性設定的 URI 配置 rdp://query_string。 查詢字串代表單一或一組的 URL 中提供的 RDP 屬性。 

RDP 屬性是以連字號 (&) 分隔。 例如，連接至電腦時，該字串是：

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

此表格提供可能搭配 iOS、 Mac 及 Android 的遠端桌面用戶端的支援屬性的完整清單。 （為 「 x 」 中的平台欄會指出在支援的屬性。 > 形箭號 (<>) 表示這些值代表所支援的遠端桌面用戶端的值。）

| **RDP 屬性**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| 允許桌面組合 = i:&lt;0 或 1&gt;                    | x       | x   | x   |
| 允許字型平滑化 = i:<0 或 1&gt;                         | x       | x   | x   |
| 替代殼層 = s:&lt;字串&gt;                              | x       | x   | x   |
| [audiomode = i:&lt;0、 1 或 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [驗證層級 = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| 連線到主控台 = i:&lt;0 或 1&gt;                           | x       | x   | x   |
| 停用游標設定 = i:&lt;0 或 1&gt;                      | x       | x   | x   |
| 停用完整視窗拖曳到物件 = i:&lt;0 或 1&gt;                     | x       | x   | x   |
| 停用功能表 anims = i:&lt;0 或 1&gt;                           | x       | x   | x   |
| 停用佈景主題 = i:&lt;0 或 1&gt;                               | x       | x   | x   |
| 停用桌布 = i:&lt;0 或 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx)（這是唯一支援的值） | x       | x   |     |
| [desktopheight = i:&lt;像素中的值&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;像素中的值&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [網域 = s:&lt;字串&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [完整記憶體位址 = s:&lt;字串&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname = s:&lt;字串&gt;                  | x | x | x |
| [gatewayusagemethod = i:&lt;1 或 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [用戶端上的認證提示 = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo = s:&lt;字串&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline = s:&lt;字串&gt;         | x | x | x |
| remoteapplicationmode = i:&lt;0 或 1&gt;            | x | x | x |
| remoteapplicationprogram = s:&lt;字串&gt;         | x | x | x |
| 殼層工作目錄 = s:&lt;字串&gt;          | x | x | x |
| 使用重新導向的伺服器名稱 = i:&lt;0 或 1&gt;      | x | x | x |
| [使用者名稱 = s:&lt;字串&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [螢幕模式 id = i:&lt;1 或 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [工作階段 bpp = i:&lt;8、 15、 16、 24、 或 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [使用 multimon = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |