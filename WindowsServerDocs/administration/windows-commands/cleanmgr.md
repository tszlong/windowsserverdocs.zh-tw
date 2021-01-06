---
title: cleanmgr
description: '設定磁片清理工具 ( # A0) 自動清除某些檔案。'
ms.reviewer: cosmosdarwin
author: JasonGerend
ms.author: jgerend
manager: daveba
ms.date: 06/20/2019
ms.topic: reference
ms.openlocfilehash: ab98e0dd28ccc3529aa6ad8416c14d0960d56125
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946354"
---
# <a name="cleanmgr"></a>cleanmgr

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2008 R2、Windows Server (半年通道) 

清除電腦硬碟中不必要的檔案。 您可以使用命令列選項來指定 **Cleanmgr** 清除暫存檔案、網際網路檔案、下載檔案，以及資源回收筒檔案。 然後，您可以使用 [ **排程** 工作] 工具，將工作排程在特定時間執行。

## <a name="syntax"></a>語法

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /d `<driveletter>` | 指定要清理磁片的磁片磁碟機。<p>**注意：****/D** 選項未使用 `/sagerun:n` 。 |
| /sageset： n | 顯示 [ **磁片清理設定** ] 對話方塊，也會建立登錄機碼來儲存您所選取的設定。 `n`儲存在登錄中的值可讓您指定要執行磁片清理的工作。 `n`值可以是從0到9999的任何整數值。 |
| /sagerun： n | 如果您使用 **/sageset** 選項，則會執行指派給 n 值的指定工作。 系統會列舉電腦上的所有磁片磁碟機，並針對每個磁片磁碟機執行選取的設定檔。 |
| /tuneup： n | 執行相同的 **/sageset** 和 **/sagerun** `n` 。 |
| /lowdisk | 以預設設定執行。 |
| /verylowdisk | 以預設設定執行，無使用者提示。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="options"></a>選項

您可以使用 **/sageset** 和 **/Sagerun** 指定進行磁片清理的檔案選項包括：

- **暫存安裝** 檔案-這些是由安裝程式所建立，且不再執行的檔案。

- **下載的程式** 檔案-下載的程式檔案是在您查看特定頁面時，會自動從網際網路下載的 ActiveX 控制項和 JAVA 程式。 這些檔案會暫時儲存在硬碟上所下載的 [Program Files] 資料夾中。 此選項包含 [View Files] 按鈕，讓您可以在 [磁片清理] 移除檔案之前查看這些檔案。 此按鈕會開啟 [C:\Winnt\Downloaded Program Files] 資料夾。

- **臨時網際網路** 檔案-[暫時性網際網路檔案] 資料夾包含儲存在硬碟上以供快速觀看的網頁。 [磁片清理] 會移除這些頁面，但會讓網頁的個人化設定保持不變。 此選項也包含 [View Files] 按鈕，此按鈕會開啟 C:\documents and 和 Settings\Username\Local Settings\Temporary Internet Files\Content.IE5 資料夾。

- **舊的 Chkdsk** 檔案-當 chkdsk 檢查磁片是否有錯誤時，chkdsk 可能會將遺失的檔案片段儲存為磁片根資料夾中的檔案。 這些檔案是不必要的。

- **資源回收筒** -資源回收筒包含您已從電腦刪除的檔案。 除非您清空資源回收筒，否則不會永久移除這些檔案。 此選項包含開啟資源回收筒的 [View Files] 按鈕。<p>**注意：** 例如，資源回收筒可能會出現在一個以上的磁片磁碟機中，而不只是% SystemRoot%。

- **暫存檔案** ：程式有時會在暫存資料夾中儲存暫存資訊。 程式結束之前，程式通常會刪除此資訊。 您可以安全地刪除過去一周內未曾修改過的暫存檔案。

- **暫時離線檔案** -暫存離線檔案是最近使用之網路檔案的本機複本。 這些檔案會自動快取，讓您可以在中斷網路連線之後使用這些檔案。 [ **View Files** ] 按鈕會開啟離線檔案資料夾。

- **離線檔案** 離線檔案是您特別想要離線使用的網路檔案的本機複本，讓您可以在中斷網路連線之後使用這些檔案。 [ **View Files** ] 按鈕會開啟離線檔案資料夾。

- **壓縮舊** 檔案-Windows 可以壓縮最近未使用的檔案。 壓縮檔案可節省磁碟空間，但您仍然可以使用這些檔案。 不刪除任何檔案。 因為檔案會以不同的速率進行壓縮，所以所顯示的磁碟空間量會是大約的。 [選項] 按鈕可讓您指定磁片清理壓縮未使用的檔案之前要等待的天數。

- **內容索引器的類別目錄** 檔案-索引服務會藉由維護磁片上的檔案索引來加速及改善檔案搜尋。 這些類別目錄檔案會保留在先前的索引編制作業中，而且可以安全地刪除。<p>**注意：** 例如，類別目錄檔案可能會出現在一個以上的磁片磁碟機中，例如不只是在中 `%SystemRoot%` 。

>[!NOTE]
> 如果您指定清除包含 Windows 安裝的磁片磁碟機，[ **磁片清理** ] 索引標籤上會提供所有這些選項。如果您指定任何其他磁片磁碟機，[ **磁片清理** ] 索引標籤上只會提供內容索引選項的資源回收筒和類別目錄檔案。

## <a name="examples"></a>範例

若要執行 [磁片清理] 應用程式，讓您可以使用其對話方塊來指定稍後使用的選項，將設定儲存至集合 **1**，請輸入下列內容：

```
cleanmgr /sageset:1
```

若要執行 [磁片清理]，並包含您使用 cleanmgr/sageset：1命令指定的選項，請輸入：

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
