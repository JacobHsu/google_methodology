## 埃及法老的迷宮問題

為了讓“遍歷”這個問題容易理解，我們還是先從一個古老的遊戲做起，就是走迷宮的遊戲。
如果你在小學玩過一筆畫的問題，很容易從左上角的入口走到左下角的出口。

對於任何一個迷宮，怎樣走能確保走出來呢？相傳古埃及有一位法老在幼年發明了一種方法，能夠做到這一點。在那個傳說中，這位少年被放到了一個迷宮中，被要求走出來。這位天才少年帶了一根很長的繩子一頭拴在迷宮的入口，然後採用下面的走法走出了迷宮。

### 兩個大原則

大原則1：他走到哪裡就把繩子拖到哪裡，也就是說，繩子記錄了他從入口經歷各個分岔口之後到當前位置的行走的路線。
大原則2：無論是遇到新的岔路口，還是回到上一個岔路口，都先走“尚未進入的最左邊的岔路”。

有了這兩個大原則，再說

### 具體的走法

1. **起始的走法**：帶著繩子進入到迷宮後，來到第一個岔口（如果有兩個以上，就選擇最左邊的先走），在這個岔口，走進最左邊的分岔，讓繩子隨著自己沿著岔路往前走，在遇到新的岔口的時候，還是沿著最左邊走，直到走到死胡同的時候往後退，每次往後退時，退到上一個岔口，我們不妨稱之為岔口X。這時進入左邊第二個岔道（因為第一個已經走完了，證明是死胡同了）。
2. **遞進走法**：當進入到第二個岔道後，重複第一步的過程，直到走不下去退回來，又走回到岔口X，這時進入左邊第三個岔口。然後再重複上述過程，直到遇到死胡同往後退，或者運氣好已經走出去了。這樣就把岔口X所能到達的所有岔路都“遍歷”一遍了。
3. **回溯走法**：當把岔口X所能到達的所有岔路都試了一遍，還沒有走通時，沿著繩子往回走，走回到X的前一個岔路口Y。重複上述這三步。
4. 上述方法能夠保證解決任何迷宮問題。當我們不斷重複上述三個步驟後，會有兩個結果，第一個結果是走出去了，第二個是回到了出發點。前一個結果說明迷宮走得通，第二個說明走不通。不管哪一個結果，這個迷宮都算走完了。

當然，這個方法，你可能會發現它有點笨，因為不斷回溯，走了很多“冤枉路”。但事實上，這個笨辦法是唯一能夠確保走出任何迷宮的方法。很多聰明人試圖忽略掉那些看似不可能的分支，然而最後能走通的路徑，常常隱藏在那些看似不可能的地方。事實上，任何試圖省略一些岔路的人，走的冤枉路會多很多。

在上述的方法中，那根繩子特別重要，因為它記錄了你是從哪裡來，哪些岔路已經走過，避免了你來回來去走重複路。大家如果真有走迷宮的經驗，特別是那些只能看見牆，看不到遠方路徑的迷宮，就會發現你根本記不住哪個地方你已經到過了，走出五倍十倍的冤枉路是很正常的事情。當然，帶一個長繩子可能不現實，事實上只要帶一小口袋豆子或者什麼其它的標誌物，在每一個路口做一個標記即可。每開始進入一個新的岔口，在那個地方放一粒豆子，等到前面走不通回溯到這個路口時，將豆子撿起來，放入下一個岔口即可。

### 三個總結

1. 走迷宮只要堅持每次遇到岔口，就走左邊尚未走過的分支，一定能走出去。這個方法由於每次都是先走左邊的岔路，因此也叫做`左手算法`，或者`順時針算法`。當然你每次走右邊也可以，那就是逆時針算法，道理是一樣的。
2. 一些看似是笨辦法的做法其實並不笨，因為如果在這個過程中有些工作看似能省掉，其實省不掉，試圖抄近路或者盲目試錯，其結果都是不必要的重複和返工。世界上很多事情，不怕按部就班做得慢，就怕來回來去兜圈子和返工。
3. 對於複雜迷宮，如果用上述算法按部就班地做，計算機很快就完成了。如果試圖投機取巧，計算機永遠做不完，要麼陷入死循環，要麼死機。
4. 我們人的生活軌跡其實就是一個迷宮。凡事我們應該允許犯一次錯誤，但是應該有一個本子（如同上面例子中的繩子），記錄我們在哪裡犯了錯誤，將來遇到同樣的情況，不犯第二次。如果沒有這根繩子或者一個本子，同樣的錯誤會不斷犯下去的。
