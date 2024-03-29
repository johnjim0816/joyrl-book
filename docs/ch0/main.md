# 前言

## 内容提要

$\qquad$ 继《$\text{EasyRL}$：强化学习教程》（俗称“蘑菇书”）之后，本书是为强化学习读者专门打造的一本深入实践的全新教程。全书大部分内容基于三位作者的实践经验，旨在帮助读者快速入门强化学习的代码实战，并辅以一套开源代码框架 $\text{JoyRL}$，便于读者适应业界应用研究风格的代码。

$\qquad$ 与“蘑菇书”不同，本书针对强化学习核心理论进行提炼并串联知识点，重视强化学习代码实践的指导，尽可能还原原论文的主要思想，而不是对于理论的详细讲解。因此，“蘑菇书”适合“细嚼慢咽”的读者阅读，而本书则适合具有一定编程基础且希望快速进入实践应用阶段的读者阅读。

## 前言

$\qquad$ 在几年前，我们 “‘磨菇书’三剑客”（笔者、杨毅远、王琦） 已经在 $\text{Github}$ 上发布过线上教程 $\text{EasyRL}$，填补了强化学习国内相关资料较少的空缺。特此再次衷心感谢李宏毅、周博磊、李科浇$3$位老师的授权与开源奉献精神，没有他们的鼓励与无私奉献，就没有深受广大强化学习初学者喜爱的“蘑菇书”。并且受到广大读者的鼓励，我们不断优化教程，以期帮助读者更好、更愉快地入门强化学习。

$\qquad$ 时光荏苒，笔者已在业界深耕多年有余， 对于强化学习实践有了更加深入的认识，并在理论与实践的结合也有了一些心得。与此同时，也发现了读者将理论应用到实践的过程中似乎遇到一定的困难。首先，很多已经有人工智能知识基础的读者只是想应用强化学习来做一些其他方面的交叉研究，但由于强化学习理论错综复杂，对于这样的读者来说很难在短时间内快速把握应用重点，并且容易陷入一些与实践关系不大的小知识点陷阱中。其次，有一些读者很难将强化学习中的公式和实际代码相对应起来，例如策略函数的设计等等，并且对于算法的各种超参数的调整也不知从何下手。

$\qquad$ 虽然市面上已经有一些关于强化学习实践的教程，但这些教程往往又过于偏重实践，忽视了理论与实践之间的把握与平衡。此外，相关的实践也往往偏向于一些简单的实验和算法，涵盖的内容不够全面。鉴于这些现状，笔者读者对强化学习知识有一个更深入全面的了解，这也是本书编写的初衷。

$\qquad$ 本书的内容主要基于我们三位作者的理论知识与实践经验，并伴有一些原创内容，例如针对策略梯度算法的两种不同的推导版本，以便让读者从不同的角度更好地理解相关知识。全书始终贯穿强化学习实践中的一些核心问题，比如优化值的估计、解决探索与利用等问题。全书的内容结构编排合理，例如从传统强化学习到深度强化学习过渡的内容中，增加对深度学习基础的总结归纳，并对一些应用十分泛广泛的强化学习算法比如 $\text{DQN}$、$\text{DDPG}$ 以及 $\text{PPO}$ 等算法进行强调，读者可选择性阅读。本书除了给出一些简单的配套代码之外，还提供了一套 $\text{JoyRL}$ 开源框架，以及更多复杂环境的实验示例，想要深入了解的读者可自行研究。

$\qquad$ 本书由开源组织 $\text{Datawhale}$ 的成员采用开源协作的方式完成，历时 $1$ 年有余，主要参与者包括笔者、王琦和杨毅远。此外，十分感谢谌蕊（清华大学）、丁立（上海交通大学）、郭事成（安徽工业大学）、孙成超（浙江理工大学）、刘二龙（南京大学）、潘笃驿（西安电子科技大学）、邱雯（日本北见工业大学）、管媛媛（西南交通大学）、王耀晨（南京邮电大学）等同学参与 $\text{JoyRL}$ 开源框架的共建，以及感谢林诗颖同学在本书编写过程中的友情帮助。在本书写作和出版过程中，人民邮电出版社提供了很多出版的专业意见和支持，在此特向信息技术出版社社长陈冀康老师和本书的责任编辑郭媛老师致谢。

$\qquad$ 由于笔者水平有限，书中难免有疏漏和不妥之处，还望读者批评指正。




