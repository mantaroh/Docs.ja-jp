---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: "Azure Service Bus での SignalR スケール アウト |Microsoft ドキュメント"
author: MikeWasson
description: "ソフトウェア バージョンでは、2 以前のバージョンのこのトピックでは、このトピックの SignalR の 1.x バージョンのこのトピックの Visual Studio 2013 .NET 4.5 SignalR バージョンで使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 7cb68d578fee8d6ee036f8fb096ba45e0c8ef3d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="e2e90-103">Azure Service Bus での SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="e2e90-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="e2e90-104">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e2e90-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="e2e90-105">このチュートリアルでは、Service Bus バック プレーンを使用して各ロール インスタンスにメッセージを配信する、Windows Azure の Web ロールに SignalR アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="e2e90-106">(Service Bus のバック プレーンとを使用することもできます[web Azure App service アプリ](https://docs.microsoft.com/azure/app-service-web/)。)。</span><span class="sxs-lookup"><span data-stu-id="e2e90-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="e2e90-107">必要条件:</span><span class="sxs-lookup"><span data-stu-id="e2e90-107">Prerequisites:</span></span>

- <span data-ttu-id="e2e90-108">Windows Azure アカウント。</span><span class="sxs-lookup"><span data-stu-id="e2e90-108">A Windows Azure account.</span></span>
- <span data-ttu-id="e2e90-109">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="e2e90-110">Visual Studio 2012 または 2013。</span><span class="sxs-lookup"><span data-stu-id="e2e90-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="e2e90-111">サービス バスのバック プレーンに互換性がも[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)、version 1.1 です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="e2e90-112">ただし、Service Bus for Windows Server のバージョン 1.0 と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="e2e90-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="e2e90-113">Pricing</span><span class="sxs-lookup"><span data-stu-id="e2e90-113">Pricing</span></span>

<span data-ttu-id="e2e90-114">Service Bus バック プレーンでは、トピックを使用して、メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="e2e90-115">最新の価格情報については、次を参照してください。 [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="e2e90-116">この記事の執筆時に、1 ドル未満の値の 1 か月あたり 1,000,000 メッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="e2e90-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="e2e90-117">バック プレーンでは、SignalR のハブ メソッドの呼び出しごとに service bus メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="e2e90-118">接続、切断、または参加したまま、グループなどの一部のコントロール メッセージもあります。</span><span class="sxs-lookup"><span data-stu-id="e2e90-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="e2e90-119">ほとんどのアプリケーションでは、メッセージ トラフィックの大部分のハブ メソッド呼び出しとなります。</span><span class="sxs-lookup"><span data-stu-id="e2e90-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="e2e90-120">概要</span><span class="sxs-lookup"><span data-stu-id="e2e90-120">Overview</span></span>

<span data-ttu-id="e2e90-121">詳細なチュートリアルを始める前に何を行うの簡単な概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="e2e90-122">Windows Azure ポータルを使用して、新しい Service Bus 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="e2e90-123">これらの NuGet パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="e2e90-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="e2e90-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="e2e90-125">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="e2e90-125">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="e2e90-126">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="e2e90-127">Startup.cs バック プレーンを構成するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="e2e90-128">このコードの既定値でのバック プレーンの構成[TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx)と[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="e2e90-129">これらの値を変更する方法については、次を参照してください。 [SignalR パフォーマンス: スケール アウト メトリック](signalr-performance.md#scaleout_metrics)です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="e2e90-130">各アプリケーションには、"YourAppName"の別の値を選択します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="e2e90-131">複数のアプリケーションは、同じ値を使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="e2e90-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="e2e90-132">Azure のサービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-132">Create the Azure Services</span></span>

<span data-ttu-id="e2e90-133">クラウド サービスを作成する」の説明に従って[を作成し、クラウド サービスを展開する方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="e2e90-134">セクションの手順に従って"する方法: 簡易作成を使用してクラウド サービスの作成"です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="e2e90-135">このチュートリアルでは、証明書をアップロードする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e2e90-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="e2e90-136">新しい Service Bus 名前空間を作成する」の説明に従って[方法を使用するサービス バス トピック/サブスクリプション](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="e2e90-137">「Service Namespace を作成する」セクションの手順をに従います。</span><span class="sxs-lookup"><span data-stu-id="e2e90-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="e2e90-138">クラウド サービスと Service Bus 名前空間の同じ領域を選択してください。</span><span class="sxs-lookup"><span data-stu-id="e2e90-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="e2e90-139">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="e2e90-140">Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-140">Start Visual Studio.</span></span> <span data-ttu-id="e2e90-141">**ファイル** メニューをクリックして**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="e2e90-142">**新しいプロジェクト** ダイアログ ボックスで、展開**Visual c#**です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="e2e90-143">**インストールされたテンプレート****クラウド**し、 **Windows Azure クラウド サービス**です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="e2e90-144">既定値を .NET Framework 4.5 を保持します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="e2e90-145">ChatService アプリケーションの名前を指定し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="e2e90-146">**新しい Windows Azure のクラウド サービス**ダイアログ ボックスで、ASP.NET Web ロールを選択します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="e2e90-147">右矢印ボタンをクリックして (**&gt;**) をソリューションにロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="e2e90-148">マウス ポインター新しいロールをそのため、鉛筆アイコンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e2e90-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="e2e90-149">ロールの名前を変更するには、このアイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2e90-149">Click this icon to rename the role.</span></span> <span data-ttu-id="e2e90-150">"SignalRChat"ロールの名前をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="e2e90-151">**新しい ASP.NET プロジェクト**ダイアログで、 **MVC**、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2e90-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="e2e90-152">チーム プロジェクト ウィザードでは、2 つのプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="e2e90-153">ChatService: このプロジェクトは、Windows Azure アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="e2e90-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="e2e90-154">これは、Azure のロールとその他の構成オプションを定義します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="e2e90-155">SignalRChat: このプロジェクトは、ASP.NET MVC 5 プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="e2e90-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="e2e90-156">SignalR チャット アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="e2e90-157">チャット アプリケーションを作成する手順のチュートリアルで[SignalR と MVC 5 の概要](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="e2e90-158">NuGet を使用すると、必要なライブラリをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="e2e90-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="e2e90-159">**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e2e90-160">**パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="e2e90-161">使用して、 `-ProjectName` Windows Azure プロジェクトではなく、ASP.NET MVC プロジェクトにパッケージをインストールするオプションです。</span><span class="sxs-lookup"><span data-stu-id="e2e90-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="e2e90-162">バック プレーンを構成します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-162">Configure the Backplane</span></span>

<span data-ttu-id="e2e90-163">アプリケーションの Startup.cs ファイルでは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="e2e90-164">Service bus の接続文字列を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2e90-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="e2e90-165">Azure ポータルで作成した service bus 名前空間を選択し、アクセス キー アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2e90-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="e2e90-166">接続文字列をクリップボードにコピーして貼り付けます、 *connectionString*変数。</span><span class="sxs-lookup"><span data-stu-id="e2e90-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="e2e90-167">Azure への配置します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-167">Deploy to Azure</span></span>

<span data-ttu-id="e2e90-168">ソリューション エクスプ ローラーで、展開、**ロール**ChatService プロジェクト内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e2e90-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="e2e90-169">SignalRChat ロールを右クリックし **プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="e2e90-170">選択、**構成**タブです。**インスタンス**2 を選択します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="e2e90-171">VM のサイズを設定することもできます。**極小**です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="e2e90-172">変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-172">Save the changes.</span></span>

<span data-ttu-id="e2e90-173">ソリューション エクスプ ローラーで、ChatService プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="e2e90-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="e2e90-174">**[発行]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="e2e90-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="e2e90-175">Windows Azure に最初に発行する、この場合は、資格情報をダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2e90-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="e2e90-176">**発行**ウィザード、[資格情報をダウンロードするサインイン] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2e90-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="e2e90-177">これは、操作によって、Windows Azure ポータルにサインインし、発行設定ファイルをダウンロードするように求められます。</span><span class="sxs-lookup"><span data-stu-id="e2e90-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="e2e90-178">をクリックして**インポート**をダウンロードした発行設定ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="e2e90-179">**[次へ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2e90-179">Click **Next**.</span></span> <span data-ttu-id="e2e90-180">**発行設定**ダイアログで、**クラウド サービス**、先ほど作成したクラウド サービスを選択します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="e2e90-181">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2e90-181">Click **Publish**.</span></span> <span data-ttu-id="e2e90-182">アプリケーションを展開し、Vm を起動するまで数分かかることができます。</span><span class="sxs-lookup"><span data-stu-id="e2e90-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="e2e90-183">今すぐチャット アプリケーションを実行すると、ロール インスタンスは、Service Bus トピックを使用して、Azure Service Bus を通じて通信します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="e2e90-184">トピックは、複数のサブスクライバーを許可するメッセージ キューです。</span><span class="sxs-lookup"><span data-stu-id="e2e90-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="e2e90-185">バック プレーンには、トピックとサブスクリプションを自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="e2e90-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="e2e90-186">サブスクリプションとメッセージ アクティビティを表示するには、Azure ポータルを開き、Service Bus 名前空間を選択および「トピック」をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2e90-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="e2e90-187">これは、ダッシュ ボードに表示するメッセージ アクティビティに対して数分かかるためです。</span><span class="sxs-lookup"><span data-stu-id="e2e90-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="e2e90-188">SignalR では、トピックの有効期間を管理します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="e2e90-189">アプリケーションが展開されている限りは、トピックの設定を手動でトピックの削除または変更しないでください。</span><span class="sxs-lookup"><span data-stu-id="e2e90-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="e2e90-190">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e2e90-190">Troubleshooting</span></span>

<span data-ttu-id="e2e90-191">**System.invalidoperationexception:「唯一サポートされる IsolationLevel は 'IsolationLevel.Serializable' です」。**</span><span class="sxs-lookup"><span data-stu-id="e2e90-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="e2e90-192">このエラーは、操作がトランザクション レベルが以外の何かに設定されている場合に発生する可能性が`Serializable`です。</span><span class="sxs-lookup"><span data-stu-id="e2e90-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="e2e90-193">他のトランザクション レベルで操作が実行されるないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e2e90-193">Verify that no operations are being performed with other transaction levels.</span></span>