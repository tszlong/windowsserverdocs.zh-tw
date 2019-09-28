---
title: AD FS 疑難排解-Fiddler
description: 本檔說明 Fiddler 是什麼，以及如何安裝和設定 Fiddler 以疑難排解 AD FS 的宣告問題
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f2abf1a0b844e8e8799458f5237d7a80059880ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366155"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>AD FS 疑難排解-Fiddler
Fiddler 是一種工具，可用來捕捉 HTTP/HTTPS web 流量。  這項工具可以用來協助針對宣告發行程式進行疑難排解。  藉由查看流量，我們可以進一步瞭解互動中斷的位置。  本檔將說明如何安裝和設定 Fiddler，以捕捉 AD FS 的流量。  如需使用 WS-同盟的範例 fiddler 追蹤，請參閱[AD FS 疑難排解-fiddler-WS-同盟](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>下載並安裝 Fiddler
您可以在[這裡](https://www.telerik.com/download/fiddler)下載 Fiddler。  下載完成後，請繼續進行安裝。

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>設定 Fiddler 來捕捉 AD FS 流量
為了捕捉 AD FS 流量，我們必須設定 Fiddler 來解密 SSL 流量。 

### <a name="configure-the-fiddler-ssl-certificate"></a>設定 Fiddler SSL 憑證
 使用下列程式設定 Fiddler 來解密 SSL 流量。

1.  開啟 Fiddler
2.  在頂端的 [**工具**] 底下，選取 [ **Fiddler 選項**]。
3.  按一下 [HTTPS] 索引標籤。
4.  勾選 [**解密 HTTPS 流量**]，然後只從下拉式選單選取**瀏覽器**。
5.  勾選 [**忽略伺服器憑證錯誤**]。
6.  按一下 [確定]。

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)