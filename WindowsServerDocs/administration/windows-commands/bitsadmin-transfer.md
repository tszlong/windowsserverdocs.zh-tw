---
title: bitsadmin transfer
description: Bitsadmin transfer 命令的參考文章，會傳輸一或多個檔案。
ms.topic: reference
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6f4171e5544b468012e308910b4601cd9bde1406
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630557"
---
# <a name="bitsadmin-transfer"></a>bitsadmin transfer

傳送一或多個檔案。 根據預設，BITSAdmin 服務會建立以 **一般** 優先權執行的下載工作，並以進度資訊更新命令視窗，直到傳輸完成或發生嚴重錯誤為止。

如果作業成功傳輸所有檔案，並在發生重大錯誤時取消作業，則服務會完成此作業。 如果服務無法將檔案新增至作業，或如果您為 *類型* 或 *job_priority*指定了不正確值，則此服務不會建立作業。 若要傳送一個以上的檔案，請指定多個 `<RemoteFileName>-<LocalFileName>` 配對。 配對必須以空格分隔。

> [!NOTE]
> 如果發生暫時性錯誤，BITSAdmin 命令會繼續執行。 若要結束命令，請按 CTRL + C。

## <a name="syntax"></a>語法

```
bitsadmin /transfer <name> [<type>] [/priority <job_priority>] [/ACLflags <flags>] [/DYNAMIC] <remotefilename> <localfilename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| NAME | 作業的名稱。 此命令不能是 GUID。 |
| 類型 | 選擇性。 設定作業的類型，包括：<ul><li>**內容.** 預設值。 針對下載作業選擇此類型。</li><li>**上.** 針對上傳作業選擇此類型。</li></ul> |
| priority | 選擇性。 設定工作的優先順序，包括：<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |
| ACLflags | 選擇性。 指出您想要使用所下載的檔案來維護擁有者和 ACL 資訊。 指定一或多個值，包括：<ul><li>**o** -複製具有檔案的擁有者資訊。</li><li>**g** -使用 file 複製群組資訊。</li><li>**d** -複製任意存取控制清單 (DACL) 資訊與檔案。</li><li>**s** -複製系統存取控制清單 (SACL) 具有檔案的資訊。</li></ul> |
| /DYNAMIC | 使用 [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/win32/api/bits5_0/ne-bits5_0-bits_job_property_id)來設定作業，放寬伺服器端需求。 |
| remotefilename | 傳送到伺服器之後的檔案名。 |
| localfilename | 位於本機的檔案名。 |

## <a name="examples"></a>範例

若要啟動名為 *myDownloadJob*的傳送作業：

```
bitsadmin /transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
