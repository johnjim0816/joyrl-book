## 免模型预测 

本章开始介绍常见的两种免模型预测方法，**蒙特卡洛方法（Monte Carlo，MC）**和**时序差分方法（Temporal-Difference，TD）**。在讲解这两个方法之前，我们需要铺垫一些重要的概念，有模型（Model base）与免模型（Model free），预测（Predicton）与控制（Control）。


### 有模型与免模型

在前面的章节中，我们其实默认了一个事实，即状态转移概率是已知的，这种情况下使用的算法称之为**有模型算法（Model based RL）**，例如动态规划算法。但大部分情况下对于智能体来说，环境是未知的，这种情况下的算法就称之为**免模型算法（Model free RL）**，目前很多经典的强化学习算法都是免模型的。当然近年来出现了一些新的强化学习算法，例如PlaNet、Dreamer和World Models等。这些算法利用了神经网络和其他机器学习方法建立一个近似的环境模型，并使用规划和强化学习的方法进行决策，这些算法也都称为有模型算法。

### 预测与控制

前面提到很多经典的强化学习算法都是免模型的，换句话说在这种情况下环境的状态转移概率是未知的，这种情况下会去近似环境的状态价值函数，这其实跟状态转移概率是等价的，我们把这个过程称为**预测（Prediction）**。换句话说，预测是在马尔可夫决策过程 $<S,A,P,R,\gamma>$ 中，输入策略 $\pi$ ，然后输出状态价值函数 $V_{\pi}$ 。相对的，**控制（Control）** 则需要我们找到一个最优策略 $\pi^*$ ，并且同时输出对应的最优状态价值函数 $V^*$ 。之所以提到这两个概念，是因为很多时候我们不能一蹴而就解决好控制问题，而需要先解决预测问题，进而解决控制问题。

### 蒙特卡洛方法

蒙特卡洛方法在强化学习中是免模型预测价值函数的方式之一，本质是一种统计模拟方法，它的发展得益于电子计算机的发明。假设我们需要计算一个不规则图形的面积，这种情况下是很难通过规则或者积分的方式得到结果的。而蒙特卡洛基于这样的想法：比如我们有一袋豆子，把豆子均匀地在一定范围内朝这个图形上撒，撒到足够多的数量时数一下这个图形中有多少颗豆子，这个豆子的数目就是图形的面积。当豆子越小撒的越多的时候，结果就越精确。此时我们借助计算机程序可以生成大量均匀分布坐标点，然后统计出图形内的点数，通过它们占总点数的比例和坐标点生成范围的面积就可以求出图形面积。

那么在强化学习中蒙特卡洛方法是怎么预测状态价值函数 $V(s)$ 的呢？我们回顾 $V(s)$ 的定义公式，如下：

$$
\begin{aligned}
V_\pi(s) &=\mathbb{E}_{\pi}[R_{t+1}+\gamma R_{t+2}+\gamma^2 R_{t+3} + \cdots |S_t=s ] \\
&=\mathbb{E}_{\pi}[G_t|S_t=s ] 
\end{aligned}
$$

由于这里的奖励函数 $R$ 是环境给到智能体的，$\gamma$ 则是设置的参数，因此其实 $V(s)$ 是可以手动计算出来的。假设机器人需要在一个 $m \times n$ 的网格中解决最短路径的问题，即从起点到终点走哪条路最短。在这种情况下，我们如果想要知道某个网格即某个状态代表的价值，那么我们可以从这个网格或状态出发，搜集到其他所有网格之间可能的轨迹。出于简化计算的考虑，我们考虑一个 $2 \times 2$的网格，如图 4.1 所示，我们用机器人的位置表示不同的状态，即$s_1,s_2,s_3,s_4$，规定机器人智能体向右或者向下走，分别用 $a_1$ 和 $a_2$ 。起点为坐标$(0,0)$，即初始状态为$S_0=s_1$，终点为右下角的网格 $s_4$，我们设置机器人每走一步接收到的奖励为 $-1$， 折扣因子 $\gamma=0.9$ 。

<div align=center>
<img width="200" src="./figs/simple_maze.png"/>
</div>
<div align=center>图 4.1 迷你网格示例</div>

那么如果要计算 $s_1$ 的价值 $V(s_1)$，我们先搜集到其他状态的所有轨迹，比如到$s_2$的轨迹是$\{s_1,a_1,r(s_1,a_1),s_2\}$，即从 $s_1$ 开始，执行动作$a_1$即向右之后到达 $s_2$。这条轨迹记为 $\tau_1$ ，对应的回报为 $G_{\tau_1} = r(s_1,a_1) = -1$，**注意这里的下标不代表时步**。同理，到 $s_3$ 的轨迹 $\tau_2$ 为 $\{s_1,a_2,r(s_1,a_2),s_3\}$，对应的回报为 $G_{\tau_2} = r(s_1,a_2)=-1$。到状态 $s_4$ 的轨迹有两条，分别是 $\tau_3 = \{s_1,a_1,r(s_1,a_1),s_2,a_2,r(s_2,a_2),s_4\}$ 和 $\tau_4 = \{s_1,a_2,r(s_1,a_2),s_3,a_1,r(s_3,a_1),s_4\}$，对应的回报分别为 $G_{\tau_3} = r(s_1,a_1) + \gamma r(s_2,a_2)= (-1) + 0.9 * (-1) = -1.9$, $G_{\tau_4} = r(s_1,a_2) + \gamma r(s_3,a_1)= (-1) + 0.9 * (-1) = -1.9$。这样我们就能得到：

$$
V(s_1) = (G_{\tau_1}+ G_{\tau_2} + G_{\tau_3} + G_{\tau_4} )/4 = 5.8 / 4 = -1.45
$$

同样的可以计算出其他状态对应的价值函数，$V(s_2)=V(s_3)= -1$，$s_4$ 由于是终止状态，因此 $V(s_4)=0$，这样就得到了所有状态的价值函数分布，如图 4.2 所示。

<div align=center>
<img width="100" src="./figs/simple_maze_2.png"/>
</div>
<div align=center>图 4.2 价值函数网格分布</div>

对于状态数量更多的情况，直接按照上述的方式去计算价值函数是不现实的。蒙特卡洛方法的思路是我们可以采样大量的轨迹，对于每个轨迹计算对应状态的回报然后取平均近似，称之为经验平均回报（empirical mean return）。根据大数定律，只要采样的轨迹数量足够多，计算出的经验平均回报就能趋近于实际的状态价值函数。当然，蒙特卡洛方法有一定的局限性，即只适用于有终止状态的马尔可夫决策过程。

蒙特卡洛方法主要分成两种算法，一种是首次访问蒙特卡洛（First-visit Monte Carlo，FVMC）方法，另外一种是每次访问蒙特卡洛（Every-visit Monte Carlo，EVMC）方法。FVMC方法主要包含两个步骤，首先是产生一个回合的完整轨迹，然后遍历轨迹计算每个状态的回报。注意，只在第一次遍历到某个状态时会记录并计算对应的回报，对应伪代码如图所示。而在 EVMC 方法中不会忽略统一状态的多个回报，在前面的示例中，我们计算价值函数的方式就是 Every-visit ，比如对于状态 $s_4$，我们考虑了所有轨迹即 $G_{\tau_3}$ 和 $G_{\tau_4}$ 的回报，而在 FVMC 我们只会记录首次遍历的回报，即 $G_{\tau_3}$ 和 $G_{\tau_4}$ 其中的一个，具体取决于遍历到 $s_4$ 时对应的轨迹是哪一条。 


<div align=center>
<img width="400" src="./figs/fvmc_pseu.png"/>
</div>
<div align=center>图 4.3 首次访问蒙特卡洛算法伪代码</div>

实际上无论是 FVMC 还是 EVMC 在实际更新价值函数的时候是不会像伪代码中体现的那样 $V\left(S_t\right) \leftarrow \operatorname{average}\left(\operatorname{Returns}\left(S_t\right)\right)$，每次计算到新的回报 $ G_t = \operatorname{average}\left(\operatorname{Returns}\left(S_t\right)\right)$ 直接就赋值到已有的价值函数中，而是以一种递进更新的方式进行的，如下：

$$
新的估计值 \leftarrow 旧的估计值 + 步长 *（目标值-旧的估计值）
$$

这样的好处就是不会因为个别不好的样本而导致更新的急剧变化，从而导致学习得不稳定，这种模式在今天的深度学习中普遍可见，这里的步长就是深度学习中的学习率。

对应到蒙特卡洛方法中，更新公式可表示为：

$$
V(s_t) \leftarrow V(s_t) + \alpha[G_t- V(s_{t})]
$$
其中 $\alpha$ 表示学习率，$G_t- V(S_{t+1})$为目标值与估计值之间的误差（error）。

此外，FVMC 是一种基于回合的增量式方法，具有无偏性和收敛快的优点，但是在状态空间较大的情况下，依然需要训练很多个回合才能达到稳定的结果。而 EVMC 则是更为精确的预测方法，但是计算的成本相对也更高。

## 时序差分方法

时序差分方法是一种基于经验的动态规划方法，它结合了蒙特卡洛和动态规划的思想。最简单的时序差分可以表示为：

$$
V(s_t) \leftarrow V(s_t) + \alpha[r_{t+1}+\gamma V(s_{t+1})- V(s_{t})]
$$

这种算法一般称为一步时序差分（one-step TD），即 $TD(0)$。可以看到，在这个更新过程中使用了当前奖励和后继状态的估计，这是类似于蒙特卡罗方法的；但同时也利用了贝尔曼方程的思想，将下一状态的值函数作为现有状态值函数的一部分估计来更新现有状态的值函数。此外，时序差分还结合了自举（Bootstrap）的思想，即未来状态的价值 $\max_{a'\in A}Q(s', a')$(对于状态值$v$而言，$\max _{a \in A} Q(s, a)$相当于 $\sum {s', r} p(s', r|s,a)[r + \gamma v(s')]$ ) 通过现有的估计 $\max _{a\in A} Q(s, a)$ 进行计算，即使用一个状态的估计值来更新该状态的估计值，没有再利用后续状态信息的计算方法。这种方法可以将问题分解成只涉及一步的预测，从而简化计算。这也是 基于模型的方法 和 TD 性质结合的重要来源。