---
title: san
description: San 命令的參考文章，為作業系統顯示或設定存放區域網路 (san) 原則。
ms.topic: reference
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8881855e77f8df579cb054954d26296edf4c971f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388367"
---
# <a name="san"></a>san

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

為作業系統顯示或設定存放區域網路 (san) 原則。 如果使用時不含參數，則會顯示目前的 san 原則。

## <a name="syntax"></a>語法

```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| 原則 = {onlineAll|offlineAll|offlineShared}] | 為目前開機的作業系統設定 san 原則。 San 原則會決定新探索到的磁片是上線或保持離線狀態，以及它是否會變成讀取/寫入或保持唯讀狀態。 當磁片離線時，可以讀取磁片版面配置，但不會透過隨插即用呈現磁片區裝置。 這表示磁片上不能裝載任何檔案系統。 當磁片在線上時，會為磁片安裝一或多個磁片區裝置。 以下是每個參數的說明：<ul><li>**onlineAll**。 指定所有新探索到的磁片都將上線並進行讀取/寫入。 **重要事項：** 在共用磁片的伺服器上指定 **onlineAll** 可能會導致資料損毀。 因此，除非伺服器是叢集的一部分，否則您不應該設定此原則。</li><li>**offlineAll**。 指定所有新探索到的磁片（啟動磁片除外）預設為離線且為唯讀。</li><li>**offlineShared**。 指定不在共用匯流排上的所有新探索到的磁片 (例如 SCSI 和 iSCSI) 會上線並進行讀寫。 預設會處於離線狀態的磁片會是唯讀的。</li></ul>如需詳細資訊，請參閱 [VDS_san_POLICY 列舉](https://docs.microsoft.com/windows/win32/api/vds/ne-vds-vds_san_policy?redirectedfrom=MSDN)。 |
| noerr | 僅供腳本使用。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要查看目前的原則，請輸入：

```
san
```

若要讓所有新發現的磁片（啟動磁片除外，預設為離線和唯讀），請輸入：

```
san policy=offlineAll
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
