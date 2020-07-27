---
title: 遠端桌面 URI 配置
description: 了解遠端桌面用戶端的統一資源識別項配置
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 06/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: 50eb7aaa9b8d4d8826f74f2a5338c93dee5d1053
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954070"
---
# <a name="remote-desktop-uri-scheme"></a>遠端桌面 URI 配置

> 適用於：Windows Server 版本 1803、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

本文件定義遠端桌面的統一資源識別項 (URI) 格式。 這些 URI 配置允許使用各種命令來叫用遠端桌面用戶端。

## <a name="ms-rd-uri-scheme"></a>ms-rd URI 配置

>[!NOTE]
> 目前只有 Windows 桌面用戶端 (MSRDC) 支援 ms-rd URI 配置。

ms-rd URI 會提供選項，讓您使用下列格式指定用戶端命令以及該命令特定的一組參數集：

```
ms-rd:command?parameters
```

參數會使用索引鍵/值組 (以 & 分隔) 的查詢字串格式，提供指定命令的其他資訊：

```
param1=value1&param2=value2&…
```

### <a name="commands-and-parameters"></a>命令和參數

以下是目前支援的命令及其對應參數的清單。

在沒有任何命令的情況下使用 `ms-rd:` 啟動用戶端。

#### <a name="subscribe"></a>訂閱

此命令會啟動用戶端並啟動訂閱程序。

**命令名稱：** subscribe

**命令參數：**

| 參數 | 說明                  | 值 |
|-----------|------------------------------|--------|
| URL       | 指定工作區 URL。 | 有效的 URL，例如 <https://contoso.com>。 |

**範例：** ms-rd:subscribe?url=https://contoso.com

## <a name="legacy-rdp-uri-scheme"></a>舊版 rdp URI 配置

>[!NOTE]
> 只有 macOS、iOS 和 Android 裝置的用戶端支援下列 URI 配置。 其正由上述的新 ms-rd URI 所取代。

Microsoft 遠端桌面會使用 URI 配置 rdp://query_string 來儲存啟動用戶端時所使用的預先設定屬性設定。 查詢字串代表在 URL 中提供的單一或一組 RDP 屬性。

RDP 屬性以 & 符號分隔。 例如，連線到電腦時，該字串是：

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

下表提供可搭配 iOS、Mac 及 Android「遠端桌面」用戶端使用的完整支援屬性清單。 (平台欄中的 "x" 表示是支援的屬性。 以 ＞形箭號 (<>) 表示的值代表「遠端桌面」用戶端所支援的值。)

| RDP 屬性                                           | Android | Mac | iOS |
|---------------------------------------------------------|---------|-----|-----|
| allow desktop composition=i:&lt;0 或 1&gt;              | x       | x   | x   |
| allow font smoothing=i:<0 或 1&gt;                      | x       | x   | x   |
| alternate shell=s:&lt;字串&gt;                        | x       | x   | x   |
| [audiomode=i:&lt;0、1 或 2&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393707(v=ws.10)) | x       | x   | x   |
| [authentication level=i:&lt;0 或 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393709(v=ws.10)) | x       | x   | x   |
| connect to console=i:&lt;0 或 1&gt;                     | x       | x   | x   |
| disable cursor settings=i:&lt;0 或 1&gt;                | x       | x   | x   |
| disable full window drag=i:&lt;0 或 1&gt;               | x       | x   | x   |
| disable menu anims=i:&lt;0 或 1&gt;                     | x       | x   | x   |
| disable themes=i:&lt;0 或 1&gt;                         | x       | x   | x   |
| disable wallpaper=i:&lt;0 或 1&gt;                      | x       | x   | x   |
| [drivestoredirect=s:*](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393728(v=ws.10)) (這是唯一支援的值) | x       | x   |     |
| [desktopheight=i:&lt;以像素為單位的值&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393702(v=ws.10)) |         | x   |     |
| [desktopwidth=i:&lt;以像素為單位的值&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393697(v=ws.10))  |         | x   |     |
| [domain=s:&lt;字串&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393673(v=ws.10))                 | x | x | x |
| [full address=s:&lt;字串&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393661(v=ws.10))           | x | x | x |
| gatewayhostname=s:&lt;字串&gt;                  | x | x | x |
| [gatewayusagemethod=i:&lt;1 或 2&gt;](/windows/win32/termserv/imsrdpclienttransportsettings-gatewayusagemethod)                | x | x | x |
| [prompt for credentials on client=i:&lt;0 或 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393660(v=ws.10)) |   | x |   |
| [loadbalanceinfo=s:&lt;字串&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393684(v=ws.10))                  | x | x | x |
| [redirectprinters=i:&lt;0 或 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393671(v=ws.10))                 |   | x |   |
| remoteapplicationcmdline=s:&lt;字串&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 或 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;字串&gt;         | x | x | x |
| shell working directory=s:&lt;字串&gt;          | x | x | x |
| Use redirection server name=i:&lt;0 或 1&gt;      | x | x | x |
| [username=s:&lt;或&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393678(v=ws.10))                  | x | x | x |
| [screen mode id=i:&lt;1 或 2&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393692(v=ws.10))            |   | x |   |
| [session bpp=i:&lt;8、15、16、24 或 32&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393680(v=ws.10)) |   | x |   |
| [use multimon=i:&lt;0 或 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393695(v=ws.10))              |   | x |   |
