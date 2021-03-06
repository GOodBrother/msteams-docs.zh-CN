---
title: 使用应用程序 Studio 创建选项卡
author: laujan
description: 了解如何使用应用程序 Studio 创建 Microsoft 团队选项卡。
keywords: 团队选项卡组频道可配置
ms.topic: conceptual
ms.author: lajanuar
ms.openlocfilehash: 4e6554a3fdba2ee53bf6d76cd6e25a55b14f7bdd
ms.sourcegitcommit: 3fc7ad33e2693f07170c3cb1a0d396261fc5c619
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "48796153"
---
# <a name="create-a-tab-using-app-studio"></a>使用应用程序 Studio 创建选项卡

> [!TIP]
> 正在寻求更快的入门方式？ 使用 Microsoft 团队工具包创建 [个人](../../build-your-first-app/build-personal-tab.md) 或 [通道和组](../../build-your-first-app/build-channel-tab.md) 选项卡。

通过选项卡，您可以为承载于频道、组聊天和个人用户的 web 内容提供服务。 在较高级别，需要完成以下步骤以创建选项卡：

1. 准备开发环境。
1.  (s) 创建页面。
1. 承载你的应用服务。
1. 创建您的应用程序包并上传到 Microsoft 团队。

[!include[prepare environment](~/includes/prepare-environment.md)]

## <a name="create-your-pages"></a> (s) 创建页面

无论您是在个人还是频道/组作用域中提供选项卡，它都将包含一个或多个您承载的 HTML 页面。 包含个人作用域的选项卡由单个内容页面组成，而具有频道或组作用域的选项卡将需要配置页面，该页面将根据安装时的用户输入设置内容页面的 URL。

有三种类型的选项卡页。 有关创建它们的完整详细信息，请参阅相应的文档页面。

1. [内容页](~/tabs/how-to/create-tab-pages/content-page.md)，在选项卡中显示的页面。
1. [配置页面](~/tabs/how-to/create-tab-pages/configuration-page.md)，用于设置或更新内容页面 URL 的页面，并将其添加到频道/组选项卡。
1. 删除[页面](~/tabs/how-to/create-tab-pages/removal-page.md)，删除频道/组选项卡时显示的可选页面。

### <a name="tab-requirements"></a>选项卡要求

无论页面的类型如何，您都需要遵循以下要求：

* 您必须允许通过 X 框架选项和/或内容安全策略 HTTP 响应标头在 IFrame 中提供页面。
  * 设置标头： `Content-Security-Policy: frame-ancestors teams.microsoft.com *.teams.microsoft.com *.skype.com`        
  * 对于 Internet Explorer 11 兼容性， `X-Content-Security-Policy` 也设置。    
  * 或者，设置标头 `X-Frame-Options: ALLOW-FROM https://teams.microsoft.com/` 。 此标头已弃用，但仍受大多数浏览器的考虑。
* 通常，作为针对 jacking 的安全措施，登录页面不会呈现在 Iframe 中。 因此，身份验证逻辑需要使用重定向 (以外的方法，例如，使用基于令牌的身份验证或基于 cookie 的身份验证) 。

> [!NOTE]
> Chrome 80，安排在早期2020中发布，默认引入新的 cookie 值并强加 cookie 策略。 建议您为 cookie 设置预期用途，而不是依赖于默认浏览器行为。 *请参阅* [SameSite cookie 属性 (2020 update)](../../resources/samesite-cookie-update.md)。

* 浏览器遵循同一个源策略限制，以防止网页发出请求，而不是为 web 页提供服务的其他域。 但是，您可能需要将配置或内容页面重定向到另一个域或子域。 您的跨域导航逻辑应允许团队客户端在加载或与选项卡通信时，针对应用程序清单中的静态 validDomains 列表验证源。

* 若要创建无缝体验，应根据团队客户端的主题、设计和意图来设计选项卡的样式。 通常，在构建以满足特定需求并将重点放在一小部分任务或与选项卡的频道位置相关的数据子集时，选项卡的工作效果最佳。

* 在内容页面中，添加对 [Microsoft 团队 JavaScript 客户端 SDK](/javascript/api/overview/msteams-client) （使用脚本标记）的引用。 在页面加载后，对进行调用 `microsoftTeams.initialize()` 。 如果不显示页面，则不会显示。

## <a name="host-your-app-service"></a>承载你的应用服务

需要将你的内容托管在可使用 HTTPS 的公开可用 URL 上。 对于测试，可以使用反向代理（如 [ngrok](https://ngrok.com/)）将本地端口公开给面向 INTERNET 的 URL。

## <a name="create-your-app-package-with-app-studio"></a>使用应用程序 Studio 创建应用程序包

您可以使用 Microsoft 团队客户端中的应用程序 Studio 应用来帮助创建应用程序清单。 如果未在团队中安装应用程序 studio，请选择 "团队" 应用的左下角的 " **应用** ![ 商店应用"， ](/microsoftteams/platform/assets/images/tab-images/storeApp.png) 然后搜索应用程序 studio。 找到该磁贴后，选择它并在弹出窗口对话框中选择 "安装"。

1. 打开 Microsoft 团队客户端—使用 [基于 web 的版本](https://teams.microsoft.com) ，您可以使用浏览器的 [开发人员工具](~/tabs/how-to/developer-tools.md)检查您的前端代码。
1. 打开应用程序 Studio 并选择 " **清单编辑器** " 选项卡。
1. 选择 " **创建新的应用程序** " 磁贴。
1. 添加应用程序详细信息 (请参阅 [清单架构定义](~/resources/schema/manifest-schema.md) ，以获取每个字段) 的完整说明。
1. 在 "功能" 部分，选择 " **选项卡** "。
    * 对于 "个人" 选项卡，选择 " *添加个人" 选项卡* ，然后选择 " **添加** "。 将显示一个弹出对话框窗口，可在其中添加选项卡详细信息。
    * 对于 "通道/组" 选项卡，在 " *团队"* 选项卡下选择 " **添加** " 并完成 "团队" 选项卡弹出窗口中的选项卡详细信息字段。 请确保 *可以更新配置？* 选中 "工作组" 和 " *组" 聊天* 框，然后选择 " **保存** "。
1. 在 " *域和权限* " 部分中，" *选项卡* " 字段中的域应包含不带 HTTPS 前缀的主机或反向代理 URL。
1. 在 " **完成**  =>  **测试和分布** " 选项卡上，您可以 **下载** 应用程序包，将包 **安装** 到团队中，或 **提交** 到团队应用商店以供审批。 *如果使用反向代理，将在右侧的 " **说明** " 字段中收到警告。在测试选项卡时可以忽略此警告* 。

## <a name="create-your-app-package-manually"></a>手动创建应用程序包

与 bot 和邮件扩展一样，您可以更新应用程序 [清单](~/resources/schema/manifest-schema.md) 以包含选项卡属性。 这些属性控制您的选项卡在中可用的范围、要使用的 Url 以及各种其他属性。

### <a name="personal-tabs"></a>个人选项卡

"个人" 选项卡的显示内容对于所有用户都是相同的，并在阵列中进行配置 `staticTabs` 。 您可以在应用程序中声明最高 16 (16) 个人选项卡。

|名称| 类型| 最大大小 | 必需 | 说明|
|---|---|---|---|---|
|`entityId`|字符串|64 个字符|✔|选项卡显示的实体的唯一标识符。|
|`name`|String|128个字符|✔|该选项卡在通道接口中的显示名称。|
|`contentUrl`|String|2048 个字符|✔|指向要在团队画布中显示的实体 UI 的 https://URL。|
|`websiteUrl`|String|2048 个字符||要指向的 https://URL，如果用户要在浏览器中查看。|
|`scopes`|枚举数组|1|✔|静态选项卡仅支持 `personal` 作用域，这意味着只能将其设置为个人应用程序的一部分。|

#### <a name="simple-personal-tab-manifest-example"></a>简单的个人选项卡清单示例

下面的示例仅展示了 `staticTabs` 应用程序清单中的数组。

```json
...
"staticTabs": [
    {
      "entityId": "idForPage",
      "name": "Display name for tab",
      "contentUrl": "https:// yourURL.com/content ",
      "websiteUrl": "https://yourURL.com/website",
      "scopes": [ "personal" ]
    }
...
```

### <a name="channelgroup-tabs"></a>通道/组选项卡

在阵列中添加通道/组选项卡 `configurableTabs` 。 您只能在数组中声明一个通道/组选项卡 `configurableTabs` 。

|名称| 类型| 最大大小 | 必需 | 说明|
|---|---|---|---|---|
|`configurationUrl`|String|2048 个字符|✔|指向 "配置" 页的 https://URL。|
|`canUpdateConfiguration`|布尔值|||一个值，指示是否可在用户创建之后更新该选项卡的配置实例。 设置 `true`|
|`scopes`|枚举数组|1|✔|可配置的选项卡仅支持 `team` 和 `groupchat` 范围。 |

#### <a name="simple-channelgroup-tab-manifest-example"></a>简单通道/组选项卡清单示例

下面的示例仅展示了 `configurableTabs` 应用程序清单中的数组。

```json
...
"configurableTabs": [
    {
      "configurationUrl": "https://yourURL.com/configure",
      "canUpdateConfiguration": true,
      "scopes": [ "team", "groupchat" ]
    }
...
```

将 `manifest.json` 其捆绑到 zip 文件夹中，并将它与两个必需的图标组合在一起。

### <a name="upload-app-package-directly-to-a-team"></a>将应用程序包直接上载到团队

1. 打开 Microsoft 团队客户端。 如果您使用的是 [基于 web 的版本](https://teams.microsoft.com) ，则可以使用浏览器的 [开发人员工具](~/tabs/how-to/developer-tools.md)检查您的前端代码。
1. 在左侧的 " *YourTeams* " 面板中，选择要用于 `...` 测试选项卡的团队旁边的菜单，然后选择 " **管理团队** "。
1. 在主面板中，从选项卡栏中选择 " **应用** "，然后选择 "上载" 位于页面右下角的 **自定义应用程序** 。
1. 打开您的项目目录，浏览到 " **/package** " 文件夹，选择 "应用程序包" zip 文件夹，然后选择 " **打开** "。 您的选项卡将上传到团队。

### <a name="view-your-tab-in-teams"></a>在团队中查看您的选项卡

1. 查看您的个人选项卡：
    * 在位于团队客户端最左侧的导航栏中，选择 `...` 菜单并从列表中选择您的应用程序。

1. 查看频道/组选项卡：
    * 返回到您的团队，选择要在其中显示选项卡的频道，从选项卡栏中选择 "➕"，然后从库中选择您的选项卡。
    * 按照有关添加选项卡的说明操作。请注意，"通道/组" 选项卡有一个自定义配置对话框。选择 " **保存** "，您的选项卡将添加到频道的选项卡栏中。

## <a name="learn-more"></a>了解详细信息

* [创建选项卡的内容页](~/tabs/how-to/create-tab-pages/content-page.md)
* [创建选项卡的配置页](~/tabs/how-to/create-tab-pages/configuration-page.md)
* [更新或删除选项卡](~/tabs/how-to/create-tab-pages/removal-page.md)
