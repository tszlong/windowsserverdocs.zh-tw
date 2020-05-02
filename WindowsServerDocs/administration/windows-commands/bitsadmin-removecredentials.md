---
title: bitsadmin removecredentials
description: Bitsadmin removecredentials 命令的參考主題，它會從作業中移除認證。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4dcfaa55847e531871c6a7ad9fd84c3861c4cd9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717051"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

從作業中移除認證。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /removecredentials <job> <target> <scheme>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| 目標 | 請使用**伺服器**或**PROXY**。 |
| scheme | 您可以使用下列其中一項：<ul><li>**基本.** 將使用者名稱和密碼以純文字傳送到伺服器或 proxy 的驗證配置。</li><li>**希.** 挑戰回應驗證配置，使用伺服器指定的資料字串來因應挑戰。</li><li>**NTLM.** 一種挑戰-回應驗證配置，會使用使用者的認證在 Windows 網路環境中進行驗證。</li><li>**NEGOTIATE （也稱為簡單且受保護的協商通訊協定）。** 一種挑戰-回應驗證配置，會與伺服器或 proxy 協調以判斷要使用哪種配置來進行驗證。 範例包括 Kerberos 通訊協定與 NTLM。</li><li>**通行證.** 由 Microsoft 提供的集中式驗證服務，提供成員網站的單一登入。</li></ul> |

## <a name="examples"></a>範例

若要從名為*myDownloadJob*的作業中移除認證：

```
bitsadmin /removecredentials myDownloadJob SERVER BASIC
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
