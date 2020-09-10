---
title: mount
description: 掛接命令的參考文章，此命令會將網路檔案系統掛接 (NFS) 網路共用。
ms.topic: reference
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cb4f3ce9d6e15aa7c6a504c1bb8e936b49f56267
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640142"
---
# <a name="mount"></a>mount

將網路檔案系統掛接 (NFS) 網路共用的命令列公用程式。 如果使用時沒有選項或引數，則 **掛接** 會顯示所有已掛接 NFS 檔案系統的相關資訊。

> [!NOTE]
> 只有在已安裝 **Client FOR NFS** 的情況下，才能使用此公用程式。

## <a name="syntax"></a>語法

```
mount [-o <option>[...]] [-u:<username>] [-p:{<password> | *}] {\\<computername>\<sharename> | <computername>:/<sharename>} {<devicename> | *}
```

### <a name="parameters"></a>參數

| 參數  | 描述 |
| ---------- | ----------- |
| -o rsize =`<buffersize>` | 設定讀取緩衝區的大小（以 kb 為單位）。 可接受的值為1、2、4、8、16和 32;預設值為 32 KB。 |
| -o wsize =`<buffersize>` | 設定寫入緩衝區的大小（以 kb 為單位）。 可接受的值為1、2、4、8、16和 32;預設值為 32 KB。 |
| -o timeout =`<seconds>` | 設定遠端程序呼叫 (RPC) 的超時時間值（以秒為單位）。 可接受的值為0.8、0.9 和範圍1-60 的任何整數;預設值為0.8。 |
| -o retry =`<number>` | 為軟掛接設定重試次數。 可接受的值是範圍1-10 中的整數;預設值為1。 |
| -o mtype =`{soft|hard}` | 設定 NFS 共用的掛接類型。 根據預設，Windows 會使用軟掛接。 當發生連線問題時，可更輕鬆地執行軟掛接的時間;不過，若要減少 NFS 伺服器重新開機期間的 i/o 中斷情形，我們建議使用硬掛接。|
| -o anon | 以匿名使用者的形式裝載。 |
| -o nolock | 停用鎖定 (預設值為 [已 **啟用** ]) 。 |
| -o casesensitive | 強制服務器上的檔案查閱必須區分大小寫。 |
| -o fileaccess =`<mode>` | 指定在 NFS 共用上建立之新檔案的預設許可權模式。 將 *模式* 指定為 *ogw*格式的三位數數位，其中 *o*、 *g*和 *w* 分別代表授與檔案擁有者、群組和世界的存取權。 數位必須在0-7 範圍內，包括：<ul><li>**0：** 沒有存取權</li><li>**1：** x (執行存取) </li><li>**2：** w (寫入存取) </li><li>**3：** wx (寫入和執行存取權) </li><li>**4：** r (讀取存取權) </li><li>**5：** rx (讀取和執行存取權) </li><li>**6：** rw (讀取和寫入存取) </li><li>**7：** rwx (讀取、寫入和執行存取權) </li></ul> |
| -o lang =`{euc-jp|euc-tw|euc-kr|shift-jis|Big5|Ksc5601|Gb2312-80|Ansi)` | 指定要在 NFS 共用上設定的語言編碼。 您只能在共用上使用一種語言。 這個值可以包含下列任何值：<ul><li>**euc-jp：** 日語</li><li>**euc-幼圓：** 中文</li><li>**euc-kr：** 朝鮮語</li><li>shift-jis **：** 日語</li><li>**Big5：** 中文</li><li>**Ksc5601：** 朝鮮語</li><li>**Gb2312-80：** 簡體中文</li><li>**Ansi：** ANSI 編碼</li></ul> |
| u-sql`<username>` | 指定要用來裝載共用的使用者名稱。 如果 *使用者* 名稱前面沒有 ( ) 的反斜線 **\\** ，則會被視為 UNIX 使用者名稱。 |
| 曲線`<password>` | 用來裝載共用的密碼。 如果您使用星號 (**&#42;**) ，系統將會提示您輸入密碼。 |
| `<computername>` | 指定 NFS 伺服器的名稱。 |
| `<sharename>` | 指定檔案系統的名稱。 |
| `<devicename>` | 指定裝置的磁碟機號和名稱。 如果您使用星號 (**&#42;**) 此值代表第一個可用的磁碟機號。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
