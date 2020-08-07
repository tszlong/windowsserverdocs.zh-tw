---
title: bitsadmin setcredentials
description: Bitsadmin setcredentials 命令的參考文章，其會將認證新增至作業。
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c13c2bd544c485bef59858e7262169a66d1fc94d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893201"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

將認證新增至作業。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /setcredentials <job> <target> <scheme> <username> <password>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| 目標 | 請使用**伺服器**或**PROXY**。 |
| scheme | 您可以使用下列其中一項：<ul><li>**基本.** 將使用者名稱和密碼以純文字傳送到伺服器或 proxy 的驗證配置。</li><li>**希.** 挑戰回應驗證配置，使用伺服器指定的資料字串來因應挑戰。</li><li>**NTLM.** 一種挑戰-回應驗證配置，會使用使用者的認證在 Windows 網路環境中進行驗證。</li><li>**NEGOTIATE (也稱為簡單且受保護的協商通訊協定) 。** 一種挑戰-回應驗證配置，會與伺服器或 proxy 協調以判斷要使用哪種配置來進行驗證。 範例包括 Kerberos 通訊協定與 NTLM。</li><li>**通行證.** 由 Microsoft 提供的集中式驗證服務，提供成員網站的單一登入。</li></ul> |
| 使用者名稱 | 使用者的名稱。 |
| 密碼 | 與提供的使用者*名稱*相關聯的密碼。 |

## <a name="examples"></a>範例

若要將認證新增至名為*myDownloadJob*的作業：

```
bitsadmin /setcredentials myDownloadJob SERVER BASIC Edward password20
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
