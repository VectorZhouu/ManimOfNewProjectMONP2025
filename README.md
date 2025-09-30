# ManimOfNewProjectMONP2025
# 常见问题
- [x] dvisvgm版本过低可能导致不成功,目前使用的是dvisvgm-3.5-win64,使用的miktex最好也是最新版
- [x] miktex报错：数学公式用MathTex格式,有中文用Text格式,中文用其他格式会报错
- [x] 复平面要注意``` npl.n2p(0 + 2j) ```才能用n2p,如果有变量可以用always_redraw(),并且经常写成``` npl.n2p(3 + fun(x) * 1j) ```,用j表示虚数i
- [x] ``` self.play(Write(sth))``` 对象只能是一个

# Manim CE 教程：创建数学动画可视化

## 1. 什么是 Manim CE？

Manim CE (Community Edition) 是一个基于 Python 的动画引擎，专为创建精美的数学动画和可视化而设计。它是 3Blue1Brown (Grant Sanderson) 创建的原始 Manim 库的社区维护版本。

## 2. 安装 Manim CE

### 系统要求

- Python 3.7 或更高版本
- FFmpeg
- LaTeX (推荐 MiKTeX 或 TeX Live)
- C++ 编译器 (Windows 用户)

### 安装步骤

```bash
# 使用 pip 安装
pip install manim

# 如果你使用 conda
conda install -c conda-forge manim

```

### 验证安装

```bash
# 验证安装是否成功
manim --version

```

## 3. Manim CE 基本概念

### 场景 (Scene)

在 Manim 中，场景是一个包含所有动画内容的容器。每个场景都是继承自 Scene 类的 Python 类。

### 数学对象 (MObject)

MObject (Mathematical Object) 是 Manim 中的基本构建单元，如文字、形状、函数图表等。

### 动画 (Animation)

Animation 类用于定义对象在场景中的变化，如淡入、移动、旋转等。

## 4. 创建第一个 Manim 动画

### 基本示例

```python
from manim import *

class SquareToCircle(Scene):
    def construct(self):
        # 创建一个方形
        square = Square(side_length=2, color=BLUE)
        
        # 显示方形
        self.play(Create(square))
        self.wait()
        
        # 将方形变成圆形
        circle = Circle(radius=1, color=RED)
        self.play(Transform(square, circle))
        self.wait()

# 运行命令：manim -pql example.py SquareToCircle

```

### 渲染命令

```bash
# 使用低质量渲染并预览
manim -pql your_file.py YourSceneClass

# 使用中等质量渲染并预览
manim -pqm your_file.py YourSceneClass

# 使用高质量渲染并预览
manim -pqh your_file.py YourSceneClass

```

## 5. 常用数学对象

### 几何形状

```python
# 圆形
circle = Circle(radius=1.0, color=RED)

# 方形
square = Square(side_length=1.0, color=BLUE)

# 三角形
triangle = Triangle(color=GREEN)

# 矩形
rectangle = Rectangle(width=2.0, height=1.0, color=YELLOW)

# 箭头
arrow = Arrow(start=LEFT, end=RIGHT, color=WHITE)

```

### 文本和数学表达式

```python
# 普通文本
text = Text("Hello, Manim!", color=WHITE)

# 数学表达式（LaTeX）
math_text = MathTex(r"\int_{a}^{b} f(x) \, dx = F(b) - F(a)", color=WHITE)

# 带颜色的数学表达式
colored_math = MathTex(
    r"e^{i\pi} + 1 = 0",
    substrings_to_isolate=[r"e", r"i\pi", r"+1", r"=0"]
)
colored_math[0].set_color(RED)    # e
colored_math[2].set_color(BLUE)   # i\pi
colored_math[4].set_color(GREEN)  # +1
colored_math[6].set_color(YELLOW) # =0

```

## 6. 动画与变换

### 基本动画

```python
# 创建动画
self.play(Create(circle))  # 绘制圆形
self.play(FadeIn(square))  # 淡入方形
self.play(FadeOut(triangle))  # 淡出三角形
self.play(Indicate(text))  # 强调文本

# 对象变换
self.play(Transform(square, circle))  # 将方形变为圆形
self.play(ReplacementTransform(circle, triangle))  # 替换变换

```

### 移动动画

```python
# 移动对象
self.play(square.animate.shift(RIGHT * 2))  # 向右移动
self.play(circle.animate.move_to(ORIGIN))  # 移动到原点
self.play(triangle.animate.scale(2))  # 放大两倍
self.play(text.animate.rotate(PI/2))  # 旋转90度

```

### 动画组和序列

```python
# 同时播放多个动画
self.play(Create(circle), FadeIn(square), run_time=2)

# 按顺序播放动画
self.play(Create(circle))
self.wait()  # 暂停
self.play(FadeIn(square))

```

## 7. 坐标系与函数图像

### 坐标系

```python
# 创建坐标轴
axes = Axes(
    x_range=[-3, 3, 1],  # x轴范围和步长
    y_range=[-3, 3, 1],  # y轴范围和步长
    axis_config={"color": WHITE},
    x_axis_config={"numbers_to_include": [-2, 0, 2]},
    y_axis_config={"numbers_to_include": [-2, 0, 2]}
)

# 添加标签
x_label = axes.get_x_axis_label(r"x")
y_label = axes.get_y_axis_label(r"y")

self.play(Create(axes), Create(x_label), Create(y_label))

```

### 函数图像

```python
# 创建函数图像
def func(x):
    return np.sin(x)

graph = axes.plot(func, color=BLUE)
graph_label = axes.get_graph_label(graph, r"\sin(x)", x_val=PI/2, direction=UP)

self.play(Create(graph), Create(graph_label))

# 创建区域填充
area = axes.get_area(graph, x_range=[0, PI], color=YELLOW, opacity=0.5)
self.play(FadeIn(area))

```

## 8. 高级功能

### 相机动画

```python
# 相机缩放
self.play(self.camera.frame.animate.scale(0.5))  # 放大
self.play(self.camera.frame.animate.scale(2))    # 缩小

# 相机移动
self.play(self.camera.frame.animate.move_to(RIGHT * 2))

```

### 3D 动画

```python
from manim import *

class ThreeDExample(ThreeDScene):
    def construct(self):
        # 配置3D视图
        self.set_camera_orientation(phi=75 * DEGREES, theta=30 * DEGREES)
        
        # 创建3D坐标轴
        axes = ThreeDAxes()
        self.add(axes)
        
        # 创建3D对象
        sphere = Sphere(radius=1, resolution=(20, 20))
        sphere.set_color(RED)
        
        # 显示
        self.play(Create(sphere))
        self.wait()
        
        # 旋转
        self.begin_ambient_camera_rotation(rate=0.1)
        self.wait(5)
        self.stop_ambient_camera_rotation()

```

### 自定义动画

```python
# 自定义动画类
class GrowFromCenter(Animation):
    def __init__(self, mobject, **kwargs):
        super().__init__(mobject, **kwargs)
        
    def interpolate_mobject(self, alpha):
        # alpha从0变化到1
        self.mobject.scale(alpha)
        self.mobject.move_to(ORIGIN)

# 使用自定义动画
circle = Circle(radius=2, color=BLUE)
self.play(GrowFromCenter(circle))

```

## 9. 实用技巧

### 图像插入

```python
# 插入图片
image = ImageMobject("path/to/your/image.png")
image.scale(2)  # 缩放图片
self.play(FadeIn(image))

```

### SVG 导入

```python
# 导入SVG文件
svg_image = SVGMobject("path/to/your/image.svg")
svg_image.scale(2)  # 缩放SVG
self.play(DrawBorderThenFill(svg_image))

```

### 配置文件

创建 manim.cfg 文件来设置默认配置：

```
[CLI]
# 默认视频分辨率
pixel_height = 1080
pixel_width = 1920
# 默认质量
quality = h
# 默认预览
preview = True
# 默认输出目录
media_dir = ./media

```

## 10. 示例项目：可视化欧拉公式

```python
from manim import *

class EulerFormula(Scene):
    def construct(self):
        # 创建标题
        title = Text("Euler's Identity", font_size=48)
        title.to_edge(UP)
        self.play(Write(title))
        
        # 创建欧拉公式
        formula = MathTex(
            "e^", "{i", "\\pi}", "+", "1", "=", "0",
            font_size=72
        )
        
        # 为公式各部分设置颜色
        formula[0].set_color(RED)    # e^
        formula[1].set_color(BLUE)   # i
        formula[2].set_color(BLUE)   # \pi
        formula[3].set_color(WHITE)  # +
        formula[4].set_color(GREEN)  # 1
        formula[5].set_color(WHITE)  # =
        formula[6].set_color(YELLOW) # 0
        
        # 显示公式
        self.play(FadeIn(formula))
        self.wait(1)
        
        # 创建解释文本
        explanation = VGroup(
            Text("Where:", font_size=36),
            MathTex("e = 2.71828...", font_size=36),
            MathTex("i = \\sqrt{-1}", font_size=36),
            MathTex("\\pi = 3.14159...", font_size=36)
        ).arrange(DOWN, aligned_edge=LEFT).next_to(formula, DOWN, buff=1)
        
        # 设置解释文本颜色
        explanation[1][0].set_color(RED)    # e
        explanation[2][0].set_color(BLUE)   # i
        explanation[3][0].set_color(BLUE)   # pi
        
        # 显示解释
        self.play(Write(explanation[0]))
        for i in range(1, 4):
            self.play(Write(explanation[i]))
        
        self.wait(2)
        
        # 整体变换
        final_group = VGroup(title, formula, explanation)
        self.play(final_group.animate.scale(0.8).to_edge(UP))
        
        # 添加结论
        conclusion = Text(
            "This elegant equation connects five fundamental\n"
            "mathematical constants in a single formula.",
            font_size=36
        ).next_to(final_group, DOWN, buff=0.5)
        
        self.play(Write(conclusion))
        self.wait(3)

# 运行命令：manim -pqh euler_formula.py EulerFormula

```

## 11. 资源与学习材料

### 官方资源

- [Manim CE 官方网站](https://www.manim.community/)
- [Manim CE 官方文档](https://docs.manim.community/)
- [Manim CE GitHub 仓库](https://github.com/ManimCommunity/manim)

### 社区资源

- [Manim Reddit 社区](https://www.reddit.com/r/manim/)
- [Manim Discord 服务器](https://discord.gg/mMRrZQW)
- [3Blue1Brown YouTube 频道](https://www.youtube.com/c/3blue1brown) (Manim 原始创建者)