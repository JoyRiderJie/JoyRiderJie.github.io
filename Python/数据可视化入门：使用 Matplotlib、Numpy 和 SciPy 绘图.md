数据可视化是数据分析不可或缺的工具，它通过图形化手段帮助我们更直观地理解数据。Python拥有多种库来实现数据可视化，其中`matplotlib`、`numpy`和`scipy`是最常用的几个。本文将详细介绍如何使用这些库来创建各种图表和曲面。

## 环境搭建

在开始之前，请确保您的Python环境中已安装这些库。如果尚未安装，可以通过以下命令进行安装：

```bash
pip install matplotlib numpy scipy
```

## 核心库简介

- `numpy`：Python中用于科学计算的基础包，提供了强大的N维数组对象。
- `matplotlib`：Python的绘图库，提供类似MATLAB的绘图框架。
- `scipy`：基于`numpy`的算法和数学工具包，提供多种科学计算函数。

## 绘制曲面

以下是使用`matplotlib`和`numpy`绘制3D曲面的示例代码：

```python
def draw_3d_surface(x, y, z, labels: list[str] = ['X', 'Y', 'Z'],
                    label_color: list[str] = ['#000000', '#000000', '#000000'], img_name='3D_Surface',
                    imgs_path: str = './imgs', is_save: bool = False, is_display: bool = True):
    """
    绘制3D曲面图
    :param x: x轴数据列表
    :param y: y轴数据列表
    :param z: 对应于x和y的z值列表
    :param labels: 坐标轴标签列表
    :param label_color: 坐标轴标签颜色列表
    :param img_name: 图形名称
    :param imgs_path: 图片保存路径
    :param is_save: 是否保存图片
    :param is_display: 是否显示图片
    example:
        x = np.linspace(-5, 5, 100)
        y = np.linspace(-5, 5, 100)
        x, y = np.meshgrid(x, y)
        z = np.sin(np.sqrt(x ** 2 + y ** 2))
        draw_3d_surface(x, y, z, labels=['X', 'Y', 'Z'], img_name='3D_Surface', is_save=True)
    """
    # 将列表转换为numpy数组
    if type(x) is not np.array:
        x = np.array(x)
    if type(x) is not np.array:
        y = np.array(y)
    if type(x) is not np.array:
        z = np.array(z)
    # 检查x和y的数据长度是否相等
    if len(x) != len(y) or len(x) != len(z):
        raise ValueError("x, y, z列表的长度必须相同")

    # 创建网格
    xg, yg = np.meshgrid(np.linspace(x.min(), x.max(), 100),
                         np.linspace(y.min(), y.max(), 100))
    # 使用最近邻插值来获取网格上的z值
    zg = griddata((x, y), z, (xg, yg), method='nearest')
    # 创建一个新的图和一个3D子图
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')
    # 绘制曲面图
    surf = ax.plot_surface(xg, yg, zg, cmap='viridis')
    # 设置坐标轴标签
    ax.set_xlabel(labels[0], color=label_color[0])
    ax.set_ylabel(labels[1], color=label_color[1])
    ax.set_zlabel(labels[2], color=label_color[2])
    # 设置图形标题
    plt.title(img_name)
    # 添加颜色条
    fig.colorbar(surf)
    # 保存图片
    if is_save:
        # 如果文件名不带.png，则添加.png
        if not img_name.endswith('.png'):
            img_name += '.png'
        file_path = os.path.join(imgs_path, img_name)
        os.makedirs(os.path.dirname(file_path), exist_ok=True)
        plt.savefig(file_path)
    if is_display:
        plt.show()

```

请注意，上述代码中的`griddata`函数用于数据插值，以便在3D空间中创建平滑的曲面。


![请添加图片描述](https://i-blog.csdnimg.cn/direct/e65851a868c741d08ee472b7efcfd84c.png)

​![请添加图片描述](https://i-blog.csdnimg.cn/direct/1f18a8011de1497282fa81ef7d3fe2f6.png)


## 绘制曲线

以下是使用`matplotlib`绘制折线图的示例代码：

```python
def draw_liners(*args, labels: list[str] = ['X', 'Y'], label_color: list[str] = ['#000000', '#000000'],
                img_name: str = 'Liner', is_grid: bool = False, imgs_path: str = './imgs',
                is_save: bool = False, is_display: bool = True):
    """
    绘制线性图，支持多组数据和不同线型
    :param args: 包含(x, y, line_style, label)的元组列表
    :param labels: 坐标轴标签列表
    :param label_color: 坐标轴标签颜色列表
    :param img_name: 图形名称
    :param is_grid: 是否显示网格
    :param imgs_path: 保存路径
    :param is_save: 是否保存图片
    :param is_display: 是否显示图片
    example:
        x1 = [1, 2, 3, 4]
        y1 = [1, 4, 9, 16]
        x2 = [1, 2, 3, 4]
        y2 = [2, 4, 6, 8]
        draw_liners((x1, y1, '-', 'Line 1'), (x2, y2, '--', 'Line 2',), labels=['Time', 'Value'],
                    img_name='Multiple Curves', is_grid=True)
    """
    # 绘制每组数据
    for arg in args:
        # x轴数据,y轴数据,线型,标签
        x, y, line_style, label = arg
        plt.plot(x, y, linestyle=line_style, label=label)
    # 是否绘制网格
    if is_grid:
        plt.grid(True)

    plt.xlabel(labels[0], color=label_color[0])
    plt.ylabel(labels[1], color=label_color[1])
    plt.title(img_name)
    # 显示图例
    plt.legend()

    # 保存图片
    if is_save:
        # 如果文件名不带.png，则添加.png
        if not img_name.endswith('.png'):
            img_name += '.png'
        file_path = os.path.join(imgs_path, img_name)
        os.makedirs(os.path.dirname(file_path), exist_ok=True)
        plt.savefig(file_path)
    if is_display:
        plt.show()

```
![请添加图片描述](https://i-blog.csdnimg.cn/direct/728380ad4ca944658796a7a0d2f03528.png)

## 分图绘制曲线

以下是使用`matplotlib`在一个图形窗口中绘制多个子图的示例代码：

```python
def plot_subplots(*args, img_name: str = None, imgs_path: str = './imgs', is_save: bool = False,
                  is_display: bool = True):
    """
    绘制多个子图
    :param :
    *args: 可变参数，每个参数是一个元组，包含三个元素 (x_data, y_data, title,x_label,y_label)，
           其中 x_data 和 y_data 是子图的数据，title 是子图的标题,x_label是x轴名称,y_label是y轴名称。
    img_name: 图片名称,默认为None，结尾不带.png
    imgs_path: 图片保存路径，默认为None，默认保存在当前目录下
    is_save: 是否保存图片，默认为False
    param is_display: 是否显示图片
    example:
        x1 = np.linspace(0, 10, 100)
        y1 = np.sin(x)
        x2 = np.linspace(0, 10, 100)
        y2 = np.cos(x)
        plot_subplots((x1, y1, 'title1', 'x_label1', 'y_label1'), (x2, y2, 'title2', 'x_label2', 'y_label2'),
        img_name='subplots', imgs_path='./imgs', is_save=True)
    """
    num_plots = len(args)
    # 计算子图的行数和列数
    rows = int(np.ceil(np.sqrt(num_plots)))
    cols = int(np.ceil(num_plots / rows))

    # 创建一个图形和子图
    fig, axs = plt.subplots(rows, cols, figsize=(5 * cols, 4 * rows))
    # 处理只有一个子图的情况
    if num_plots == 1:
        axs = [axs]

    # 遍历所有子图的数据并绘制
    for (x_data, y_data, title, x_label, y_label), ax in zip(args, axs.flatten()):
        ax.plot(x_data, y_data)
        ax.set_title(title)
        ax.set_xlabel(x_label)
        ax.set_ylabel(y_label)

    # 隐藏多余的子图
    for ax in axs.flatten()[num_plots:]:
        ax.remove()

    # 调整子图间距
    plt.tight_layout()
    # 保存图片
    if is_save:
        # 如果文件名不带.png，则添加.png
        if not img_name.endswith('.png'):
            img_name += '.png'
        file_path = os.path.join(imgs_path, img_name)
        os.makedirs(os.path.dirname(file_path), exist_ok=True)
        plt.savefig(file_path)
    if is_display:
        plt.show()

```
![请添加图片描述](https://i-blog.csdnimg.cn/direct/82e3cb4c3b714b53bde1876719075834.png)


## 结论

结合使用`numpy`, `matplotlib`, 和`scipy`，Python提供了强大的数据可视化工具。无论是简单的折线图还是复杂的三维曲面，这些库都能帮助您以直观的方式展示数据。

- [matplotlib官方文档](https://matplotlib.org/)：了解更多绘图选项。
- [numpy文档](https://numpy.org/doc/stable/)：深入学习数组操作。
- [scipy文档](https://docs.scipy.org/doc/scipy/reference/)：探索科学计算功能。


