---
title: cmstp
description: Cmstp 的參考文章，會安裝或移除連線管理員的服務設定檔。
ms.topic: reference
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 72c874597bfcb92dfe26ee300621dcac435b5081
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629521"
---
# <a name="cmstp"></a>cmstp

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

安裝或移除連線管理員服務設定檔。 使用時不含選擇性參數， **cmstp** 會安裝服務設定檔，其中包含適用于作業系統的預設設定和使用者的許可權。

## <a name="syntax"></a>語法

語法 1-這是自訂安裝應用程式中所使用的一般語法。 若要使用此語法，您必須**cmstp**從包含檔案的目錄執行 cmstp `<serviceprofilefilename>.exe` 。

```
<serviceprofilefilename>.exe /q:a /c:cmstp.exe <serviceprofilefilename>.inf [/nf] [/s] [/u]
```

語法2
```
cmstp.exe [/nf] [/s] [/u] [drive:][path]serviceprofilefilename.inf
```

#### <a name="parameters"></a>參數
| 參數 | 描述 |
| --------- | ----------- |
| `<serviceprofilefilename>.exe` | 指定包含您要安裝之設定檔的安裝套件（依名稱）。<p>語法1是必要的，但在語法2中無效。 |
| /q： a | 指定應在不提示使用者的情況下安裝設定檔。 仍會出現安裝成功的驗證訊息。<p>語法1是必要的，但在語法2中無效。 |
| [磁片磁碟機：]路徑 `<serviceprofilefilename>.inf` | 必要。 依名稱指定設定檔，以決定應該如何安裝設定檔。<p>語法1的 [drive：] [path] 參數無效。 |
| /nf | 指定不應該安裝支援檔案。 |
| /s | 指定應以無訊息方式安裝或卸載服務設定檔 (不提示使用者回應或顯示驗證訊息) 。 這是您可以搭配 **/u**使用的唯一參數。|
| /U | 指定應卸載服務設定檔。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要在沒有任何支援檔案的情況下安裝 *小說* 服務設定檔，請輸入：

```
fiction.exe /c:cmstp.exe fiction.inf /nf
```

若要以無訊息方式安裝單一使用者的 *小說* 服務設定檔，請輸入：

```
fiction.exe /c:cmstp.exe fiction.inf /s /su
```

若要以無訊息方式卸載 *小說* 服務設定檔，請輸入：

```
fiction.exe /c:cmstp.exe fiction.inf /s /u
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
