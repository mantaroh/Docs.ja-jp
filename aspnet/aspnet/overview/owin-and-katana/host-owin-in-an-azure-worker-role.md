---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: "ホストする Azure Worker ロールで OWIN |Microsoft ドキュメント"
author: MikeWasson
description: "このチュートリアルでは、Microsoft Azure ワーカー ロールで OWIN を自己ホストする方法を示します。 Open Web Interface の .NET (OWIN) では、.NET の web サーバー間の抽象化を定義しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 8c0fdfdf60ff3bde34b6869adf3f8693b4d9615d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="5d23f-104">ホストする Azure Worker ロールで OWIN</span><span class="sxs-lookup"><span data-stu-id="5d23f-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="5d23f-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5d23f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="5d23f-106">このチュートリアルでは、Microsoft Azure ワーカー ロールで OWIN を自己ホストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="5d23f-107">[.NET 用 Web インターフェイスを開く](http://owin.org/)(OWIN) .NET web サーバーと web アプリケーション間で抽象型を定義します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="5d23f-108">OWIN OWIN を自己ホスト型 IIS の外部で独自のプロセス内の web アプリケーションに最適ですが、サーバーから web アプリケーションの分離 – Azure ワーカー ロール内などです。</span><span class="sxs-lookup"><span data-stu-id="5d23f-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="5d23f-109">このチュートリアルでは、Microsoft Azure のワーカー ロール内の OWIN アプリケーションを自己ホストする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="5d23f-110">ワーカー ロールの詳細については、次を参照してください。 [Azure 実行モデル](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices)です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5d23f-111">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="5d23f-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="5d23f-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5d23f-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="5d23f-113">Azure SDK for .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="5d23f-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="5d23f-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="5d23f-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="5d23f-115">Microsoft Azure プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="5d23f-116">管理者特権で Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="5d23f-117">Azure コンピューティング エミュレーターを使用してローカルで、アプリケーションをデバッグするには、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="5d23f-118">**ファイル** メニューのをクリックして**新規**をクリックし、**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="5d23f-119">**インストールされたテンプレート**、Visual c# をクリックして**クラウド** をクリックし、 **Windows Azure クラウド サービス**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="5d23f-120">プロジェクト"AzureApp"の名前し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="5d23f-121">**新しい Windows Azure のクラウド サービス**ダイアログ ボックスをダブルクリックして**ワーカー ロール**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="5d23f-122">既定の名前 ("WorkerRole1") のままにします。</span><span class="sxs-lookup"><span data-stu-id="5d23f-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="5d23f-123">この手順は、ワーカー ロールをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="5d23f-124">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d23f-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="5d23f-125">Visual Studio ソリューション作成するにはには、2 つのプロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5d23f-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="5d23f-126">&quot;AzureApp&quot;ロールと Azure アプリケーションの構成を定義します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="5d23f-127">&quot;WorkerRole1&quot;ワーカー ロールのコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5d23f-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="5d23f-128">一般に、Azure アプリケーションは、このチュートリアルは、1 つの役割を使用していますが、複数のロールを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="5d23f-129">OWIN 自己ホスト パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="5d23f-130">**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**をクリックし、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="5d23f-131">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="5d23f-132">HTTP エンドポイントを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="5d23f-133">ソリューション エクスプ ローラーで、AzureApp プロジェクトを展開します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="5d23f-134">ロール ノードを展開し、WorkerRole1 を右クリックし **プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="5d23f-135">をクリックして**エンドポイント**、クリックして**エンドポイントの追加**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="5d23f-136">**プロトコル**ドロップダウン リストで、"http"を選択します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="5d23f-137">**パブリック ポート**と**プライベート ポート**80」と入力します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="5d23f-138">これらのポート番号は別にすることはできます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-138">These port numbers can be different.</span></span> <span data-ttu-id="5d23f-139">パブリック ポートが、クライアントがロールに、要求を送信する際に使用します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="5d23f-140">OWIN 起動クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="5d23f-141">ソリューション エクスプ ローラーで、WorkerRole1 プロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="5d23f-142">クラスに `Startup` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-142">Name the class `Startup`.</span></span>

<span data-ttu-id="5d23f-143">次のようにすべての定型コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="5d23f-144">`UseWelcomePage`拡張メソッドが、サイトの動作を確認する、アプリケーションに、単純な HTML ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="5d23f-145">OWIN ホストを起動します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-145">Start the OWIN Host</span></span>

<span data-ttu-id="5d23f-146">WorkerRole.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="5d23f-147">このクラスは、ワーカー ロールが開始および停止時に実行されるコードを定義します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="5d23f-148">次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="5d23f-149">追加、 **IDisposable**メンバーを`WorkerRole`クラス。</span><span class="sxs-lookup"><span data-stu-id="5d23f-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="5d23f-150">`OnStart`メソッド、ホストを起動する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="5d23f-151">**WebApp.Start**メソッドは、OWIN ホストを開始します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="5d23f-152">名前、`Startup`クラスは、型パラメーター、メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="5d23f-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="5d23f-153">慣例により、ホストを呼び出すが、`Configure`このクラスのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="5d23f-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="5d23f-154">上書き、`OnStop`を破棄する、 *\_アプリ*インスタンス。</span><span class="sxs-lookup"><span data-stu-id="5d23f-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="5d23f-155">WorkerRole.cs の完全なコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="5d23f-156">ソリューションをビルドし、Azure コンピューティング エミュレーターでアプリケーションをローカルで実行する場合は F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="5d23f-157">ファイアウォールの設定によっては、エミュレーター、ファイアウォールを通過を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d23f-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="5d23f-158">コンピューティング エミュレーターは、エンドポイントに、ローカル IP アドレスを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="5d23f-159">コンピューティング エミュレーター UI を表示することによって IP アドレスを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="5d23f-160">タスク バー通知領域に表示されたエミュレーター アイコンを右クリックし  **コンピューティング エミュレーター UI**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="5d23f-161">サービスの展開、展開 [id]、サービスの詳細情報の下の IP アドレスを検索します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="5d23f-162">Web ブラウザーを開き、http:// に移動*アドレス*ここで、*アドレス*;、コンピューティング エミュレーターによって割り当てられた IP アドレスは、たとえば、`http://127.0.0.1:80`です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-162">Open a web browser and navigate to http://*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="5d23f-163">OWIN へようこそ ページを参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d23f-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="5d23f-164">Azure への配置します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-164">Deploy to Azure</span></span>

<span data-ttu-id="5d23f-165">この手順では、Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="5d23f-166">既に持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="5d23f-167">詳細については、「 [Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="5d23f-168">ソリューション エクスプ ローラーで、AzureApp プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="5d23f-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="5d23f-169">**[発行]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="5d23f-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="5d23f-170">Azure アカウントにサインインしていない場合はクリックして**サイン イン**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="5d23f-171">サブスクリプションを選択しをクリックするサインイン後、**次**です。</span><span class="sxs-lookup"><span data-stu-id="5d23f-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="5d23f-172">クラウド サービスの名前を入力し、地域を選択します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="5d23f-173">**[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d23f-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="5d23f-174">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d23f-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="5d23f-175">Azure のアクティビティ ログ ウィンドウでは、展開の進行状況を示します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="5d23f-176">アプリが展開されると、参照`http://appname.cloudapp.net/`ここで、 *appname*クラウド サービスの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="5d23f-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d23f-177">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5d23f-177">Additional Resources</span></span>

- [<span data-ttu-id="5d23f-178">プロジェクト Katana の概要</span><span class="sxs-lookup"><span data-stu-id="5d23f-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="5d23f-179">GitHub の Katana プロジェクト</span><span class="sxs-lookup"><span data-stu-id="5d23f-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)