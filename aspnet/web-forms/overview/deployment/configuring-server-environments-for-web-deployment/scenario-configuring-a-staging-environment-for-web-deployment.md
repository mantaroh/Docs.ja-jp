---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: "シナリオ: Web 配置用のステージング環境の構成 |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、ステージング環境の一般的な web 展開シナリオについて説明しのような環境を設定するために完了する必要があるタスクについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 683a0cf88225fee762e82925afe3785a2defd5bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a><span data-ttu-id="aeadb-103">シナリオ: Web 配置用のステージング環境の構成</span><span class="sxs-lookup"><span data-stu-id="aeadb-103">Scenario: Configuring a Staging Environment for Web Deployment</span></span>
====================
<span data-ttu-id="aeadb-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="aeadb-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="aeadb-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="aeadb-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="aeadb-106">このトピックでは、ステージング環境の一般的な web 展開シナリオについて説明し、類似した環境を設定するために完了する必要があるタスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="aeadb-106">This topic describes a typical web deployment scenario for a staging environment and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="aeadb-107">多くの組織では、ステージング環境を使用して、web アプリケーションや web サイトの更新プログラムのプレビューします。</span><span class="sxs-lookup"><span data-stu-id="aeadb-107">Lots of organizations use staging environments to preview updates to web applications or websites.</span></span> <span data-ttu-id="aeadb-108">これは、方法により、組織内のユーザーを探索し、サイト"、"またはつまりは実稼働環境に展開する前に、新しい機能やコンテンツを確認します。</span><span class="sxs-lookup"><span data-stu-id="aeadb-108">This gives people within the organization a chance to explore and review new functionality or content before the site "goes live," or in other words is deployed to a production environment.</span></span> <span data-ttu-id="aeadb-109">ステージング環境は現実的なプレビューを提供するために、可能な限り、実稼働環境をレプリケートするよう設計されています。</span><span class="sxs-lookup"><span data-stu-id="aeadb-109">The staging environment is designed to replicate the production environment as closely as possible, in order to provide a realistic preview.</span></span> <span data-ttu-id="aeadb-110">この種類のステージング環境には、通常、これらの特徴があります。</span><span class="sxs-lookup"><span data-stu-id="aeadb-110">This kind of staging environment typically has these characteristics:</span></span>

- <span data-ttu-id="aeadb-111">環境は、複数の負荷分散された web サーバーおよびフェールオーバー クラスタ リングとデータベース ミラーリングで多くの場合、1 つまたは複数のデータベース サーバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="aeadb-111">The environment consists of multiple load-balanced web servers and one or more database servers, often with failover clustering and database mirroring.</span></span>
- <span data-ttu-id="aeadb-112">開発チームによってまたは自動的にチームがビルド サーバーは、アプリケーションを手動で配置する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="aeadb-112">Applications may be deployed manually by a development team or automatically by a Team Build server.</span></span>
- <span data-ttu-id="aeadb-113">ユーザーまたはアプリケーションの展開プロセスのアカウントでは、ステージング サーバーに管理者特権を持っているやすくします。</span><span class="sxs-lookup"><span data-stu-id="aeadb-113">The users or process accounts that deploy applications are unlikely to have administrator privileges on the staging servers.</span></span>
- <span data-ttu-id="aeadb-114">アプリケーションへの変更は、頻繁に展開されているため、環境をシングル ステップをサポートする必要があります。 または、展開を自動化します。</span><span class="sxs-lookup"><span data-stu-id="aeadb-114">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="aeadb-115">このチュートリアルの範囲を超えては複数のサーバーのスケール アウト データベースの配置です。</span><span class="sxs-lookup"><span data-stu-id="aeadb-115">Scaling out a database deployment across multiple servers is beyond the scope of this tutorial.</span></span> <span data-ttu-id="aeadb-116">この領域の詳細についてを参照してください[SQL Server オンライン ブック](https://technet.microsoft.com/library/ms130214.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="aeadb-116">For more information on this area, please consult [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).</span></span>


<span data-ttu-id="aeadb-117">たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)、Team Foundation Server (TFS) にお問い合わせください Manager ソリューションを管理します。</span><span class="sxs-lookup"><span data-stu-id="aeadb-117">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) manages the Contact Manager solution.</span></span> <span data-ttu-id="aeadb-118">Rob Walters、TFS 管理者には、により、開発者は必要に応じてステージング環境にデプロイをトリガーするビルド定義が作成されます。</span><span class="sxs-lookup"><span data-stu-id="aeadb-118">The TFS administrator, Rob Walters, has created a build definition that lets developers trigger a deployment to the staging environment as required.</span></span>

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="aeadb-119">ほとんどの場合、されません必ずしもする最新のビルドをステージング環境にデプロイに注意してください。</span><span class="sxs-lookup"><span data-stu-id="aeadb-119">Note that in most cases, you won't necessarily want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="aeadb-120">代わりに、検証とテスト環境で検証を既に受けている特定のビルドを配置する可能性の高いずっとできました。</span><span class="sxs-lookup"><span data-stu-id="aeadb-120">Instead, you're a lot more likely to want to deploy a specific build that has already undergone validation and verification in the test environment.</span></span>

## <a name="solution-overview"></a><span data-ttu-id="aeadb-121">ソリューションの概要</span><span class="sxs-lookup"><span data-stu-id="aeadb-121">Solution Overview</span></span>

<span data-ttu-id="aeadb-122">このシナリオでは、展開の要件の分析から次の点を推測できます。</span><span class="sxs-lookup"><span data-stu-id="aeadb-122">In this scenario, you can deduce these facts from an analysis of the deployment requirements:</span></span>

- <span data-ttu-id="aeadb-123">ステージングの web サーバーは、管理者以外の展開をサポートする必要がありますので、展開を実行するユーザーまたはプロセスのアカウントのステージング サーバー上の管理者特権ことはできません。</span><span class="sxs-lookup"><span data-stu-id="aeadb-123">The user or process account that performs the deployment won't have administrator privileges on the staging servers, so the staging web servers must support non-administrator deployment.</span></span> <span data-ttu-id="aeadb-124">そのため、リモート エージェントではなく、Web 配置のハンドラーを使用するステージング web サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aeadb-124">As such, you'll need to configure the staging web servers to use the Web Deploy Handler rather than the remote agent.</span></span>
- <span data-ttu-id="aeadb-125">ステージング環境には、複数の web サーバーが含まれていますが、ため Web Farm Framework (WFF) を使用して、サーバー ファームを作成する必要がありますに 1 回のクリックまたは自動の展開をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="aeadb-125">The staging environment includes multiple web servers, but it needs to support one-click or automated deployment, so you'll need to use the Web Farm Framework (WFF) to create a server farm.</span></span> <span data-ttu-id="aeadb-126">このアプローチを使用して、アプリケーションを 1 つの web サーバー (プライマリ サーバー) を展開できます。 また WFF がステージング環境の他のすべての web サーバー上の展開にレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="aeadb-126">Using this approach, you can deploy an application to one web server (the primary server), and WFF will replicate the deployment on all the other web servers in the staging environment.</span></span>
- <span data-ttu-id="aeadb-127">ユーザーまたはプロセス アカウントの展開を実行するには、データベースを作成する権限がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="aeadb-127">The user or process account that performs the deployment must have permissions to create databases.</span></span> <span data-ttu-id="aeadb-128">そのため、アカウントを追加する必要があります、 **dbcreator**リモート アクセス展開をサポートするデータベース サーバーを構成するだけでなく、データベース サーバーのサーバー ロール。</span><span class="sxs-lookup"><span data-stu-id="aeadb-128">As such, you'll need to add the account to the **dbcreator** server role on the database server, in addition to configuring the database server to support remote access and deployment.</span></span>

<span data-ttu-id="aeadb-129">これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="aeadb-129">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="aeadb-130">[サーバー ファームを作成する Web Farm Framework で](creating-a-server-farm-with-the-web-farm-framework.md)です。</span><span class="sxs-lookup"><span data-stu-id="aeadb-130">[Create a Server Farm with the Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md).</span></span> <span data-ttu-id="aeadb-131">このトピックでは、作成して、web platform 製品とコンポーネント、構成設定、および web サイトおよびアプリケーションが複数の負荷分散された web サーバー間でレプリケートできるように、WFF を使用して、サーバー ファームを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="aeadb-131">This topic describes how to create and configure a server farm using WFF, so that web platform products and components, configuration settings, and websites and applications are replicated across multiple load-balanced web servers.</span></span>
- <span data-ttu-id="aeadb-132">[Web デプロイの発行の Web サーバーを構成する (Web Deploy ハンドラー)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)です。</span><span class="sxs-lookup"><span data-stu-id="aeadb-132">[Configure a Web Server for Web Deploy Publishing (Web Deploy Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span></span> <span data-ttu-id="aeadb-133">このトピックでは、Web Deploy の発行をリモート エージェントのアプローチを使用して、クリーンな Windows Server 2008 R2 ビルドから起動をサポートする web サーバーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="aeadb-133">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="aeadb-134">[Web デプロイの発行のデータベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)です。</span><span class="sxs-lookup"><span data-stu-id="aeadb-134">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="aeadb-135">このトピックでは、リモート アクセスと、SQL Server 2008 R2 の既定のインストールからの展開をサポートするデータベース サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="aeadb-135">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="aeadb-136">関連項目</span><span class="sxs-lookup"><span data-stu-id="aeadb-136">Further Reading</span></span>

<span data-ttu-id="aeadb-137">開発者の標準的なテスト環境を構成する方法については、次を参照してください。[シナリオ: Web 配置用のテスト環境構成](scenario-configuring-a-test-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="aeadb-137">For guidance on configuring a typical developer test environment, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="aeadb-138">通常の運用環境を構成する方法については、次を参照してください。[シナリオ: Web 配置の実稼働環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="aeadb-138">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="aeadb-139">[前へ](scenario-configuring-a-test-environment-for-web-deployment.md)
[次へ](scenario-configuring-a-production-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="aeadb-139">[Previous](scenario-configuring-a-test-environment-for-web-deployment.md)
[Next](scenario-configuring-a-production-environment-for-web-deployment.md)</span></span>