# Mason Veg Generator v0.1

这是一个基于 Houdini 的程序化植被分发工具（HDA），旨在通过地形坡度逻辑实现植被的自动化生成。

## 项目背景
本项目是作为 TA（技术美术）学习路径中的第一个实验性项目。核心逻辑基于向量点积（Dot Product），实现了植被在复杂几何体表面的智能分布。

## 环境配置 (Prerequisites)
- **软件版本**: Houdini 19.5 / 20.0 (NC/FX 版均可)
- **操作系统**: 跨平台支持 (Windows / macOS / Linux)
- **依赖项**: 无需任何第三方插件，纯 VEX 逻辑驱动

## 安装与“编译”说明 (Installation & Build)
对于 HDA (Houdini Digital Asset)，安装即是部署：
1. **下载资产**: 前往本仓库右侧的 [Releases](https://github.com/skywa1keri7/Houdini_Veg_Gen_Project_by_noob/releases) 页面下载最新的 `.hdanc` 文件。
2. **导入**: 在 Houdini 中选择 `Assets -> Install Digital Asset` 并选中该文件。
3. **调用**: 在 SOP 层级搜索 `Mason_Veg_Generator` 节点即可使用。
*注：本项目已将核心逻辑源码存放于 `/src` 文件夹中，方便开发者进行 Code Review。*

## 使用指南 (Usage)
1. **输入端**: 
   - **Input 0**: 接入基础地形或任何几何体。
2. **参数面板说明**:
   - **Threshold**: 调整生长阈值。值越高，植被越倾向于只在平坦区域生长。
   - **Hardness**: 调整生长区域边缘的过渡硬度。
   - **Global Scale**: 全局控制生成物体的缩放比例。

## 核心逻辑 (Source Logic)
核心 VEX 算法如下：
```cpp
// 通过法线与上方向的点积计算坡度遮罩
f@growth_mask = smooth(threshold, threshold + hardness, dot(normalize(@N), {0,1,0}));
// 基于遮罩与随机值控制 pscale
@pscale = (0.05 + f@growth_mask * rand(@ptnum)) * global_scale;
