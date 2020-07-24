---
title: bitsadmin transfer
description: Bitsadmin 傳輸命令的參考文章，它會傳送一或多個檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b2d03fb379c879f445a30dd0f3daf762fed23c7
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955430"
---
# <a name="bitsadmin-transfer"></a>bitsadmin transfer

傳送一或多個檔案。 根據預設，BITSAdmin 服務會建立以**一般**優先權執行的下載作業，並以進度資訊更新命令視窗，直到傳輸完成或發生嚴重錯誤為止。

如果作業成功傳輸所有檔案，且在發生嚴重錯誤時取消作業，服務就會完成工作。 如果無法將檔案加入至作業，或如果您為*類型*或*job_priority*指定了不正確值，服務就不會建立作業。 若要傳送一個以上的檔案，請指定多個 `<RemoteFileName>-<LocalFileName>` 配對。 配對必須以空格分隔。

> [!NOTE]
> 如果發生暫時性錯誤，BITSAdmin 命令會繼續執行。 若要結束命令，請按 CTRL + C。

## <a name="syntax"></a>語法

```
bitsadmin /transfer <name> [<type>] [/priority <job_priority>] [/ACLflags <flags>] [/DYNAMIC] <remotefilename> <localfilename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| NAME | 作業的名稱。 此命令不可以是 GUID。 |
| 類型 | 選擇性。 設定作業的類型，包括：<ul><li>**下載.** 預設值。 請為下載作業選擇此類型。</li><li>**上傳.** 請為上傳作業選擇此類型。</li></ul> |
| priority | 選擇性。 設定作業的優先順序，包括：<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |
| ACLflags | 選擇性。 表示您想要使用所要下載的檔案來維護擁有者和 ACL 資訊。 指定一或多個值，包括：<ul><li>**o** -使用 file 複製擁有者資訊。</li><li>**g** -使用 file 複製群組資訊。</li><li>**d** -使用檔案複製任意存取控制清單（DACL）資訊。</li><li>**s** -使用檔案複製系統存取控制清單（SACL）資訊。</li></ul> |
| /DYNAMIC | 使用[**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/win32/api/bits5_0/ne-bits5_0-bits_job_property_id)設定作業，以放寬伺服器端需求。 |
| remotefilename | 檔案傳送到伺服器之後的名稱。 |
| localfilename | 位於本機的檔案名。 |

## <a name="examples"></a>範例

若要啟動名為*myDownloadJob*的傳送作業：

```
bitsadmin /transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
