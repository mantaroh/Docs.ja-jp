---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: "MSBuild プロジェクト ファイルから Windows PowerShell スクリプトを実行している |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、ビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。 スクリプトをローカルで実行することができます (言い換えると、b 上..。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: afee7b0621df42a8bc70fc6f7c4a8fd0383fa83a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="a4b90-104">MSBuild プロジェクト ファイルから Windows PowerShell スクリプトの実行</span><span class="sxs-lookup"><span data-stu-id="a4b90-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="a4b90-105">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a4b90-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a4b90-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a4b90-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a4b90-107">このトピックでは、ビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="a4b90-108">(つまり、ビルド サーバー上) にローカルでスクリプトを実行したり、リモートで、ターゲット web サーバーまたはデータベース サーバー上と同様にできます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="a4b90-109">さまざまな理由により、配置後の Windows PowerShell スクリプトを実行する理由があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="a4b90-110">たとえば、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="a4b90-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="a4b90-111">カスタム イベント ソースをレジストリに追加します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="a4b90-112">アップロードのファイル システム ディレクトリを生成します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="a4b90-113">ビルド ディレクトリをクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="a4b90-113">Clean up build directories.</span></span>
> - <span data-ttu-id="a4b90-114">カスタム ログ ファイルにエントリを記述します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="a4b90-115">新しくプロビジョニングされた web アプリケーションへのユーザーの招待の電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="a4b90-116">適切なアクセス許可を持つユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="a4b90-117">SQL Server インスタンス間のレプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="a4b90-118">このトピックでは、Microsoft Build Engine (MSBuild) プロジェクト ファイルでカスタム ターゲットからローカルとリモートの両方に Windows PowerShell スクリプトを実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="a4b90-119">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="a4b90-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="a4b90-120">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="a4b90-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="a4b90-121">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="a4b90-122">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="a4b90-122">Task Overview</span></span>

<span data-ttu-id="a4b90-123">自動または 1 ステップの展開プロセスの一環として、Windows PowerShell スクリプトを実行するには、これらの高度なタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="a4b90-124">ソリューションとソース管理には、Windows PowerShell スクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="a4b90-125">Windows PowerShell スクリプトを起動するコマンドを作成します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="a4b90-126">コマンド内の予約済み XML 文字をエスケープします。</span><span class="sxs-lookup"><span data-stu-id="a4b90-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="a4b90-127">カスタム MSBuild プロジェクト ファイルでターゲットを作成しを使用して、 **Exec**コマンドを実行するタスク。</span><span class="sxs-lookup"><span data-stu-id="a4b90-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="a4b90-128">このトピックでは、これらの手順を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="a4b90-129">タスクとここでチュートリアルは、ことに慣れている既に MSBuild ターゲットおよびプロパティ、およびカスタム MSBuild プロジェクト ファイルを使用してビルドおよび配置プロセスを制御する方法を理解することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="a4b90-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="a4b90-130">詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。</span><span class="sxs-lookup"><span data-stu-id="a4b90-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="a4b90-131">作成して、Windows PowerShell スクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="a4b90-132">このトピックの実習では、という名前のサンプル Windows PowerShell スクリプトを使用して**LogDeploy.ps1** MSBuild からスクリプトを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="a4b90-133">**LogDeploy.ps1**スクリプトには、ログ ファイルに単一行エントリを書き込む単純な関数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a4b90-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="a4b90-134">**LogDeploy.ps1**スクリプトは 2 つのパラメーターを受け付けます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="a4b90-135">最初のパラメーターでは、完全なパスを表す、エントリを追加するログ ファイルに、2 番目のパラメーターは、ログ ファイルに記録する、配置先を表します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="a4b90-136">スクリプトを実行するときに、この形式でログ ファイルに行が追加されます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="a4b90-137">させる、 **LogDeploy.ps1** msbuild の使用可能なスクリプト、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="a4b90-138">ソース管理には、スクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-138">Add the script to source control.</span></span>
- <span data-ttu-id="a4b90-139">スクリプトを Visual Studio 2010 のソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="a4b90-140">ビルド サーバー上またはリモート コンピューターでスクリプトを実行する予定があるかに関係なく、ソリューションのコンテンツを含むスクリプトを展開する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a4b90-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="a4b90-141">1 つのオプションでは、ソリューション フォルダーにスクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="a4b90-142">連絡先のマネージャーの例で、展開プロセスの一部として、Windows PowerShell スクリプトを使用するため理にかなって発行ソリューション フォルダーにスクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="a4b90-143">ソリューション フォルダーの内容は、ソース マテリアルとしてビルド サーバーにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="a4b90-144">ただし、なしの一部を形成プロジェクト出力します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="a4b90-145">ビルド サーバー上の Windows PowerShell スクリプトの実行</span><span class="sxs-lookup"><span data-stu-id="a4b90-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="a4b90-146">一部のシナリオでは、プロジェクトをビルドしているコンピューターで Windows PowerShell スクリプトを実行することがあります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="a4b90-147">たとえば、ビルド フォルダーをクリーンアップまたはカスタムのログ ファイルにエントリを書き込むに Windows PowerShell スクリプトを使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="a4b90-148">構文の観点から MSBuild プロジェクト ファイルから Windows PowerShell スクリプトの実行は正規のコマンド プロンプトから Windows PowerShell スクリプトを実行すると同じです。</span><span class="sxs-lookup"><span data-stu-id="a4b90-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="a4b90-149">実行可能ファイル、powershell.exe を起動し、使用する必要があります、 **– コマンド**を実行する Windows PowerShell のコマンドを提供するスイッチです。</span><span class="sxs-lookup"><span data-stu-id="a4b90-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="a4b90-150">(Windows PowerShell v2 で使用することも、 **– ファイル**切り替える)。</span><span class="sxs-lookup"><span data-stu-id="a4b90-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="a4b90-151">コマンドは、この形式を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="a4b90-152">例:</span><span class="sxs-lookup"><span data-stu-id="a4b90-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="a4b90-153">スクリプトへのパスには、スペースが含まれている場合は、ファイルのパスを単一引用符の前にアンパサンドで囲む必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="a4b90-154">コマンドを囲むために既に使用しているため、二重引用符を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="a4b90-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="a4b90-155">MSBuild からこのコマンドを呼び出す場合は、いくつか追加の考慮事項にもあります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="a4b90-156">最初を含めるように、 **– NonInteractive**スクリプトがサイレント モードで実行されることを確認するフラグ。</span><span class="sxs-lookup"><span data-stu-id="a4b90-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="a4b90-157">次を含めるように、 **– ExecutionPolicy**適切な引数の値を含むフラグ。</span><span class="sxs-lookup"><span data-stu-id="a4b90-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="a4b90-158">これには、実行ポリシーと Windows PowerShell スクリプトが適用され、スクリプトの実行を防ぐことがあります既定の実行ポリシーをオーバーライドすることができることを指定します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="a4b90-159">これらの引数の値から選択できます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="a4b90-160">値**Unrestricted** Windows PowerShell スクリプトが署名されているかどうかに関係なく、スクリプトを実行できるようになります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="a4b90-161">値**RemoteSigned**をローカル コンピューターで作成された未署名のスクリプトを実行する Windows PowerShell を許可します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="a4b90-162">ただし、他の場所で作成されたスクリプトを署名する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="a4b90-163">(実際には、していない場合、Windows PowerShell スクリプトをビルド サーバーでローカルに作成する多くの場合)。</span><span class="sxs-lookup"><span data-stu-id="a4b90-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="a4b90-164">値**AllSigned**署名済みスクリプトのみを実行する Windows PowerShell を許可します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="a4b90-165">既定の実行ポリシーは**Restricted**、Windows PowerShell のスクリプト ファイルを実行できなくなります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="a4b90-166">最後に、Windows PowerShell コマンドで発生する予約済み XML 文字をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="a4b90-167">単一引用符を置き換える **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="a4b90-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="a4b90-168">二重引用符で置換 **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="a4b90-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="a4b90-169">アンパサンドを置き換える **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="a4b90-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="a4b90-170">これらの変更を行うと、コマンドはのようになります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="a4b90-171">カスタムの MSBuild プロジェクト ファイル内には、新しいターゲットを作成して使用することができます、 **Exec**タスクを次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="a4b90-172">この例に注意してください。</span><span class="sxs-lookup"><span data-stu-id="a4b90-172">In this example, note that:</span></span>

- <span data-ttu-id="a4b90-173">パラメーター値と、Windows PowerShell 実行可能ファイルの場所と同様に、任意の変数は、MSBuild プロパティとして宣言されます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="a4b90-174">コマンドラインからこれらの値を上書きするようにするのには、条件が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a4b90-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="a4b90-175">**MSDeployComputerName**プロパティがプロジェクト ファイルで別の場所で宣言されています。</span><span class="sxs-lookup"><span data-stu-id="a4b90-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="a4b90-176">ビルド プロセスの一部としてこのターゲットを実行するときに、Windows PowerShell は、コマンドを実行し、指定したファイルにログ エントリを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="a4b90-177">リモート コンピューター上の Windows PowerShell スクリプトの実行</span><span class="sxs-lookup"><span data-stu-id="a4b90-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="a4b90-178">Windows PowerShell は経由でリモート コンピューターでスクリプトを実行できる[Windows リモート管理](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx)(WinRM)。</span><span class="sxs-lookup"><span data-stu-id="a4b90-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="a4b90-179">これを行うには、使用する必要があります、 [Invoke-command](https://technet.microsoft.com/library/dd347578.aspx)コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="a4b90-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="a4b90-180">これにより、リモート コンピューターに、スクリプトをコピーすることがなく 1 つまたは複数のリモート コンピューターに対して、スクリプトを実行できます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="a4b90-181">スクリプトを実行した、ローカル コンピューターには、すべての結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="a4b90-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="a4b90-182">使用する前に、 **Invoke-command**コマンドレットを Windows PowerShell を実行するスクリプトのリモート コンピューターで、リモートのメッセージを受け入れるように WinRM リスナーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="a4b90-183">コマンドを実行してこれを行う**winrm quickconfig**リモート コンピューターにします。</span><span class="sxs-lookup"><span data-stu-id="a4b90-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="a4b90-184">詳細については、次を参照してください。[インストールと構成の Windows リモート管理](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="a4b90-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="a4b90-185">Windows PowerShell ウィンドウを実行するこの構文を使用すると、 **LogDeploy.ps1**リモート コンピューター上のスクリプト。</span><span class="sxs-lookup"><span data-stu-id="a4b90-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="a4b90-186">使用する別の方法でさまざまな**Invoke-command**スクリプトを実行するファイルがこのアプローチは最も簡単なパラメーター値を提供し、スペースを含むパスを管理する必要がある場合。</span><span class="sxs-lookup"><span data-stu-id="a4b90-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="a4b90-187">コマンド プロンプトからこれを実行するときを実行可能ファイル、Windows PowerShell を起動し、使用する必要があります、 **– コマンド**指示を提供するパラメーター。</span><span class="sxs-lookup"><span data-stu-id="a4b90-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="a4b90-188">前に、追加の一部のスイッチを提供し、MSBuild からコマンドを実行すると、予約済み XML 文字をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a4b90-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="a4b90-189">最後に、以前と同様を使えば、 **Exec**コマンドを実行するカスタム MSBuild ターゲット内のタスク。</span><span class="sxs-lookup"><span data-stu-id="a4b90-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="a4b90-190">Windows PowerShell はで指定したコンピューターで、スクリプトを実行して、ビルド プロセスの一部としてこのターゲットを実行するときに、 **– computername**引数。</span><span class="sxs-lookup"><span data-stu-id="a4b90-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a4b90-191">まとめ</span><span class="sxs-lookup"><span data-stu-id="a4b90-191">Conclusion</span></span>

<span data-ttu-id="a4b90-192">このトピックでは、MSBuild プロジェクト ファイルから Windows PowerShell スクリプトを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="a4b90-193">このアプローチを使用すると、自動またはシングル ステップ ビルドおよび配置プロセスの一部としてローカルまたはリモート コンピューター上の Windows PowerShell スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="a4b90-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="a4b90-194">関連項目</span><span class="sxs-lookup"><span data-stu-id="a4b90-194">Further Reading</span></span>

<span data-ttu-id="a4b90-195">Windows PowerShell スクリプトへの署名と、実行ポリシーの管理に関するガイダンスについては、次を参照してください。 [Windows PowerShell スクリプトの実行](https://technet.microsoft.com/library/ee176949.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="a4b90-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="a4b90-196">リモート コンピューターから Windows PowerShell コマンドを実行する方法については、次を参照してください。[リモート コマンドの実行](https://technet.microsoft.com/library/dd819505.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="a4b90-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="a4b90-197">展開プロセスを制御するカスタム MSBuild プロジェクト ファイルを使用する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。</span><span class="sxs-lookup"><span data-stu-id="a4b90-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a4b90-198">[前へ](taking-web-applications-offline-with-web-deploy.md)
[次へ](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="a4b90-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>