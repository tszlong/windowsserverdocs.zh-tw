---
title: 遠端桌面用戶端 URI 配置
description: 了解遠端桌面用戶端的統一資源識別項配置
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 56d11a2ab0afab1e119666aea17bb6d54cd5638b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404122"
---
# <a name="remote-desktop-client-universal-resource-identifier-uri-scheme-support"></a>遠端桌面用戶端的統一資源識別項 (URI) 配置支援

>適用於：Windows Server 版本 1803、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

啟用「統一資源識別項」(URI) 配置可讓 IT 專業人員和開發人員整合各個平台的「遠端桌面」用戶端功能並增強使用者體驗，方法是允許： 

- 協力廠商應用程式啟動「Microsoft 遠端桌面」，並使用預先定義的設定 (在 URI 字串中提供) 來啟動遠端工作階段。
- 使用者使用預先設定的 URL 來啟動遠端連線。

>[!NOTE]
> Windows 作業系統不支援使用 URI 連線到 RD 用戶端 - 請搭配 MacOS、iOS 和 Android 裝置使用 URI。

Microsoft 遠端桌面會使用 URI 配置 rdp://query_string 來儲存啟動用戶端時所使用的預先設定屬性設定。 查詢字串代表在 URL 中提供的單一或一組 RDP 屬性。 

RDP 屬性以 & 符號分隔。 例如，連線到電腦時，該字串是：

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

下表提供可搭配 iOS、Mac 及 Android「遠端桌面」用戶端使用的完整支援屬性清單。 (平台欄中的 "x" 表示是支援的屬性。 以 ＞形箭號 (<>) 表示的值代表「遠端桌面」用戶端所支援的值。)

| **RDP 屬性**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| allow desktop composition=i:&lt;0 或 1&gt;                    | x       | x   | x   |
| allow font smoothing=i:<0 或 1&gt;                         | x       | x   | x   |
| alternate shell=s:&lt;字串&gt;                              | x       | x   | x   |
| [audiomode=i:&lt;0、1 或 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [authentication level=i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| connect to console=i:&lt;0 或 1&gt;                           | x       | x   | x   |
| disable cursor settings=i:&lt;0 或 1&gt;                      | x       | x   | x   |
| disable full window drag=i:&lt;0 或 1&gt;                     | x       | x   | x   |
| disable menu anims=i:&lt;0 或 1&gt;                           | x       | x   | x   |
| disable themes=i:&lt;0 或 1&gt;                               | x       | x   | x   |
| disable wallpaper=i:&lt;0 或 1&gt;                            | x       | x   | x   |
| [drivestoredirect=s:*](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (這是唯一支援的值) | x       | x   |     |
| [desktopheight=i:&lt;以像素為單位的值&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth=i:&lt;以像素為單位的值&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [domain=s:&lt;字串&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [full address=s:&lt;字串&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname=s:&lt;字串&gt;                  | x | x | x |
| [gatewayusagemethod=i:&lt;1 或 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [prompt for credentials on client=i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo=s:&lt;字串&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters=i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline=s:&lt;字串&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 或 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;字串&gt;         | x | x | x |
| shell working directory=s:&lt;字串&gt;          | x | x | x |
| Use redirection server name=i:&lt;0 或 1&gt;      | x | x | x |
| [username=s:&lt;或&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [screen mode id=i:&lt;1 或 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [session bpp=i:&lt;8、15、16、24 或 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [use multimon=i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |