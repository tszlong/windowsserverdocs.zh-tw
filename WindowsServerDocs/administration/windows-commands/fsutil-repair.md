---
title: fsutil repair
description: Fsutil repair 命令的參考文章，可管理和監視 NTFS 自我修復作業。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 9c26f2be8e586205271ba1d150bb2378cc8b2045
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037326"
---
# <a name="fsutil-repair"></a>fsutil repair

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

管理及監視 NTFS 自我修復的修復作業。 自我修復 NTFS 會嘗試在線上修正 NTFS 檔案系統損毀，而不需要執行 **Chkdsk.exe** 。 如需詳細資訊，請參閱 [自我修復 NTFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771388(v=ws.10))。

## <a name="syntax"></a>語法

```
fsutil repair [enumerate] <volumepath> [<logname>]
fsutil repair [initiate] <volumepath> <filereference>
fsutil repair [query] <volumepath>
fsutil repair [set] <volumepath> <flags>
fsutil repair [wait][<waittype>] <volumepath>

```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 枚舉 | 列舉磁片區損毀記錄檔的 entires。 |
| `<logname>` | 可以是磁片區中已確認損毀的 `$corrupt` 集合 `$verify` ，或磁片區中的一組可能的未經驗證損毀。 |
| 啟動 | 起始 NTFS 自我修復。 |
| `<filereference>` | 指定 NTFS 磁片區特定的檔案識別碼)  (檔案參考號碼。 檔案參考包含檔案的區段編號。 |
| 查詢 | 查詢 NTFS 磁片區的自我修復狀態。 |
| set | 設定磁片區的自我修復狀態。 |
| `<flags>` | 指定當設定磁片區的自我修復狀態時，所要使用的修復方法。<p>這個參數可以設定為三個值：<ul><li>**0x01** -啟用一般修復。</li><li>**0x09** -警告可能遺失資料，而不修復。</li><li>**0x00** -停用 NTFS 自我修復作業。</li></ul> |
| state | 查詢系統的損毀狀態或指定磁片區的損毀狀態。 |
| wait | 等候修復 (s) 完成。 如果 NTFS 偵測到正在執行修復的磁片區發生問題，則此選項可讓系統等到修復完成，然後再執行任何暫止的腳本。 |
| `[waittype {0|1}]` | 指出是否等候目前的修復完成，或等候所有修復完成。 *Waittype*參數可以設定為下列值：<ul><li>**0** -等候所有修復完成。 (預設值)</li><li>**1** -等候目前的修復完成。</li></ul> |

### <a name="examples"></a>範例

若要列舉已確認的磁片區損毀，請輸入：

```
fsutil repair enumerate C: $Corrupt
```

若要在磁片磁碟機 C 上啟用自我修復修復，請輸入：

```
fsutil repair set c: 1
```

若要停用磁片磁碟機 C 的自我修復修復，請輸入：

```
fsutil repair set c: 0
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [自我修復 NTFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771388(v=ws.10))
