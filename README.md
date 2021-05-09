# Sayu

[Sayu](https://sayu.jonbgua.com/) 是一个使用 Berlekamp-Massey 算法求解比特串线性复杂度和联结多项式的 Web App，系《密码学导论》课程的大作业之三。



## 流程图

### 主程序

todo

「载入示例」的随机数由 `Crypto.getRandomValues()` 函数提供，此函数可以获取符合密码学要求的安全的随机值，详见 [MDN Web Docs](https://developer.mozilla.org/zh-CN/docs/Web/API/Crypto/getRandomValues) 上的说明。



### Berlekamp-Massey 算法

算法逻辑参考了上课的 PPT 与 [Stack Overflow](https://stackoverflow.com/questions/50517576/berlekamp-massey-minimal-lfsr-issues)（如下图）等。

<img src="docs/img/2f2f634a739e44a8bdf4e4556d3e7886.png" alt="2f2f634a739e44a8bdf4e4556d3e7886" style="zoom: 33%;" />



## 效果预览

### 主界面

![image-20210509214422692](docs/img/image-20210509214422692.png)



### PPT 提供的样例

#### 样例 1

PPT Page 72：`1001101001101`。

![image-20210509214639802](docs/img/image-20210509214639802.png)

#### 样例 2

PPT Page 49：`001101110`。

![image-20210509214746081](docs/img/image-20210509214746081.png)



### 与 AES 的联动

点击右下角 [Berlekamp-Massey × AES](https://aes.jonbgua.com) 链接或直接访问 https://aes.jonbgua.com 。

![image-20210509215050991](docs/img/image-20210509215050991.png)

点击「载入短示例（BM友好）」，并「执行加密」。（完整示例易导致浏览器进程因长期处于运算状态而假死）

**注意：因为 AES 密钥每次都是随机生成，故复现的结果与图中结果可能有不同。**

![image-20210509215424917](docs/img/image-20210509215424917.png)

不建议点击「显示结果表格」，因单页渲染的元素过多，可能会导致浏览器假死。