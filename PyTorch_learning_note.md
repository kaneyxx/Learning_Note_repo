This note is based on PyTorch official tutorial. Just leave some notes for myself to search it quickly. [PyTorch Official Tutorial](https://pytorch.org/tutorials/)

## Tensors 張量
> 張量類似於Numpy中的ndarray, 而張量可以被用於GPU加速運算使用  
> 
建立一個初始化(空)矩陣
```
x = torch.empty(5, 3)
```
建立一個隨機矩陣
```
x = torch.rand(5, 3)
```
建立一個零矩陣, 並指定資料型態(其他操作也都能用)
```
x = torch.zeros(5, 3, dtype=torch.long)
```
直接從資料建立
```
x = torch.tensor([5.5, 3])
```
藉由已存在的矩陣來建立
```
# new_* 方式是取其尺寸

x = x.new_ones(5, 3, dtype=torch.double)

# 重新指定資料型態

x = torch.randn_like(x, dtype=torch.float)
```
取得張量的size
```
x.size()   # 輸出為torch.Size([m, n])格式, 支持tuple操作
```

> 支持各種四則運算, 以加法為例  
> 
簡單的直接相加
```
y = torch.rand(5, 3)
print(x + y)
```
利用函數相加, 並能指定結果給變數
```
torch.add(x, y, out=result)
print(result)
```
in-place的加法, ==特別注意, PyTorch中所有_( )的inplace操作都會改變原值要小心==
```
# 將x加給y, y會變
y.add_(x)
print(y)
```

同樣能用numpy中的選取方式
```
print(x[:, 1])
```
如果要進行resize, reshape動作則是使用torch.view
```
x = torch.randn(4, 4)
y = x.view(16)
z = x.view(-1, 8)  # -1即是由機器自己幫你運算
print(x.size(), y.size(), z.size())
```
如果張量中僅有一個值, 可以用.item()來快速取得該值(型態為一python number)
```
x = torch.randn(1)
print(x)
print(x.item())
```
* 更多相關的操作細節在這官方文檔中 [Tensors](https://pytorch.org/docs/stable/torch.html)  

> 變換numpy/torch tensor/GPU tensor  

用torch.from_numpy( )將numpy array轉成torch tensor
```
a = np.ones(5)
b = torch.from_numpy(a)
```
直接創建或是指定給GPU使用
```
if torch.cuda.is_available():
    device = torch.device("cuda")                # 指定CUDA裝置
    y = torch.ones_like(x, device=device)  # 直接創建一個GPU張量
    x = x.to(device)                                             # 指定給該裝置或是用.cuda()
```

## 