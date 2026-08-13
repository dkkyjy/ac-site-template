---
title: "与Claude进行计算机和浏览器操作的最佳实践 | Claude by Anthropic"
description: "---"
pubDate: "2026-05-13"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/best-practices-for-computer-and-browser-use-with-claude"
---

# 与Claude进行计算机和浏览器操作的最佳实践 | Claude by Anthropic

---

Claude的[最新模型](https://www.anthropic.com/news/claude-sonnet-4-6)在计算机和浏览器操作能力上迈出了重要一步。得益于这些特性，LLM现在能够驱动日益复杂的智能体系统，用于完成实际工作，如构建软件应用程序以及跨多种不相关技术自动化工作流程。

在这篇博文中，我们分享将Claude用于计算机和浏览器操作的最佳实践，涵盖从简单的配置更改到更高级的集成模式。我们希望本文能为您开始将Claude的计算机和浏览器操作能力集成到您的产品中提供帮助。我们还发布了一个新的[演示实现](https://github.com/anthropics/claude-quickstarts/tree/main/computer-use-best-practices)，它封装了其中一些最佳实践，并提供了在Claude计算机操作能力上进行开发时有用的额外工具。

*请注意，除非另有说明，这些建议适用于Claude 4.6系列（Opus 4.6、Sonnet 4.6、Haiku 4.5）和Claude Opus 4.7。若4.6系列与Opus 4.7的指导有差异，我们会在文中标明。我们的发现基于内部实验，随着新模型和技术的出现可能会在将来更新。*

# **入门指南：分辨率与缩放**

点击准确性是任何计算机操作集成的基础。如果点击不能落在应有的位置，那么下游的所有工作都会失败：表单无法填写，按钮无法按下，工作流程功亏一篑。性价比最高的优化也是最简单的一个：在将截图发送到API之前，先预降采样。

## **确保正确缩放**

当你向Claude的计算机操作API发送截图时，模型会看到它，并在你指定的 display\_width\_px / display\_height\_px 坐标空间中返回点击坐标。但有一个重要的约束：API对图像大小有内部处理限制。超过这些限制的图像会在模型看到之前被降采样，这意味着模型是基于降质后的图像点击的，而你的执行框架期望坐标与原始分辨率对齐。

对于我们的Claude 4.6系列模型，API的限制是：

* **最大长边：** 1568像素
* **最大总像素：** 1.15兆像素
* 超过**任一**限制的图像会被内部降采样

我们的Opus 4.7模型支持更高的分辨率。限制如下：

* **最大长边：** 2576像素
* **最大总像素：** 3.75兆像素
* 超过**任一**限制的图像会被内部降采样

当坐标空间与模型感知到的图像不匹配时，模型预测的点击会落在与实际看到的图像不同比例的显示上。这是高分辨率下点击不准确的主要原因。解决方法很简单：在将截图发送到API之前，始终将其降采样到这些限制之内。我们一致观察到，当图像超过限制时，准确性会显著下降，而这一单项更改的价值超过几乎所有其他优化。

## **推荐分辨率**

**从1280x720开始。** 对于大多数用例来说，这是一个安全实用的默认值。它使用约80%的像素预算，远低于长边和总像素限制，并且是模型在训练期间见过的标准分辨率。它同样适用于现代Web UI和传统桌面应用程序。

**如果你使用Opus 4.7，我们建议从1080p开始**，因为这相比720p带来了有意义的质量提升，并且在令牌使用和性能之间提供了良好的平衡。

**对于希望最大化模型接收的视觉信息的开发者，** 我们还推荐一种“最大API适配”方法：根据源图像的本机宽高比，为每张图像计算最佳分辨率：

```python
import math

# 4.6系列为1568，Opus 4.7为2576
MAX_LONG_EDGE = 1568

# 4.6系列为1.15MP，Opus 4.7为3.75MP
MAX_PIXELS = 1_150_000

def compute_max_api_fit(native_w, native_h):
    """计算适合API限制的最大分辨率，同时保持宽高比。"""
    aspect = native_w / native_h

    # 从像素预算计算最大尺寸
    h_from_pixels = math.sqrt(MAX_PIXELS / aspect)
    w_from_pixels = h_from_pixels * aspect

    # 应用长边约束
    if native_w >= native_h:
        w = min(w_from_pixels, MAX_LONG_EDGE)
        h = w / aspect
    else:
        h = min(h_from_pixels, MAX_LONG_EDGE)
        w = h * aspect

    # 绝不超过原生尺寸进行放大
    w = min(w, native_w)
    h = min(h, native_h)

    return int(w), int(h)
```

这种方法稍微复杂一些，但避免了宽高比失真，并充分利用了每张图像可用的像素预算。相比固定的1280x720，准确性提升不大，但它是一种直接的实现，避免了将16:9的源图像强制压入4:3显示分辨率时产生的失真。

**应避免的分辨率：**

* **原生分辨率（未缩放）：** 除非你的源图像恰好低于分辨率限制，否则发送原生分辨率的截图是最常见的导致点击准确性差的原因。
* **非常低的分辨率（低于960x540）：** 在低分辨率图像中，丢失的细节太多，模型无法准确识别小的UI元素。
* **如果在MacOS上：** 浏览器的常见问题是MacOS上的截图通常以设备像素比为2捕获，这意味着你可能得到分辨率是屏幕坐标2倍的图像。
* **如果你使用4.6系列，避免1920x1080及以上：** 这些超过像素限制，会被静默降采样。在Opus 4.7上，上限更高（3.75 MP），因此1080p和1440p在预算内；仍需避免在没有降采样的原生4K。

## **坐标缩放**

当你发送前调整截图大小时，模型在你指定的显示分辨率下返回点击坐标。你必须在执行点击之前将这些坐标缩放回你实际的屏幕分辨率：

```python
# 你的屏幕是 screen_w x screen_h
# 你发送的截图已调整为 display_w x display_h
scale_x = screen_w / display_w
scale_y = screen_h / display_h

screen_x = int(api_returned_x * scale_x)
screen_y = int(api_returned_y * scale_y)
```

这很简单但至关重要，因为如果你忘记缩放，或者 `display_width_px` / `display_height_px` 与发送的图像实际尺寸不匹配，那么每次点击都会出现一致的偏移。

## **消息数组中的内容顺序**

构建消息内容数组时，将文本指令放在*图像之前*，如下代码片段所示。这让模型在处理截图时知道它要找什么，从而提高点击准确性。

```python
# 推荐——先文本指令，后截图：
content = [
    {"type": "text", "text": "点击提交按钮"},
    {"type": "image", "source": {"type": "base64", "media_type": "image/png", "data": screenshot_b64}},
]

# 不推荐——先图像，后文本：
content = [
    {"type": "image", "source": {"type": "base64", "media_type": "image/png", "data": screenshot_b64}},
    {"type": "text", "text": "点击提交按钮"},
]
```

## **诊断点击问题**

如果点击偏离目标，通常归结为以下原因之一：

| 症状 | 可能的原因 | 尝试此方法 |
| --- | --- | --- |
| 点击朝一个方向持续偏移 | * `display_width_px` / `display_height_px` 与发送的实际图像尺寸不匹配 * 截图超过API限制并被静默降采样 * 内容顺序是图像在先而非文本在先 | * 确保显示尺寸与你调整后的截图完全匹配，而不是原生分辨率 * 预降采样至1280x720或使用 `compute_max_api_fit` * 将文本指令移到内容数组中的图像之前 |
| 点击落在大致区域但未命中目标 | * 目标非常小（复选框、图标、开关） * 源图像分辨率非常高（4K+），降采样过程中细节丢失 * 强制非原生宽高比导致的宽高比失真 | * 为密集UI启用 `enable_zoom: True` * 以较低DPI捕获，或在降采样前裁剪至相关屏幕区域 * 调整大小时保持源宽高比 |
| 模型点击了完全错误的元素 | * 指令模糊（当存在多个类似“提交”按钮时的“点击提交”） * 目标附近有视觉上相似的元素 * UI过于复杂，无法通过单一指令处理 | * 使用更具体的提示，包含位置信息（“点击表单右下角的蓝色提交按钮”） * 将复杂交互分解为更小的步骤 * 提供关于页面布局的额外上下文 |
| 整体准确性差 | * 截图的发送尺寸超过API限制 * 源图像来自极高分辨率显示（4K+），压缩比极端 * 分辨率过低，丢失关键细节 | * 将所有截图预降采样至限制内 * 对于4.6系列上的4K+源，Sonnet比Opus 4.6对重度降采样更鲁棒。Opus 4.7上这一差距基本消失，使用4.7的像素预算（最高3.75 MP），这样一开始所需的降采样就更少。 * 尝试1280x720作为基线；如果丢失过多，使用 `compute_max_api_fit` |

## **用于点击任务的模型选择**

根据我们的内部测试，Claude Sonnet 4.6在点击方面往往机械上更精确（更好的空间准确性，更少的接近失误），而Claude Opus 4.6则带来更强的推理能力。当源图像需要重度降采样时，Sonnet 4.6也更鲁棒。

Opus 4.7缩小了这一差距：通过测试，我们发现其点击精度大致与Sonnet 4.6相当，而其更高的分辨率预算减少了所需的降采样量，使其成为当你想要Opus级别的推理与强点击精度相结合时的有力选择。

对于大多数任务，我们建议从Sonnet 4.6开始，它在点击准确性、推理和成本之间提供了最佳平衡。当你需要更强的推理时，尤其是在使用高分辨率源图像时，选择Opus 4.7。当延迟是首要考虑因素时，Haiku 4.5仍然是一个出色的选择。高级工作流程仍然可以从编排器+子智能体模式中受益，即推理模型负责规划和决策，而Sonnet或Haiku执行机械点击步骤。

## **处理小目标**

点击准确性随着目标变小而下降。大型和中等UI元素（按钮、输入字段和标准菜单项）在安全区域内的所有分辨率下都是可靠的。挑战在于小型和微型目标，如复选框、系统托盘图标、下拉箭头、小开关和树形视图的展开/折叠按钮。

如果你的应用程序需要频繁点击小目标，请考虑以下策略：

**对密集UI使用缩放。** Claude 4.6和4.7模型支持缩放能力，允许模型在点击之前以更高分辨率检查特定屏幕区域。在你的[工具配置](https://platform.claude.com/docs/en/agents-and-tools/tool-use/computer-use-tool)中启用它：

```json
{
    "type": "computer_20251124",
    "name": "computer",
    "display_width_px": 1280,
    "display_height_px": 720,
    "enable_zoom": True
}
```

**使目标更大。** 如果你能控制被自动化的UI，增大点击目标的大小（即使只稍大一点）对可靠性有巨大影响。这可能意味着使用较低的系统DPI、在浏览器中放大，或调整UI缩放设置。

**对极小的目标使用键盘替代方案。** 对于非常小的元素，例如系统托盘图标或微型复选框，键盘快捷键或基于Tab的导航可能比点击更可靠。如果你的工作流程允许，提示模型对特定步骤使用键盘交互可以提高成功率。

**考虑源图像分辨率。** 从4K+显示器截屏压缩到720p会丢失大量细节（例如，一个16px的复选框在3840x2160原生下压缩到1280x720显示时大约变成5px，这使目标变得更小、更难命中）。如果你使用超高分辨率显示器，考虑使用Opus 4.7，它比之前的模型具有更高的分辨率限制。如果使用4.6模型，考虑以较低DPI捕获、使用显示缩放来放大UI元素，或将截图聚焦在屏幕的相关部分而不是整个显示器。由于这些模型用更少的像素表示更多的信息，我们观察到随着源图像比例的增加（即需要更多压缩），性能会下降。

## **我们测试过但没有帮助的方法**

我们在内部评估中试验了几种流行的优化技术，没有发现这些方法带来一致性的提升，尽管结果可能因具体情况而异：

* **将图像分割成较小的瓦片：** 将截图分成象限或区域并分别发送，并没有提高点击准确性。
* **叠加带坐标的网格图案：** 在截图上添加视觉坐标网格以帮助模型定位目标，没有产生可靠增益。
* **调整算法选择：** PIL LANCZOS、sips和其他常见的调整算法产生相同的结果。选择你技术栈中方便使用的即可。

## **检查失败**

如果在尝试上述修复后模型仍然行为不可预测，请记录完整的转录，并将预测的点击覆盖到源截图上，以理解模型实际看到和决定了什么。

有些失败根本不是点击准确性的问题。例如，某些下拉菜单可能会调用系统级UI，而浏览器视口无法捕获——模型看起来像是在任务上失败了，但它实际上只是看不到需要交互的菜单。在这种情况下，模型应该依赖替代方法，如JavaScript执行、键盘导航或直接DOM操作，而不是点击。

## **快速参考**

*如何为计算机操作缩放和准备图像*

```python
import math
from PIL import Image
import base64
import io

# 4.6系列为1568，Opus 4.7为2576
MAX_LONG_EDGE = 1568

# 4.6系列为1.15MP，Opus 4.7为3.75MP
MAX_PIXELS = 1_150_000

def prepare_screenshot(screenshot: Image.Image, native_w: int, native_h: int) -> tuple[str, int, int]:
    """将截图调整为适合API限制的尺寸，并返回base64和显示尺寸。"""

    # 选项A：固定720p（简单、可靠）
    display_w, display_h = 1280, 720

    # 选项B：最大API适配（最大化保真度）
    # display_w, display_h = compute_max_api_fit(native_w, native_h)

    resized = screenshot.resize((display_w, display_h), Image.LANCZOS)

    buffer = io.BytesIO()
    resized.save(buffer, format="PNG")
    b64 = base64.standard_b64encode(buffer.getvalue()).decode()

    return b64, display_w, display_h

def scale_coordinates(api_x: int, api_y: int, display_w: int, display_h: int,
                      screen_w: int, screen_h: int) -> tuple[int, int]:
    """将API返回的坐标缩放到原生屏幕空间。"""
    screen_x = int(api_x * (screen_w / display_w))
    screen_y = int(api_y * (screen_h / display_h))
    return screen_x, screen_y

def compute_max_api_fit(native_w: int, native_h: int) -> tuple[int, int]:
    """计算适合API限制的最大分辨率，同时保持宽高比。"""
    aspect = native_w / native_h
    h_from_pixels = math.sqrt(MAX_PIXELS / aspect)
    w_from_pixels = h_from_pixels * aspect

    if native_w >= native_h:
        w = min(w_from_pixels, MAX_LONG_EDGE)
        h = w / aspect
    else:
        h = min(h_from_pixels, MAX_LONG_EDGE)
        w = h * aspect

    w = min(w, native_w)
    h = min(h, native_h)
    return int(w), int(h)
```

**用法：**

```python
import anthropic
from PIL import Image

client = anthropic.Anthropic()

# 捕获截图（你的方法）
screenshot = Image.open("screenshot.png")
native_w, native_h = screenshot.size

# 准备给API
b64, display_w, display_h = prepare_screenshot(screenshot, native_w, native_h)

# 发送给Claude——文本在图像之前
response = client.beta.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    betas=["computer-use-2025-11-24"],
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "点击提交按钮"},
            {"type": "image", "source": {"type": "base64", "media_type": "image/png", "data": b64}},
        ]
    }],
    tools=[{
        "type": "computer_20251124",
        "name": "computer",
        "display_width_px": display_w,
        "display_height_px": display_h,
    }],
)

# 将坐标缩回到执行
api_x, api_y = extract_click_coords(response)  # 你的解析逻辑
screen_x, screen_y = scale_coordinates(api_x, api_y, display_w, display_h, native_w, native_h)
```

# **调整计算机操作中的思考投入**

Claude的最新模型支持[自适应思考](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking)，这是一种设置，让Claude决定在行动之前需要在中间步骤上推理多少。自适应思考不是手动设置思考令牌预算，而是让Claude根据每个请求的复杂度动态决定何时以及多少使用扩展思考。对于计算机操作，这意味着Claude可以思考它在屏幕上看到的内容，规划多步交互，并在提交点击或按键之前自我纠正。

通过自适应思考，Claude的思考深度由thinking参数控制，并带有投入水平：low、medium、high、xhigh（Opus 4.7上）和max。更多思考意味着每次行动更多推理，但也会产生更多输出令牌、更高延迟和更高成本。

自然的问题是：根据模型的不同，计算机操作的最佳思考投入是多少？

## **Claude Opus 4.7**

我们在一系列端到端UI自动化任务上测试了每个思考投入水平，这些任务涵盖桌面应用程序、浏览器和多应用工作流。

**Opus 4.7优于4.6系列。** 在OSWorld Verified基准测试中，我们发现Opus在相同令牌使用和投入设置下优于所有4.6系列模型。低投入的Opus 4.7得分与最大投入下的Sonnet 4.6相似，而每个任务使用的令牌数量约为其1/10。对于困难任务，Opus 4.7是明显的选择。

**将投入设置为`high`** 实现了接近最高的任务成功率，同时使用的输出令牌大约是`max`的一半。与Opus 4.6相比，low、medium和high使用大约相同数量的令牌，同时在OSWorld上提高了分数。在我们的内部测试中，最大投入使用了更多令牌并提供了最佳得分。下表列出了我们对何时使用每种思考投入水平的建议。

### **针对投入水平的建议**

| 场景 | 思考投入 | 原因 |
| --- | --- | --- |
| 大多数用例的默认值 | `high` | Opus 4.7最适合困难任务。使用high将为模型提供足够的推理来规划复杂的多步交互，同时不会显著增加令牌使用。 |
| 高吞吐量/注重成本 | `low` | 令牌使用更低，同时提供介于Opus 4.6的high和最大投入设置之间的质量。 |
| 简单的、定义良好的工作流程/最快 | 建议尝试Sonnet 4.6 | 如果低延迟是最高优先级时使用。对于UI一致且工作流程已知的短、可预测任务足够。 |
| 复杂的一次性任务 | `max` | 当任务极具挑战性且需要在第一次尝试时就做对时使用。 |

## **Claude 4.6模型**

我们在一系列端到端UI自动化任务上测试了每个思考投入水平，这些任务涵盖桌面应用程序、浏览器和多应用工作流。

两个模式脱颖而出：

**中等投入是甜点。** 将投入设置为medium实现了接近最高的任务成功率，同时使用的输出令牌大约是high的一半。超过中等后，性能几乎不再提升。值得注意的是，当任务被重试时，medium和high收敛到相同的成功率。这意味着高投入可能帮助模型在第一次尝试时就做好困难任务，但给定多次尝试，medium可能以更低成本同样可靠地完成。

**一点思考大有裨益。** low投入是一个出奇强大的选项。它实际上使用的总输出令牌比完全禁用思考还要少（模型犯的错误更少，需要的重试循环更少），同时匹配或略超无思考的准确性。这使其成为注重成本、高吞吐量工作流程的最佳选择。下表列出了我们的投入建议。

### **针对投入水平的建议**

| 场景 | 思考投入 | 原因 |
| --- | --- | --- |
| 大多数用例的默认值 | `medium` | 最佳的准确性成本比。为模型提供足够的推理来规划多步交互，而不会过度思考。通过重试，在令牌成本一半的情况下达到与high相同的性能。 |
| 高吞吐量/注重成本 | `low` | 比无思考更准确，但由于错误和重试减少，令牌使用更低。 |
| 简单的、定义良好的工作流程/最快 | 禁用思考 | 如果低延迟是最高优先级时使用。对于UI一致且工作流程已知的短、可预测任务足够。 |
| 复杂的一次性任务 | `high` | 当任务具有挑战性且需要在第一次尝试时就做对时使用。如果你的系统支持重试，medium可能达到相同的最终成功率。 |

我们建议不要为计算机操作使用`max`投入。在我们的测试中，它没有比`high`提供准确性优势，同时还增加了输出令牌成本。UI任务主要是感知性的，而不是深度逻辑性的，额外的推理预算要么未被使用，要么导致过度思考。请记住，随着模型的发展，这一建议将会改变。

## **中等投入水平的配置示例**

```python
import anthropic

client = anthropic.Anthropic()

response = client.beta.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=16000,
    betas=["computer-use-2025-11-24"],
    thinking={"type": "adaptive"},
    output_config={"effort": "medium"},
    messages=[...],
    tools=[
        {
            "type": "computer_20251124",
            "name": "computer",
            "display_width_px": 1280,
            "display_height_px": 720,
        }
    ],
)
```

## **为什么更多的思考并不总是有帮助**

UI自动化任务本质上不同于编码或数学问题。大多数计算机操作行为是感知和机械性的：识别正确的元素、点击正确的位置，而非深度逻辑性。思考在以下情况下最有用：

* 在开始前规划多步序列（例如，“我需要打开设置，导航到隐私，然后禁用跟踪”）
* 从未预期的UI状态中恢复（例如，出现了一个未被预料的对话框）
* 在屏幕上的内容与任务指令之间交叉引用信息
* 在专业软件上完成具有挑战性的项目

# **提高安全性：利用提示注入分类器**

*本节涵盖提示注入保护，如果你使用我们官方的计算机操作工具头，该保护默认提供且免费。但是，如果你有兴趣在自定义计算机或浏览器操作工具上启用此功能，请填写我们的* [*提示注入分类器兴趣表*](https://docs.google.com/forms/d/e/1FAIpQLSfXj6rXC-SUQEYHCLabwUe5JuYiYyJ29Ja-KP7EhLIPlyz0tw/viewform?usp=dialog)。

计算机操作智能体在设计上与不可信内容进行交互。Claude处理的每张截图、每个网页或应用程序UI都可能包含对抗性指令，包括隐藏文本、被操纵的图像、欺骗性UI元素或试图劫持智能体行为的社会工程尝试。这个攻击面与您控制输入的典型API集成有着根本不同。对于计算机操作而言，模型的输入是开放的互联网以及智能体正在导航的任何软件。

随着计算机操作智能体变得更加强大并更广泛地部署，提示注入成为相应更严重的风险。一个能够点击、输入和导航的智能体可以被操纵执行真实世界的操作，例如填写表单、下载文件或导航到恶意URL。针对这些攻击建立强大的防御对于任何生产部署都是必需的。

## **我们如何应对提示注入**

我们已经详细撰写了关于我们[针对浏览器和计算机操作的提示注入防御方法](https://www.anthropic.com/research/prompt-injection-defenses)。我们的防御策略在多个层次上运作：

**训练时鲁棒性。** 我们使用强化学习将提示注入抵抗能力直接构建到Claude的能力中。在训练期间，Claude会接触嵌入在模拟网页和应用程序UI中的注入内容，并在正确识别并拒绝遵循恶意指令时获得奖励。这意味着Claude的第一道防线就是模型本身，因为它在执行任务时已经学会了区分合法用户指令和对抗性内容。

**实时分类器。** 我们运行探针来扫描进入Claude上下文窗口的内容，并标记潜在的提示注入尝试。这些探针跨多种模态检测对抗性命令，例如隐藏在页面内容中的文本、嵌入在图像中的指令，以及旨在欺骗智能体的欺骗性UI元素，并在识别到攻击时调整Claude的行为。

**持续红队。** 我们的安全研究人员不断进攻这些防御，我们参与外部对抗性评估，以衡量针对不断演变的攻击技术的鲁棒性。

自我们最初的计算机操作研究预览以来，我们继续在所有三个层面大力投入。每一个新模型代次都融入了更强的训练时防御和更强大的分类器，并且我们扩大了红队评估所针对的攻击技术范围。

## **使用Claude内置的分类器**

当你通过API使用Claude的[官方计算机操作工具](https://docs.anthropic.com/en/docs/agents-and-tools/computer-use)时，提示注入分类器会在每个请求上自动运行。这些分类器与主模型推理并行运行，几乎为零的额外延迟和零额外成本添加到你的请求中。

你无需配置任何东西来启用这种保护。当你使用官方的 `computer_20251124` 工具类型时，它默认开启。分类器评估截图和其他内容是否存在提示注入迹象，并相应地影响Claude的响应。

```python
# 使用官方CU工具时，分类器自动运行——无需额外配置
tools = [
    {
        "type": "computer_20251124",
        "name": "computer",
        "display_width_px": 1280,
        "display_height_px": 720,
    }
]
```

## **如果你不使用官方计算机操作工具**

许多开发者使用自定义工具定义而不是官方的 `computer_20251124` 工具类型来构建计算机操作集成，例如定义他们自己的截图和点击工具。如果你的设置属于这种情况，上述内置分类器目前不会在你的请求上运行。

我们正在积极探索如何将提示注入保护扩展到这些自定义实现。如果你在没有官方工具类型的情况下构建计算机操作或浏览器操作集成，并且对提示注入分类器感兴趣，请[填写此兴趣表](https://docs.google.com/forms/d/e/1FAIpQLSfXj6rXC-SUQEYHCLabwUe5JuYiYyJ29Ja-KP7EhLIPlyz0tw/viewform?usp=dialog)，我们会在此功能可用时跟进。

## **无论是否使用分类器的最佳实践**

分类器只是一层防御，而非完整解决方案。我们建议对任何计算机操作部署采取以下做法：

**对高风险操作实施人工介入。** 让智能体在执行不可逆操作之前暂停并请求用户确认，例如提交表单、进行购买、发送消息或修改数据。无论分类器性能如何，这都是针对提示注入最有效的缓解措施。

**限定智能体的权限。** 限制智能体可以做什么。如果你的工作流程不需要文件下载，就不要给智能体访问文件下载的权限。如果不需要发送电子邮件，就不要给它访问邮件客户端的权限。减少成功注入的爆炸半径与防止注入本身同样重要。

**监控并记录智能体操作。** 记录智能体采取的完整操作序列，包括每一步的截图。这使你能够检测异常行为、审计出错时发生的事情，并建立反馈循环以不断提高系统的鲁棒性。

**将所有Web内容视为不可信。** 设计智能体的系统提示，明确区分用户指令和在任务执行过程中遇到的内容。提醒模型，网页、电子邮件或应用程序UI中的文本并非来自用户，不应被视为指令。

# **计算机操作中的上下文管理**

在构建计算机操作智能体时，截图快速累积。每个操作都会生成一张新图像，每张图像根据分辨率消耗大约1,000–1,800个令牌。在系统提示、工具定义和文本内容之后，一个200k的上下文窗口可以在远低于100张截图时就被填满。

管理好上下文有两个目标：1）保持总令牌数有界；2）保持提示缓存有效，这样你就不会重复为相同的前缀支付全价。我们发现，有效的[上下文管理](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)对长时间运行的智能体成本和延迟的影响几乎超过任何其他优化。本节涵盖三层干净组合的方法：放置缓存断点、在不破坏缓存的情况下修剪旧截图，以及在修剪不足时总结历史。

## **放置缓存断点**

提示缓存只有在断点落在跨轮次重复出现的内容上时才有效。API总共支持四个缓存断点。将所有四个断点放在一个稳定的前缀（系统提示、工具定义）上会浪费它们，因为该前缀已经被命中一次且从未失效，所以一个断点就够了。其余三个更值得花在最近的历史上，那里的失效风险最高，并且在长时间会话中节省会累积。

我们建议：

* **一个断点在系统提示或尾部工具定义上。** 这个前缀在会话中很少变化。
* **最多三个额外的断点放在最近的工具结果上**，每轮推进并清除上一轮的断点，这样就不会超过四个断点的限制。

将断点分布在最近的位置上可以实现优雅降级。如果你最近的断点被失效（例如由于图像修剪、压缩或工具定义更改），较早的断点仍然可能命中，这样你只需支付完整输入成本的10%，而不是100%。

*缓存控制和设置断点示例：*

```python
def set_trailing_cache_control(messages, max_breakpoints=3):
    """在最近的tool_result块上放置最多`max_breakpoints`个临时缓存控制标记，
    同时清除任何已有的标记。"""
    for msg in messages:
        for block in msg.get("content", []):
            if isinstance(block, dict):
                block.pop("cache_control", None)

    placed = 0
    for msg in reversed(messages):
        for block in reversed(msg.get("content", [])):
            if placed >= max_breakpoints:
                return
            if isinstance(block, dict) and block.get("type") == "tool_result":
                block["cache_control"] = {"type": "ephemeral"}
                placed += 1
```

## **方法1：滚动缓冲区（缓存感知）**

保持令牌数量有界的最简单方法是只保留最近N张截图，丢弃其余的。在每次API调用之前，遍历消息数组，用简短的占位符（例如文本块“\[图像已省略]”）替换较旧的图像块。

这种模式的朴素版本是当截图过期时逐个丢弃，这会在每一轮改变前缀并持续使提示缓存失效。这就是滚动缓冲区因为破坏缓存而声名狼藉的原因。解决办法是批量修剪，使得前缀在几轮中保持字节一致，然后失效一次，再保持稳定。

我们测试过的一个具体模式是：

1. 保留最近的 keep\_n 张完整分辨率的截图。
2. 一旦总截图数量超过 keep\_n + interval，在单次传递中最旧的 interval 张截图替换为占位符。
3. 在修剪事件之间，消息数组跨轮次字节一致，因此你的缓存断点持续命中。

开始时的合理默认值：keep\_n = 3，interval = 25。这些是可调的，更高的 interval 意味着更少的修剪事件（更好的缓存效率）但上下文中保留更多完整分辨率截图（更多令牌）。在代表性的轨迹上测量缓存命中率和总输入令牌，并相应调整。

*修剪旧截图同时保留缓存断点的示例：*

```python
def prune_old_screenshots(messages, keep_n=3, interval=25):
    """批量将较旧的截图替换为文本占位符。
    仅当总数量超过 keep_n + interval 时才修剪，使得消息前缀在修剪之间保持
    `interval` 轮的字节稳定。"""
    image_positions = [
        (msg_idx, block_idx)
        for msg_idx, msg in enumerate(messages)
        for block_idx, block in enumerate(msg.get("content", []))
        if isinstance(block, dict) and block.get("type") == "image"
    ]
    if len(image_positions) <= keep_n + interval:
        return messages

    to_prune = image_positions[:-keep_n][-interval:]
    for msg_idx, block_idx in to_prune:
        messages[msg_idx]["content"][block_idx] = {
            "type": "text",
            "text": "[Image omitted]",
        }
    return messages
```

滚动缓冲区仍然有一个真正的限制：缓冲区之外的任何内容都会丢失。原始指令、智能体已经尝试过的操作以及它在任务中的位置，都在被修剪的截图中消失了。对于短任务（约50个操作以下），这没问题。对于更长的任务，将其与压缩结合使用。

## **方法2：基于LLM的压缩**

与其静默丢弃旧图像，不如在丢弃之前总结整个对话。摘要保留了发生了什么、用户要求什么、已完成什么以及从何处继续。同时保留几张最近的截图，以便智能体可以看到它当前在查看什么。

压缩和缓存感知的滚动缓冲区是互补的。使用滚动缓冲区来逐轮控制令牌增长；偶尔使用压缩来回收窗口的其余部分，而不会丢失较早的上下文。每个压缩事件本身都会导致缓存失效，因此你希望它很少发生，而不是每几轮一次。

### **总结提示**

此示例提示提供了一个框架，每个部分针对特定的失效模式。提示必须捕获智能体继续任务所需的所有内容，而无需重新阅读原始对话，如下例所示：

```python
COMPACT_PROMPT = """你的任务是创建这个对话的详细摘要，该摘要将替换对话历史。
智能体将仅使用此摘要和几张最近的截图作为上下文继续工作。

关键：逐字保留所有用户指令。用户指令是最关键的元素。如果它们丢失了，
智能体将偏离任务。

在提供摘要之前，在<analysis>标签内分析对话：
1. 提取每条用户指令、要求和约束
2. 确定这是否是一个可重复的工作流程（例如，处理N个项目）
3. 按时间顺序追踪采取了哪些行动以及发生了什么

你的摘要必须包含以下部分：

1. 用户指令：
   - 完整的初始任务定义（尽可能逐字）
   - 所有具体的要求和标准
   - 每一条“不要”、“始终”、“必须”的指令
   - 任何改变方法的纠正或反馈

2. 任务模板（如果这是可重复的工作流程）：
   - 正在重复的模式
   - 每次迭代的决策标准
   - 标准工作流程步骤
   - 一个已完成迭代的示例

3. 约束和规则：
   - 所有用户指定的规则和限制
   - 发现的边缘情况和例外

4. 已采取的行动：
   - 访问的页面和交互的元素
   - 填写的表单和点击的按钮

5. 错误和修复：
   - 出了什么问题以及如何解决的
   - 失败的方法（这样就不会被重试）

6. 进度跟踪：
   - 已完成的项目 vs 剩余的项目
   - 在工作流程中的当前位置

7. 当前状态：
   - 当前应用程序、URL和域名（可选）
   - 重要的页面状态（已登录、表单进度等）

8. 下一步：
   - 应确切执行什么操作以继续
"""
```

在上面这个提示中，**用户指令**防止任务漂移：没有它们，智能体在压缩后会偏离方向。**任务模板**捕获可重复的模式，以便智能体在压缩后无需从头推导工作流程即可继续迭代。**约束和规则**保留在任务之前设置或过程中发现的限制和边缘情况，这样智能体不会违反它已知的规则。**已采取的行动**帮助跟踪过往进度。 **错误和修复**防止重试失败的方法（“我已经试过点击提交；直到勾选条款复选框之前它都不起作用”）。**进度跟踪**防止重启和跳过项目。**当前状态** & **下一步**提供了明确的重入点来继续。

### **服务端压缩（测试版）**

使用此提示最简单的方法是通过[服务端压缩](https://docs.anthropic.com/en/docs/build-with-claude/compaction)（测试版）让API处理压缩。将你自定义的总结提示作为 `context_management` 中的 `instructions` 参数传递，API会在输入令牌超过触发阈值时自动总结。`instructions` 参数完全替换默认的总结提示，因此模型将遵循上述部分。设置 `pause_after_compaction` 以在压缩事件中附加最近的消息（包括截图）。

*使用自动压缩工具的示例：*

```python
# 最小化——使用API默认值开启自动压缩
response = client.beta.messages.create(
    model="claude-opus-4-7",
    max_tokens=16000,
    betas=["compact-2026-01-12", "computer-use-2025-11-24"],
    context_management={"edits": [{"type": "compact_20260112"}]},
    messages=[...],
    tools=[...],
)

# 自定义——设置你自己的触发阈值和总结提示
response = client.beta.messages.create(
    model="claude-opus-4-7",
    max_tokens=16000,
    betas=["compact-2026-01-12", "computer-use-2025-11-24"],
    context_management={
        "edits": [
            {
                "type": "compact_20260112",
                "trigger": {"type": "input_tokens", "value": 150_000},
                "instructions": COMPACT_PROMPT,
            }
        ]
    },
    messages=[...],
    tools=[...],
)
```

### **在客户端截断以匹配服务端**

当API运行服务端压缩时，它会替换自身一侧的压缩前内容，但你的本地消息数组仍然保留完整历史记录。如果你在后续每一轮继续发送完整历史，你会为服务端不再需要的令牌付费，此外你的滚动缓冲区修剪器将基于与服务器实际看到的不同消息切片进行操作，这可能会破坏你精心维护的缓存稳定前缀。

解决办法是在客户端镜像服务端的截断，如下代码片段所示。当响应报告发生了压缩时，在下一轮之前从本地消息数组中删除压缩标记之前的所有内容。这使客户端和服务端视图保持一致，并让滚动缓冲区继续正常工作。

```python
def truncate_to_last_compaction(messages, response):
    """如果服务端在这一轮进行了压缩，则本地删除压缩前的消息，
    以便下一轮的缓存前缀与服务器看到的一致。"""
    context_mgmt = getattr(response, "context_management", None)
    if not context_mgmt or not context_mgmt.get("applied_edits"):
        return messages

    compaction = next(
        (e for e in context_mgmt["applied_edits"] if e["type"] == "compact"),
        None,
    )
    if compaction is None:
        return messages

    keep_from = compaction["message_index_after_compaction"]
    return messages[keep_from:]
```

## **客户端压缩**

如果你使用的模型不支持服务端压缩，或者你想要完全控制，可以在客户端使用相同的提示实现压缩。在每次API调用后，从响应 usage 字段检查总输入令牌数。当超过某个阈值（例如上下文窗口的90%）时，将对话发送给一个使用 COMPACT\_PROMPT 作为系统提示的总结模型。用摘要加上几张最近的截图替换消息历史，然后继续智能体循环。

## **综合应用**

一个长时间运行的计算操作智能体的良好默认配置如下：

* 一个缓存断点在稳定前缀上，三个在尾部工具结果上，每轮清除并重新放置。
* 缓存感知的滚动缓冲区，保持 keep\_n = 3，interval = 25，分批用占位符替换较旧的截图。
* 使用自定义提示，在大约150k输入令牌时触发服务端压缩，加上客户端截断步骤以保持两个视图一致。

有了这三层，一个典型的长时间CU会话将在绝大多数轮次中命中提示缓存，将总输入令牌控制远低于上下文窗口，并通过压缩事件保留足够的历史记录，使智能体不会丢失任务线索。

# **改进计算机和浏览器操作的实验性设置**

以下模式是我们正在实施中测试的技术，它们显示出前景但还不是全面推荐。每种模式都以复杂性或成本为代价，换取特定类型工作负载的潜在提升。我们将其包含在此，以便你可以在自己的工作流中尝试，但预计本节中的指导将迅速发展。

## **批量工具**

在更新的参考实现中，我们在标准计算机和浏览器工具之外提供了两个工具：`computer_batch` 和 `browser_batch`。每个都接受一个子操作列表，并在单个工具调用中执行它们。例如，而不是分开发送点击、输入和按键轮次，模型可以发出一个包含所有三个操作的 computer\_batch 调用。

吸引力在于效率：一个有N个机械操作的工作流只需一次往返而不是N次，这在长时间任务上有意义地减少了挂钟时间和输出令牌消耗。风险是错误累积：如果操作2依赖于操作1改变的视觉状态，而操作1错过了，那么批处理的其余部分将基于过时的假设运行，智能体可能在从未看到实际状态的截图的情况下偏离方向。

我们建议在子操作是自包含且不依赖于彼此视觉结果的情况下使用批量工具（填写表单中的多个字段、链接键盘快捷键、滚动并点击已知目标）。我们建议在探索性导航、错误恢复序列或任何“如果操作1失败我需要重新规划”是真实状态的工作流中避免使用它们。

由于批量工具是你自己的自定义定义，它们可以与标准计算机或浏览器工具干净地堆叠。两者都保持可用，让模型选择。

## **顾问工具（测试版）**

[顾问工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool)将一个执行模型与一个更高智能的顾问模型配对，执行模型可以在生成过程中咨询顾问模型以获得战略指导。执行模型运行循环，当遇到需要更深推理的问题时，它调用顾问，接收计划或修正，然后继续。这在服务端单次请求内完成，你这一侧无需额外往返。

具体到计算机操作，这种模式在长时间任务中最有用，其中大多数轮次是机械点击，但偶尔的规划时刻（选择要打开哪个标签页、从未预期的模态框中恢复、决定是否放弃某个策略）受益于Opus级别的推理。你可以获得接近顾问单独运行的质量，而大部分令牌生成以执行模型的费率发生。

*启用顾问工具的示例：*

```python
response = client.beta.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=16000,
    betas=["advisor-tool-2026-03-01", "computer-use-2025-11-24"],
    tools=[
        {
            "type": "advisor_20260301",
            "name": "advisor",
            "model": "claude-opus-4-7",
        },
        {
            "type": "computer_20251124",
            "name": "computer",
            "display_width_px": 1280,
            "display_height_px": 720,
        },
    ],
    messages=[...],
)
```

顾问工具的有用控制包括：

* **`max_uses`：** 限制每个请求中顾问调用的次数。当你想限制最坏情况成本时很有用。
* **执行框架中的会话级上限：** 顾问每次咨询按Opus 4.7费率计费，因此在非常长的会话中，你可能希望在达到一定使用次数后停止提供顾问。
* **顾问侧缓存：** 在多轮对话中，缓存顾问的前缀在大约三次咨询后就会产生收益。在参考实现中，我们默认使用5分钟的临时缓存。

两个不明显的值得了解的事情：顾问工具在没有工具和没有上下文管理的情况下运行，因此它不能代你点击或浏览，只返回文本建议。而且由于执行模型在长时间任务中并不总是记得顾问的存在，请参阅下面的提醒提示部分。

## **清理孤立顾问块**

当顾问工具触发时，执行模型会发出一个 `server_tool_use` 块，其中包含 name: "advisor"，随后是返回内容中的 `advisor_tool_result` 块。这些块与其他内容一起存在于你的消息数组中。

如果你之后从工具数组中移除顾问工具——因为达到了会话级上限、更改了配置或切换了模型——那么之前的 `server_tool_use` / `advisor_tool_result` 块就会变成孤立的。API会在下一个请求上返回400，因为引用的工具不再被声明。

解决方法是简单的发送前处理：每当在一轮中禁用顾问时，遍历消息历史并移除类型为 `server_tool_use`（且name为"advisor"）和 `advisor_tool_result` 的所有内容块。

*移除陈旧顾问块的示例：*

```python
def strip_orphaned_advisor_blocks(messages):
    """从历史中移除顾问 server_tool_use / tool_result 块。
    在未包含顾问工具的任何请求之前调用此函数。"""
    for msg in messages:
        content = msg.get("content")
        if not isinstance(content, list):
            continue
        msg["content"] = [
            block for block in content
            if not (
                isinstance(block, dict)
                and (
                    (block.get("type") == "server_tool_use"
                     and block.get("name") == "advisor")
                    or block.get("type") == "advisor_tool_result"
                )
            )
        ]
    return messages
```

## **定期提醒提示**

在长时间会话中，执行模型可能会忘记哪些工具可用或应优先使用哪些工具。两个简短的提醒模式在我们的测试中有所帮助：

**批量提醒。** 如果你在标准工具之外还暴露了 `computer_batch` 或 `browser_batch`，并且观察到模型在适合批量处理时却链式调用单动作调用，可以在下一个工具结果后附加一个短的系统级提示：“记住，你可以使用 `computer_batch` 将不依赖中间截图的连续操作组合到单个工具调用中。”目标是引导模型回归批量处理，而不具体规定何时使用。

**顾问提醒。** 顾问工具很容易被执行模型遗忘，特别是如果它已经多个轮次没有被调用。在超过约20轮没有顾问调用的会话中，附加一个简短的提醒，表明顾问可用于规划或修正方向。在参考实现中，我们使用20轮的周期并附加一行提示。

两种提醒都是轻量级的上下文注入，而不是系统提示重写。每次附加的成本只有几十个输入令牌。如果你的系统提示已经很长或缓存断点已经精心放置，需要权衡这种提升是否值得增加的失效风险。

## **参考实现中的调试模式**

当某些行为异常而你无法确定问题是出在执行框架、截图还是模型时，在开始添加日志记录之前，参考实现中有三个侧面实用工具值得尝试：

* **轨迹查看器（streamlit run viewer/app.py）。** 加载记录的轨迹，让你逐步查看智能体的轮次，包括截图、思考、工具调用和每步的使用情况。最适合回答“模型实际看到了什么，它决定了什么？”
* **工具调试面板（uvicorn debug.server:app --reload）。** 一个小型Web UI，允许你单独测试每个工具：截图、捕获点击坐标、输入、滚动、缩放。有助于确认你的捕获管道和坐标缩放确实产生了你期望的结果。
* **定位沙盒（uvicorn localize.server:app --reload --port 8001）。** 上传任意图像并让模型指向一个目标。将预测的坐标以显示分辨率和原生分辨率渲染回图像上。这是诊断点击失误是调整大小错误、坐标缩放错误还是真正的模型错误的最快方法。当客户报告点击不良而你想在隔离环境中复现失败时，这尤其有用。

这些都不是构建工作集成所必需的；它们是在默认反馈循环（记录、重跑、眯眼看转录）不够快时的调试辅助工具。

## **提高可靠性：教Claude**

与其在文本提示上不断迭代直到Claude正确完成工作流，你可以直接向它展示正确的行为。录制自己执行任务的过程，捕捉每一步的截图、操作以及可选的语音旁白，然后当Claude执行相同工作流时将该演示作为上下文回放。录制的内容成为一个可重用的规范，Claude可以遵循，同时适应实时UI状态的差异。

我们在Claude in Chrome内部使用这种模式（我们称之为“教模式”），并在此分享，因为底层方法对于任何构建计算机操作或浏览器操作产品的人都广泛有用。它在两方面有帮助：提高Claude基本能处理但偶尔出错的工件的可靠性，以及解锁从文本提示完全无法完成的全新工作流。核心理念（捕获演示，作为上下文反馈）实现简单，并且能很好地适应浏览器和桌面环境。

### **核心理念：展示，而非告知**

传统的提示工程要求用户用语言描述他们想要什么，然后在AI误解时反复迭代。这种模式反过来：用户演示任务，系统记录他们的操作、截图和（可选的）语音旁白。在回放过程中，Claude收到完整的演示作为上下文，并遵循相同的步骤序列，同时适应当前UI状态的任何差异。

关键洞察是回放不是严格的重放。Claude在推理实时环境的同时将演示作为指导。如果按钮移动了或菜单重新组织了，Claude可以在当前UI中找到等效元素，而不是盲目点击记录的坐标。

### **数据模型**

基本单元是“工作流步骤”，即录制过程中捕获的单个操作。每个步骤捆绑了做了什么、发生在哪里以及屏幕当时的样子：

```python
from dataclasses import dataclass, field
from typing import Literal, Optional

@dataclass
class WorkflowStep:
    action: Literal["click", "type", "navigate", "scroll", "select"]
    description: str                         # 人类可读，例如“点击提交按钮”
    timestamp: float
    selector: Optional[str] = None           # CSS选择器或XPath
    coordinates: Optional[dict] = None       # {"x": int, "y": int}
    url: Optional[str] = None
    screenshot: Optional[str] = None         # Base64编码截图
    viewport_dimensions: Optional[dict] = None  # {"width": int, "height": int}
    speech_transcript: Optional[str] = None  # 语音旁白（如果捕获）
    value: Optional[str] = None              # 用于输入操作

@dataclass
class SavedWorkflow:
    id: str
    name: str                                # 例如“提交费用报告”
    steps: list[WorkflowStep] = field(default_factory=list)
    description: Optional[str] = None        # AI生成的工作流摘要
    start_url: Optional[str] = None
    created_at: float = 0.0
    usage_count: int = 0
```

有意同时捕获选择器和坐标：选择器对布局变化更鲁棒，但坐标提供了当选择器失效时Claude可以使用的视觉后备。存储视口维度，以便在回放环境与录制环境不同时可以缩放坐标。

### **录制：捕获什么**

至少，捕获点击事件、键盘输入、导航更改和每次操作的截图。对于每次点击，生成人类可读的描述（从aria标签、文本内容或通过快速Claude调用），并在点击位置用视觉标记注释截图：

```python
def on_click(event):
    step = WorkflowStep(
        action="click",
        selector=generate_selector(event.target),
        coordinates={"x": event.client_x, "y": event.client_y},
        url=current_url(),
        description=generate_description(event.target),
        timestamp=now(),
        viewport_dimensions=get_viewport_size(),
    )
    # 在截图点击位置用圆圈注释
    screenshot = capture_screenshot()
    step.screenshot = annotate_with_circle(screenshot, event.client_x, event.client_y)
    workflow_steps.append(step)
```

注释（点击位置的有色圆圈）有两个目的：帮助用户验证录制捕获到了正确的元素，并在回放时向Claude精确显示操作发生的位置。你的回放提示应说明这些标记是录制的产物，而非实时UI的一部分。

### **回放：构建提示**

这是最重要的一部分。当用户触发已保存的工作流时，你构建一条给Claude的消息，包含三样东西：用户的意图、解释演示格式的上下文块以及录制的截图。

上下文块告诉Claude如何解释带注释的截图，以及当实时UI不同时如何适应：

```python
def generate_playback_context(steps: list[WorkflowStep]) -> str:
    steps_description = "\n".join(
        f"步骤 {i+1}: {step.description}"
        for i, step in enumerate(steps)
    )

    return f"""<demonstration_context>
用户已录制了一个演示，展示如何执行此任务。

录制步骤：
{steps_description}

关于这些截图：
- 每张截图显示执行操作时的屏幕状态
- 蓝色圆圈标记用户点击的位置——这是录制注释
- 蓝色高亮并非实际界面的一部分
- 你自己的截图不会有这些标记

如何使用此演示：
1. 查看所有步骤和截图以理解完整的工作流
2. 自己截图以查看当前页面状态
3. 蓝色高亮显示要交互的元素——在你的当前视图中找到它
4. 遵循相同的操作序列，适应任何差异
5. 如果UI已显著变化，运用判断找到等效元素
</demonstration_context>"""
```

然后组装完整的消息，包含用户提示、上下文块以及每一步的截图作为图像：

```python
import anthropic

client = anthropic.Anthropic()

content = [
    {"type": "text", "text": user_prompt},
    {"type": "text", "text": generate_playback_context(workflow.steps)},
]

for i, step in enumerate(workflow.steps):
    if step.screenshot:
        content.append({"type": "text", "text": f"[步骤 {i+1}: {step.description}]"})
        content.append({
            "type": "image",
            "source": {"type": "base64", "media_type": "image/jpeg", "data": step.screenshot},
        })

response = client.beta.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    betas=["computer-use-2025-11-24"],
    messages=[{"role": "user", "content": content}],
    tools=[{
        "type": "computer_20251124",
        "name": "computer",
        "display_width_px": 1280,
        "display_height_px": 720,
    }],
)
```

### **回放模式**

并非每个工作流都需要相同程度的遵循录制演示。有些工作流太长，消耗大量输入令牌，最终会影响延迟和增加成本。考虑在上下文提示中支持一个严格度参数：

**严格：** 严格执行步骤；如果UI变化太大则停止并报告。适用于合规敏感的工作流，其中精确顺序很重要。

**自适应：** 以演示为指导但适应UI变化。这是大多数用例的最佳默认值——它能优雅地处理布局微调、更新按钮标签和重新组织的菜单。

**目标导向：** 关注最终结果；将录制步骤视为提示而非指令。当UI频繁变化但目标保持不变时有用。使用一个模型来总结录制的演示，使用类似于下一节所述的策略，然后将该摘要传递给CU模型。

### **示例：端到端费用报告工作流**

以下是已保存工作流的实际示例。该工作流捕获五个步骤：导航到费用表单、选择费用类型、从下拉菜单中选择“差旅”、输入金额和点击提交。

```python
expense_workflow = SavedWorkflow(
    id="wf_abc123",
    name="提交费用报告",
    start_url="https://expenses.company.com/new",
    steps=[
        WorkflowStep(
            action="navigate",
            url="https://expenses.company.com/new",
            description="导航到新费用表单",
            timestamp=1700000000,
        ),
        WorkflowStep(
            action="click",
            selector="#expense-type-dropdown",
            coordinates={"x": 400, "y": 200},
            description="点击费用类型下拉菜单",
            timestamp=1700000001,
        ),
        WorkflowStep(
            action="click",
            selector="[data-value='travel']",
            coordinates={"x": 400, "y": 280},
            description="选择“差旅”费用类型",
            timestamp=1700000002,
        ),
        WorkflowStep(
            action="type",
            selector="#amount-input",
            value="150.00",
            description="输入费用金额",
            timestamp=1700000003,
        ),
        WorkflowStep(
            action="click",
            selector="#submit-expense-btn",
            coordinates={"x": 1150, "y": 420},
            description="点击提交按钮",
            speech_transcript="现在我将点击提交以发送报告进行审批",
            timestamp=1700000004,
        ),
    ],
)
```

当用户随后说“提交我团队午餐的费用报告（85.50美元）”时，回放服务会构建一个包含演示上下文、所有五张带注释截图以及新请求中特定值的提示。Claude看到确切要点击的位置、要遵循的顺序，并调整金额和描述以匹配当前任务。如果你的工作流对于这种方法因输入令牌数过多而不切实际，请考虑先压缩工作流再将其用作示例。请参阅下一节关于管理上下文的提示。

# **计算机和浏览器操作入门**

这些实践反映了我们当前对如何使计算机操作集成在生产中可靠的最佳理解。它们适用于Claude 4.6系列模型和Opus 4.7，随着新模型和技术的出现将会更新。

随着你的集成趋于成熟，最重要的模式将取决于你的特定环境、目标应用程序和可靠性要求。

*从* [*计算机操作文档*](https://platform.claude.com/docs/en/agents-and-tools/tool-use/computer-use-tool) *开始，查看我们新的* [*演示实现*](https://github.com/anthropics/claude-quickstarts/tree/main/computer-use-best-practices) *以应用这些最佳实践，或者重新阅读* [*原始计算机操作研究文章*](https://www.anthropic.com/news/developing-computer-use) *以了解这些能力是如何构建的以及它们的发展方向。*

*致谢：本文及相应的演示由Lucas Gonzalez和Luca Weihs编写。作者感谢Molly Vorwerck、Javier Rando、Maya Nielan、Gabe Mulley和Brigit Brown的贡献。*
