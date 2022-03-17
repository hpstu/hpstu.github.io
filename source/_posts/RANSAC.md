---
title: RANSAC
top: false
cover: false
toc: true
mathjax: true
date: 2022-03-14 21:20:39
password:
summary: NO
tags: RANSAC
categories:
- SLAM
- RANSAC
keywords: RANSAC
description: RANSAC算法
---

## 算法流程

对于N个点构成的集合P，我们假定最少通过n个点可以拟合出正确的模型，也就是说样本集合中有N − n个噪声点。具体的执行下列操作：

1. 从数据集中随机选择一组最小样本拟合模型H，
2. 使用其他所有数据验证模型并从中取出符合阈值的内点，
3. 如果内点数量达到阈值则保存该次所有内点，否则抛弃步骤2中的内点，执行步骤4，
4. 重复以上步骤k次（即迭代k次）并更新内点（即保存步骤2中的最多内点）【可设置提前退出迭代机制，即内点足够多时退出（很难判断）】
5. 然后使用最小二乘法重新拟合模型（步骤4中的所有内点）

## 参数说明

1. 初始内点的确定

算法第1步中选择内点时，通常选择能确定模型的最少点，比如两点确定一条直线，3点确定一个平面。在求解单应性矩阵的时候，就是4个点对8个点确定一个矩阵模型。

2. 迭代次数的确定

通常来说N的值比较大，那么随机选择点的时候，点与点的组合就很多，如果不对迭代次数进行限制，运算量就会很大。因为算法第2步要用剩余点对模型进行测试，如果点的数量较多，这一步的运算量很大。包括后面的第3，4步。其实我们只要保证我们估计模型的时候使用的都是内点即可。

假设是内点的概率为$p$:

$$p= \frac{n_i}{n_i + n_o}$$

 $n_i$为内点数量,$n_o$ 为外点数量。那么我们每次计算模型使用$n$个点的情况下，选取的点集中至少有一个外点的概率就是：$1 - t^n$那么在迭代$k$次的情况下，$(1-t^n)^k$就是次迭代都至少有1个外点的概率。
那么能够得到$n$个正确的点来估计模型的概率就是：

$$P=1 - (1 - t^n)^t$$

两边取对数，就可以得到最少迭代次数:

$$k = \frac{\log(1-P)}{\log(1-t^n)}$$

这里$P$是希望通过算法得到正确模型的最少概率，$k$是关于$P$单调递增的,$k$就是在$P$确定情况下的最少迭代次数。$t$通常未知，那么我们可以采用自适应的办法，在开始的时候设置一个较大的迭代次数$k$，然后通过每次迭代中更新$t$来更新迭代次数。

## 代码示例

这里使用RANSAC算法来拟合直线:python代码如下：

```python
import numpy as np
import matplotlib.pyplot as plt
import random
import math

class ransacMatchingTest(object):

    def __init__(self, X, Y, sigma=0.2, prob=0.95):

        self.X = X
        self.Y = Y
        self.sigma = sigma
        self.prob = prob

    def runMatch(self):

        preInlier = 0
        iters = 10000
        best_a = 0
        best_b = 0
        for i in range(iters):
            # sample points
            sampleIndex = random.sample(range(self.X.shape[0]), 2)
            x1 = self.X[sampleIndex[0]]
            y1 = self.Y[sampleIndex[0]]

            x2 = self.X[sampleIndex[1]]
            y2 = self.Y[sampleIndex[1]]

            # Compute mode
            a = (y2 - y1) / (x2 - x1)
            b = y1 - a * x1

            # Get number of inlier
            inlier = 0
            for index in range(self.X.shape[0]):
                y_estimate = a * self.X[index] + b
                if abs(y_estimate - self.Y[index]) < self.sigma:
                    inlier = inlier + 1

            if inlier > preInlier:
                # Update iteration times
                iters = math.log(1-self.prob) / math.log(1 - pow(inlier / self.X.shape[0], 2))			# 这句好像没起作用
                preInlier = inlier
                best_a = a
                best_b = b
            # While inliers over half of input set then break
            if inlier > (self.X.shape[0] / 2):
                break

        return best_a, best_b

if __name__ == '__main__':
    X = np.linspace(0, 10, 50)
    Y = 4 * X + 6

    randomX = []
    randomY = []
    # Add mode noise
    for i in range(50):
        randomX.append(X[i] + random.uniform(-0.5, 0.5))
        randomY.append(Y[i] + random.uniform(-0.5, 0.5))
    # Add random noise
    for _ in range(50):
        randomX.append(random.uniform(0, 6))
        randomY.append(random.uniform(6, 30))

    RANDOMX = np.array(randomX)
    RANDOMY = np.array(randomY)

    ransac = ransacMatchingTest(RANDOMX, RANDOMY, sigma=0.2, prob=0.95).runMatch()

    preY = ransac[0] * RANDOMX + ransac[1]

    fig = plt.figure()
    ax1 = fig.add_subplot(1, 1, 1)
    ax1.scatter(RANDOMX, RANDOMY)

    ax1.plot(RANDOMX, preY)
    plt.show()


```

