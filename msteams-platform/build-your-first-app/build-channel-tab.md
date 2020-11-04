---
title: 入门-构建通道和组选项卡
author: heath-hamilton
description: 使用 Microsoft 团队工具包快速创建 Microsoft 团队频道和分组选项卡。
ms.author: lajanuar
ms.date: 10/09/2020
ms.topic: tutorial
ms.openlocfilehash: f890754cdf4ca43f39c25e3ba24fcf47b08c5a9f
ms.sourcegitcommit: d61f14053fc695bc1956bf50e83956613c19ccca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "48452853"
---
# <a name="build-a-channel-and-group-tab-for-microsoft-teams"></a>为 Microsoft 团队构建通道和组选项卡

在本教程中，你将构建基本的 *频道选项卡* (也称为 " *组" 选项* 卡) （它是团队频道或聊天的全屏页面）。 与 "个人" 选项卡不同，用户可以配置此类选项卡的某些方面 (例如，重命名选项卡，使其对频道) 有意义。

## <a name="your-assignment"></a>您的分配

以前，你的组织创建了一个 "团队" 选项卡，其中包含有关如何联系重要功能 (技术支持、人力资源等 ) 的信息。 但是，由于该选项卡的作用范围仅供个人使用，因此每个用户都必须安装该选项卡，才能看到它，并且采用低于预期。 换句话说，工作线程太多仍不知道如何联系技术支持人员。

您可以通过构建通道选项卡使此信息更易于查找，这将消除要求每个人安装应用程序的负担。 相反，一个用户可以在频道或聊天中安装该选项卡，以了解整个组的好处。

## <a name="what-youll-learn"></a>你将了解的内容

> [!div class="checklist"]
>
> * 使用 Microsoft 团队工具包创建适用于 Visual Studio Code 的应用程序项目
> * 确定与通道和组选项卡相关的一些应用程序清单属性和基架
> * 在本地托管应用程序
> * 创建选项卡内容
> * 创建选项卡的配置页的内容
> * 允许配置和安装选项卡
> * 提供建议的选项卡名称

## <a name="1-create-your-app-project"></a>1. 创建您的应用程序项目

Microsoft 团队工具包可帮助您设置与通道和组选项卡相关的应用程序清单和基架，包括显示 "Hello，World！" 的基本配置页和内容页。 消息。

> [!TIP]
> 如果之前尚未创建团队应用程序项目，您可能会发现，请按照 [以下说明](../build-your-first-app/build-and-run.md) 更详细地说明项目。

1. 在 Visual Studio Code 中，选择左侧活动栏上的 " **Microsoft 团队**"， :::image type="icon" source="../assets/icons/vsc-toolkit.png"::: 然后选择 " **创建新的团队应用**"。
1. 为你的团队应用输入名称。  (这是您的应用程序的默认名称，也是本地计算机上的应用程序项目目录的名称。 ) 
1. 在 " **添加功能** " 屏幕上，依次选择 **"选项卡" 和 "** **团队通道" 选项卡**。
1. 选择屏幕底部的 " **完成** " 以配置项目。  

## <a name="2-identify-relevant-app-project-components"></a>2. 确定相关的应用程序项目组件

大多数应用程序清单和基架是在使用团队工具包创建项目时自动设置的。 我们来看看构建通道和组选项卡的主要组件。

### <a name="app-manifest"></a>应用程序清单

应用程序清单中的以下代码段显示了 [`configurableTabs`](../resources/schema/manifest-schema.md#configurabletabs) ，其中包括与通道和组选项卡相关的属性和默认值。

```JSON
"configurableTabs": [
    {
        "configurationUrl": "{baseUrl0}/config",
        "canUpdateConfiguration": true,
        "scopes": [
            "team",
            "groupchat"
        ]
    }
],
```

* `configurationUrl`：您的选项卡配置页的主机 URL (必须是 HTTPS) 。
* `canUpdateConfiguration`：如果设置为 `true` ，则用户可以更改选项卡设置、重命名选项卡或将其从频道或聊天中删除。
* `scopes`：指定用户是否可以在频道中安装应用程序 (`team`) 和聊天 (`groupchat`) 。 至少需要一个值。

### <a name="app-scaffolding"></a>应用程序基架

应用程序基架提供一个 `TabConfig.js` 文件，该文件位于 `src/components` 项目目录中，用于呈现您的选项卡的配置页面即将在此) 中 (详细信息。

## <a name="3-run-your-app"></a>3. 运行您的应用程序

在时间方面，你将在本地构建和运行应用程序。

1. 在终端中，转到您的应用程序项目的根目录并运行该目录 `npm install` 。
1. 运行 `npm start` 。

完成后，已 **成功编译了！** 终端中的邮件。

## <a name="4-set-up-a-secure-tunnel-to-your-app"></a>4. 设置应用程序的安全隧道

出于测试目的，让我们将您的选项卡托管在本地 web 服务器上 (端口 3000) 。

1. 在终端中，运行 `ngrok http 3000` 。
1. 复制你提供的 HTTPS URL。
1. 在 `.publish` 目录中，打开 `Development.env` 。
1. 将 `baseUrl0` 值替换为复制的 URL。  (例如，改 `baseUrl0=http://localhost:3000` 为 `baseUrl0=https://85528b2b3ca5.ngrok.io` 。 ) 

你的应用程序清单指向你承载该选项卡的位置。

## <a name="5-customize-your-tab-content-page"></a>5. 自定义 "选项卡内容" 页

在目录中打开应用程序清单 (`manifest.json`) ， `.publish` 并在中设置以下属性 [`staticTabs`](../resources/schema/manifest-schema.md#statictabs) 来定义您的选项卡的内容页面。

```JSON
"staticTabs": [
    {
        "entityId": "index",
        "name": "My Contacts",
        "contentUrl": "{baseUrl0}/tab",
        "scopes": [ "personal" ]
    }
],
```

使用与您的组织相关的信息复制和更新以下代码段，如果有时间，请使用代码。

```JSX
<div>
  <h1>Important Contacts</h1>
    <ul>
      <li>Help Desk: <a href="mailto:support@company.com">support@company.com</a></li>
      <li>Human Resources: <a href="mailto:hr@company.com">hr@company.com</a></li>
      <li>Facilities: <a href="mailto:facilities@company.com">facilities@company.com</a></li>
    </ul>
</div>
```

转到 `src/components` 目录并打开 `Tab.js` 。 找到 `render()` 函数并将内容粘贴 (中， `return()` 如) 所示。

```JavaScript
render() {

    let userName = Object.keys(this.state.context).length > 0 ? this.state.context['upn'] : "";

    return (
    <div>
      <h1>Important Contacts</h1>
        <ul>
          <li>Help Desk: <a href="mailto:support@company.com">support@company.com</a></li>
          <li>Human Resources: <a href="mailto:hr@company.com">hr@company.com</a></li>
          <li>Facilities: <a href="mailto:facilities@company.com">facilities@company.com</a></li>
        </ul>
    </div>
    );
}
```

将以下规则添加到 `App.css` ，无论使用哪个主题，电子邮件链接更易于阅读。

```CSS
a {
  color: inherit;
}
```

## <a name="6-create-your-tab-configuration-page"></a>6. 创建选项卡配置页

频道或聊天中的每个选项卡都有一个配置页面，其中包含至少一个安装选项的模式，在用户安装您的应用程序时显示。 默认情况下，配置页面会询问用户是否要在安装该选项卡时通知频道或聊天。

将一些内容添加到您的配置页面。 转到项目的 `src/components` 目录，打开 `TabConfig.js` ，并在 (中插入一些内容， `return()` 如) 所示。

```JavaScript
return (
    <div>
      <h1>Add My Contoso Contacts</h1>
      <div>
        Select <b>Save</b> to add our organization's important contacts to this workspace.
      </div>
    </div>
);
```
 
> [!TIP]
> 至少，在此页面上提供有关您的应用程序的一些简要信息，因为这可能是用户首次了解它。 您还可以包括自定义配置选项或 [身份验证工作流](../tabs/how-to/authentication/auth-aad-sso.md)，这在选项卡配置页上很常见。

## <a name="7-allow-the-tab-to-be-configured-and-installed"></a>7. 允许配置和安装该选项卡

若要使用户能够成功配置和安装该选项卡，您必须将 [设置的安全主机 URL](#4-set-up-a-secure-tunnel-to-your-app) 添加到配置页面组件。

转到 `TabConfig.js` 并找到 `microsoftTeams.settings.setSettings` 。 对于 `"contentUrl"` ，将 URL 的一部分替换为 `localhost:3000` 您在其中承载选项卡内容的域 (如) 所示。

```JavaScript
microsoftTeams.settings.setSettings({
  "contentUrl": "https://<MY_HOST_DOMAIN>/tab"
});
```

此外，请确保 `microsoftTeams.settings.setValidityState(true);` 。 默认情况下，它是默认设置，但如果设置为 `false` ，将禁用 "配置" 页上的 " **保存** " 按钮。

## <a name="8-provide-a-suggested-tab-name"></a>8. 提供建议的选项卡名称

在安装用于个人用途的选项卡时，显示名称是 `name` `staticTabs` 应用部件清单的部分中的属性 (例如，我的 **联系人**) 。 安装通道选项卡时，默认情况下，应用名称会显示 (例如， **第一个应用程序**) 。

这可能会很好，具体取决于您调用应用程序的内容，但您可能希望提供一个名称，以便在组协作的上下文中更有意义 (例如， **工作组联系人**) 。

在中 `TabConfig.js` ，返回到 `microsoftTeams.settings.setSettings` 。 添加 `suggestedDisplayName` 默认情况下要显示的选项卡名称的属性 (如) 所示。 使用提供的名称或创建自己的名称。 请记住，在清单中，如果您允许用户根据需要更改名称。

```JavaScript
microsoftTeams.settings.setSettings({
  "contentUrl": "https://<MY_HOST_DOMAIN>/tab",
  "suggestedDisplayName": "Team Contacts"
});
```

## <a name="9-view-the-tab"></a>9. 查看选项卡

若要查看您的选项卡的配置和内容页，必须在频道或聊天中安装它。

1. 在 "团队客户端" 中，选择 " **应用**"。
1. 选择 " **上载自定义应用程序** "，然后选择您的应用程序 `Development.zip` 。
1. 选择 " **添加到团队** " 或 " **添加到聊天** "，找到可用于测试的频道或聊天。
1. 选择 **"设置选项卡"**。将显示 "配置" 页。<br/>
   :::image type="content" source="../assets/images/tabs/channel-tab-tutorial-content.png" alt-text="频道选项卡配置页的屏幕截图。&quot;:::
1. 选择 &quot; **保存** " 以配置选项卡。将显示内容。<br/>
   :::image type="content" source="../assets/images/tabs/channel-tab-tutorial-content-installed.png" alt-text="频道选项卡配置页的屏幕截图。&quot;:::
1. 选择 &quot; **保存** ":::

## <a name="well-done"></a>干的好

恭喜你！ 您有一个包含选项卡的团队应用程序，用于在频道和聊天中显示有用的内容。

## <a name="learn-more"></a>了解详细信息

* [使用 Sso 对选项卡用户进行身份验证](../tabs/how-to/authentication/auth-aad-sso.md)：如果您仅希望授权用户查看您的选项卡，请通过 Azure Active DIRECTORY (AD) 设置单一登录 (SSO) 。
* [从现有 web 应用或网页嵌入内容](../tabs/how-to/add-tab.md#tab-requirements)：我们向您介绍了如何为个人选项卡创建新内容，但您也可以从外部 URL 加载内容。
* [为您的选项卡创建无缝体验](../tabs/design/tabs.md)：有关设计团队选项卡的建议指南，请参阅。
* [为移动设备构建选项卡](../tabs/design/tabs-mobile.md)：了解如何为电话和平板电脑开发选项卡。
* [创建不带工具箱的选项卡](../tabs/how-to/add-tab.md)

## <a name="next-lesson"></a>下一课

您知道如何构建协作选项卡。 想要尝试构建不同种类的团队应用吗？

> [!div class="nextstepaction"]
> [创建机器人](../build-your-first-app/build-bot.md)