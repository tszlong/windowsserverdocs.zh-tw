---
title: AD FS 疑難排解-AD FS 端點
description: 本文件說明如何疑難排解 AD FS 端點
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13b830c0317341280bd87499e3abd8dcd1a33fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857559"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS 疑難排解-AD FS 的中繼資料端點
端點可讓同盟伺服器功能的 AD FS，例如發佈同盟中繼資料的存取。  若要確認 AD FS 伺服器會回應至 web 要求，我們可以檢查的各種端點。


## <a name="federation-metadata-test"></a>同盟中繼資料的測試
被動同盟是指您的瀏覽器重新導向至 AD FS 登入頁面的案例。  我們可以測試中繼資料端點來判斷是否要將 AD FS 伺服器回應這些被動案例中的 web 要求。  您可以使用下列程序來測試端點。

1.  使用網頁瀏覽器，瀏覽至您的 AD FS 同盟中繼資料端點。  例如：  https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. Xml 檔案應該在本機下載至您的電腦。
3. 開啟它，並確認它所包含的資訊類似下列資訊：![被動](media/ad-fs-tshoot-endpoints/meta2.png)

## <a name="ws-mex-test-active-test"></a>WS-MEX 測試 （作用中的測試）
Ws-metadataexchange 是 web 服務通訊協定，而且是 WS-同盟藍圖的一部分。  它會使用 SOAP 訊息要求中繼資料。  藉由測試端點中，我們可以判斷如果 AD FS 伺服器回應 web 要求的 Ws-metadataexchange。  您可以使用下列程序來測試端點。
1.  使用網頁瀏覽器，瀏覽至您的 AD FS 同盟中繼資料端點。  例如：  https://sts.contoso.com/adfs/services/trust/mex
2. Xml 檔案應該會自動顯示在瀏覽器。  看起來應該像下面的影像：

![使用中](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)