# Matlab / Octave 簡易紀錄
## 變數宣告
* 類似python, x = 1, 如果最後加分號(;)就不會顯示結果
* 乘法間一定要加上*

## 清除變數
* 使用`clear 變數` 來清除掉目前workspace儲存的變數
* 如果僅用 `clear` 則清除所有變數
## 顯示
* disp()
* `format long` - 使顯示小數達14位
* `format short` - 使顯示小數回歸預設
## 函式使用
* sqrt = 開根號
* y = fun(x) 就會是我們用的 $f(x)$ 

| Matlab | 函式 |
| -------- | -------- | 
| y = fun(x) | $y=f(x)$| 
| y = sin(x) | $y=sin(x)$|
| y = log10(x)|$y=log_{10}x$|
| y = exp(x) | $y=e^{x}$ |
* 而matlab能處理矩陣/向量, 所以可以接受其為input, 例如  
 $$
  y=sin(
 \begin{bmatrix}
  0  \\
 \frac{\pi}{2}\\
 \pi  \\
 \frac{3\pi}{2}\\
  2\pi
\end{bmatrix}
)
 $$
* a = min(y) -> return -1
* 更進一步如果想知道它的位置(index)可以用(a,I) = min(y), 要特別注意的是這邊的index從0開始算起不太一樣
* 簡單顯示圖形plot(x,y)例如
```
x = [-2, -1, 0, 1, 2]
y = [2, -5, -6, -1, 10]
plot(x,y)  
```
## 建構向量
> x = [-2, -1, 0, 1, 2]  
> 如果要建構成column vector只需這樣改動
> x = [-2; -1; 0; 1; 2]  
* 建構uniformly spaced vector(固定間距)
> 例如:  
> a = 0;  
> b = 100;  
> c = a : 30 : 100 (下限=起點:步伐:上限)  
> 可以看到c = 0 30 60 90
* 另一種建構固定間距的方式為使用linspace()
> linspace(a, b, numPoints)  
> 必含上下限, numPoints是一共要多少個數(含頭尾)不是步伐(spacing)  
> spacing = $\frac{b-a}{numPoints}$
## 從向量中條件式萃取/修改我們想要的資料
* (就不寫成直式了)
> data = [0.01, 0.02, 0.05, 0.11, 0.99, 0.18, 0.009]  
> 我們只想要 >0.1的資料  
> 建構 `ind = data > 0.1;`  
> 我們能得到邏輯陣列/遮罩/濾鏡 ind = [0, 0, 0, 1, 1, 1, 0]  
> 接著用 `activity = data(ind)` 就能得到我們要的結果(data(遮罩;logical array)的概念)  
> 我們就能獲取我們的結果了 `activity = 0.11 0.99 0.18`  
> 如果想要修改,例如我們想把小於等於0.1的值改成0  
> 就這樣打`data(ind) = 0` 就可以了  
> 
> 總結:  
> Step1:藉由邏輯運算建構邏輯向量(<,>,==,~=,...)  
> Step2:用邏輯向量來對原本的資料作操作  
## 向量的計算
* 由於乘(*)除(/)次方(^)matlab保留給矩陣使用, 因此向量在做這三種運算時要加個.變成(.*)(./)(.^)此稱之為element-wise operations, 而加減法則不受限, 以下來看個範例
> 我們給定一個函式$f(x)=3x^2+2x-6$  
> 我們希望其中代入多個數值計算後丟入plot函數中繪圖  
> 利用上面間距的做法  
> `x = -2:0.1:2;` 建構40個數值  
> `y = 3*x.^2+2*x-6;`  
> `plot(x,y)`  
## 向量的反轉(transpose)
* row vector與 column vector的轉換, 只需要加一個單引號(')  
> v = [2, -1, 8.5]  
> c = v' 即可
## 逆矩陣(inverse matrix)
* 有分逆(inv)跟偽逆(pinv), 不過在ML課堂中都用pinv即可
> `pinv(matrix)` 就這樣   
## 繪圖
* 使用plot(x,y, paras)繪製線條, 必須確保元素個數相同
* paras參數部分為'顏色/線條組成/點', 例如:  
> 'm:s' - 表示酒紅色,點狀線條,方形點  
> 'g--' - 表示綠色, 虛線, 星號點  
> 'r-' - 表示紅色, 實線, 沒有點  
* 使用`figure(1); plot()`指令來指定figure幾放哪個圖
* 使用subplot(row, column, access); 來設定多張圖
> 例如`subplot(1,2,1);` 表示做成1x2的方形布局放圖, 然後目前取第一個 
* 使用`clf` 清除figure塊上的圖形
* 使用`imagesc(目標矩陣)` 能顯示類似熱能感測的圖形
> `imagesc(matrix), colorbar, colormap gray` 可以使圖像變成灰階
## 圖片上的註解
> `xlabel('x座標註解')`  
> `ylabel('y座標註解')`  
> `title('標題')`  
> `legend('線條圖示')` 用於多條線段時  
> `annotation()`這個比較多設定, 可以看官方文檔  
> `axis([x最小值 x最大值 y最小值 y最大值])` 設定顯示座標
## 圖片上同時顯示不同的圖
* 直接用例子說明
> `bar(month, maxTempLondon, 'yellow')` 畫出直方圖  
> `hold on` 修但幾咧, 這樣等下畫的圖才不會蓋掉這圖  
> `plot(month, maxTempLondon, 'red'` 這樣就能又跟著畫出一條紅線  

## 建構矩陣(Matrix)
* 我們用[,]來建構row vector, 用[;]來建構column vector, 然後併用來建構矩陣  
> M = [1,2,3;4,5,6]像這樣就能建構出一個2x3的矩陣

## 矩陣的計算
* 當我們有兩個3x2矩陣, 我們希望的是"分別對應"的元素做運算時, 我們使用的是element-wise operations(.* ./ .^ + -), 而同時要變大變小加減某個數則直接運算

## 矩陣元素的提取
* 直接使用索引提取
> `data(row, column)`
* 如果我們想要提取某段, 例如1,5row的2~4column元素
> rows = [1 5]; 注意沒有用逗號或是分號隔開
> cols = 2:4;
> subdata = data(rows, cols); 這樣就提出了
* 提取某個條件的元素位置(index)
> 例如:[r,c] = find(矩陣 > 8), 會把row/column存入, rc可以自己改想要的變數

## 特殊矩陣建構
> eye(n)建構一個nxn的identity matrix  
> ones同zeros  
> zeros(n)只用一個數就是nxn, 用(row,column)就能建構自己想要的  
> rand(size,class)sieze同zeros(此函數建構在均勻分布下, 從0~1取值)  
> randn(size,class)size同zeros(此函數建構在常態分佈下)  
> randi(最大值,size,class)size同zeros作法(此函數建構是均勻分布下)  
> diag(1:4)建構一個1~4的斜方矩陣  
> linsapce  
> magic(n) 建構一個nxn的神奇矩陣(n不能為2), row/column加總都一樣

## 矩陣的合併
* Concatenation  
> 作法很簡單就跟創建矩陣很像  
> 如果資料是"上下接"則稱作垂直式(vertically), 倆矩陣的column數要一樣  
> **`=[上矩陣;下矩陣]`** 注意其中是用**分號**隔開  
> 如果資料是"左右接"則稱作水平式(horizontally)  
> **`=[左矩陣,右矩陣]`** 注意這個中間是用**逗號**隔開, 倆矩陣的row數要一樣  

## 決定陣列/向量/矩陣的大小
* 如果是向量或陣列, 可以單用`length()`直接獲取, 然而如果是用在矩陣上, 則會回傳最大的那個數字, 可能是row也可能是column  
* 所以在矩陣上我們使用size(), 會回傳一個1x2矩陣來表達[row, column]
* 所以還能這樣用size(目標矩陣,1)回傳row數, 2則回傳column數  

## 矩陣的乘法
* 終於, 記得不是使用element-wise operation, 直接使用(*)就可以了
* 務必記得確認前column數要跟後row數相同

## 將陣列/向量/矩陣做reshape
* 使用reshape(要改動的目標, row, column)  
* 要注意提取和填入順序都是從第一個column依次提取也從第一個column依次填入  
* 如果我們已經知道要改變的row數但不確定column時可以用這樣(目標, 2, []), 反之亦然  
* 如果想變成向量(單column), 可以這樣 `目標(:)`  

## 統計函數在矩陣上的使用
* mean(矩陣), 這樣會計算出各個column的平均值
* 如果想改算各個row的則加入參數, mean(矩陣, 2)表示針對第二個維度做計算
* 如果要算整個矩陣的平均呢?可以利用上面的 矩陣(:) 先將其轉成單column直接用mean()算  
* 其他函數如max(), min(), std() 用法及更詳盡的函數說明看文檔  

## 邏輯運算
* 關係運算子(relational operators)
  - < : 小於
  - \> : 大於
  - <= : 小於或等於
  - \>= : 大於或等於
  - == : 等於
  - ~= : 不等於
* 邏輯運算子(logical operators)
  - & : 和
  - | : 或
  - ~ : 非(not)
  - 短路運算下 && 及 || 都是存在的

## if-else statement
* 寫法:
```
if 條件    <--不用冒號
    敘述 ; <--注意分號結束
elseif 條件 <--用elseif 不是elif
    敘述 ;
else
    敘述 ;
end <--還要end
``` 

## for loop
* 寫法:
```
for num = 起始 : 結尾 (含頭含尾)
    敘述 ;
end <-- loop結束
```

## while loop
* 寫法:
```
while 條件
    敘述;
end
```

## 客製化函數
* 寫法:
```
function [out1, out2, ...] = functionName(in1, in2, ...)
    敘述 1;
    敘述 2;
    ...
end
```
例如我們想算這個式子
$$
\frac{1-r^n}{1-r}, r\ne1
$$
```
function s = geoSum(r,n)
if r == 1
    s = n;
else
    s = (1-r^n)/(1-r);
end
end
```
* out就是我們要return的代表
* 如果想不斷的使用這個funcion, 可以在同一個資料夾中創造一個函數名.m的檔案, 編輯在內就能不斷呼叫使用

## 將函數以參數形式輸入
* 第一種方式:要代入使用的函數必須是匿名函數形式, 而創建的寫法如下
```
function_handle = @(變數) 函數本身
例如:
f = @(x) 3*x^3 + 2*x - 1
```
* 第二種方式:當代入的函式較為複雜無法一行寫完時, 創建另個函數檔案, 取好名字後在代入時函數名稱前面加個@符號即可, 這種方式也適用任何已內建的matlab函數

# 求助文檔
[不懂就看文檔](https://www.mathworks.com/help)  
* 或是命令列中打入help 函數就能顯示文檔, 按q就能離開

## which file/variable using
* 用which 名稱來查詢
* 其實直接避免用重複的就好了- -

## debug
* 有debug mode, 可以設定breakpoint, 另外在debug模式下我們可以試著去輸入不同操作來看問題, 這些輸入在離開debug模式後並不會留存
* breakpoint點擊editor前面的行數的dash就可以了

## 讀取data
* 使用`load file.blabla` 或 `load('file.blabla')`都可以
* 讀取後會用檔名作為變數使用, 如果不確定可以看workspace或用`who`指令查看

## 儲存data
* 使用`save 儲存的檔名.副檔名 變數` 而且會記住儲存的變數
> 例如 `save hello.mat v` 就是儲存成binary型式給matlab,但人讀不了
* 如果想存成txt之類的可以這樣做
> `save hello.txt v -ascii` 表示存成ascii格式的txt檔
* 也能將plot的圖片存起來
> print -dpng "檔名.png"