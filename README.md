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

