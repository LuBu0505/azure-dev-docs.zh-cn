---
title: 教程：通过 Azure 门户使用 PostgreSQL 部署 Django 应用
description: 在 Azure 上预配 Web 应用和 PostgreSQL 数据库，然后从 GitHub 部署应用代码。
ms.devlang: python
ms.topic: tutorial
ms.date: 07/23/2020
ms.custom: devx-track-python
ms.openlocfilehash: 19c0dda48b0fb7b5b0c3af75a1d94d3c9b9e4080
ms.sourcegitcommit: 4824cea71195b188b4e8036746f58bf8b70dc224
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2020
ms.locfileid: "89753764"
---
# <a name="tutorial-deploy-a-django-web-app-with-postgresql-using-the-azure-portal"></a>教程：通过 Azure 门户使用 PostgreSQL 部署 Django Web 应用

通过 Azure 门户，可以将数据驱动的 Python [Django](https://www.djangoproject.com/) Web 应用部署到 [Azure 应用服务](/azure/app-service/overview#app-service-on-linux)，并将其连接到 [Azure Database for PostgreSQL](/azure/postgresql/) 数据库。 你可以从可在以后任何时候纵向扩展的免费定价层着手。

在本例中，Web 应用代码来自 GitHub 存储库，你可以配置 Web 应用以便从 GitHub 进行持续部署。 配置后，可以在本地计算机上执行进一步的开发，并将更改提交到存储库。 然后，Azure 上的 Web 应用会自动部署这些更改。

在本教程中，使用 Azure 门户完成以下任务：

> [!div class="checklist"]
> - 在 Azure 中预配从 GitHub 存储库部署的 Web 应用
> - 在 Azure 中预配 PostgreSQL 服务器和数据库，然后将其连接到 Web 应用。
> - 更新代码并提交更改，以便从 GitHub 自动重新部署。
> - 查看诊断日志
> - 在 Azure 门户中管理 Web 应用

你还可以使用[本教程中基于 Azure CLI 的版本](/azure/app-service/tutorial-python-postgresql-app)。

## <a name="fork-the-sample-repository"></a>创建示例存储库分支

在浏览器中，导航到 [https://github.com/Azure-Samples/djangoapp](https://github.com/Azure-Samples/djangoapp)，并创建存储库分支到自己的 GitHub 帐户。

你可以创建此存储库的分支，以便在后续步骤中进行更改并重新部署代码。

（可选）关于本示例：djangoapp 示例包含数据驱动的 Django 投票应用，该应用是根据 Django 文档中的[编写你的第一个 Django 应用](https://docs.djangoproject.com/en/2.1/intro/tutorial01/)创建的。 此外，使用 [Django 部署清单](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/) 修改了该示例，以便在 Azure 应用服务等生产环境中运行。 （这些更改适用于任何生产环境，并不特定于 Azure。）

- 生产设置位于 azuresite/production.py 文件中。 开发详细信息位于 azuresite/settings.py 中。

- 当 `DJANGO_ENV` 环境变量设置为“生产”时，应用将使用生产设置。 你将稍后在本教程中创建此环境变量以及用于 PostgreSQL 数据库配置的其他环境变量。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="provision-the-web-app-in-azure"></a>在 Azure 中预配 Web 应用

1. 打开 [Azure 门户](https://portal.azure.com)

1. 选择“创建资源”。

1. 在“新建”页的“常用”列中，选择“Web 应用”  。 （如果未立即显示“Web 应用”，请在“Azure 市场”下选择“Web”，然后在“特别推荐”下选择“Web 应用”。    ） 

1. 在“创建 Web 应用”页面上，输入以下信息：

    | 字段 | 值 |
    | --- | --- |
    | 订阅 | 如果你不想使用默认订阅，可选择想要使用的订阅。 |
    | 资源组 | 选择“新建”，然后输入“DjangoPostgres-Tutorial-rg”。 |
    | 应用程序名称 | Web 应用的名称，该名称在所有 Azure 中都是唯一的（应用的 URL 为 `https://\<app-name>.azurewebsites.net`）。 允许的字符有 `A`-`Z`、`0`-`9` 和 `-`。 良好的模式是结合使用公司名称和应用标识符。 |
    | 发布 | 选择“代码”。 |
    | 运行时堆栈 | 从下拉列表中选择“Python 3.8”。 |
    | 区域 | 选择附近的位置。 |
    | Linux 计划 | 门户将基于资源组使用应用服务计划名称填充此字段。 如果要更改名称，请选择“新建”。 |
    | SKU 和大小 | 为获得最佳性能，请使用默认计划，尽管它会在订阅中产生费用。 若要避免产生费用，请选择“更改大小”，然后依次选择“开发/测试”、“B1”（免费 30 天）和“应用”   。 后续可以扩展计划以获得更好的性能。 |

1. 依次选择“查看 + 创建”、“创建” 。 Azure 预配 Web 应用需要数分钟。

1. 完成预配后，选择“转到资源”，打开 Web 应用的概述页面。 使该浏览器窗口或选项卡处于打开状态，以供后续步骤使用。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="provision-the-postgresql-database-server-in-azure"></a>在 Azure 中预配 PostgreSQL 数据库服务器

1. 通过 [Azure 门户](https://portal.azure.com)打开新的浏览器窗口或选项卡。 使用新的选项卡预配数据库，因为你需要将部分信息从数据库页面传输到上一节中仍处于打开状态的 Web 应用页面。

1. 选择“创建资源”。

1. 在“新建”页面上，选择“Azure 市场”下的“数据库”，然后选择“特别推荐”下的“Azure Database for PostgreSQL”。    ）

1. 在下一页上，选择“单个服务器”下的“创建”。

1. 在“单个服务器”页面上，输入以下信息：

    | 字段 | 值 |
    | --- | --- |
    | 订阅 | 如果你不想使用默认订阅，可选择想要使用的订阅。 |
    | 资源组 | 选择在上一节中创建的“DjangoPostgres-Tutorial-rg”组。 |
    | 服务器名称 | 数据库服务器的名称，该名称在所有 Azure 中都是唯一的（应用的 URL 为 `https://\<server-name>.postgres.database.azure.com`）。 允许的字符有 `A`-`Z`、`0`-`9` 和 `-`。 良好的模式是结合使用公司名称和服务器标识符。 |
    | 数据源 | **无** |
    | 位置 | 选择附近的位置。 |
    | 版本 | 保留默认值（即最新版）。 |
    | 计算 + 存储 | 选择“配置服务器”，然后选择“基本”和“第 5 代”  。 将“vCore”设置为 1，将“存储”设置为 5 GB，然后选择“确定”  。 这些选项可预配成本最低的服务器，该服务器可用于 Azure 上的 PostgreSQL。 |
    | 管理员用户名、密码、确认密码 | 在数据库服务器上输入管理员帐户的凭据。 记下这些凭据，稍后在本教程中将需要使用。 |

1. 选择“查看 + 创建”，然后选择“创建” 。 Azure 预配 Web 应用需要数分钟。

1. 完成预配后，选择“转到资源”，打开数据库服务器的概述页面。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="create-the-pollsdb-database-on-the-postgresql-server"></a>在 PostgreSQL 服务器上创建 pollsdb 数据库

在本节中，连接 Azure Cloud Shell 中的数据库服务器，并使用 PostgreSQL 命令在服务器上创建 pollsdb 数据库。 示例应用代码需要此数据库。

1. 在 PostgreSQL 服务器的概述页面中，选择“连接安全性”（在左侧的“设置”下方） 。

    ![防火墙规则的门户连接安全性页面](media/tutorial-python-postgresql-app-portal/server-firewall-rules.png)

1. 选择标记为“添加 0.0.0.0 - 255.255.255.255”的按钮，然后在显示的弹出消息中选择“继续”，接着在页面顶部选择“保存”  。 这些操作会添加一条规则，该规则使你可以从 Cloud Shell 和 SSH 连接到数据库服务器（就像在后续某节中为运行 Django 数据模型迁移所执行的操作一样）。

1. 选择窗口顶部的 Cloud Shell 图标，从 Azure 门户打开 Azure Cloud Shell：

    ![Azure 门户工具栏中的 Cloud Shell 按钮](media/tutorial-python-postgresql-app-portal/portal-launch-icon.png)

1. 在 Cloud Shell 中运行以下命令：

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --port=5432 --username=<user-name>@<server-name> --dbname=postgres
    ```

    将 `<server-name>` 和 `<user-name>` 替换为上一节中配置服务器时所用的名称。 请注意，完整的用户名值为 `<user-name>@<server-name>`。

    可以复制上述命令，然后单击右键，选择“粘贴”将命令粘贴到 Cloud Shell 中。

    在出现提示时输入管理员密码。

1. Shell 成功建立连接后，你将看到提示 `postgres=>`。 该提示表示你已连接到名为“postgres”的默认管理数据库。 （“postgres”数据库不适用于应用。）

1. 在提示下，运行命令 `CREATE DATABASE pollsdb;`。 请确保包含结束分号，这样命令才完整。

1. 如果数据库创建成功，则命令应显示 `CREATE DATABASE`。 若要验证是否已创建数据库，请运行 `\c pollsdb`。 此命令应将提示更改为 `pollsdb=>`，表示成功创建。

1. 运行命令 `exit` 以退出 psql。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="connect-the-database"></a>连接数据库

在本节中，为 Web 应用创建它连接 `pollsdb` 数据库所需的设置。 这些设置在应用代码中显示为环境变量。 （有关详细信息，请参阅[访问环境变量](/azure/app-service/configure-language-python#access-environment-variables)。）

1. 切换回上一节中所创建的 Web 应用的浏览器选项卡或窗口。

1. 选择“配置”（在左侧的“设置”下），然后在页面顶部选择“应用程序设置”  。

    ![Web 应用的门户设置配置](media/tutorial-python-postgresql-app-portal/web-app-settings.png)

1. 使用“新建应用程序设置”按钮为以下每个值创建设置：

    | 设置名称 | 值 |
    | --- | --- |
    | DJANGO_ENV | `production`（该值告知应用使用前面[示例概述](#fork-the-sample-repository)中所述的生产配置。） |
    | DBHOST | 上一节中数据库服务器的 URL，格式为 `<server-name>.postgres.database.azure.com`。 可以从数据库服务器的“概述”页复制整个 URL。 |
    | DBNAME | `pollsdb` |
    | DBUSER | 上一节中使用的完整管理员用户名。 同样，完整的用户名为 `<user-name>@<server-name>`。 |
    | DBPASS | 之前创建的管理员密码。 |

1. 选择“保存”，然后选择“继续”以应用设置 。

    > [!IMPORTANT]
    > 更改设置后一定要选择“保存”，这点至关重要。 选择“保存”后，你使用“新建应用程序设置”按钮创建的所有设置才会应用 。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="deploy-app-code-to-the-web-app-from-a-repository"></a>将应用代码从存储库部署到 Web 应用

数据库和连接设置就绪后，现可配置 Web 应用以直接从 GitHub 存储库部署代码。

1. 在 Web 应用的浏览器窗口或选项卡中，选择“部署中心”（在左侧的“部署”下） 。

1. 在“源代码管理”步骤中，选择“GitHub”，然后选择“遵循登录提示”或选择“继续”以使用当前的 GitHub 登录名  。

1. 在“生成提供程序”步骤中，选择“应用服务生成服务”，然后选择“继续”  。

1. 在“配置”步骤中，选择以下值：

    | 字段 | Value |
    | --- | --- |
    | 组织 | 你向其创建示例存储库分支的 GitHub 帐户。 |
    | 存储库 | djangoapp |
    | 分支 | 主 |

1. 选择“继续”以选择该存储库，然后选择“完成” 。 Azure 应在数秒内部署代码并启动应用。

    应用服务将通过在每个子文件夹中查找 wsgi.py 文件来检测 Django 项目。 应用服务找到该文件后，就会加载 Django Web 应用。 有关详细信息，请参阅[配置内置的 Python 映像](/azure/app-service/configure-language-python)。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="run-django-database-migrations"></a>运行 Django 数据库迁移

代码部署完成且数据库就绪后，基本就可以使用该应用了。 余下的唯一任务就是在数据库本身中建立必要的架构。 将 Django 应用中的数据模型“迁移”到数据库即可实现这一点。

1. 在 Web 应用的浏览器窗口或选项卡中，选择“SSH”（在左侧的“部署工具”下），然后选择“开始”，以在 Web 应用服务器上打开 SSH 控制台  。 由于需要启动 Web 应用容器，因此首次连接可能需要一点时间。

1. 在控制台中，更改为 Web 应用的文件夹：

    ```bash
    cd site/wwwroot
    ```

1. 激活容器的虚拟环境：

    ```bash
    source /antenv/bin/activate
    ```

1. 安装 Python 包：

    ```bash
    pip install -r requirements.txt
    ```

1. 运行数据库迁移：

    ```bash
    python manage.py migrate
    ```

1. 创建应用的管理员登录名：

    ```bash
    python manage.py createsuperuser
   ```

    `createsuperuser` 命令提示你输入 Web 应用中使用的 Django 超级用户（或管理员）凭据。 为实现本教程的目的，使用默认用户名 `root`，按 Enter 使电子邮件地址留空，然后输入 `Pollsdb1` 作为密码。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

### <a name="create-a-poll-question-in-the-app"></a>在应用中创建投票问题

你现在可以对应用运行快速测试，以证明其适用于 PostgreSQL 数据库。

1. 在 Web 应用的浏览器窗口或选项卡中，返回到“概述”页面，然后选择 Web 应用的 URL（格式为 `http://<app-name>.azurewebsites.net`） 。

1. 应用应显示消息“无可用投票”，因为数据库中尚没有特定的投票。

1. 浏览到 `http://<app-name>.azurewebsites.net/admin`（“Django 管理”页面），然后使用上一节中的超级用户凭据（`root` 和 `Pollsdb1`）登录。

1. 在“投票”下，选择“问题”旁边的“添加”，创建一个包含一些选项的投票问题  。

1. 再次浏览到 `http://<app-name>.azurewebsites.net/`，确认现在是否向用户显示了问题。 回答你希望如何在数据库中生成某些数据。

祝贺你！ 你将在适用于 Linux 的 Azure 应用服务中使用活动 PostgreSQL 数据库运行 Python Django Web 应用。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="update-the-app-and-redeploy"></a>更新应用并重新部署

如本教程前面所述，Azure 会在你将更改提交到 GitHub 存储库时重新部署应用代码。

但如果更改 Django 应用的数据模型，你必须将这些更改迁移到数据库：

1. 如[运行 Django 数据库迁移](#run-django-database-migrations)中所述，通过 SSH 再次连接到 Web 应用。

1. 使用 `cd site/wwwroot` 更改为应用文件夹。

1. 使用 `source /antenv/bin/activate` 激活虚拟环境。

1. 使用 `python manage.py migrate` 再次运行迁移。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="view-diagnostic-logs"></a>查看诊断日志

可以访问在 Azure 上托管应用的容器内部生成的控制台日志。

在 Azure 门户的 Web 应用页面上，选择“日志流”（在左侧的“监视”下） 。 日志显示为控制台输出。

也可通过浏览器在 `https://<app-name>.scm.azurewebsites.net/api/logs/docker` 中检查日志文件。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="clean-up-resources"></a>清理资源

如果希望进行进一步的开发工作，可以将应用和数据库保持为运行状态。 否则，为避免产生持续的费用，请删除为本教程创建的资源组，该操作将删除其中包含的所有资源：

1. 在 Azure 门户的窗口顶部的搜索栏中，输入“DjangoPostgres-Tutorial-rg”，然后在“资源组”下选择相同名称。

1. 在资源组页上，选择“删除资源组”。

1. 出现提示时，输入资源组的名称，然后选择“删除”。

[存在问题？请告诉我们。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="next-steps"></a>后续步骤

了解应用服务如何运行 Python 应用：

> [!div class="nextstepaction"]
> [配置 Python 应用](/azure/app-service/configure-language-python)
