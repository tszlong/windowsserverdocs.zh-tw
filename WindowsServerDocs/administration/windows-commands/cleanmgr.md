---
title: cleanmgr
description: " ( # A0) 設定磁片清理工具，以自動清除特定檔案。"
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.date: 06/20/2019
ms.openlocfilehash: c0b1cb2ff31bbf3fa25d5ac5e4be0e4b35260019
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880201"
---
# <a name="cleanmgr"></a>cleanmgr

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2008 R2、Windows Server (半年通道) 

從您的電腦硬碟清除不必要的檔案。 您可以使用命令列選項來指定**cleanmgr.exe 釋放**清除暫存檔案、網際網路檔案、下載的檔案和回收站檔案。 接著，您可以使用 [**排定**的工作] 工具，將工作排程在特定時間執行。

## <a name="syntax"></a>語法

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /d`<driveletter>` | 指定您想要清理磁片的磁片磁碟機。<p>**注意：****/D**選項不會與搭配使用 `/sagerun:n` 。 |
| /sageset： n | 顯示 [**磁片清理設定**] 對話方塊，並建立登錄機碼來儲存您所選取的設定。 `n`儲存在登錄中的值可讓您指定要執行磁片清理的工作。 此 `n` 值可以是從0到65535的任何整數值。 |
| /sagerun： n | 如果您使用**\sageset**選項，則會執行指派給 n 值的指定工作。 系統會列舉電腦上的所有磁片磁碟機，並針對每個磁片磁碟機執行選取的設定檔。 |
| /tuneup： n | 針對相同的執行 **/sageset**和 **/sagerun** `n` 。 |
| /lowdisk | 以預設設定執行。 |
| /verylowdisk | 以預設設定執行，不提示使用者。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="options"></a>選項。

您可以使用 **/sageset**和 **/Sagerun**針對 [磁片清理] 指定的檔案選項包括：

- **暫存安裝**檔-這些是由安裝程式所建立，且不再執行的檔案。

- **下載的程式**檔案-下載的程式檔案是當您查看特定頁面時，自動從網際網路下載的 ActiveX 控制項和 JAVA 程式。 這些檔案會暫時儲存在硬碟上下載的 [Program Files] 資料夾中。 此選項包含 [查看檔案] 按鈕，讓您可以在磁片清理移除之前看到檔案。 此按鈕會開啟 [C:\Winnt\Downloaded Program Files] 資料夾。

- **網際網路暫**存檔-[暫存網際網路檔案] 資料夾包含儲存在硬碟上供快速查看的網頁。 [磁片清理] 會移除這些頁面，但不會保持網頁的個人化設定。 此選項也包含 [查看檔案] 按鈕，它會開啟 [C:\documents and and Settings\Username\Local Settings\Temporary Internet Files\Content.IE5] 資料夾。

- **舊的 chkdsk**檔案-當 chkdsk 檢查磁片是否有錯誤時，chkdsk 可能會將遺失的檔案片段儲存為磁片上根資料夾中的檔案。 這些檔案是不必要的。

- **回收站**-[回收站] 包含您已從電腦中刪除的檔案。 直到您清空回收站之後，才會永久移除這些檔案。 此選項包含開啟 [回收站] 的 [View Files] 按鈕。<p>**注意：** 回收站可能會出現在多個磁片磁碟機中，例如，不只是% SystemRoot%。

- **暫存檔案**-程式有時候會將暫存資訊儲存在暫存資料夾中。 程式結束之前，程式通常會刪除此資訊。 您可以安全地刪除過去一周內未曾修改過的暫存檔案。

- **暫存離線檔案**-暫存離線檔案是最近使用過的網路檔案的本機複本。 系統會自動快取這些檔案，讓您可以在中斷與網路的連線之後使用這些檔案。 [ **View Files** ] 按鈕會開啟 [離線檔案] 資料夾。

- **離線檔案**-離線檔案是您特別想要讓離線使用的網路檔案本機複本，讓您可以在中斷與網路的連線之後使用這些檔案。 [ **View Files** ] 按鈕會開啟 [離線檔案] 資料夾。

- **壓縮舊**檔案-Windows 可以壓縮您最近未使用過的檔案。 壓縮檔案可節省磁碟空間，但您仍然可以使用這些檔案。 不刪除任何檔案。 因為檔案是以不同的速率進行壓縮，所以您將獲得大約的磁碟空間量。 [選項] 按鈕可讓您指定磁片清理壓縮未使用的檔案之前等待的天數。

- **內容索引器的類別目錄**檔案-索引服務會藉由維護磁片上檔案的索引，來加速並改善檔案搜尋。 這些類別目錄檔案會保留在先前的索引編制作業中，而且可以安全地刪除。<p>**注意：** 類別目錄檔案可能會出現在多個磁片磁碟機中，例如，而不是在中 `%SystemRoot%` 。

>[!NOTE]
> 如果您指定清除包含 Windows 安裝的磁片磁碟機，所有這些選項都可以在 [**磁片清理**] 索引標籤上取得。如果您指定任何其他磁片磁碟機，則 [**磁片清理**] 索引標籤上只會提供 [回收站] 和內容索引選項的類別目錄檔案。

## <a name="examples"></a>範例

若要執行磁片清理應用程式，讓您可以使用其對話方塊來指定稍後使用的選項，請將設定儲存至集合**1**，並輸入下列內容：

```
cleanmgr /sageset:1
```

若要執行磁片清理並包含您使用 cleanmgr.exe 釋放/sageset：1命令所指定的選項，請輸入：

```
cleanmgr /sagerun:1
```

若要 `cleanmgr /sageset:1` 同時執行和 `cleanmgr /sagerun:1` ，請輸入：

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>其他參考資料

- [在 Windows 10 中釋放磁碟機空間](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

- [命令列語法關鍵](command-line-syntax-key.md)
