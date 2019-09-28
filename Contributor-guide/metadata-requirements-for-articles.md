---
title: 將必要的元資料標記新增至 Windows Server 相關文章
description: 您必須在 Windows Server 相關文章頂端新增為元資料標記的資訊清單。 視您的報告和小組需求而定，所需的標記可能會有所變更。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 9bedc7ec13aedab9cfaa655f537d5e431d20cccb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363964"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>將必要的元資料標記新增至 Windows Server 相關文章

在每篇文章的頂端，有特定的中繼資料必須納入以供追蹤和 SEO 之用。 視報告需求而定，所需的標記可能會變更。 不過，如果您需要新增/移除任何欄位，應該會通知您。

它看起來應該像這樣，包括頂端和底部的三個連字號（---）：

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they're looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager's Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>範例

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```