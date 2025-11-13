# mixtex-paper

重新仔细阅读后，我整理出以下**所有问题和质疑**：

## 一、核心方法论的根本性问题

### 1. **"Pseudo-data"生成方法完全是黑箱**
- **如何生成pseudo-formulas？** 完全没说
- **如何生成pseudo-tables？** 完全没说
- **"perturbed text"如何扰动？** 只在Figure 1看到：
  - 红色高亮：随机插入的词
  - 粉色高亮：字母打乱的拼写错误
  - 浅蓝色：随机插入的行内公式
- **但具体算法是什么？** 
  - 插入概率多少？
  - 从哪里采样这些扰动？
  - 如何保证"controlled"？

### 2. **数据比例完全未说明**
- "measured proportion"到底是多少？
- 40M真实tokens → 120M总tokens，那么：
  - 80M是pseudo的吗？
  - 还是有overlap？
  - Fake模型训练时用了多少数据？
  - Real模型训练时用了多少数据？
- **这是方法的核心参数，却完全缺失**

### 3. **"Mixed"的混合策略不清楚**
- 是每个训练样本都混合真假数据？
- 还是一半样本是真的，一半是假的？
- 还是按token级别混合？
- Figure 1只展示了一个heavily perturbed的例子，但实际训练数据的扰动程度分布是什么？

## 二、问题定义和动机的矛盾

### 4. **"Bias"的定义前后不一致**
- Abstract说：**bias = 上下文过度依赖**（$e-t$ 误识为 $e^{-t}$）
- Introduction说：**bias = 数据集特征**（小学数学语料会把求和误识为求平均）
- Section 3说：**要preserve contextual understanding**
- **到底是要减少上下文依赖还是保留上下文能力？这是矛盾的**

### 5. **"Unambiguous recognition"这个术语有问题**
- 标题说"Unambiguous Recognition Should Not Rely Solely on Real Data"
- 但LaTeX OCR本质上**不是unambiguous的**：
  - 手写公式可能模糊
  - 低分辨率图像可能模糊
  - 符号可能有歧义（0 vs O，1 vs l vs I）
- 论文真正想说的是：**clear, high-resolution printed text** 不应该被上下文override
- 但标题和abstract的表述容易误导

### 6. **核心claim缺乏证据**
- Abstract声称："when processing clear and unambiguous images, the model adheres strictly to the image rather than over-relying on contextual cues"
- **哪里证明了这一点？**
  - Table 1-3只有overall metrics
  - 没有case study展示Mixed模型如何正确识别 $e-t$ 而Real/Nougat识别错
  - 没有定量分析不同类型错误的发生率

## 三、实验设计的严重缺陷

### 7. **测试集构建完全不透明**
- "curated selection of articles authored by individuals who employ unconventional symbols"
  - 多少篇文章？
  - 多少个公式？
  - 什么样的"unconventional symbols"？
  - 谁来curate？标准是什么？
- **这个测试集无法被其他研究者复现**

### 8. **"typos" vs "typo-free"版本的意义不明**
- 论文说："allows us to assess the model's ability to accurately identify both 'typos'"
- **但这不合理**：
  - OCR模型的任务是识别图像上的内容
  - 图像上有typo，模型就应该输出typo
  - 不应该"纠正"typo
- **那为什么要准备typo-free版本？**
  - 如果是测试"模型是否会hallucinate出不存在的typo"，应该反过来：
    - 图像是typo-free的
    - 看模型是否错误地输出了typo
  - 但Table 1和2的设置似乎反了

### 9. **Table 3（handwritten）的逻辑断裂**
- 论文说："These results suggest that our method maintains robust performance in ambiguity scenarios"
- **但数据显示**：
  - Mixed = 0.092
  - Real = 0.092
  - **完全一样！**
- 这说明fake data在ambiguous场景下**没有任何作用**
- 那么Mixed的优势到底在哪里？只在clear image上有用？
- **这与"maintains robust performance in ambiguity scenarios"直接矛盾**

### 10. **Fake模型的崩溃没有被分析**
- Table 3中Fake模型完全崩溃（Edit Distance 0.257）
- 但Table 1-2中Fake模型虽然差，但还能用（0.105, 0.163）
- **为什么在handwritten数据上彻底失败？**
- 论文没有任何分析

### 11. **性能提升的统计显著性**
- Table 1: Mixed (0.075) vs Real (0.081) 
  - 差距 = 0.006
- Table 2: Mixed (0.069) vs Real (0.071)
  - 差距 = 0.002
- **没有标准差、置信区间、p-value**
- 无法判断这是真实提升还是随机波动

### 12. **与Nougat的对比不公平**
- Nougat是在arxiv数据上训练的通用模型
- MixTex是在specially curated数据上训练的
- Nougat可能不是为"unconventional symbols"设计的
- **应该说明Nougat在这个测试集上表现差是否因为out-of-distribution**

## 四、技术细节的关键缺失

### 13. **Encoder初始化 vs Decoder随机初始化的不对等**
- Encoder用预训练模型：swin-tiny-patch4-window7-224
- Decoder随机初始化
- **为什么不用预训练的GPT2权重？**
  - 论文说因为"retraining tokenizer"
  - 但可以只retrain embedding layer，保留transformer层
- 这个选择严重影响了对比的公平性

### 14. **Tokenizer的训练完全未说明**
- 在什么数据上训练？
- 词汇表大小是多少？
- 是BPE还是WordPiece还是其他？
- LaTeX有大量特殊符号（\、{、}、^、_），tokenizer如何处理？

### 15. **"smaller decoder"的claim缺乏对比**
- 论文说："MixTex's decoder is notably smaller than Nougat's"
- **但没有给出Nougat的参数量**
- GPT2 with 4 layers, hidden size 768, 12 heads
  - 这大概是多少参数？
  - 占78M总参数的多少？
- 没有对比就无法验证"smaller"的claim

### 16. **图像尺寸的处理**
- 输入图像 500×400 pixels
- **对于不同尺寸的原始图像如何处理？**
  - Resize？会导致文字模糊
  - Pad？会浪费计算
  - Crop？会丢失信息
- LaTeX文档通常是A4或letter size，500×400可能太小

### 17. **最大序列长度296 tokens的合理性**
- 对于复杂的多行公式或表格，296够吗？
- 超过296怎么办？截断？

## 五、数据集声明的问题

### 18. **"Multilingual"的claim被测试集undermined**
- Title和Abstract强调"multilingual"
- 数据集包含7种语言
- **但测试集只有英文**
- 论文承认："exclusively in English due to...scarcity"
- **那怎么证明multilingual能力？**
- 这是严重的over-claim

### 19. **$40M$ tokens的来源不清楚**
- "authentic mathematical formulas, tables, and multilingual text"
- 来自哪里？
  - arxiv？
  - 教科书？
  - 网络爬取？
- 是否有版权问题？
- 如何确保质量？

### 20. **数据处理成本的claim缺乏支撑**
- 论文说："reducing data processing costs"
- **相比什么的成本？**
- Nougat收集arxiv papers是自动化的，成本很低
- 而MixTex需要：
  - 收集40M真实tokens
  - 设计pseudo-data生成算法
  - 生成120M tokens
  - 编译LaTeX文件
  - 转换为图像
- **这真的更便宜吗？**

## 六、Related Work的问题

### 21. **对现有工作的characterization不准确**
- 说LaTeX-OCR "overall recognition accuracy tends to be suboptimal"
  - 有数据支撑吗？
  - 在什么测试集上？
- 说Pix2Text "process elements in isolation, without considering broader context"
  - 但Pix2Text用了语言模型后处理
  - 这个characterization可能不准确

### 22. **相关工作遗漏**
- 没有提到其他重要的LaTeX OCR工作：
  - TexTeller
  - InftyReader  
  - Mathpix
  - Im2LaTeX
- 对领域的survey不够全面

## 七、Writing和Presentation问题

### 23. **Figure 1的问题**
- 颜色编码（红、粉、浅蓝）在灰度打印时无法区分
- "non-highlighted portions represent authentic text"
  - 但图中几乎全是高亮的
  - 哪里是authentic的？
- 没有scale bar或font size说明

### 24. **Figure 2过于简化**
- 只是一个框图
- 没有显示：
  - Cross-attention机制
  - Position encoding
  - 如何处理variable-length input
  - Beam search或其他decoding策略

### 25. **Figure 3的增强例子太少**
- 只展示了一个样本的多个增强版本
- 没有展示不同类型的公式/表格/文本
- 无法判断增强的diversity

### 26. **术语使用不一致**
- "bias" / "underspecification" / "over-relying on context" 混用
- "pseudo" / "fake" / "synthetic" 混用
- "typos" / "spelling perturbations" / "erroneous notes" 混用

### 27. **Conclusion过于简短**
- 只有一个段落
- 没有总结主要发现
- 没有讨论limitations
- 直接跳到教育应用，但没有实验支撑

## 八、Over-claim和未支撑的声明

### 28. **音乐演奏错误识别的声明**
- Abstract和Conclusion都提到："accurate identification of erroneous notes in musical performances"
- **完全没有实验、数据或分析支撑**
- 这是严重的over-claim
- 应该删除或移到future work

### 29. **"broader applicability"的声明**
- "this method has broader applicability to various disambiguation recognition tasks"
- 只在LaTeX OCR上做了实验
- 没有证据表明可以泛化到其他任务

### 30. **"low latency"的声明**
- 多处提到"low latency"
- **完全没有推理时间数据**
- 没有与Nougat/Pix2Text的速度对比
- 无法验证

### 31. **"high accuracy"的声明**
- Abstract说："high accuracy"
- 但Table 1-3显示：
  - Edit Distance仍然有7-9%
  - 在handwritten上甚至9.2%
- 这算"high"吗？
- 没有与human performance比较

## 九、逻辑和因果关系的问题

### 32. **Section 3中的解释逻辑混乱**
- 说Real模型："decoder exhibited difficulty in effectively utilizing image features"
- 说Fake模型："displayed greater dependence on encoder-provided features"
- **这两个相反的问题怎么通过"混合"就都解决了？**
- 没有mechanistic explanation
- 听起来像post-hoc rationalization

### 33. **为什么smaller decoder能减少bias？**
- 论文声称："smaller decoder"让模型"more reliant on encoder's extracted features"
- **这个因果链条不成立**：
  - Smaller decoder = 更少的参数 = 更弱的建模能力
  - 但不一定意味着"更依赖encoder"
  - 可能只是整体能力下降
- 需要实验验证：固定decoder大小，只改变数据配比

### 34. **Data augmentation的作用机制不清楚**
- Section 4说handwritten实验验证了方法在"ambiguity scenarios"的有效性
- 但Table 3显示Mixed = Real
- **那到底是data augmentation有效，还是pseudo-data有效？**
- 这两个是不同的技术
- 没有分离开来测试

## 十、Reproducibility问题

### 35. **代码和数据可用性**
- 论文没有提到是否会开源代码
- 没有说是否会公开数据集
- 没有说是否会公开模型权重
- **这严重影响reproducibility**

### 36. **随机种子和multiple runs**
- 没有提到使用的随机种子
- 没有说是否做了multiple runs
- Table中的数字是单次运行还是平均值？

### 37. **训练时间和计算资源**
- 没有说明训练时间
- 没有说明使用的GPU类型和数量
- 对于78M参数的模型，5 epochs需要多久？

## 十一、Missing Baselines和Ablations（虽然你说不用，但缺失仍是问题）

### 38. **缺少关键的baseline**
- **No pseudo data, no augmentation**: 只用40M真实数据
- **Only augmentation, no pseudo**: 用40M真实数据+图像增强
- **Only pseudo formulas, no text perturbation**: 分别测试不同类型的noise

### 39. **缺少模型变体**
- **不同的encoder**: 用其他vision backbone
- **不同的decoder size**: 验证"smaller is better"的假设
- **预训练vs随机初始化**: decoder用预训练权重

## 总结：最critical的问题

按严重程度排序：

1. ⚠️ **方法描述不完整**（问题1-3）：无法复现
2. ⚠️ **核心claim缺乏直接证据**（问题6）：说减少了bias，但没有证明
3. ⚠️ **测试集不透明**（问题7-8）：结果不可信
4. ⚠️ **逻辑矛盾**（问题4, 9, 32）：前后说法不一致
5. ⚠️ **Over-claim**（问题18, 28-31）：说得太满，证据不足
6. ⚠️ **统计显著性缺失**（问题11）：不知道提升是否真实
7. ⚠️ **关键技术细节缺失**（问题13-17）：影响理解和复现

这篇论文**有一个有趣的idea**（用pseudo data减少contextual bias），但**执行和presentation都有严重问题**。需要大幅修改才能达到发表标准。
