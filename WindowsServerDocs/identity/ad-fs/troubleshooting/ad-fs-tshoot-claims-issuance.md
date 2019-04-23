---
title: AD FS 疑難排解-宣告發行
description: 本文件說明如何使用 AD FS 的權杖發佈問題進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdf8851fe9b35f82191458ba3313fda2dc3ee4cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839659"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>AD FS 疑難排解-宣告發行
宣告是陳述式某主體對於本身或另一個主旨。  信賴憑證者的合作對象所發出的宣告，而且它們會指定一個或多個值，然後封裝在 AD FS 伺服器所發出的安全性權杖。  因為此程序中有數個移動組件，可以為這些重要的部分細分宣告發行。

>[!NOTE]  
>您可以使用[ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)上[ADFS 協助](https://adfshelp.microsoft.com)站台，以協助疑難排解問題的宣告。   

## <a name="token-request"></a>權杖要求
當您移至 信賴憑證者的合作對象時它會將您重新導向至 AD FS 權杖的要求。  問題可能發生的要求。  最值得注意的是：

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>與第 3 方 (特別是，SAML) 格式的要求

### <a name="pre-formated-urls-that-have-typos"></a>Pre 格式的 Url 有錯字
時從 WS Federaion 信賴憑證者簽發的權杖，權杖的要求會隨附 URL 查詢字串參數。  如果信賴憑證者的合作對象不會在 URL 中指定正確的參數，它將會重新導向至 AD FS，則可能會發生問題導致與要求。


為了驗證權杖的格式，您可以使用 web 偵錯工具


## <a name="token-response"></a>權杖回應

## <a name="authentication"></a>驗證

## <a name="claim-rule-processing"></a>宣告規則處理