---
title: bitsadmin setcredentials
description: 適用于**bitsadmin setcredentials**的 Windows 命令主題，其會將認證新增至作業。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96b3973e9b5c01e2577873fa292e4c0725498f91
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123037"
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
| 工作 | 作業的顯示名稱或 GUID。 |
| 目標 | 請使用**伺服器**或**PROXY**。 |
| scheme (結構描述) | 使用下列其中一項：<ul><li>**基本.** 將使用者名稱和密碼以純文字傳送到伺服器或 proxy 的驗證配置。</li><li>**希.** 挑戰回應驗證配置，使用伺服器指定的資料字串來因應挑戰。</li><li>**NTLM.** 一種挑戰-回應驗證配置，會使用使用者的認證在 Windows 網路環境中進行驗證。</li><li>**NEGOTIATE （也稱為簡單且受保護的協商通訊協定）。** 一種挑戰-回應驗證配置，會與伺服器或 proxy 協調以判斷要使用哪種配置來進行驗證。 範例包括 Kerberos 通訊協定與 NTLM。</li><li>**通行證.** 由 Microsoft 提供的集中式驗證服務，提供成員網站的單一登入。</li></ul> |
| 使用者名稱 | 使用者的名稱。 |
| 密碼 | 與提供的使用者*名稱*相關聯的密碼。 |

## <a name="examples"></a>範例

下列範例會將認證新增至名為*myDownloadJob*的作業。

```
C:\>bitsadmin /setcredentials myDownloadJob SERVER BASIC Edward password20
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)