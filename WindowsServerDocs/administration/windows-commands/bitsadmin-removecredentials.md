---
title: bitsadmin removecredentials
description: 適用于 bitsadmin **removecredentials**的 Windows 命令主題，可從作業中移除認證。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55ff7e2a813c7cc6b60e04d55ef63804a2aed796
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849841"
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
| 工作 | 作業的顯示名稱或 GUID。 |
| 目標 | 請使用**伺服器**或**proxy**。 |
| scheme (結構描述) | 使用下列其中一項：<ul><li>**基本.** 將使用者名稱和密碼以純文字傳送到伺服器或 proxy 的驗證配置。</li><li>**希.** 挑戰回應驗證配置，使用伺服器指定的資料字串來因應挑戰。</li><li>**NTLM.** 一種挑戰-回應驗證配置，會使用使用者的認證在 Windows 網路環境中進行驗證。</li><li>**negotiate （也稱為簡單且受保護的協商通訊協定）。** 一種挑戰-回應驗證配置，會與伺服器或 proxy 協調以判斷要使用哪種配置來進行驗證。 範例包括 Kerberos 通訊協定與 NTLM。</li><li>**通行證.** 由 Microsoft 提供的集中式驗證服務，提供成員網站的單一登入。</li></ul> |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會從名為*myDownloadJob*的作業中移除認證。

```
C:\>bitsadmin /removecredentials myDownloadJob server basic
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)