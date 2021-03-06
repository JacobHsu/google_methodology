## 如何下載整個互聯網上的全部網頁？

如何構建一個網絡爬蟲，下載整個互聯網上全部的網頁。

_"整個互聯網是彼此連通的，從任何一個網頁出發，跟隨它的超鏈接採用深度優先算法，或者廣度優先算法，就可以下載整個互聯網了。"_

這個道理是對的，事實上你如果用開源的軟件，從一家門戶網站比如雅虎的首頁出發，先下載這個網頁，然後通過分析這個網頁，可以找到頁面裡的所有超鏈接，也就等於知道了這家門戶網站首頁所直接鏈接的全部網頁，諸如雅虎郵件、雅虎財經、雅虎新聞等。接下來訪問、下載並分析這家門戶網站的郵件等網頁，又能找到其他相連的網頁。讓計算機不停地做下去，就能下載很多很多網頁。

### 實際問題

但是，如果一個候選人對圖論或者網絡爬蟲的理解僅僅到這一步，他一定無法搭建起一個實用的程序，當然也無法通過Google的面試。因為他忽略了太多的實際問題。對於這道面試題，我一般會按照下面的次序一步步追問。

首先我們要知道，現在的互聯網非常龐大，今天Google的索引中有超過1萬億個網頁，即使更新最頻繁的基礎索引也有幾百億個網頁，假如下載一個網頁需要一秒鐘，下載這100億個網頁則需要317年。如果下載10000億個網頁則需要32000年左右，是我們人類有文字記載歷史的5到6倍的時間，這顯然不現實。

下載每一個網頁因為需要有網絡通信的時間，網速再快這個時間也省不了，因此時間不會太短，即便縮短10倍，100億個網頁花31.7年或者1萬億個網頁花3200年也是一個不可接受的時長。

當然，有點計算機工作經驗的人會馬上想到使用`並行計算`，比如讓1000台服務器同時工作，這樣下載100億個網頁大約只需要半個月的時間，這是可以接受的，當然，下載1萬億個網頁還有點挑戰性。然而，世界上任何事情都是有成本的，並行計算也是如此。

為了保障這1000台服務器不會把某個網頁訪問好幾次，而把另外一些網頁漏掉，需要有一個小本本，記錄已經下載過的網頁。這個小本本需要多大呢？如果每一個網頁寫上一筆只需要8個字節，那麼存100億個網頁的記錄也需要80GB的內存，今天還沒有哪台通用的服務器有這麼大的內存。即使有，如果我們把這個關於已經下載網頁記錄的"小本本"放在一台服務器上，所有其它服務器在決定是否下載某個網頁之前先要詢問它一次，它也一定會成為瓶頸。然而，如果不把"小本本"集中管理，而是分散到各個服務器中，那麼每一次下載，都要詢問所有的服務器，也顯然不現實。

因此，裡面有很多玄機。事實上，一個商業的網絡爬蟲需要有成千上萬個服務器，並且通過高速網絡連接起來。如何建立起這樣複雜的網絡系統，如何協調這些服務器的任務，就是`網絡設計和程序設計`的藝術了。

至於小本本怎麼管理，其實就要用到講到的`隨機化`的原理了，隨機化之後看上去無序，其實反而變得有規律可循，

於是每個服務器看到一個網頁，都知道這個網頁是否屬於自己的工作範圍，如果不屬於，則通知相應的服務器去處理，而不需要到處廣播。

### 圖論中給出了廣度優先遍歷（BFS）和深度優先遍歷（DFS）兩種算法，到底該用哪個呢？

雖然從理論上講，這兩個算法（在不考慮時間因素的前提下）都能夠在大致相同的時間裡"爬下"整個"靜態"互聯網上的內容，但這只是理論上的可行性，它有兩個假設——**不考慮時間因素，互聯網靜態不變**，都是現實中做不到的。

搜索引擎的網絡爬蟲問題更應該定義成`"如何在有限時間裡最多地爬下最重要的網頁"`。

顯然各個網站最重要的網頁應該是它的首頁。在最極端的情況下，如果爬蟲非常小，只能下載非常有限的網頁，那麼應該下載的是所有網站的首頁，如果把爬蟲再擴大些，應該爬下從首頁直接鏈接的網頁（就如同和北京直接相連的城市），因為這些網頁是網站設計者自己認為相當重要的網頁。在這個前提下，顯然**BFS明顯優於DFS**。

事實上在搜索引擎的爬蟲裡，雖然不是簡單地採用BFS，
但是先爬哪個網頁，後爬哪個網頁的調度程序，原理上基本上是BFS(廣度優先遍歷)。

那麼是否DFS就不使用了呢？

也不是這樣的。這是和爬蟲的分佈式結構以及`網絡通信的握手成本`有關。所謂"握手"就是指下載服務器和網站的服務器建立通信的過程。這個過程需要額外的時間（Overhead Time），如果握手的次數太多，下載的效率就降低了。

實際的網絡爬蟲都是一個由成百上千甚至成千上萬台服務器組成的分佈式系統。對於某個網站，一般是由特定的一台或者幾台服務器專門下載。`這些服務器下載完一個網站，然後再進入下一個網站`，而不是每個網站先輪流下載5%，然後再回過頭來下載第二批。這樣可以避免握手的次數太多。如果是下載完第一個網站再下載第二個，那麼這又有點像DFS，雖然下載同一個網站（或者子網站）時，還是需要用BFS的。

總結起來，網絡爬蟲對網頁遍歷的次序不是簡單的BFS或者DFS，而是有一個`相對複雜的下載優先級排序的方法`。管理這個優先級排序的子系統一般稱為`調度系統（Scheduler）`，由它來決定當一個網頁下載完成後，接下來下載哪一個。

當然在調度系統裡需要存儲那些已經發現但是尚未下載的網頁的URL，它們一般存在一個`優先級隊列（Priority Queue）`裡。而用這種方式遍歷整個互聯網，在工程上和BFS更相似。因此，`在爬蟲中，BFS的成分多一些`。

大家對今天信息的量級要有體會。在圖論出現後的很長時間裡，現實世界中圖的大小都是在幾千個節點以下的規模（比如公路圖、鐵路圖等）。那時候，圖的遍歷是一件很簡單的事情，因此在工業界沒有多少人專門研究這個問題。但是等到了互聯網出現後，圖的大小就從幾千增加到上萬億了。

### 如何分析一個HTML頁面，然後把裡面的超鏈接地址URL提取出來？

這件事在過去並不難，因為過去的網站都是明碼編寫這些URL，前後都有明顯的標識，很容易提取出來。但是現在很多URL的提取就不那麼直接了，因為很多網頁如今是用一些`腳本語言（比如JavaScript）生成的`。打開網頁的源代碼，URL不是直接可見的文本，而是運行這一段腳本後才能得到的結果。

因此，網絡爬蟲的頁面分析就變得複雜很多，它要模擬瀏覽器運行一個網頁，才能得到裡面隱含的URL。有些網頁的腳本寫得非常不規範，以至於解析起來非常困難。可是，這些網頁還是可以在瀏覽器中打開，說明瀏覽器可以解析。因此，需要做瀏覽器內核的工程師來寫網絡爬蟲中的解析程序，可惜出色的瀏覽器內核工程師在全世界數量並不多。

除此之外，建立互聯網的下載爬蟲還有很多技術細節，比如有些網站網頁更新快，如何處理？有些幾乎是靜態的網頁，平時沒有更新，又該如何處理等等。

你可能會好奇Google的索引大約有多大，我只能告訴你基本的索引，即上百萬的索引每一份使用了上萬台服務器，而在全世界它有很多份。此外，在亞洲的索引，中日韓文的內容會多些，而在歐洲的法、意、德、西（FIGS）語言的內容會多些。
