---
title: 遠端桌面-比較用戶端應用程式
description: 了解不同的 RD 應用程式的比較方式說到支援的功能和函式。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0e001b590f524711185e3dd70db3bc52a9b8d9af
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447121"
---
# <a name="compare-the-client-apps"></a>比較用戶端應用程式

>適用於：Windows 10，Windows 8.1、 Windows Server 2019、 Windows Server 2016、windows Server 2012 R2

我們經常被詢問不同的遠端桌面用戶端應用程式如何互相比較。 它們全都執行相同的動作嗎？ 以下是這些問題的答案。

## <a name="redirection-support"></a>重新導向支援

下表會比較支援的裝置和其他遠端桌面連線應用程式、 通用應用程式、 Android 應用程式、 iOS 應用程式、 macOS 應用程式和 web 用戶端上的重新導向。 這些表格涵蓋您可以存取一次在遠端工作階段中的重新導向。 

如果您遠端連線至您的個人桌面，還有其他您可以在設定的重新導向**其他設定**工作階段。 如果您的遠端桌面或應用程式由您的組織管理，您的系統管理員可以啟用或停用透過群組原則設定的重新導向。

### <a name="input-redirection"></a>輸入重新導向

| 重新導向 | 遠端桌面<br> 連線 | 通用 | Android | iOS | macOS |          web 用戶端           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  鍵盤   |               X               |     X     |    X    |  X  |   X   |               X               |
|    滑鼠    |               X               |     X     |    X    | X\* |   X   |               X               |
|    觸控    |               X               |     X     |    X    |  X  |       | X （Edge 及 IE 不應是支援） |
|    其他    |              手寫筆              |           |         |     |       |                               |

* 檢視[遠端桌面 iOS Beta 用戶端支援的輸入裝置的清單](remote-desktop-ios.md#supported-input-devices)。

### <a name="port-redirection"></a>連接埠重新導向   

| 重新導向 | 遠端桌面 <br>連線 | 通用 | Android | iOS | macOS | web 用戶端 |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| 序列連接埠 | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

當您啟用 USB 連接埠重新導向時，遠端工作階段已自動辨識任何 USB 連接埠連接的 USB 裝置。

### <a name="other-redirection-devices-etc"></a>其他重新導向 （裝置等等）



| 重新導向         | 遠端桌面連線 | 通用   | Android | iOS         | macOS                                    | web 用戶端    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| 相機             | X                         |             |         |             |                                          |               |
| 剪貼簿           | X                         | 文字、 映像 | 文字    | 文字、 映像 | X                                        | 文字          |
| 本機磁碟機/儲存體 | X                         |             | X       |             | x                                        |               |
| Location            | X                         |             |         |             |                                          |               |
| 麥克風         | X                         |X            |         |             | X                                        |               |
| 印表機            | X                         |             |         |             | X (僅 CUP)                            | PDF 列印     |
| 掃描器            | X                         |             |         |             |                                          |               |
| 智慧卡         | X                         |             |         |             | X （不支援 Windows 驗證） |               |
| 喇叭            | X                         | X           | X       | X           | X                                        | X （除了 IE) |

* 針對印表機重新導向-macOS 應用程式發行者網印表機驅動程式預設支援。 不支援重新導向原生印表機驅動程式。
