---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: "ビューの追加 |Microsoft ドキュメント"
author: shanselman
description: "これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 8725d054861c857ceac10a42b0cc3f2afe056aea
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-view"></a><span data-ttu-id="22d3f-104">ビューの追加</span><span class="sxs-lookup"><span data-stu-id="22d3f-104">Adding a View</span></span>
====================
<span data-ttu-id="22d3f-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="22d3f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="22d3f-106">これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="22d3f-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="22d3f-107">データベースから読み取り/書き込みを単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="22d3f-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="22d3f-109">このセクションで行うビュー テンプレート ファイルを使用してクライアントに HTML 応答を生成するを明確にカプセル化、HelloWorldController クラスがあっても方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-109">In this section we are going to look at how we can have our HelloWorldController class use a View template file to cleanly encapsulate generating HTML responses back to a client.</span></span>

<span data-ttu-id="22d3f-110">テンプレートを使用して、ビュー、インデックスのメソッドを使用して開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="22d3f-110">Let's start by using a View template with our Index method.</span></span> <span data-ttu-id="22d3f-111">このメソッドはインデックスであり、HelloWorldController です。</span><span class="sxs-lookup"><span data-stu-id="22d3f-111">Our method is called Index and it's in the HelloWorldController.</span></span> <span data-ttu-id="22d3f-112">現在、Index() メソッドには、コント ローラー クラスにハードコードされているメッセージを含む文字列が返されます。</span><span class="sxs-lookup"><span data-stu-id="22d3f-112">Currently our Index() method returns a string with a message that is hardcoded within the Controller class.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

<span data-ttu-id="22d3f-113">ようになりましたインデックスの代わりに次のような方法を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="22d3f-113">Let's now change the Index method to instead look like this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

<span data-ttu-id="22d3f-114">この Index() メソッドを使用できるプロジェクトにテンプレートの表示を今すぐ追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="22d3f-114">Let's now add a View template to our project that we can use for our Index() method.</span></span> <span data-ttu-id="22d3f-115">これを行うには、インデックス メソッドの途中で任意の場所をマウスで右クリックし、[ビューの追加] をクリックしてください.</span><span class="sxs-lookup"><span data-stu-id="22d3f-115">To do this, right-click with your mouse somewhere in the middle of the Index method and click Add View...</span></span>

![イメージ](getting-started-with-mvc-part3/_static/image1.png)

<span data-ttu-id="22d3f-117">このインデックス メソッドで使用できるビュー テンプレートを作成する方法の一部のオプションを提供する「ビューの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="22d3f-117">This will bring up the "Add View" dialog which provides us some options for how we want to create a view template that can be used by our Index method.</span></span> <span data-ttu-id="22d3f-118">ここでは、何も、変更おらず、のみ、[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="22d3f-118">For now, don't change anything, and just click the Add button.</span></span>

<span data-ttu-id="22d3f-119">[![[追加] ダイアログの表示](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="22d3f-119">[![Add View Dialog](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)</span></span>

<span data-ttu-id="22d3f-120">追加 をクリックした後、新しいフォルダーとファイルの新規フォルダーに表示されます、ソリューション、次に示すようです。</span><span class="sxs-lookup"><span data-stu-id="22d3f-120">After you click Add, a new folder and a new file will appear in the Solution Folder, as seen here.</span></span> <span data-ttu-id="22d3f-121">そのフォルダー内にビュー、および Index.aspx ファイルの下の HelloWorld フォルダーがあります。</span><span class="sxs-lookup"><span data-stu-id="22d3f-121">I now have a HelloWorld folder under Views, and an Index.aspx file inside that folder.</span></span>

<span data-ttu-id="22d3f-122">[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="22d3f-122">[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)</span></span>

<span data-ttu-id="22d3f-123">新しいインデックスのファイルも既に開かれていると編集の準備ができています。</span><span class="sxs-lookup"><span data-stu-id="22d3f-123">The new Index file is also already opened and ready for editing.</span></span> <span data-ttu-id="22d3f-124">1 つ目の下にあるテキストを追加&lt;h2&gt;インデックス&lt;/h2&gt;などの"Hello World"</span><span class="sxs-lookup"><span data-stu-id="22d3f-124">Add some text under the first &lt;h2&gt;Index&lt;/h2&gt; like "Hello World."</span></span>

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

<span data-ttu-id="22d3f-125">アプリケーションを実行しを参照してください[ `http://localhost:xx/HelloWorld` ](http://localhostxx)お使いのブラウザーでもう一度です。</span><span class="sxs-lookup"><span data-stu-id="22d3f-125">Run your application and visit [`http://localhost:xx/HelloWorld`](http://localhostxx) again in your browser.</span></span> <span data-ttu-id="22d3f-126">この例では、コント ローラーにインデックス メソッドは、作業を行っていないが、"戻り View()"クライアントへの応答を表示するためにテンプレート ファイルの表示を使用する場合は、示されるを呼び出すでした。</span><span class="sxs-lookup"><span data-stu-id="22d3f-126">The Index method in our controller in this example didn't do any work, but did call "return View()" which indicated that we wanted to use a view template file to render a response back to the client.</span></span> <span data-ttu-id="22d3f-127">明示的を使用するビューのテンプレート ファイルの名前を指定していない、ために、ASP.NET MVC の既定値は、ファイルを使用して、Index.aspx ビュー \Views\HelloWorld フォルダー内にします。</span><span class="sxs-lookup"><span data-stu-id="22d3f-127">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the Index.aspx view file within the \Views\HelloWorld folder.</span></span> <span data-ttu-id="22d3f-128">今すぐハード コーディングされた、ビューで、文字列を確認します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-128">Now we see the string we hard-coded in our View.</span></span>

<span data-ttu-id="22d3f-129">[![Windows Internet Explorer のインデックス](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="22d3f-129">[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)</span></span>

<span data-ttu-id="22d3f-130">なかなか良いを検索します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-130">Looks pretty good.</span></span> <span data-ttu-id="22d3f-131">ただし、ブラウザーのタイトルは"Index"、そのページで、大きなタイトルは「My MVC アプリケーションです」に注意してください。</span><span class="sxs-lookup"><span data-stu-id="22d3f-131">However, notice that the Browser's title says "Index" and the big title on the page says "My MVC Application."</span></span> <span data-ttu-id="22d3f-132">これらの名前を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="22d3f-132">Let's change those.</span></span>

### <a name="changing-views-and-master-pages"></a><span data-ttu-id="22d3f-133">ビューおよびマスター ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-133">Changing Views and Master Pages</span></span>

<span data-ttu-id="22d3f-134">最初に、「My MVC アプリケーションです」というテキストを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="22d3f-134">First, let's change the text "My MVC Application."</span></span> <span data-ttu-id="22d3f-135">そのテキストを使用して、共有は、すべてのページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="22d3f-135">That text is shared and appears on every page.</span></span> <span data-ttu-id="22d3f-136">アプリ内の各ページ上にあるいても実際には、コード内の 1 か所に表示します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-136">It actually appears in only one place in our code, even though it's on every page in our app.</span></span> <span data-ttu-id="22d3f-137">ソリューション エクスプ ローラーで/Views/Shared フォルダーに移動し、Site.Master ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="22d3f-137">Go to the /Views/Shared folder in the Solution Explorer and open the Site.Master file.</span></span> <span data-ttu-id="22d3f-138">このファイルは、マスター ページと呼ばれ、共有「シェル」、その他のすべてのページを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="22d3f-138">This file is called a Master Page and it's the shared "shell" that all our other pages use.</span></span>

<span data-ttu-id="22d3f-139">このファイルには、ContentPlaceholder「メイン」と表示されているいくつかのテキストに注意してください。</span><span class="sxs-lookup"><span data-stu-id="22d3f-139">Notice some text that says ContentPlaceholder "MainContent" in this file.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

<span data-ttu-id="22d3f-140">プレース ホルダーは、ここで作成するすべてのページ表示されます、マスター ページで「ラップされた」です。</span><span class="sxs-lookup"><span data-stu-id="22d3f-140">That placeholder is where all the pages you create will show up, "wrapped" in the master page.</span></span> <span data-ttu-id="22d3f-141">次に、アプリを実行し、複数のページを参照してください、タイトルを変更してください。</span><span class="sxs-lookup"><span data-stu-id="22d3f-141">Try changing the title, then run your app and visit multiple pages.</span></span> <span data-ttu-id="22d3f-142">複数のページで、1 つの変更が表示されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="22d3f-142">You'll notice that your one change appears on multiple pages.</span></span>

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

<span data-ttu-id="22d3f-143">「My MVC ムービー アプリケーションです」の - H1 はこれで、すべてのページにプライマリ見出しになります</span><span class="sxs-lookup"><span data-stu-id="22d3f-143">Now every page will have the Primary Heading - that's H1 - of "My MVC Movie Application."</span></span> <span data-ttu-id="22d3f-144">上部にあるすべてのページで共有される白いテキストを処理するとします。</span><span class="sxs-lookup"><span data-stu-id="22d3f-144">That handles the white text at the top there that's shared across all the pages.</span></span>

<span data-ttu-id="22d3f-145">タイトルを変更しましたで全体として、Site.Master を次に示します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-145">Here is the Site.Master in its entirety with our changed title:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

<span data-ttu-id="22d3f-146">ここで、インデックス ページのタイトルを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="22d3f-146">Now, let's change the title of the Index page.</span></span>

<span data-ttu-id="22d3f-147">Open /HelloWorld/Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="22d3f-147">Open /HelloWorld/Index.aspx.</span></span> <span data-ttu-id="22d3f-148">2 つの場所を変更するのには</span><span class="sxs-lookup"><span data-stu-id="22d3f-148">There's two places to change.</span></span> <span data-ttu-id="22d3f-149">最初に、タイトルに表示される、セカンダリ ヘッダー - H2 - も、ブラウザーのタイトル。</span><span class="sxs-lookup"><span data-stu-id="22d3f-149">First, the Title that appears in the title of the browser, then the secondary header - that's H2 - as well.</span></span> <span data-ttu-id="22d3f-150">それらをどの少量のコードを確認できるように、若干異なる、アプリのどの部分を変更する各を作成します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-150">I'll make them each slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

<span data-ttu-id="22d3f-151">アプリを実行し、/Movies を参照してください。</span><span class="sxs-lookup"><span data-stu-id="22d3f-151">Run your app and visit /Movies.</span></span> <span data-ttu-id="22d3f-152">ブラウザーのタイトル、プライマリ見出しおよびセカンダリの見出しが変更されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="22d3f-152">Notice that the browser title, the primary heading and the secondary headings have changed.</span></span> <span data-ttu-id="22d3f-153">変更が小さいと、アプリ内のビューに大きな変更を加えるには簡単です。</span><span class="sxs-lookup"><span data-stu-id="22d3f-153">It's easy to make big changes in your app with small changes to your view.</span></span>

<span data-ttu-id="22d3f-154">[![ムービーの一覧]-[Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="22d3f-154">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)</span></span>

<span data-ttu-id="22d3f-155">"Data"(この場合は、"Hello World!"を少し</span><span class="sxs-lookup"><span data-stu-id="22d3f-155">Our little bit of "data" (in this case the "Hello World!"</span></span> <span data-ttu-id="22d3f-156">メッセージ) ハードコードされていた場合。</span><span class="sxs-lookup"><span data-stu-id="22d3f-156">message) was hard coded though.</span></span> <span data-ttu-id="22d3f-157">安心感の V (Views) と、C (コント ローラー) がないの M (モデル) まだきました。</span><span class="sxs-lookup"><span data-stu-id="22d3f-157">We've got V (Views) and we've got C (Controllers), but no M (Model) yet.</span></span> <span data-ttu-id="22d3f-158">方法を見てみましょう間もなくデータベースを作成し、モデル データを取得します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-158">We'll shortly walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-a-viewmodel"></a><span data-ttu-id="22d3f-159">ViewModel を渡す</span><span class="sxs-lookup"><span data-stu-id="22d3f-159">Passing a ViewModel</span></span>

<span data-ttu-id="22d3f-160">データベースに移動し、モデルについて説明する前に、しましょう。 まず"ViewModels"について</span><span class="sxs-lookup"><span data-stu-id="22d3f-160">Before we go to a database and talk about Models, though, let's first talk about "ViewModels."</span></span> <span data-ttu-id="22d3f-161">これらは、クライアントへの HTML 応答を表示するために必要とビューのテンプレートを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="22d3f-161">These are objects that represent what a View template requires to render an HTML response back to a client.</span></span> <span data-ttu-id="22d3f-162">コント ローラー クラスによって、ビュー テンプレートに渡され、ビュー テンプレートが必要です - データおよび以上のみを含める必要があります、通常は、作成します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-162">They are typically created and passed by a Controller class to a View template, and should only contain the data that the View template requires - and no more.</span></span>

<span data-ttu-id="22d3f-163">以前、HelloWorld サンプルを使用して、Welcome() アクション メソッド名前および numTimes パラメーターと、ブラウザーに出力します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-163">Previously with our HelloWorld sample, our Welcome() action method took a name and a numTimes parameter and output it to the browser.</span></span> <span data-ttu-id="22d3f-164">この応答を直接表示するために続行コント ローラーがあるのではなく、小さなクラスにそのデータを保持し、バックアップを使用して、HTML 応答をレンダリングするビュー テンプレートを経由で渡すことを加えてみましょう代わりに。</span><span class="sxs-lookup"><span data-stu-id="22d3f-164">Rather than have the Controller continue to render this response directly,let's instead make a small class to hold that data and then pass it over to a View template to render back the HTML response using it.</span></span> <span data-ttu-id="22d3f-165">こうすると、コント ローラーは、1 つの点と、ビュー テンプレート別 – クリーン"関心の分離"、アプリケーション内での管理を有効にするとします。</span><span class="sxs-lookup"><span data-stu-id="22d3f-165">This way the Controller is concerned with one thing and the View template another – enabling us to maintain clean "separation of concerns" within our application.</span></span>

<span data-ttu-id="22d3f-166">HelloWorldController.cs ファイルに戻り、新しい"WelcomeViewModel"クラスを追加、およびコント ローラー内の開始方法を変更します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-166">Return to the HelloWorldController.cs file and add a new "WelcomeViewModel" class and change the Welcome method inside your controller.</span></span> <span data-ttu-id="22d3f-167">同じファイルに新しいクラスを使用した完全な HelloWorldController.cs を次に示します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-167">Here is the complete HelloWorldController.cs with the new class in the same file.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

<span data-ttu-id="22d3f-168">複数行にある場合でもへようこそ メソッドは本当に 2 つのコード ステートメントにします。</span><span class="sxs-lookup"><span data-stu-id="22d3f-168">Even though it's on multiple lines, our Welcome method is really only two code statements.</span></span> <span data-ttu-id="22d3f-169">最初のステートメントでは、ビューには、結果のオブジェクトが ViewModel オブジェクト、および 2 番目のパスに、2 つのパラメーターをパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-169">The first statement packages up our two parameters into a ViewModel object, and the second passes the resulting object onto the View.</span></span>

<span data-ttu-id="22d3f-170">ようこそビュー テンプレートいく必要があります。</span><span class="sxs-lookup"><span data-stu-id="22d3f-170">Now we need a Welcome View template!</span></span> <span data-ttu-id="22d3f-171">ようこそメソッドを右クリックし、追加のビューを選択します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-171">Right click in the Welcome method and select Add View.</span></span> <span data-ttu-id="22d3f-172">このとき、おします「厳密に型指定されたビューを作成する」を確認し、ドロップ ダウン リストから、WelcomeViewModel クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-172">This time, we'll check "Create a strongly-typed view" and select our WelcomeViewModel class from the drop down list.</span></span> <span data-ttu-id="22d3f-173">この新しいビューは、WelcomeViewModels しない他の種類のオブジェクトは次のトピックではのみです。</span><span class="sxs-lookup"><span data-stu-id="22d3f-173">This new view will only know about WelcomeViewModels and no other types of objects.</span></span>

> <span data-ttu-id="22d3f-174">*注: ドロップダウン リストに表示、WelcomeViewModel 用に追加した後に 1 回コンパイルする必要があります。*</span><span class="sxs-lookup"><span data-stu-id="22d3f-174">*NOTE: You'll need to have compiled once after adding your WelcomeViewModel for to show up in the drop down list.*</span></span>


<span data-ttu-id="22d3f-175">ここでは新機能、ビューの追加 ダイアログのようになります。</span><span class="sxs-lookup"><span data-stu-id="22d3f-175">Here's what your Add View dialog should look like.</span></span> <span data-ttu-id="22d3f-176">[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="22d3f-176">Click the Add button.</span></span> ![ビューの丸を追加します。](getting-started-with-mvc-part3/_static/image10.png)

<span data-ttu-id="22d3f-178">このコードを追加、 &lt;h2&gt;新しい Welcome.aspx でします。</span><span class="sxs-lookup"><span data-stu-id="22d3f-178">Add this code under the &lt;h2&gt; in your new Welcome.aspx.</span></span> <span data-ttu-id="22d3f-179">うまくループを作成し、ユーザーの質問がお回数だけ挨拶!</span><span class="sxs-lookup"><span data-stu-id="22d3f-179">We'll make a loop and say Hello as many times as the user says we should!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

<span data-ttu-id="22d3f-180">またにため言いましたこの、WelcomeViewModel に関するビューにを入力しているときに注意してください (既婚、保存しますか?) の下のスクリーン ショットに見られるよう、モデル オブジェクトを参照するたびに役立つ Intellisense を取得します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-180">Also, notice while you're typing that because we told this View about the WelcomeViewModel (they are married, remember?) that we get helpful Intellisense each time we reference our Model object as seen in the screenshot below:</span></span>

<span data-ttu-id="22d3f-181">[![NumTime ソース コード](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="22d3f-181">[![NumTime Source Code](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)</span></span>

<span data-ttu-id="22d3f-182">アプリケーションを実行しを参照してください`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`もう一度です。</span><span class="sxs-lookup"><span data-stu-id="22d3f-182">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` again.</span></span> <span data-ttu-id="22d3f-183">URL からデータを移動しています、これで、コント ローラーに自動的に渡されるという、当社のコント ローラーが、ViewModel にデータをパッケージ化し、ビューには、そのオブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="22d3f-183">Now we're taking data from the URL, it's passed into our Controller automatically, our Controller packages up the data into a ViewModel and passes that object onto our View.</span></span> <span data-ttu-id="22d3f-184">ビューよりも、ユーザーに html 形式でデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="22d3f-184">The View than displays the data as HTML to the user.</span></span>

<span data-ttu-id="22d3f-185">[![Windows Internet Explorer の開始](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="22d3f-185">[![Welcome - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)</span></span>

<span data-ttu-id="22d3f-186">モデル、"M"の種類が、データベースの種類ではありませんでした。</span><span class="sxs-lookup"><span data-stu-id="22d3f-186">Well, that was a kind of an "M" for Model, but not the database kind.</span></span> <span data-ttu-id="22d3f-187">学んだこと確認し、ムービーのデータベースを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="22d3f-187">Let's take what we've learned and create a database of Movies.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="22d3f-188">[前へ](getting-started-with-mvc-part2.md)
[次へ](getting-started-with-mvc-part4.md)</span><span class="sxs-lookup"><span data-stu-id="22d3f-188">[Previous](getting-started-with-mvc-part2.md)
[Next](getting-started-with-mvc-part4.md)</span></span>