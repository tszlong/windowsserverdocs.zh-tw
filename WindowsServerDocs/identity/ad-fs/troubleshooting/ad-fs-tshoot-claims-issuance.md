---
title: AD FS 疑難排解-宣告發行
description: 本檔說明如何針對 AD FS 的權杖發行問題進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.openlocfilehash: 9eb5ce1ee92e828cc1fd6ceb40ddddec453afe87
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954184"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>AD FS 疑難排解-宣告發行
「宣告」（claim）是一個主體對自己或其他主旨的相關語句。  宣告是由信賴憑證者所發出，而且會指定一個或多個值，然後封裝在 AD FS 伺服器所發出的安全性權杖中。  由於此程式中有數個移動元件，因此宣告發行可以細分成這些主要部分。

>[!NOTE]
>您可以在[ADFS](https://adfshelp.microsoft.com)說明網站上使用[ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) ，協助針對宣告問題進行疑難排解。

## <a name="token-request"></a>權杖要求
當您移至信賴憑證者時，它會將您重新導向至具有權杖要求的 AD FS。  要求可能會發生問題。  最值得注意的是：

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>協力廠商的要求格式 (特別是 SAML) 

### <a name="pre-formated-urls-that-have-typos"></a>預先格式化的 Url，具有錯誤輸入
從 Federaion 信賴憑證者發出權杖時，權杖要求會跨越 URL 查詢字串參數。  如果信賴憑證者在重新導向至 AD FS 時未在該 URL 中指定正確的參數，則可能會導致要求發生問題。


為了驗證權杖格式，可以使用 web 偵錯工具工具


## <a name="token-response"></a>權杖回應

## <a name="authentication"></a>驗證

## <a name="claim-rule-processing"></a>宣告規則處理