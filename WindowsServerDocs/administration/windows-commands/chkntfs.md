---
title: chkntfs
description: Chkntfs 命令的參考文章，此命令會在電腦啟動時顯示或修改自動磁片檢查。
ms.topic: reference
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 202594ebef8f65ae8c508fa8a00e83314dbdb38b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629697"
---
# <a name="chkntfs"></a>chkntfs

當電腦啟動時，顯示或修改自動磁片檢查。 如果在沒有選項的情況下使用， **chkntfs** 會顯示指定磁片區的檔案系統。 如果自動檔案檢查已排程執行， **chkntfs** 會顯示指定的磁片區是否已變更，或是否已排程在下次電腦啟動時進行檢查。

> [!NOTE]
> 若要執行 **chkntfs**，您必須是 Administrators 群組的成員。

## <a name="syntax"></a>語法

```
chkntfs <volume> [...]
chkntfs [/d]
chkntfs [/t[:<time>]]
chkntfs [/x <volume> [...]]
chkntfs [/c <volume> [...]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<volume>` [...] | 指定一或多個要在電腦啟動時檢查的磁片區。 有效的磁片區包括磁碟機號 (後面接著冒號) 、掛接點或磁片區名稱。 |
| /d | 還原所有的 **chkntfs** 預設設定，但自動檔案檢查的倒數時間除外。 依預設，當電腦啟動時，系統會檢查所有磁片區，而 **chkdsk** 會在已變更的磁片區上執行。 |
| /t [ `:<time>` ] | 將 Autochk.exe 初始倒數計時時間變更為指定的時間量（以秒為單位）。 如果您沒有輸入時間， **/t** 會顯示目前的倒數計時時間。 |
| /x `<volume>` [...] | 指定一或多個要排除的磁片區，使其不會在電腦啟動時檢查，即使磁片區標示為需要 **chkdsk**也一樣。 |
| /c `<volume>` [...] | 排程一或多個要在電腦啟動時檢查的磁片區，並在已變更的磁片區上執行 **chkdsk** 。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要顯示磁片磁碟機 C 的檔案系統類型，請輸入：

```
chkntfs c:
```

> [!NOTE]
> 如果自動檔案檢查已排程執行，則會顯示其他輸出，指出磁片磁碟機是否已變更，或已手動排程在下次電腦啟動時檢查。

若要顯示 Autochk.exe 起始倒數計時時間，請輸入：

```
chkntfs /t
```

若要將 Autochk.exe 起始倒數計時時間變更為30秒，請輸入：

```
chkntfs /t:30
```

> [!NOTE]
> 雖然您可以將 Autochk.exe 起始倒數計時時間設定為零，但這麼做會讓您無法取消可能耗用時間的自動檔案檢查。

若要排除多個要檢查的磁片區，您必須在單一命令中列出每個磁片區。 例如，若要排除 D 和 E 磁片區，請輸入：

```
chkntfs /x d: e:
```

> [!IMPORTANT]
> **/X**命令列選項不是累加。 如果您多次輸入，最新的專案會覆寫前一個專案。

若要排程 D 磁片區（而非 C 或 E 磁片區）上的自動檔案檢查，請依序輸入下列命令：

```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

> [!IMPORTANT]
> **/C**命令列選項為累加。 如果您多次輸入 **/c** ，則每個專案都會保持不變。 若要確保只檢查特定磁片區，請重設預設值以清除所有先前的命令，並排除所有磁片區的檢查，然後排程所需磁片區的自動檔案檢查。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
