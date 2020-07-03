---
title: fsutil repair
description: Fsutil repair 命令的參考文章，它會管理和監視 NTFS 自我修復修復作業。
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 700e1f713d503565321ab29f5384d74382c64f21
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931198"
---
# <a name="fsutil-repair"></a>fsutil repair

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

管理和監視 NTFS 自我修復修復作業。 自我修復 NTFS 會嘗試在線上更正 NTFS 檔案系統的損毀，而不需要執行**Chkdsk.exe** 。 如需詳細資訊，請參閱[自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))。

## <a name="syntax"></a>語法

```
fsutil repair [enumerate] <volumepath> [<logname>]
fsutil repair [initiate] <volumepath> <filereference>
fsutil repair [query] <volumepath>
fsutil repair [set] <volumepath> <flags>
fsutil repair [wait][<waittype>] <volumepath>

```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 列出 | 列舉磁片區損毀記錄檔的 entires。 |
| `<logname>` | 可以是磁片區中已確認的損毀 `$corrupt` 集 `$verify` ，或磁片區中一組潛在的未驗證損毀。 |
| 初始 | 起始 NTFS 自我修復。 |
| `<filereference>` | 指定 NTFS 磁片區特定的檔案識別碼（檔案參考編號）。 檔案參考包含檔案的區段編號。 |
| 查詢 | 查詢 NTFS 磁片區的自我修復狀態。 |
| set | 設定磁片區的自我修復狀態。 |
| `<flags>` | 指定在設定磁片區的自我修復狀態時要使用的修復方法。<p>這個參數可以設定為三個值：<ul><li>**0x01** -啟用一般修復。</li><li>**0x09** -警告不進行修復的潛在資料遺失。</li><li>**0x00** -停用 NTFS 自我修復修復作業。</li></ul> |
| state | 查詢系統的損毀狀態或指定的磁片區。 |
| wait | 等待修復完成。 如果 NTFS 在執行修復的磁片區上偵測到問題，此選項可讓系統等到修復完成後，再執行任何擱置中的腳本。 |
| `[waittype {0|1}]` | 指出是否等待目前的修復完成，或等候所有修復完成。 *Waittype*參數可以設定為下列值：<ul><li>**0** -等候所有修復完成。 (預設值)</li><li>**1** -等候目前的修復完成。</li></ul> |

### <a name="examples"></a>範例

若要列舉磁片區的已確認損毀，請輸入：

```
fsutil repair enumerate C: $Corrupt
```

若要在 C 磁片磁碟機上啟用自我修復修復，請輸入：

```
fsutil repair set c: 1
```

若要停用 C 磁片磁碟機上的自我修復修復，請輸入：

```
fsutil repair set c: 0
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))
