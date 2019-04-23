---
title: AD FS 疑難排解-Fiddler
description: 本文件說明 Fiddler 為何以及如何安裝和設定 AD FS 宣告問題進行疑難排解的 Fiddler
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 822300d0e4b6ae462a3c942e22530bbed5f93e86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865629"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>AD FS 疑難排解-Fiddler
Fiddler 是工具，可用來擷取 HTTP/HTTPS 網路流量。  這項工具可用來協助疑難排解宣告發行處理程序。  藉由查看流量中，我們可以取得進一步了解互動分解其中。  本文件將說明如何安裝和設定 Fiddler 擷取 AD FS 的流量。  適用於使用 WS-同盟範例 fiddler 追蹤，請參閱[AD FS 疑難排解-Fiddler-WS-同盟](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>下載並安裝 Fiddler
您可以下載 Fiddler[此處](https://www.telerik.com/download/fiddler)。  下載之後它會繼續，並安裝它。

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>設定 Fiddler 擷取 AD FS 流量
若要擷取 AD FS 的流量，我們要設定 Fiddler 以解密 SSL 流量。 

### <a name="configure-the-fiddler-ssl-certificate"></a>設定 Fiddler SSL 憑證
 您可以使用下列程序來設定 Fiddler 以解密 SSL 流量。

1.  開啟 Fiddler
2.  在頂端，底下**工具**，選取**Fiddler 選項**。
3.  按一下 [HTTPS] 索引標籤。
4.  勾選**解密 HTTPS 流量**，然後選取**從瀏覽器只**從下拉式清單。
5.  勾選**略過伺服器憑證錯誤**。
6.  按一下 [確定] 。

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)