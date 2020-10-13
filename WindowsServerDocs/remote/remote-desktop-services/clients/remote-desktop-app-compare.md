---
title: 遠端桌面 - 比較用戶端應用程式
description: 了解不同的 RD 應用程式在支援特性和功能方面的比較。
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 10/07/2020
ms.localizationpriority: medium
ms.openlocfilehash: 2c34ab9c769beb25437e26442d85f75d40db163e
ms.sourcegitcommit: ed77cfe8140a480b025907716e3635d40a5e3303
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/07/2020
ms.locfileid: "91806633"
---
# <a name="compare-the-clients"></a>比較用戶端

>適用於：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

我們經常被問到與不同的遠端桌面用戶端相較之下有何差異。 其功能是否完全相同？ 以下是這些問題的解答。

## <a name="redirection-support"></a>重新導向支援

下表比較不同用戶端上對裝置和其他重新導向的支援。 這些表格涵蓋您可以在遠端工作階段中存取一次的重新導向。

如果您遠端連線到個人桌面，您可以在 [其他設定]  中為工作階段設定其他重新導向。 如果您的遠端桌面或應用程式是由您組織所管理，則您的系統管理員可以透過群組原則設定或 RDP 屬性來啟用或停用重新導向。

### <a name="input-redirection"></a>輸入重新導向

| 重新導向 | Windows 收件匣</br>(MSTSC) | Windows 桌面</br>(MSRDC) | Microsoft Store 用戶端</br>(URDC) | Android | iOS | macOS | 網頁用戶端    |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|---------------|
| 鍵盤    | X                         | X                           | X             | X       | X   | X     | X             |
| 滑鼠       | X                         | X                           | X             | X       | X\* | X     | X             |
| 觸控       | X                         | X                           | X             | X       | X   |       | X (IE 除外) |
| 手寫筆         | X                         | X                           |               | X (如觸控) |  X (如觸控)  |       |               |

*檢視[遠端桌面 iOS 用戶端支援的輸入裝置清單](remote-desktop-ios.md#supported-input-devices)。

### <a name="port-redirection"></a>連接埠重新導向

| 重新導向 | Windows 收件匣</br>(MSTSC) | Windows 桌面</br>(MSRDC) | Microsoft Store 用戶端</br>(URDC) | Android | iOS | macOS | 網頁用戶端 |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|------------|
| 序列埠 | X                         | X                           |               |         |     |       |            |
| USB         | X                         | X                           |               |         |     |       |            |

當您啟用 USB 連接埠重新導向時，會在遠端工作階段中自動辨識連結到 USB 連接埠的任何 USB 裝置。

### <a name="other-redirection-devices-etc"></a>其他重新導向 (裝置等)

| 重新導向         | Windows 收件匣</br>(MSTSC) | Windows 桌面</br>(MSRDC) | Microsoft Store 用戶端</br>(URDC) | Android | iOS         | macOS                           | 網頁用戶端    |
|---------------------|---------------------------|-----------------------------|---------------|---------|-------------|---------------------------------|---------------|
| 相機             | X                         | X                           |               |     X    |   X         | X                               |               |
| 剪貼簿           | X                         | X                           | X             | Text    | 文字、影像 | X                               | 文字          |
| 本機磁碟機/存放區 | X                         | X                           |               | X       |   X        | X                               |               |
| 位置            | X                         | X                           |               |         |             |                                 |               |
| 麥克風         | X                         | X                           | X             |         |  X          | X                               |               |
| 印表機            | X                         | X                           |               |         |             | X (僅限 CUPS)                   | PDF 列印     |
| 掃描器            | X                         | X                           |               |         |             |                                 |               |
| Smart Cards         | X                         | X                           |               |         |             | X (不支援 Windows 登入) |               |
| Speakers            | X                         | X                           | X             | X       | X           | X                               | X (IE 除外) |

*對於印表機重新導向 - macOS 應用程式預設支援 Publisher 網片輸出機印表機驅動程式。 不支援重新導向原生印表機驅動程式。
