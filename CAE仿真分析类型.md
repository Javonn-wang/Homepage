# CAE仿真分析类型

## 一、结构力学分析

### 1. 静力学分析（Static Analysis）
- **特点**：假设载荷和响应不随时间变化，忽略惯性项和阻尼项。
- **平衡方程**： $\nabla \cdot \boldsymbol{\sigma} + \mathbf{f} = 0$
（ $\boldsymbol{\sigma}$ ：应力张量， $\mathbf{b}$ ：体积力）
- **本构方程**： $\boldsymbol{\sigma} = \mathbf{C} \cdot \boldsymbol{\varepsilon}$
（ $\mathbf{C}$ ：弹性刚度张量， $\boldsymbol{\varepsilon}$ ：应变张量）
- **几何方程**： $\boldsymbol{\varepsilon} = \frac{1}{2}(\nabla \mathbf{u} + \nabla \mathbf{u}^T)$ 
（ $\mathbf{u}$ ：位移场）
- **离散形式**：
- **有限元方程**：
 $$[K] \{\mathbf{u}\} = \{\mathbf{F}\}$$ ，其中 $[K]$ ：刚度矩阵， $\{\mathbf{u}\}$ ：节点位移向量， $\{\mathbf{F}\} $：外力向量。

- **特点**：
  - 假设小变形、线弹性材料、无时间依赖性。
  - 直接求解线性方程组，计算效率高。
- **应用场景**：桥梁、建筑结构的承载能力校核，齿轮静载荷分析。
  
---

### 2. 非线性分析（Nonlinear Analysis）
- **控制方程**：
  - **材料非线性**： $$\boldsymbol{\sigma} = \mathbf{C}(\boldsymbol{\varepsilon}) \cdot \boldsymbol{\varepsilon}$$ （如弹塑性、超弹性本构模型）
  - **几何非线性**： $$\boldsymbol{\varepsilon} = \frac{1}{2}(\nabla \mathbf{u} \cdot \nabla \mathbf{u}^T + \nabla \mathbf{u} + \nabla \mathbf{u}^T)$$ （大变形问题）
  - **接触非线性**（摩擦、分离）：引入接触边界条件（如罚函数法）。
- **离散形式**：
  - **迭代求解**： $$[K^{(n)}]\{\Delta \mathbf{u}^{(n)}\} = \{\mathbf{R}^{(n)}\}$$，其中 $[K^{(n)}]$ ：切线刚度矩阵， $\{\mathbf{R}^{(n)}\}$ ：残差向量。
  - **Newton-Raphson方法**：逐步逼近解。
- **应用场景**：金属成形、橡胶密封件、飞机起落架着陆冲击。

---

### 3. 动力学分析（Dynamic Analysis）
- **子类型**：
  - **瞬态动力学**（显式/隐式）：求解时间依赖的位移场（如冲击、爆炸）。
  - **模态分析**（频率响应）：求解结构固有频率和振型（如振动模态）。
- **控制方程**：
  $$\rho \frac{\partial^2 \mathbf{u}}{\partial t^2} = \nabla \cdot \boldsymbol{\sigma} + \mathbf{f} \quad \text{（动力学平衡方程）}   $$

- **离散形式**：
  - **有限元-时域离散**：
$$[M]\{\ddot{\mathbf{u}}\} + [C]\{\dot{\mathbf{u}}\} + [K]\{\mathbf{u}\} = \{\mathbf{F}(t)\}$$ <br>
其中 $[M]$ ：质量矩阵， $[C]$ ：阻尼矩阵， $\{\ddot{\mathbf{u}}\}$ ：加速度向量。
  - **显式/隐式时间积分**：如Newmark法、Houbolt法。

- **特点**：
  - 考虑惯性力和阻尼效应。
  - 分为瞬态动力学（时间域）和频域分析（傅里叶变换）。

- **应用场景**：地震响应分析、机械振动优化、高速碰撞仿真。

---

### **4. 屈曲分析（Buckling Analysis）**
- **控制方程**：
  - **线性屈曲**：
$$([K] + \lambda [K_{\sigma}])\{\mathbf{u}\} = 0$$ <br>
其中 $[K_{\sigma}]$ ：应力刚度矩阵， $\lambda$ ：临界载荷因子。
  - **非线性屈曲**：结合大变形和材料非线性。
- **离散形式**：
  - **特征值问题**：
$$[K][\Phi] = \lambda [K_{\sigma}][\Phi]$$ ，其中 $\Phi$：屈曲模态。
- **特点**：
  - 线性屈曲仅适用于小变形，非线性屈曲需迭代求解后屈曲行为。
- **应用场景**：
  - 薄壁结构稳定性分析（如飞机机翼、压杆）。

---

### 5. 疲劳分析（Fatigue Analysis）
- **控制方程**：
  - **高周疲劳**：基于S-N曲线和Miner准则： $$\sum \frac{n_i}{N_i} \geq 1$$ （ $n_i$ ：实际循环次数， $N_i$ ：材料寿命）
  - **低周疲劳**：基于应变-寿命模型（如Coffin-Manson模型）。
- **离散形式**：
  - **应力/应变循环提取**：通过雨流计数法（Rainflow Counting）。
  - **累积损伤计算**：基于局部应力或应变历史。
- **特点**：
  - 需结合动力学或热分析结果输入。
- **应用场景**：
  - 齿轮、轴承、焊接接头寿命预测。

---

## 二、流体动力学分析

### 1. 可压缩流分析（Compressible Flow）
- **特点**：马赫数 $Ma \geq 0.3$，密度变化显著（如高速气流、爆轰）。
- **控制方程**：
  - **连续性方程**：
    $$\frac{\partial \rho}{\partial t} + \nabla \cdot (\rho \mathbf{u}) = 0       $$
  - **动量方程**（包含压力梯度项）：
    $$\rho \left( \frac{\partial \mathbf{u}}{\partial t} + \mathbf{u} \cdot \nabla \mathbf{u} \right) = -\nabla p + \mu \nabla^2 \mathbf{u} + \mathbf{f}       $$
  - **能量方程**（考虑压缩热效应）：
    $$\rho c_p \frac{\partial T}{\partial t} = \nabla \cdot (k \nabla T) + \Phi + \frac{\partial p}{\partial t} \cdot \frac{\partial \rho}{\partial p}       $$
- **应用场景**：超音速飞行器、喷气发动机、声波传播。

---

### 2. 不可压缩流分析（Incompressible Flow）
- **特点**：马赫数 $Ma < 0.3$，密度恒定（如水、低速气流）。
- **控制方程**：
  - **连续性方程**（质量守恒）：
    $$\nabla \cdot \mathbf{u} = 0       $$
  - **动量方程**（压力梯度主导）：
    $$\rho \left( \frac{\partial \mathbf{u}}{\partial t} + \mathbf{u} \cdot \nabla \mathbf{u} \right) = -\nabla p + \mu \nabla^2 \mathbf{u} + \mathbf{f}       $$
- **应用场景**：管道流动、船舶阻力分析、风洞实验。

---

### 3. 湍流分析（Turbulent Flow）
- **子类型**：
  - **RANS（雷诺平均纳维-斯托克斯方程）**：
    - 通过湍流模型（如 $k-\epsilon$、$k-\omega$）封闭方程，计算平均流动。
  - **LES（大涡模拟）**：
    - 直接模拟大尺度涡旋，小尺度涡旋通过模型处理。
  - **DES（分离涡模拟）**：
    - 结合RANS和LES，适用于分离流动。
- **控制方程**（以RANS为例）：
  $$\rho \left( \frac{\partial \overline{\mathbf{u}}}{\partial t} + \overline{\mathbf{u}} \cdot \nabla \overline{\mathbf{u}} \right) = -\nabla \overline{p} + \nabla \cdot \left[ \mu \left( \nabla \overline{\mathbf{u}} + (\nabla \overline{\mathbf{u}})^T \right) - \rho \overline{\mathbf{u}' \mathbf{u}'} \right] + \mathbf{f}     $$
  其中 $\overline{\mathbf{u}}$ 为平均速度，$\mathbf{u}'$ 为脉动速度。
- **应用场景**：飞机机翼湍流、城市风环境、燃烧室流动。

---

## 三、区别与联系

| **分析类型**       | **核心特征**                     | **典型方程**                     | **应用场景**                     |
|---------------------|----------------------------------|----------------------------------|----------------------------------|
| **静力学分析**       | 忽略惯性项，线性响应             | $\nabla \cdot \boldsymbol{\sigma} + \mathbf{f} = 0$ | 桥梁静载荷、齿轮强度校核         |
| **非线性分析**       | 材料/几何/接触非线性             | 非线性本构方程（如塑性屈服）     | 金属成形、大变形橡胶仿真         |
| **动力学分析**       | 考虑惯性项，时间依赖             | $\rho \frac{\partial^2 \mathbf{u}}{\partial t^2} = \nabla \cdot \boldsymbol{\sigma} + \mathbf{f}$ | 地震响应、高速碰撞               |
| **疲劳分析**         | 循环载荷下的损伤累积             | Miner准则或应变-寿命模型         | 航空发动机叶片寿命预测           |
| **可压缩流**         | 密度变化显著（ $Ma\geq0.3$  ）    | 完整Navier-Stokes方程+能量方程   | 超音速飞行器、喷气发动机         |
| **不可压缩流**       | 密度恒定（ $Ma < 0.3$ ）           | $\nabla \cdot \mathbf{u} = 0$    | 管道流动、船舶阻力               |
| **湍流分析（RANS）** | 湍流模型封闭平均方程             | RANS方程+湍流模型（如 $k-\epsilon$ ） | 飞机机翼湍流、燃烧室流动         |
| **湍流分析（LES）**  | 直接模拟大尺度涡旋               | LES过滤方程+亚格子模型           | 高精度分离流动、风能涡轮机       |

---

## 四、总结

1. **结构力学分析**：
   - 静力学与动力学的核心区别在于是否考虑惯性项；
   - 非线性分析需处理材料、几何或接触非线性；
   - 疲劳分析关注长期循环载荷下的损伤累积。

2. **流体动力学分析**：
   - 可压缩流与不可压缩流的划分基于马赫数；
   - 湍流分析需通过RANS、LES等模型处理非定常小尺度流动。

3. **选择建议**：
   - 低速流体问题优先选择不可压缩流分析；
   - 高速或高温问题需启用可压缩流和能量方程；
   - 精度要求高的湍流问题可采用LES，但计算成本较高。
