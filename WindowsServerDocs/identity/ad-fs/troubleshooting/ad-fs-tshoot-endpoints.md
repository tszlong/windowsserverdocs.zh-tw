---
title: AD FS 疑難排解-AD FS 端點
description: 本檔說明如何針對 AD FS 端點進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 807b5c5de14bf6a43419d0b9d2d3a4e6953d0075
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366219"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS 疑難排解-AD FS 中繼資料端點
端點可讓您存取 AD FS 的同盟伺服器功能，例如發佈同盟中繼資料。  若要確認 AD FS 伺服器正在回應 web 要求，我們可以檢查各種端點。


## <a name="federation-metadata-test"></a>同盟中繼資料測試
被動同盟是指將瀏覽器重新導向至 AD FS 登入頁面的案例。  藉由測試中繼資料端點，我們可以判斷 AD FS 伺服器是否正在回應這些被動案例中的 web 要求。  請使用下列程式來測試端點。

1.  使用網頁瀏覽器，流覽至您的 AD FS 同盟中繼資料端點。  例如： https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. Xml 檔案應該會在本機下載至您的電腦。
3. 開啟該檔案，並確認它包含類似下面資訊的資訊：![Passive @ no__t-1

## <a name="ws-mex-test-active-test"></a>WS-MEX 測試（現用測試）
透過 ws-metadataexchange 是一種 web 服務通訊協定，屬於 WS-同盟藍圖的一部分。  它會使用 SOAP 訊息來要求中繼資料。  藉由測試端點，我們可以判斷 AD FS 伺服器是否正在回應透過 ws-metadataexchange 的 web 要求。  請使用下列程式來測試端點。
1.  使用網頁瀏覽器，流覽至您的 AD FS 同盟中繼資料端點。  例如： https://sts.contoso.com/adfs/services/trust/mex
2. Xml 檔案應該會自動顯示在瀏覽器中。  看起來應該如下圖所示：

![使用中](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)