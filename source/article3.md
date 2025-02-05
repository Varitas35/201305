## R 統計軟體(3) – 中央極限定理 (作者：陳鍾誠)

中央極限定理是機率統計上最重要的定理之一，整個統計的估計與檢定幾乎都建立在這個定理之上，因此
對「中央極限定理」有清楚的理解是學好機率統計的一個關鍵。

在本文中，我們將利用 R 軟體實作並驗證「中央極限定理」，讓讀者能透過程式實際體會該定理的重要性與用途。

### 中央極限定理簡介

中央極限定理的數學式：  ![](../timg/_frac_x__5884aa99cd8217c2afb62761ea0e4934.jpg) 

如果用白話文陳述，那就是說 n 個樣本的平均數  ![](../timg/_frac_x__894e240322d2234d15449f707fb01d9f.jpg)  會趨近於常態分布。

更精確一點的說，當您從某個母體 X 取出 n 個樣本，則這 n 個樣本的平均數  ![](../timg/_frac_x__894e240322d2234d15449f707fb01d9f.jpg)  會趨近於以平均期望值  ![](../timg/_mu_c9faf6ead2cd2c2187bd943488de1d0a.jpg)  為中心，
以母體標準差  ![](../timg/_sigma_a2ab7d71a0f07f388ff823293c147d21.jpg)   除以  ![](../timg/_sqrt_n__be183cdd1e68ce30a59b96233609b08f.jpg)  的值  ![](../timg/_sigma_s_b902c90353f0130390ad6d58bccf041c.jpg)  為標準差的常態分布。

如果採用另一種正規化後的公式寫法，也可以將上述的「中央極限定理」改寫為：  ![](../timg/_frac_ba_5808b18298954a6703f73eadcd9acb03.jpg) 

其中的 Z 是指標準常態分部，也就是  ![](../timg/_frac_ba_41f620914dc6a86571eb622d2c929eb5.jpg)  會趨近標準常態分布。

### 中央極限定理的用途

根據上述的定義，我們知道當樣本數 n 足夠大時 (通常 20 個以上就夠大了)， n 個樣本的平均值  ![](../timg/_frac_x__894e240322d2234d15449f707fb01d9f.jpg) 
會趨近於常態分布，換句話說也就是  ![](../timg/_frac_ba_41f620914dc6a86571eb622d2c929eb5.jpg)  會趨近於標準常態分布。

因此、當我們取得一組樣本之後，我們就可以計算其平均數  ![](../timg/_frac_x__894e240322d2234d15449f707fb01d9f.jpg) ，如果有人告訴我們說母體的
平均數  ![](../timg/_mu_c9faf6ead2cd2c2187bd943488de1d0a.jpg)  的值是多少，我們就可以看看  ![](../timg/_bar_x__6fbdf291cda891b99cf211417ad1df18.jpg)  與  ![](../timg/_mu_c9faf6ead2cd2c2187bd943488de1d0a.jpg)  是否差距很遠，如果差距很遠，
導致  ![](../timg/_bar_x__6fbdf291cda891b99cf211417ad1df18.jpg)  來自平均數  ![](../timg/_mu_c9faf6ead2cd2c2187bd943488de1d0a.jpg) 
母體的機率很小，那麼很可能是此組樣本是非常罕見的特例，或者該組樣本的抽樣有所偏差，也就是該組樣本很可能並非來自平均數為  ![](../timg/_mu_c9faf6ead2cd2c2187bd943488de1d0a.jpg) 
的母體。

以下是一些標準常態分布的重要數值，

1.  ![](../timg/P_sigma__e258ddb9fece89d2a1eff035497f6032.jpg) 
2.  ![](../timg/P_2_sigm_00c3fe6de89d7f4a7f3dd19d11b48f27.jpg) 
3.  ![](../timg/P_3_sigm_83a5e13e11e1c3cf89a98da3b9735358.jpg) 
4.  ![](../timg/P_4_sigm_352e43bbeb3afe137cef2896293a6a4e.jpg) 
5.  ![](../timg/P_5_sigm_83ffc195699b28ac9bdb4847f943f7fb.jpg) 
6.  ![](../timg/P_6_sigm_62ffbb4b34fa82d0e887a09f46b8a21c.jpg) 


```R
> pnorm(1)-pnorm(-1)
[1] 0.6826895
> pnorm(2)-pnorm(-2)
[1] 0.9544997
> pnorm(3)-pnorm(-3)
[1] 0.9973002
> pnorm(4)-pnorm(-4)
[1] 0.9999367
> pnorm(5)-pnorm(-5)
[1] 0.9999994
> pnorm(6)-pnorm(-6)
[1] 1
> options(digits=10)
> pnorm(6)-pnorm(-6)
[1] 0.999999998
```

從上面的數值您可以看出來，管理學上所謂的六標準差其實是很高的一個要求，也就是良率必須要達到 99.9999998% 以上才行。

如果您今天所取的 n 個樣本，與母體平均數  ![](../timg/_mu_c9faf6ead2cd2c2187bd943488de1d0a.jpg)  距離兩個標準差以上，那就很可能有問題了，這種推論稱為檢定，我們可以用 R 軟體中的
t.test 函數來檢驗這件事，我們將在下一期當中說明如何用 R 軟體進行統計檢定的主題，讓我們先將焦點移回到中央極限定理身上，用 R 軟體
來驗證該定理。

### R 程式範例：驗證中央極限定理

```R
> u <- matrix ( runif(500000), 50, 10000 )
> y <- apply ( u, 2, mean )
> hist(u[,1])
> hist(y)
> ?apply
> 
```

說明：

1. u 乃是將 50 萬個 uniform 樣本分配成 50*10000 的矩陣，
2. y 對 u 進行列統計 apply ( u, 2, mean ) 代表對每行取平均值 mean(col of u) 的結果。
3. 因此 y 代表從 Uniform Distribution 中每次取出 50 個樣本，然後進行加總平均的結果，也就是   ![](../timg/_frac_x__8c510cfb84836f2f9c81b601b1f3dbd3.jpg)  。
4. 從下列的 hist(y) 圖形中，我們可以看到中央極限定理的證據：也就是   ![](../timg/_frac_x__8c510cfb84836f2f9c81b601b1f3dbd3.jpg)  會趨向常態分布。

![圖、hist(u[,1]) 畫出的圖形](../img/HistU.jpg)

![圖、hist(y) 畫出的圖形](../img/HistY.jpg)


```R
CLT = function(x) {
  op<-par(mfrow=c(2,2)) # 設為 2*2 的四格繪圖版
  hist(x, nclass=50)	 # 繪製 x 序列的直方圖 (histogram)。
  m2 <- matrix(x, nrow=2, )	 # 將 x 序列分為 2*k 兩個一組的矩陣 m2。
  xbar2 <- apply(m2, 2, mean)	# 取每兩個一組的平均值 (x1+x2)/2 放入 xbar2 中。
  hist(xbar2, nclass=50)	 # 繪製 xbar2 序列的直方圖 (histogram)。
  m10 <- matrix(x, nrow=10, )	# 將 x 序列分為 10*k 兩個一組的矩陣 m10。
  xbar10 <- apply(m10, 2, mean)	# 取每10個一組的平均值 (x1+..+x10)/10 放入 xbar10 中。
  hist(xbar10, nclass=50)	 # 繪製 xbar10 序列的直方圖 (histogram)。
  m20 <- matrix(x, nrow=20, )	# 將 x 序列分為 25*k 兩個一組的矩陣 m25。
  xbar20 <- apply(m20, 2, mean)	# 取每20個一組的平均值 (x1+..+x20)/20 放入 xbar20 中。
  hist(xbar20, nclass=50)	 # 繪製 xbar20 序列的直方圖 (histogram)。
}

CLT(rbinom(100000, 20, 0.5)) # 用參數為 n=20, p=0.5 的二項分布驗證中央極限定理。
CLT(runif(100000)) # 用參數為 a=0, b=1 的均等分布驗證中央極限定理。
CLT(rpois(100000, 4)) # 用參數為 lambda=4 的布瓦松分布驗證中央極限定理。
CLT(rgeom(100000, 0.5)) # 用參數為 n=20, m=10, k=5 的超幾何分布驗證中央極限定理。
CLT(rhyper(100000, 20, 10, 5)) # 用參數為 p=0.5 的幾何分布驗證中央極限定理。
CLT(rnorm(100000)) # 用參數為 mean=0, sd=1 的標準常態分布驗證中央極限定理。
CLT(sample(1:6, 100000, replace=T))	# 用擲骰子的分布驗證中央極限定理。
CLT(sample(0:1, 100000, replace=T))	# 用丟銅板的分布驗證中央極限定理。
```

以下是這些指令的執行結果，從這些圖中您可以看到當樣本數到達 20 個的時候，幾乎每種樣本都會趨向常態分布。

![圖、指令 CLT(rbinom(100000, 20, 0.5)) 的執行結果](../img/CLT_rbinom.jpg)

![圖、指令 CLT(runif(100000)) 的執行結果](../img/CLT_runif.jpg)

![圖、指令 CLT(rpois(100000, 4)) 的執行結果](../img/CLT_rpois.jpg)

![圖、指令 CLT(rgeom(100000, 0.5)) 的執行結果](../img/CLT_rgeom.jpg)

![圖、指令 CLT(rhyper(100000, 20, 10, 5)) 的執行結果](../img/CLT_rhyper.jpg)

![圖、指令 CLT(rnorm(100000)) 的執行結果](../img/CLT_rnorm.jpg)

![圖、指令 CLT(sample(1:6, 100000, replace=T)) 的執行結果](../img/CLT_sample1_6.jpg)

![圖、指令 CLT(sample(0:1, 100000, replace=T)) 的執行結果](../img/CLT_sample0_1.jpg)

### 參考文獻
1. [The Central Limit Theorem (Part 1)](http://msenux.redwoods.edu/math/R/CentralLimit.php)
2. [洋洋 -- 淺談機率上的幾個極限定理](http://episte.math.ntu.edu.tw/articles/mm/mm_09_3_07/index.html)
3. Proof of Central Limit Theorem, H. Krieger, Mathematics 157, Harvey Mudd College, Spring, 2005
    * <http://www.math.hmc.edu/~krieger/m157cltproof.pdf>
4. [Central limit theorem:From Wikipedia, the free encyclopedia](http://en.wikipedia.org/wiki/Central_limit_theorem)
5. Two Proofs of the Central Limit Theorem, Yuval Filmus, January/February 2010
    * <http://www.cs.toronto.edu/~yuvalf/CLT.pdf>
