# CIE 1931 色彩空间分析工具 (CIE 1931 Color Space Converter)

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Vanilla JS](https://img.shields.io/badge/Tech-Vanilla%20JS-f7df1e.svg)]()
[![Color Science](https://img.shields.io/badge/Domain-Color%20Science-ff69b4.svg)]()
[![在线演示](https://img.shields.io/badge/在线试用-点我-brightgreen?style=for-the-badge&logo=github)](https://zq-moonlight.github.io/cie1931-color-converter/)  

*(2026 年 3 月部署，点击上方按钮即可访问)*

## 💡 项目简介

一个无依赖的纯 Vanilla JS 前端 CIE 1931 色彩空间转换与可视化工具。支持从 RGB 到 XYZ、xyY、CIELAB、LCH、CIELUV 及 CCT 的完整计算链路，配备交互式 CIE 1931 色度图、动态色域三角、色域溢出警告与色度适应变换 (Chromatic Adaptation) 功能。

**设计原则**：不引入第三方色彩计算库（如 chroma.js）。所有转换矩阵、电光转换函数 (OETF/EOTF)、非线性编码、跨白点适应算法及空间映射公式，均基于标准数学模型由底层实现，以确保对数据流和计算过程的完全控制。

🌊 Built with "Vibe Coding"

## 🔗 在线体验

部署地址：[https://zq-moonlight.github.io/cie1931-color-converter/](https://zq-moonlight.github.io/cie1931-color-converter/)

- 📱 **响应式布局**：支持跨设备（桌面/平板/移动端）访问。
- 🌐 **国际化**：内置中英双语 (i18n) 界面切换。
- ⚡ **执行模式**：支持实时数据同步与手动执行计算双模式（便于独立验证公式）。
- 🖱️ **图表交互**：支持在 CIE 色度图中点击/拖拽，全空间坐标与色温数据将根据光量方程自动求解同步。

## 🧮 功能模块

| 功能模块                            | 实现方式                           | 技术规范与说明                                               |
| :---------------------------------- | :--------------------------------- | :----------------------------------------------------------- |
| **RGB ↔ XYZ ↔ xyY**                 | $3 \times 3$ 转换矩阵 + 传递函数   | 支持 sRGB / Adobe / Display P3 / Rec.709 / Rec.2020 / ProPhoto |
| **色度适应 (Chromatic Adaptation)** | Bradford / Von Kries / XYZ Scaling | 动态求解适应矩阵，实现跨参考白点（如 D65 至 D50）的坐标映射  |
| **CIE L\*a\*b\* / LCH(ab)**         | CIE 1976 标准公式，独立计算        | 包含 Epsilon/Kappa 阈值判定与立方根非线性解析                |
| **CIE L\*u\*v\* / LCH(uv)**         | CIE 标准公式，独立计算             | 并列双向推导，极坐标系基于 `atan2` 函数转换                  |
| **CCT (相关色温)**                  | McCamy 近似方程 + 普朗克轨迹       | 支持色温 (K) 与色度坐标 (xy) 的双向换算                      |
| **CIE 1931 xy 交互图**              | Canvas 像素填充 + 谱轨迹采样       | 动态计算当前色度坐标下的 sRGB 最大物理亮度，控制边界         |
| **色域警告 (Out of Gamut)**         | 线性 RGB 边界检查                  | 设置浮点容差，超出物理极限时显示警告，并对 RGB 码值执行裁剪 (Clamping) |

## 📖 使用说明

1. **环境参数配置**：
   在顶部控制栏设定基础物理环境：
   - 参考白点 (Illuminant)：设定观察光源（D65, D50, D75, A, C, E）。
   - RGB 色彩空间：选择源数据的转换函数及原色坐标 (Primaries)。
   - 色度适应模型：选择跨白点转换时的矩阵算法。
2. **计算模式切换**：
   勾选 `启用实时计算` 时，任意输入将瞬间触发全局重算；取消勾选则进入手动模式，需点击 `执行计算` 按钮触发运算，适用于录入多位小数或进行特定数值测试。
3. **坐标数据双向绑定**：
   在任意卡片（如 CIELAB）中输入坐标，系统将其作为输入源，逆推绝对三刺激值 (XYZ)，并正向求解更新其余数据面板。
4. **图表拾色逻辑**：
   图表内部实线三角形标识当前选定 RGB 空间的色域覆盖范围。鼠标交互拾取坐标时，系统默认执行“最大亮度归一化”，求解对应坐标下的最高合理光量 (Y) 并同步全表。

## 🛠️ 技术实现

- **无依赖架构**：矩阵乘法、逆矩阵求解、非线性编码等核心逻辑由纯原生 JS 实现。
- **渲染优化**：利用离屏绘制（Offscreen Canvas）预计算 sRGB 最大亮度像素填充，处理负值裁剪与归一化，降低重绘性能开销。
- **状态同步机制**：采用单向数据流与 `activeSource` 追踪机制，避免表单双向绑定引发的循环触发与更新死锁。

## 📚 参考资料

1. **Bruce Lindbloom**（本工具核心数学模型参考）：
   - 主页：[http://www.brucelindbloom.com](http://www.brucelindbloom.com) 
   - 核心方程式：[Math Equations](http://www.brucelindbloom.com/Math.html) 
   - 对标产品功能：[Color Calculator](http://www.brucelindbloom.com/ColorCalculator.html)
2. **CIE 官方规范**：
   - CIE 1931 标准观察者 (Standard Observer) 与色度图定义。
   - Publication 15:2004 (Colorimetry)。
3. **RGB 色彩空间规范**：
   - IEC 61966-2-1 (sRGB)。
   - 包含 Adobe RGB (1998), Display P3, Rec.2020, ProPhoto RGB 的公开物理白皮书参数。

## 🚀 已知小问题 & 未来可能优化

- CCT 高色温 (>15000 K) 区域推导精度略有下降（受限于 McCamy 近似方程的固有局限）。
- 光谱轨迹 (Spectral Locus) 点目前精简至 ~30 个控制点，视觉平滑度足够，但未达到严格的 1nm 采样间隔。
- Canvas 内部彩色填充目前固定以 sRGB 映射基准渲染。若需改为随目标色彩空间动态重算全图像素，性能开销较大，未来可考虑引入 WebGL/Shader 实现。
- 暗色模式 (Dark Mode) 支持筹备中。

## 📦 部署与运行

1. **本地环境**：克隆仓库后，使用现代浏览器打开 `index.html` 即可运行。
2. **在线部署**：当前分支已通过 GitHub Pages 自动部署。
3. **自托管**：Fork 仓库后，通过 GitHub Settings → Pages 设置 `main` 分支发布即可。

## 📜 License

[MIT License](LICENSE) 

ZQ @ 2026.3

# CIE 1931 Color Space Converter

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Vanilla JS](https://img.shields.io/badge/Tech-Vanilla%20JS-f7df1e.svg)]()
[![Color Science](https://img.shields.io/badge/Domain-Color%20Science-ff69b4.svg)]()
[![Live Demo](https://img.shields.io/badge/Live%20Demo-Click%20Here-brightgreen?style=for-the-badge&logo=github)](https://zq-moonlight.github.io/cie1931-color-converter/)  

*(Deployed March 2026, click the button above to access)*

## 💡 Project Overview

A dependency-free, pure Vanilla JS frontend tool for CIE 1931 color space conversion and visualization. It supports a complete calculation pipeline from RGB to XYZ, xyY, CIELAB, LCH, CIELUV, and CCT, featuring an interactive CIE 1931 chromaticity diagram, dynamic gamut triangle, out-of-gamut warnings, and Chromatic Adaptation.

**Design Principles**: No third-party color calculation libraries (like chroma.js) are introduced. All transformation matrices, opto-electronic transfer functions (OETF/EOTF), non-linear encodings, cross-white-point adaptation algorithms, and spatial mapping formulas are implemented from scratch based on standard mathematical models to ensure total control over the data flow and calculation processes.

🌊 Built with "Vibe Coding"

## 🔗 Live Experience

Deployment URL: [https://zq-moonlight.github.io/cie1931-color-converter/](https://zq-moonlight.github.io/cie1931-color-converter/)

- 📱 **Responsive Layout**: Supports cross-device access (desktop/tablet/mobile).
- 🌐 **Internationalization**: Built-in bilingual (English/Chinese) interface switching (i18n).
- ⚡ **Execution Modes**: Supports dual modes of real-time data synchronization and manual calculation execution (convenient for independent formula verification).
- 🖱️ **Chart Interaction**: Supports clicking/dragging on the CIE chromaticity diagram; full-space coordinates and color temperature data are automatically solved and synchronized based on the luminance equation.

## 🧮 Core Modules

| Module                              | Implementation                     | Specifications & Notes                                       |
| :---------------------------------- | :--------------------------------- | :----------------------------------------------------------- |
| **RGB ↔ XYZ ↔ xyY** | $3 \times 3$ Matrix + Transfer Functions | Supports sRGB / Adobe / Display P3 / Rec.709 / Rec.2020 / ProPhoto |
| **Chromatic Adaptation** | Bradford / Von Kries / XYZ Scaling | Dynamically solves the adaptation matrix for cross-illuminant mapping (e.g., D65 to D50) |
| **CIE L\*a\*b\* / LCH(ab)** | CIE 1976 standard formulas         | Includes Epsilon/Kappa threshold checks and cube-root non-linear resolution |
| **CIE L\*u\*v\* / LCH(uv)** | CIE standard formulas              | Parallel bidirectional derivation, polar coordinates based on `atan2` conversion |
| **CCT (Color Temperature)** | McCamy approximation + Planckian locus | Supports bidirectional conversion between color temp (K) and chromaticity coordinates (xy) |
| **Interactive CIE 1931 xy Diagram** | Canvas pixel fill + Spectral locus | Dynamically calculates the max sRGB physical luminance at current chromaticity, boundary controlled |
| **Out of Gamut Warning** | Linear RGB boundary check          | Sets floating-point tolerance; triggers warning when exceeding physical limits and applies clamping to RGB values |

## 📖 Usage Instructions

1. **Environment Configuration**:
   Set the base physical environment in the top control bar:
   - Reference Illuminant: Set the observation light source (D65, D50, D75, A, C, E).
   - RGB Color Space: Select the transfer function and primary coordinates of the source data.
   - Chromatic Adaptation Model: Select the matrix algorithm for cross-white-point conversion.
2. **Calculation Mode Switch**:
   When `Enable Real-time Sync` is checked, any input triggers global recalculation instantly; unchecking it enters manual mode, requiring a click on the `Calculate` button to trigger calculation, suitable for entering multi-decimal values or testing specific numbers.
3. **Bidirectional Coordinate Binding**:
   Input coordinates in any card (e.g., CIELAB), and the system uses it as the input source to inversely derive the absolute tristimulus values (XYZ) and forward-solve to update the rest of the data panels.
4. **Chart Color-Picking Logic**:
   The solid triangle inside the chart indicates the gamut coverage of the currently selected RGB space. When picking coordinates via mouse interaction, the system defaults to "Maximum Luminance Normalization," solving for the highest reasonable physical luminance (Y) at that coordinate and synchronizing across all tables.

## 🛠️ Technical Implementation

- **Dependency-Free Architecture**: Core logic such as matrix multiplication, matrix inversion, and non-linear encoding are implemented in pure Vanilla JS.
- **Rendering Optimization**: Utilizes Offscreen Canvas to precalculate the sRGB max luminance pixel fill, handling negative value clipping and normalization to reduce redraw performance overhead.
- **State Synchronization Mechanism**: Adopts unidirectional data flow and an `activeSource` tracking mechanism to prevent circular triggers and update deadlocks caused by bidirectional form binding.

## 📚 References

1. **Bruce Lindbloom** (Core mathematical model reference for this tool):
   - Homepage: [http://www.brucelindbloom.com](http://www.brucelindbloom.com) 
   - Core Equations: [Math Equations](http://www.brucelindbloom.com/Math.html) 
   - Benchmark Product: [Color Calculator](http://www.brucelindbloom.com/ColorCalculator.html)
2. **CIE Official Specifications**:
   - CIE 1931 Standard Observer and Chromaticity Diagram definitions.
   - Publication 15:2004 (Colorimetry).
3. **RGB Color Space Specifications**:
   - IEC 61966-2-1 (sRGB).
   - Includes public physical whitepaper parameters for Adobe RGB (1998), Display P3, Rec.2020, and ProPhoto RGB.

## 🚀 Known Issues & Future Optimizations

- Derivation accuracy for high CCT (>15000 K) slightly decreases (limited by the inherent constraints of the McCamy approximation equation).
- Spectral Locus points are currently simplified to ~30 control points; visual smoothness is sufficient, but it does not reach a strict 1nm sampling interval.
- The color fill inside the Canvas is currently fixed to the sRGB mapping baseline for rendering. Dynamically recalculating all pixels based on the target color space incurs high performance overhead; future implementations may consider WebGL/Shaders.
- Dark Mode support is in the pipeline.

## 📦 Deployment & Execution

1. **Local Environment**: After cloning the repository, simply open `index.html` in a modern browser to run.
2. **Online Deployment**: The current branch is automatically deployed via GitHub Pages.
3. **Self-Hosting**: After forking the repository, navigate to GitHub Settings → Pages and set the `main` branch to publish.

## 📜 License

[MIT License](LICENSE) 

ZQ @ 2026.3
