# MixTex Challenge Cases Design
## 100 Test Cases (50 Chinese + 50 English)

**Purpose**: Test MixTex's ability to avoid contextual over-reliance and accurately recognize visual content.

**Structure**: Each case includes:
- **Context**: Surrounding text that might mislead the model
- **Image Content**: The actual LaTeX content to be recognized
- **Expected Output**: Correct recognition
- **Trap**: What a context-biased model might incorrectly output

---

## Category A: Contextual Traps (30 cases: 15 Chinese + 15 English)

These cases test whether the model can resist being misled by contextual cues.

### Chinese Contextual Traps (A-CN-01 to A-CN-15)

**A-CN-01: Exponential vs Subtraction**
- Context: "在指数衰减模型中，我们研究函数随时间的变化。考虑以下表达式："
- Image: `定义 $z = e - t$，其中 $e$ 是某个常数`
- Expected: `z = e - t`
- Trap: `z = e^{-t}` (exponential decay context)

**A-CN-02: Sum vs Formula**
- Context: "在小学数学中，我们学习了等差数列求和公式。例如："
- Image: `计算 $\sum_{i=1}^{100} i$ 的值`
- Expected: `\sum_{i=1}^{100} i`
- Trap: `\frac{100 \times 101}{2}` (auto-computed)

**A-CN-03: Force Equation**
- Context: "在牛顿第二定律中，力与质量和加速度的关系非常重要。定义："
- Image: `设 $F = m - a$ 为某个量`
- Expected: `F = m - a`
- Trap: `F = ma` (physics knowledge)

**A-CN-04: Derivative Notation**
- Context: "研究函数的导数时，我们经常使用莱布尼茨记号。"
- Image: `令 $dy/dx = 0$ 表示常数函数`
- Expected: `dy/dx = 0`
- Trap: `\frac{dy}{dx} = 0` (auto-formatted)

**A-CN-05: Integration Bounds**
- Context: "在定积分的计算中，上下限的选择很重要。"
- Image: `$\int_{0}^{1} f(x) dx$，这里上限是 $1$`
- Expected: `\int_{0}^{1} f(x) dx`
- Trap: `\int_{0}^{\infty} f(x) dx` (if context mentions "improper integral")

**A-CN-06: Division vs Fraction**
- Context: "在代数运算中，除法和分数是等价的。"
- Image: `计算 $a / b$ 的值`
- Expected: `a / b`
- Trap: `\frac{a}{b}` (auto-converted to fraction)

**A-CN-07: Logarithm Base**
- Context: "自然对数在微积分中应用广泛，通常写作 ln。"
- Image: `定义 $\log(x) = y$`
- Expected: `\log(x) = y`
- Trap: `\ln(x) = y` (context suggests natural log)

**A-CN-08: Matrix Multiplication**
- Context: "矩阵乘法不满足交换律，即 AB ≠ BA。"
- Image: `设 $C = A + B$`
- Expected: `C = A + B`
- Trap: `C = AB` (context about multiplication)

**A-CN-09: Dot vs Times**
- Context: "在向量运算中，点乘和叉乘是两种重要运算。"
- Image: `计算 $\mathbf{a} \times \mathbf{b}$`
- Expected: `\mathbf{a} \times \mathbf{b}`
- Trap: `\mathbf{a} \cdot \mathbf{b}` (context mentions dot product)

**A-CN-10: Summation Index**
- Context: "求和符号中，索引变量通常从 1 开始。"
- Image: `$\sum_{i=0}^{n} a_i$`
- Expected: `\sum_{i=0}^{n} a_i`
- Trap: `\sum_{i=1}^{n} a_i` (context bias)

**A-CN-11: Inequality Direction**
- Context: "在优化问题中，我们寻找函数的最小值，要求导数小于零。"
- Image: `条件：$f'(x) > 0$`
- Expected: `f'(x) > 0`
- Trap: `f'(x) < 0` (context suggests minimization)

**A-CN-12: Binomial Coefficient**
- Context: "组合数 C(n,k) 也可以写作二项式系数形式。"
- Image: `计算 $C(n, k)$`
- Expected: `C(n, k)`
- Trap: `\binom{n}{k}` (auto-converted)

**A-CN-13: Limit Notation**
- Context: "在极限理论中，我们研究函数在无穷远处的行为。"
- Image: `$\lim_{x \to 0} f(x)$`
- Expected: `\lim_{x \to 0} f(x)`
- Trap: `\lim_{x \to \infty} f(x)` (context about infinity)

**A-CN-14: Square vs Cube**
- Context: "立方体的体积公式是边长的三次方。"
- Image: `计算 $V = a^2$`
- Expected: `V = a^2`
- Trap: `V = a^3` (context about cube)

**A-CN-15: Prime Notation**
- Context: "在微分几何中，撇号常表示导数。"
- Image: `设 $y' = 0$ 为某个条件`
- Expected: `y' = 0`
- Trap: `y'' = 0` (second derivative in context)

---

### English Contextual Traps (A-EN-01 to A-EN-15)

**A-EN-01: Exponential vs Subtraction**
- Context: "In the study of exponential functions, we often encounter decay processes."
- Image: `Let $y = e - x$ where $e$ is a constant`
- Expected: `y = e - x`
- Trap: `y = e^{-x}`

**A-EN-02: Sum vs Average**
- Context: "When calculating the average of consecutive integers, we use the formula."
- Image: `Find $\sum_{k=1}^{n} k$`
- Expected: `\sum_{k=1}^{n} k`
- Trap: `\frac{n(n+1)}{2}`

**A-EN-03: Energy Equation**
- Context: "Einstein's famous mass-energy equivalence relates mass and energy."
- Image: `Define $E = m + c^2$`
- Expected: `E = m + c^2`
- Trap: `E = mc^2`

**A-EN-04: Velocity Formula**
- Context: "The relationship between velocity, distance, and time is fundamental."
- Image: `Set $v = d - t$`
- Expected: `v = d - t`
- Trap: `v = d/t`

**A-EN-05: Pythagorean Theorem**
- Context: "In right triangles, the Pythagorean theorem states the relationship."
- Image: `Given $a^2 = b^2 + c^2$`
- Expected: `a^2 = b^2 + c^2`
- Trap: `a^2 + b^2 = c^2`

**A-EN-06: Quadratic Formula**
- Context: "The solutions to quadratic equations are given by the quadratic formula."
- Image: `Solve using $x = (-b + \sqrt{b^2 - 4ac}) / 2a$`
- Expected: `x = (-b + \sqrt{b^2 - 4ac}) / 2a`
- Trap: `x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}` (completed formula)

**A-EN-07: Derivative of Sine**
- Context: "The derivative of sine is cosine, a fundamental trigonometric identity."
- Image: `Calculate $\frac{d}{dx} \cos(x)$`
- Expected: `\frac{d}{dx} \cos(x)`
- Trap: `\frac{d}{dx} \sin(x)` (context swapped)

**A-EN-08: Chain Rule**
- Context: "The chain rule is used for composite functions."
- Image: `Apply the product rule to $\frac{d}{dx}[f(x)g(x)]$`
- Expected: `\frac{d}{dx}[f(x)g(x)]`
- Trap: `\frac{d}{dx}[f(g(x))]` (context confusion)

**A-EN-09: Logarithm Properties**
- Context: "The logarithm of a product equals the sum of logarithms."
- Image: `Simplify $\log(ab)$`
- Expected: `\log(ab)`
- Trap: `\log(a) + \log(b)` (auto-simplified)

**A-EN-10: Probability Addition**
- Context: "For independent events, probabilities multiply."
- Image: `Calculate $P(A) + P(B)$`
- Expected: `P(A) + P(B)`
- Trap: `P(A) \times P(B)` (context about multiplication)

**A-EN-11: Complex Conjugate**
- Context: "The complex conjugate of a + bi is a - bi."
- Image: `Find the conjugate of $x + yi$`
- Expected: `x + yi`
- Trap: `x - yi` (auto-conjugated)

**A-EN-12: Vector Magnitude**
- Context: "The magnitude of a vector is calculated using the Euclidean norm."
- Image: `Let $\|\mathbf{v}\| = x + y$`
- Expected: `\|\mathbf{v}\| = x + y`
- Trap: `\|\mathbf{v}\| = \sqrt{x^2 + y^2}`

**A-EN-13: Factorial Notation**
- Context: "Factorials grow very rapidly and are defined recursively."
- Image: `Compute $n! / (n-1)!$`
- Expected: `n! / (n-1)!`
- Trap: `n` (auto-simplified)

**A-EN-14: Trigonometric Identity**
- Context: "The fundamental Pythagorean identity relates sine and cosine."
- Image: `Use the identity $\sin^2(x) = 1 - \cos^2(x)$`
- Expected: `\sin^2(x) = 1 - \cos^2(x)`
- Trap: `\sin^2(x) + \cos^2(x) = 1`

**A-EN-15: Set Operations**
- Context: "The union of two sets combines all elements."
- Image: `Find $A \cap B$`
- Expected: `A \cap B`
- Trap: `A \cup B` (context about union)

---

## Category B: Unconventional Notation (30 cases: 15 Chinese + 15 English)

These cases test recognition of non-standard but valid notation.

### Chinese Unconventional Notation (B-CN-01 to B-CN-15)

**B-CN-01: Custom Differential**
- Context: "在某些物理文献中，作者使用特殊的微分记号。"
- Image: `$\mathrm{d}\tau$ 表示固有时微分`
- Expected: `\mathrm{d}\tau`
- Common: Most models trained on `dt`

**B-CN-02: Circle Dot Operator**
- Context: "定义一个新的运算符用于特殊乘法。"
- Image: `$a \odot b = ab + 1$`
- Expected: `a \odot b`
- Common: Might expect `\otimes` or `\cdot`

**B-CN-03: Double Bracket**
- Context: "使用双括号表示向量的内积。"
- Image: `$\langle\langle x, y \rangle\rangle$`
- Expected: `\langle\langle x, y \rangle\rangle`
- Common: Single bracket `\langle x, y \rangle`

**B-CN-04: Blackboard Bold for Indicator**
- Context: "指示函数的记号在不同文献中有所不同。"
- Image: `$\mathbb{1}_{A}(x)$ 为集合 $A$ 的指示函数`
- Expected: `\mathbb{1}_{A}(x)`
- Common: `\mathbf{1}` or `I_{A}`

**B-CN-05: Star for Convolution**
- Context: "卷积运算可以用星号表示。"
- Image: `$(f * g)(t) = \int f(\tau)g(t-\tau) \mathrm{d}\tau$`
- Expected: `f * g`
- Common: `f \ast g` with \ast

**B-CN-06: Parallel Symbol**
- Context: "在电路分析中，并联用特殊符号。"
- Image: `$R_1 \| R_2$`
- Expected: `R_1 \| R_2`
- Common: `R_1 \parallel R_2`

**B-CN-07: Custom Sum Symbol**
- Context: "某些作者使用大写希腊字母表示求和。"
- Image: `$\Sigma_{i=1}^{n} x_i$`
- Expected: `\Sigma` (capital Greek)
- Common: `\sum` (sum symbol)

**B-CN-08: Triangle Operator**
- Context: "定义三角算子用于特殊运算。"
- Image: `$a \triangle b$`
- Expected: `a \triangle b`
- Common: Might expect `\Delta`

**B-CN-09: Custom Integral**
- Context: "路径积分用特殊记号表示。"
- Image: `$\oint_C f \, \mathrm{d}s$`
- Expected: `\oint` (closed contour)
- Common: Regular `\int`

**B-CN-10: Overline for Mean**
- Context: "平均值可以用上划线表示。"
- Image: `$\overline{x}$ 表示样本均值`
- Expected: `\overline{x}`
- Common: `\bar{x}` (shorter bar)

**B-CN-11: Custom Limit Notation**
- Context: "极限的另一种写法。"
- Image: `$f(x) \to L$ as $x \to a$`
- Expected: `\to` (arrow)
- Common: `\lim_{x \to a}` format

**B-CN-12: Boxed Plus**
- Context: "模加法用方框加号表示。"
- Image: `$a \boxplus b$`
- Expected: `a \boxplus b`
- Common: Regular `+`

**B-CN-13: Custom Derivative**
- Context: "某些作者用点表示时间导数。"
- Image: `$\dot{x}$ 表示 $x$ 对时间的导数`
- Expected: `\dot{x}`
- Common: `dx/dt` or `x'`

**B-CN-14: Floor/Ceiling Brackets**
- Context: "取整函数的记号。"
- Image: `$\lfloor x \rfloor$ 和 $\lceil x \rceil$`
- Expected: `\lfloor x \rfloor` and `\lceil x \rceil`
- Common: Might use `[x]`

**B-CN-15: Custom Set Notation**
- Context: "集合的构造记号。"
- Image: `$S = \{x \mid x > 0\}$`
- Expected: `\mid` (vertical bar)
- Common: `:` or `|` without backslash

---

### English Unconventional Notation (B-EN-01 to B-EN-15)

**B-EN-01: Alternative Differential**
- Context: "In differential geometry, proper time uses special notation."
- Image: `The differential $\mathrm{d}\sigma$ appears in the metric`
- Expected: `\mathrm{d}\sigma`
- Common: `dt` or `ds`

**B-EN-02: Hadamard Product**
- Context: "Element-wise multiplication uses the circle operator."
- Image: `$A \circ B$ denotes the Hadamard product`
- Expected: `A \circ B`
- Common: `A \odot B` or `A * B`

**B-EN-03: Double Bar for Norm**
- Context: "Some authors use double bars for norms."
- Image: `$||x||_2$ is the Euclidean norm`
- Expected: `||x||_2` (double pipe)
- Common: `\|x\|_2` with backslash

**B-EN-04: Custom Expectation**
- Context: "Expected value notation varies."
- Image: `$\mathbb{E}[X]$ denotes expectation`
- Expected: `\mathbb{E}[X]`
- Common: `E(X)` with parentheses

**B-EN-05: Alternative Infinity**
- Context: "Infinity symbol variations."
- Image: `As $n \to +\infty$`
- Expected: `+\infty` (with plus)
- Common: Just `\infty`

**B-EN-06: Custom Gradient**
- Context: "The gradient operator in tensor notation."
- Image: `$\nabla_\mu$ is the covariant derivative`
- Expected: `\nabla_\mu` (subscript)
- Common: `\nabla` without subscript

**B-EN-07: Alternative Implies**
- Context: "Logical implication symbols."
- Image: `$P \implies Q$`
- Expected: `\implies`
- Common: `\Rightarrow`

**B-EN-08: Custom Transpose**
- Context: "Matrix transpose notation."
- Image: `$A^T$ denotes transpose`
- Expected: `A^T`
- Common: `A^\top` or `A'`

**B-EN-09: Alternative Approximately**
- Context: "Approximation symbols vary."
- Image: `$\pi \sim 3.14$`
- Expected: `\sim`
- Common: `\approx`

**B-EN-10: Custom Proportional**
- Context: "Proportionality symbol."
- Image: `$F \propto r^2$`
- Expected: `\propto`
- Common: Might write out "proportional to"

**B-EN-11: Alternative Subset**
- Context: "Subset notation variants."
- Image: `$A \subset B$ (strict subset)`
- Expected: `\subset`
- Common: `\subseteq` (subset or equal)

**B-EN-12: Custom Angle Brackets**
- Context: "Quantum mechanics bra-ket notation."
- Image: `$\langle \psi | \hat{H} | \psi \rangle$`
- Expected: `\langle \psi | \hat{H} | \psi \rangle`
- Common: Simpler brackets

**B-EN-13: Alternative Dot Product**
- Context: "Inner product notation."
- Image: `$(x, y)$ denotes inner product`
- Expected: `(x, y)` (parentheses)
- Common: `\langle x, y \rangle` or `x \cdot y`

**B-EN-14: Custom Partial Derivative**
- Context: "Partial derivative variants."
- Image: `$\partial_x f$ means partial derivative`
- Expected: `\partial_x f` (subscript)
- Common: `\frac{\partial f}{\partial x}`

**B-EN-15: Alternative Intersection**
- Context: "Set intersection symbol."
- Image: `$A \bigcap_{i=1}^n B_i$`
- Expected: `\bigcap` (large cap)
- Common: `\cap` (regular size)

---

## Category C: Ambiguous Symbols (20 cases: 10 Chinese + 10 English)

These cases test recognition when symbols look similar.

### Chinese Ambiguous Symbols (C-CN-01 to C-CN-10)

**C-CN-01: Zero vs O**
- Context: "定义集合运算。"
- Image: `$O(n)$ 表示复杂度`
- Expected: `O` (letter O, not zero)
- Ambiguous: Could be misread as `0`

**C-CN-02: One vs l vs I**
- Context: "线性代数中的单位矩阵。"
- Image: `$I$ 是单位矩阵`
- Expected: `I` (capital i)
- Ambiguous: Could be `1` or `l`

**C-CN-03: Multiplication vs x**
- Context: "变量命名。"
- Image: `设 $x$ 为未知数`
- Expected: `x` (variable)
- Ambiguous: Could be `\times`

**C-CN-04: Greek alpha vs a**
- Context: "参数设定。"
- Image: `令 $\alpha = 0.05$`
- Expected: `\alpha` (Greek)
- Ambiguous: Could be `a`

**C-CN-05: Capital Sigma vs Greek**
- Context: "求和记号。"
- Image: `$\Sigma x_i$ 表示总和`
- Expected: `\Sigma` (Greek letter)
- Ambiguous: Could be `\sum`

**C-CN-06: Vertical bar vs l**
- Context: "条件概率。"
- Image: `$P(A | B)$`
- Expected: `|` (vertical bar)
- Ambiguous: Could be `l` (letter)

**C-CN-07: Minus vs dash**
- Context: "减法运算。"
- Image: `$a - b$`
- Expected: `-` (minus)
- Ambiguous: Could be dash or underscore

**C-CN-08: Apostrophe vs prime**
- Context: "导数记号。"
- Image: `$f'(x)$`
- Expected: `'` (prime)
- Ambiguous: Might be different quote marks

**C-CN-09: Comma vs period**
- Context: "坐标表示。"
- Image: `点 $(1, 2)$`
- Expected: `,` (comma)
- Ambiguous: In some locales, `.` is decimal separator

**C-CN-10: Colon vs division**
- Context: "比例关系。"
- Image: `$a : b = 2 : 3$`
- Expected: `:` (colon)
- Ambiguous: Could be division `/`

---

### English Ambiguous Symbols (C-EN-01 to C-EN-10)

**C-EN-01: Zero vs O in Big-O**
- Context: "Algorithm complexity analysis."
- Image: `The time complexity is $O(n \log n)$`
- Expected: `O` (letter)
- Ambiguous: Not `0` (zero)

**C-EN-02: Identity Matrix I**
- Context: "Matrix operations."
- Image: `Let $I_n$ be the $n \times n$ identity matrix`
- Expected: `I` (capital i)
- Ambiguous: Not `1` or `l`

**C-EN-03: Variable x vs times**
- Context: "Solving equations."
- Image: `Find $x$ such that $x = 5$`
- Expected: `x` (variable)
- Ambiguous: Not `\times`

**C-EN-04: Greek beta vs B**
- Context: "Statistical parameters."
- Image: `The coefficient $\beta$ is estimated`
- Expected: `\beta` (Greek)
- Ambiguous: Not `B`

**C-EN-05: Greek nu vs v**
- Context: "Physics notation."
- Image: `The frequency $\nu$ is measured in Hz`
- Expected: `\nu` (Greek nu)
- Ambiguous: Not `v` (vee)

**C-EN-06: Greek rho vs p**
- Context: "Density calculation."
- Image: `The density $\rho = m/V$`
- Expected: `\rho` (Greek)
- Ambiguous: Not `p`

**C-EN-07: Greek chi vs X**
- Context: "Chi-squared test."
- Image: `The $\chi^2$ statistic is computed`
- Expected: `\chi` (Greek)
- Ambiguous: Not `X`

**C-EN-08: Lowercase L vs 1 vs I**
- Context: "Length measurement."
- Image: `The length $l = 10$ meters`
- Expected: `l` (lowercase L)
- Ambiguous: Not `1` or `I`

**C-EN-09: Capital K vs k**
- Context: "Constants in equations."
- Image: `Where $K$ is a constant and $k = 2$`
- Expected: Both `K` and `k` correctly
- Ambiguous: Case sensitivity

**C-EN-10: Parentheses in subscript**
- Context: "Indexed variables."
- Image: `$x_{(i)}$ denotes the $i$-th order statistic`
- Expected: `x_{(i)}` with parentheses
- Ambiguous: Might drop parentheses

---

## Category D: Complex Formulas (20 cases: 10 Chinese + 10 English)

These cases test handling of long, nested, or multi-line formulas.

### Chinese Complex Formulas (D-CN-01 to D-CN-10)

**D-CN-01: Deeply Nested Fraction**
- Context: "连分数的表示。"
- Image:
```latex
$\frac{1}{a + \frac{1}{b + \frac{1}{c + \frac{1}{d}}}}$
```
- Expected: All four levels correctly nested
- Challenge: Deep nesting

**D-CN-02: Multi-line Aligned Equations**
- Context: "方程组的求解。"
- Image:
```latex
\begin{aligned}
x + 2y + 3z &= 14 \\
2x - y + z &= 5 \\
3x + y - 2z &= -1
\end{aligned}
```
- Expected: All alignment and line breaks preserved
- Challenge: Multi-line structure

**D-CN-03: Large Matrix**
- Context: "矩阵运算。"
- Image:
```latex
$\begin{pmatrix}
1 & 2 & 3 & 4 & 5 \\
6 & 7 & 8 & 9 & 10 \\
11 & 12 & 13 & 14 & 15 \\
16 & 17 & 18 & 19 & 20 \\
21 & 22 & 23 & 24 & 25
\end{pmatrix}$
```
- Expected: Full 5×5 matrix
- Challenge: Large structure

**D-CN-04: Piecewise Function**
- Context: "分段函数的定义。"
- Image:
```latex
$f(x) = \begin{cases}
x^2 & \text{if } x \geq 0 \\
-x^2 & \text{if } x < 0 \\
0 & \text{if } x = 0
\end{cases}$
```
- Expected: All three cases
- Challenge: Cases environment

**D-CN-05: Double Summation**
- Context: "二重求和。"
- Image:
```latex
$\sum_{i=1}^{n} \sum_{j=1}^{m} a_{ij} b_{ji}$
```
- Expected: Both summations with correct indices
- Challenge: Nested sums

**D-CN-06: Complex Integral**
- Context: "多重积分。"
- Image:
```latex
$\int_{0}^{1} \int_{0}^{1-x} \int_{0}^{1-x-y} f(x,y,z) \, dz \, dy \, dx$
```
- Expected: All three integrals with correct bounds
- Challenge: Triple integral

**D-CN-07: Long Polynomial**
- Context: "高次多项式。"
- Image:
```latex
$P(x) = a_0 + a_1 x + a_2 x^2 + a_3 x^3 + a_4 x^4 + a_5 x^5 + a_6 x^6 + a_7 x^7 + a_8 x^8$
```
- Expected: All 9 terms
- Challenge: Long sequence

**D-CN-08: Binomial Expansion**
- Context: "二项式定理。"
- Image:
```latex
$(x + y)^n = \sum_{k=0}^{n} \binom{n}{k} x^{n-k} y^k$
```
- Expected: Complete formula with binomial coefficient
- Challenge: Mixed symbols

**D-CN-09: Matrix Determinant**
- Context: "行列式的展开。"
- Image:
```latex
$\det(A) = \sum_{\sigma \in S_n} \text{sgn}(\sigma) \prod_{i=1}^{n} a_{i,\sigma(i)}$
```
- Expected: Complete determinant formula
- Challenge: Multiple operators

**D-CN-10: Limit with Fraction**
- Context: "极限计算。"
- Image:
```latex
$\lim_{x \to 0} \frac{\sin(x) - x + \frac{x^3}{6}}{x^5}$
```
- Expected: Nested fraction in limit
- Challenge: Complex numerator

---

### English Complex Formulas (D-EN-01 to D-EN-10)

**D-EN-01: Quadruple Nested Fraction**
- Context: "Continued fraction representation."
- Image:
```latex
$x = a_0 + \cfrac{1}{a_1 + \cfrac{1}{a_2 + \cfrac{1}{a_3 + \cfrac{1}{a_4}}}}$
```
- Expected: All levels with cfrac
- Challenge: Deep nesting

**D-EN-02: System of Equations**
- Context: "Solving linear systems."
- Image:
```latex
\begin{aligned}
3x - 2y + 4z &= 7 \\
x + 5y - 3z &= -2 \\
-2x + y + 6z &= 11
\end{aligned}
```
- Expected: All three equations aligned
- Challenge: Multiple lines

**D-EN-03: Block Matrix**
- Context: "Partitioned matrices."
- Image:
```latex
$M = \begin{pmatrix}
A & B \\
C & D
\end{pmatrix} = \begin{pmatrix}
1 & 2 & | & 3 & 4 \\
5 & 6 & | & 7 & 8 \\
\hline
9 & 10 & | & 11 & 12 \\
13 & 14 & | & 15 & 16
\end{pmatrix}$
```
- Expected: Block structure preserved
- Challenge: Mixed notation

**D-EN-04: Taylor Series**
- Context: "Taylor expansion."
- Image:
```latex
$f(x) = f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + \frac{f'''(a)}{3!}(x-a)^3 + \cdots$
```
- Expected: Multiple terms with derivatives
- Challenge: Long formula

**D-EN-05: Cauchy-Schwarz Inequality**
- Context: "Vector inequalities."
- Image:
```latex
$\left( \sum_{i=1}^{n} a_i b_i \right)^2 \leq \left( \sum_{i=1}^{n} a_i^2 \right) \left( \sum_{i=1}^{n} b_i^2 \right)$
```
- Expected: Three summations correctly placed
- Challenge: Multiple paired delimiters

**D-EN-06: Fourier Transform**
- Context: "Frequency domain analysis."
- Image:
```latex
$\mathcal{F}\{f(t)\} = \int_{-\infty}^{\infty} f(t) e^{-i\omega t} \, dt$
```
- Expected: Complete transform with complex exponential
- Challenge: Multiple symbols

**D-EN-07: Probability Distribution**
- Context: "Normal distribution."
- Image:
```latex
$p(x) = \frac{1}{\sigma \sqrt{2\pi}} \exp\left( -\frac{(x-\mu)^2}{2\sigma^2} \right)$
```
- Expected: Complete formula with exp
- Challenge: Nested fractions and exp

**D-EN-08: Leibniz Integral Rule**
- Context: "Differentiation under integral sign."
- Image:
```latex
$\frac{d}{dx} \left( \int_{a(x)}^{b(x)} f(x,t) \, dt \right) = f(x,b(x)) b'(x) - f(x,a(x)) a'(x) + \int_{a(x)}^{b(x)} \frac{\partial f}{\partial x} \, dt$
```
- Expected: Complete long formula
- Challenge: Very long, multiple parts

**D-EN-09: Eigenvalue Equation**
- Context: "Linear algebra."
- Image:
```latex
$\det(A - \lambda I) = \begin{vmatrix}
a_{11} - \lambda & a_{12} & a_{13} \\
a_{21} & a_{22} - \lambda & a_{23} \\
a_{31} & a_{32} & a_{33} - \lambda
\end{vmatrix} = 0$
```
- Expected: Determinant with matrix
- Challenge: Matrix in equation

**D-EN-10: Green's Theorem**
- Context: "Vector calculus."
- Image:
```latex
$\oint_C (L \, dx + M \, dy) = \iint_D \left( \frac{\partial M}{\partial x} - \frac{\partial L}{\partial y} \right) dA$
```
- Expected: Line integral equals double integral
- Challenge: Multiple integral types

---

## Summary Statistics

- **Total Cases**: 100
- **Chinese**: 50 (A:15, B:15, C:10, D:10)
- **English**: 50 (A:15, B:15, C:10, D:10)
- **Primary Focus**: Contextual over-reliance (Category A)
- **Secondary Focus**: Notation flexibility (Category B)
- **Ambiguity Testing**: Symbol recognition (Category C)
- **Complexity Testing**: Long formulas (Category D)

---

## Implementation Notes for Data Collection

### How to Create These Test Cases

1. **Context Creation**: Write short paragraphs (2-3 sentences) with misleading context
2. **Image Generation**:
   - Create LaTeX source file with the exact content
   - Compile with XeLaTeX
   - Convert to PNG (300 DPI recommended)
3. **Ground Truth**: The LaTeX source code is the ground truth
4. **Annotation**: Mark expected traps for analysis

### Recommended Tools

- LaTeX compiler: XeLaTeX (for Chinese support)
- Image conversion: ImageMagick or pdftoppm
- Font selection: Mix of common fonts (Times, Computer Modern, etc.)

### Quality Control

- Verify each image is readable
- Ensure ground truth matches image exactly
- Check that context is genuinely misleading but natural
- Test with human annotators first

---

## Expected Outcomes

**MixTex should excel because:**
1. Training without semantic coherence reduces contextual bias
2. Noise injection forces visual dependence
3. Diverse formula sources prevent notation bias

**Baseline models may struggle with:**
1. Category A: Over-relying on context from training data
2. Category B: Defaulting to common notation
3. Category C: Visual ambiguity without strong image features
4. Category D: Long sequences where context helps predict

---

## Metrics to Compute

For these 100 cases specifically:

1. **Edit Distance** (standard)
2. **Hallucination Rate**:
   - Count cases where output differs from image but matches context
   - Formula: (hallucinated cases) / 100
3. **Repetition Rate**:
   - Count cases with repeated tokens
   - Pattern: `(.+)\1{3,}` (regex for 3+ repetitions)
4. **Category Breakdown**:
   - Report accuracy for each category A/B/C/D separately
   - Identifies specific weaknesses

---

END OF DOCUMENT
