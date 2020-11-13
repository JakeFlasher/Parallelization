# 凸优化第七次作业
## 总结不等式约束问题的求解方法及相互联系
### 一、解析法
假设问题Slater约束品性成立，即$\exists x\in D,Ax=b,f_i(x)<0(i=1,2\cdots,m)$，最优解存在的充要条件$\exists x^*\in\R^n$和最优对偶$\lambda^*\in\R^m,v^*\in\R^p$满足KKT方程：
$$\begin{cases}
    Ax^*=b,f_i(x^*)\leqslant 0,(i=1,2\cdots,m)\\
    \lambda_i^*\geqslant 0,\lambda_i^*f_i(x^*)=0,(i=1,2\cdots,m)\\
    \nabla f_0(x^*)+\sum\limits_{i=1}^m\lambda_i^*\nabla f_i(x^*)+A^\top v^*=0
\end{cases}$$
通过求解KKT方程即可求解不等式约束问题。
### 二、障碍法
把不等式约束问题转化成等式约束问题求解。
示性函数$I_-(u)=\begin{cases}
    0,u\leqslant 0\\
    +\infty,u>0
\end{cases}$在$u=0$不可微，所以使用近似示性函数$\hat{I}_-(u)=-\dfrac{1}{t}\log(-u)$和对数障碍函数$\phi:\R^n\rightarrow\R,\phi(x)=-\sum\limits_{i=1}^m\log[-f_i(x)]$。不等式约束问题转化为
$$\min f_0(x)+\dfrac{1}{t}\phi(x)$$
$$\text{s.t.}~~Ax=b$$
中心路径法求解的点存在对偶间隙$\dfrac{m}{t}$，即$p^*\geqslant f_0(x^*)-\dfrac{m}{t}$。

算法：
给定严格可行初始点$x_0,t^0>0,\mu>>1,\varepsilon>0,k=0$

1.中心点步骤：从$x_0$开始，求解$x^*=\arg\min f_0(x)+\phi(x),~~\text{s.t.}Ax=b$；

2.迭代：$x:=x^*$；

3.停止准则：若$\dfrac{m}{t}\leqslant\varepsilon$，退出；

4.增加$t$：$t:=\mu t$，回到步骤1，继续下一次迭代。
### 三、原对偶内点法
残差矩阵$-r_t(x,\lambda,v)=
-\begin{bmatrix}
r_\text{dual}\\
r_\text{cent}\\
r_\text{pri}
\end{bmatrix}
=\begin{bmatrix}
    \nabla f_0(x)+Df(x)^\top\lambda+A^\top v\\
    -\text{diag}(\lambda)f(x)-\dfrac{\boldsymbol{1}}{t}\\
    Ax-b
\end{bmatrix}$

算法：
初始化步骤：确定$x$满足$f_i(x)<0(i=1,2,\cdots,m),\lambda>0,\mu>1,\varepsilon_f>0,\varepsilon>0$。
重复基本步骤

1.确定$t$：$t:=\dfrac{\mu m}{\eta(x,\lambda)}=\dfrac{\mu m}{-f(x)^\top\lambda}$，当前的代理对偶间隙对应的$t$值；

2.计算原对偶搜索方向$\Delta y_\text{pd}=(\Delta x_\text{pd},\Delta \lambda_\text{pd},\Delta v_\text{pd})$满足$\\
\begin{bmatrix}
\nabla f_0(x)+\sum\limits_{i=1}^m\lambda_i\nabla^2 f_i(x) & Df(x)^\top & A^\top\\
-\text{diag}(\lambda)Df(x) & -\text{diag}[f(x)] & 0\\
A & 0 & 0
\end{bmatrix}
\begin{bmatrix}
\Delta x_\text{pd}\\
\Delta \lambda_\text{pd}\\
\Delta v_\text{pd}
\end{bmatrix}=-
\begin{bmatrix}
r_\text{dual}\\
r_\text{cent}\\
r_\text{pri}
\end{bmatrix}
$:

3.以减少$\|r_t(y+s\Delta y_\text{pd})\|_2$为目标进行直线搜索，确定步长$s>0,y:=y+s\Delta y_\text{pd}$。

停止准则：$\|r_\text{pri}\|_2\leqslant\varepsilon_f,\|r_\text{dual}\|_2\leqslant\varepsilon_f,\eta(x,\lambda)=-f(x)^\top\lambda\leqslant\varepsilon$。

### 四、对偶障碍函数和中心路径
#### 近似精度随着参数𝑡增加而不断改进
#### 当参数$t$很大时，很难用牛顿方法极小化函数$f_0+\dfrac{1}{t}\phi$因为其Hessian矩阵在靠近可行集边界时会剧烈变动
#### 求解一系列参数$t$逐渐增加（解的精度也逐渐增加）的优化问题：对每个问题应用牛顿方法求解时可以用上一个$t$值对应问题的最优解为初始点开始迭代。

### 五、原对偶内点法与障碍方法对比
#### 原对偶方法仅有一次迭代，每次迭代同时更新原对偶变量；
#### 通过将牛顿方法应用于修改的KKT方程（对数障碍中心点问题的最优性条件）确定原对偶内点法的搜索方向；
#### 原对偶方法中，原对偶迭代值不需要是可行的，一般更有效，展现超线性收敛性质。
