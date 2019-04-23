---
標題： bitsadmin 快取和 setexpirationtime 描述：「 適用於 Windows 命令主題**bitsadmin 快取和 setexpirationtime** -設定快取到期時間。 」
ms.custom: na ms.prod: windows-server-threshold ms.reviewer: na ms.suite: na ms.technology: manage-windows-commands ms.tgt_pltfrm: na ms.topic: article ms.assetid:00ea6e4e-b707-4b31-88dd-b61a78565c8d author: coreyp-at-msft ms.author: coreyp manager: dongill ms.date:10/16/2017 --

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin 快取和 setexpirationtime
設定快取到期時間。
## <a name="syntax"></a>語法
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|秒|快取到期之前的秒數。|
## <a name="BKMK_examples"></a>範例
下列範例會在 60 秒到期的快取。
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)
