---
title: "使用Claude进行计算机和浏览器操作的最佳实践"
description: ""
pubDate: "2026-05-13"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/best-practices-for-computer-and-browser-use-with-claude"
---
**日期:** 2026-05-13 00:00 UTC
**链接:** https://claude.com/blog/best-practices-for-computer-and-browser-use-with-claude
box-sizing: border-box;
}
html,
body {
overflow-x: hidden;
}
}
margin: 0 !important;
}
overflow-x: auto !important;
-webkit-overflow-scrolling: touch;
}
table-layout: fixed !important;
}
}
}
}
overflow-wrap: anywhere;
word-break: break-word;
white-space: normal;
}
min-width: 0 !important;
}
}
overflow-x: auto;
-webkit-overflow-scrolling: touch;
}
}
Claude的[最新模型](https://www.anthropic.com/news/claude-sonnet-4-6)在计算机和浏览器操作能力方面迈出了重要一步。由于这些功能，LLM现在能够驱动日益复杂的智能体系统，这些系统可以执行实际工作，如构建软件应用程序和跨多种不同技术自动化工作流程。
在这篇博客文章中，我们分享了使用Claude进行计算机和浏览器操作的最佳实践，涵盖从简单的配置更改到更高级的集成模式。我们希望这篇文章能帮助您开始将Claude的计算机和浏览器操作能力集成到您的产品中。我们还发布了一个新的[演示实现](https://github.com/anthropics/claude-quickstarts/tree/main/computer-use-best-practices)，它封装了其中一些最佳实践，并提供了额外的工具，可用于在Claude的计算机操作能力之上进行开发。
*请注意，这些建议适用于Claude 4.6系列（Opus 4.6、Sonnet 4.6、Haiku 4.5）和Claude Opus 4.7，除非另有说明。如果4.6系列和Opus 4.7之间的指导有所不同，我们会在文中内联指出。我们的发现基于内部实验，并可能随着新模型和技术的出现而更新。*
点击精度是任何计算机操作集成的基础。如果点击没有落在应有的位置，下游的一切都会失败：表单无法填写，按钮无法按下，工作流程也会失败。单一影响最大的优化也是最简单的之一：在将截图发送到API之前，预先降低其分辨率。
当您向Claude的计算机操作API发送截图时，模型会看到它，并在您指定的display_width_px / display_height_px坐标空间中返回点击坐标。但有一个重要的限制：API对图像大小有内部处理限制。超过这些限制的图像会在模型看到之前被降采样，这意味着模型是基于降级后的图像进行点击，而您的工具链期望坐标与原始分辨率对齐。
对于我们的Claude 4.6模型系列，API的限制是：
* **最大长边**：1568像素
* **最大总像素**：1.15百万像素
* 超过**任一**限制的图像会被内部降采样
我们的Opus 4.7模型支持更高的分辨率。限制是：
* **最大长边**：2576像素
* **最大总像素**：3.75百万像素
* 超过**任一**限制的图像会被内部降采样
当坐标空间与模型感知到的图像不匹配时，模型预测的点击会落在与实际看到的图像不同的显示比例上。这是高分辨率下点击不准确的主要原因。解决方法很简单：在发送到API之前，始终将截图降采样到这些限制范围内。我们一致观察到，当图像超过限制时，精度会显著下降，而这一单一更改的价值几乎超过任何其他优化。
**从1280x720开始。** 对于大多数用例来说，这是一个安全、实用的默认值。它使用了大约80%的像素预算，完全在长边和总像素限制之内，并且是模型在训练期间见过的标准分辨率。它适用于现代Web UI和传统桌面应用程序。
**如果您使用Opus 4.7，我们建议从1080p开始**，因为这比720p带来了有意义的质量提升，并在令牌使用和性能之间提供了良好的平衡。
**对于希望最大化模型接收的视觉信息的开发者**，我们还推荐一种"最大API适配"方法：根据源图像的原生宽高比计算每张图像的最佳分辨率：
```
import math
MAX_LONG_EDGE = 1568
MAX_PIXELS = 1_150_000
def compute_max_api_fit(native_w, native_h):
    """计算符合API限制的最大分辨率，同时保持宽高比。"""
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
这种方法稍微复杂一些，但避免了宽高比失真，并为每张图像使用了完整的像素预算。与固定的1280x720相比，精度提升并不显著，但它是一种直接的实现方式，避免了将16:9源图像强制转换为4:3显示分辨率时产生的失真。
**应避免的分辨率：**
* **原生分辨率（未缩放）**：除非您的源图像恰好低于分辨率限制，否则发送原生分辨率截图是点击精度差的最常见原因。
* **非常低的分辨率（低于960x540）**：在低分辨率图像中，丢失的细节太多，模型无法准确识别小的UI元素。
* **如果在MacOS上：** 浏览器操作的一个常见问题是，MacOS上的截图通常以设备像素比2捕获，这意味着您最终得到的图像可能是屏幕坐标分辨率的两倍。
* **如果您使用4.6系列，避免1920x1080及以上：** 这些分辨率超过像素限制，会被静默降采样。在Opus 4.7上，上限更高（3.75 MP），因此1080p和1440p在预算内；但仍需避免未降采样的原生4K。
当您在发送前调整截图大小时，模型会在您指定的显示分辨率中返回点击坐标。您必须将这些坐标缩放回您的实际屏幕分辨率，然后才能执行点击：
```
# 您的屏幕是 screen_w x screen_h
# 您发送的截图已调整为 display_w x display_h
scale_x = screen_w / display_w
scale_y = screen_h / display_h
screen_x = int(api_returned_x * scale_x)
screen_y = int(api_returned_y * scale_y)
```
这很简单但至关重要，因为如果您忘记缩放，或者`display_width_px` / `display_height_px`与您发送的图像的实际尺寸不匹配，那么每次点击都会出现一致的偏移。
在构建您的消息内容数组时，将文本指令放在*图像之前*，如下面的代码片段所示。这可以让模型在处理截图时知道它在寻找什么，从而提高点击精度。
```
# 推荐——先文本指令，后截图：
content = [
    {"type": "text", "text": "点击提交按钮"},
    {"type": "image", "source": {"type": "base64", "media_type": "image/png", "data": screenshot_b64}},
]
content = [
    {"type": "image", "source": {"type": "base64", "media_type": "image/png", "data": screenshot_b64}},
    {"type": "text", "text": "点击提交按钮"},
]
```
如果点击未命中目标，通常可以归结为以下原因之一：
| 症状 | 可能的原因 | 尝试此方法 |
| --- | --- | --- |
| 点击始终朝一个方向偏移 | * `display_width_px` / `display_height_px` 与发送的实际图像尺寸不匹配 * 截图超过API限制并被静默降采样 * 内容排序是图像优先而非文本优先 | * 确保显示尺寸与调整后的截图完全匹配，而非原生分辨率 * 预先降采样到1280x720或使用 `compute_max_api_fit` * 将文本指令移到内容数组中图像之前 |
| 点击落在大致正确的区域但未命中目标 | * 目标非常小（复选框、图标、切换开关） * 源图像分辨率非常高（4K+），降采样过程中细节丢失 * 强制使用非原生宽高比导致宽高比失真 | * 为密集UI启用 `enable_zoom: True` * 以较低的DPI捕获，或在降采样前裁剪到相关屏幕区域 * 调整大小时保留源图像的宽高比 |
| 模型点击了完全错误的元素 | * 指令模糊（当存在多个类似提交按钮时使用"点击提交"） * 目标附近有视觉上相似的元素 * UI过于复杂，单一指令无法处理 | * 使用更具体的提示，包含位置上下文（"点击表单右下角的蓝色提交按钮"） * 将复杂交互分解为更小的步骤 * 提供有关页面布局的额外上下文 |
| 整体精度差 | * 发送的截图超过API限制 * 源图像来自极高分辨率显示器（4K+），压缩比极端 * 分辨率过低，丢失关键细节 | * 预先降采样所有截图以符合限制 * 对于4.6系列的4K+源图像，Sonnet比Opus 4.6更能承受重度降采样。在Opus 4.7上，这一差距基本消失，使用4.7的像素预算（高达3.75 MP），因此一开始就不需要那么多降采样。 * 尝试1280x720作为基线；如果损失太大，使用 `compute_max_api_fit` |
根据我们的内部测试，Claude Sonnet 4.6在点击的机械精度方面往往更好（空间精度更高，接近失误更少），而Claude Opus 4.6则带来更强的推理能力。Sonnet 4.6在源图像需要重度降采样时也更加稳健。
Opus 4.7缩小了这一差距：通过测试，我们发现其点击精度大致与Sonnet 4.6相当，而其更高的分辨率预算减少了所需的降采样量，使其成为您想要Opus级别推理能力与强点击精度相结合时的有力选择。
对于大多数任务，我们建议从Sonnet 4.6开始，它在点击精度、推理能力和成本之间提供了最佳平衡。当您需要更强的推理能力时，尤其是使用高分辨率源图像时，选择Opus 4.7。当延迟是首要考虑因素时，Haiku 4.5仍然是一个优秀的选择。高级工作流程仍可能受益于编排器+子智能体模式，其中推理模型负责规划和决策，而Sonnet或Haiku执行机械的点击步骤。
随着目标变小，点击精度会下降。大和中等尺寸的UI元素（按钮、输入字段和标准菜单项）在安全区内的所有分辨率下都是可靠的。挑战在于小型和微型目标，如复选框、系统托盘图标、下拉箭头、小型切换开关和树状视图展开/折叠按钮。
如果您的应用程序涉及频繁点击小目标，请考虑以下策略：
**对密集UI使用缩放。** Claude 4.6和4.7模型支持缩放功能，允许模型在点击之前以更高分辨率检查特定屏幕区域。在您的[工具配置](https://platform.claude.com/docs/en/agents-and-tools/tool-use/computer-use-tool)中启用它：
```
{
    "type": "computer_20251124",
    "name": "computer",
    "display_width_px": 1280,
    "display_height_px": 720,
    "enable_zoom": True
}
```
**使目标更大。** 如果您控制要自动化的UI，增加点击目标的大小（即使是适度的）对可靠性有不成比例的影响。这可能意味着使用较低的系统DPI、在浏览器中放大或调整UI缩放设置。
**对微小目标使用键盘替代方案。** 对于非常小的元素，如系统托盘图标或微小的复选框，键盘快捷键或基于Tab的导航可能比点击更可靠。如果您的工作流程允许，提示模型在特定步骤使用键盘交互可以提高成功率。
**考虑源图像分辨率。** 从4K+显示器截取的图像压缩到720p会丢失大量细节（例如，在3840x2160原生分辨率下为16px的复选框，在1280x720显示分辨率下变为大约5px，这使得目标更小，因此更难命中）。如果您使用极高分辨率的显示器，请考虑使用Opus 4.7，它具有比先前模型更高的分辨率限制。如果使用4.6模型，请考虑以较低的DPI捕获，使用显示缩放来放大UI元素，或者将截图集中在屏幕的相关部分而不是整个显示器。由于这些模型用更少的像素表示更多信息，我们观察到性能随着源图像比例的增加而下降，这意味着需要更多的压缩。
我们在内部评估中试验了几种流行的优化技术，但没有发现这些方法带来一致的提升，尽管结果可能因具体情况而异：
* **将图像分解为更小的瓦片**：将截图分割为象限或区域并分别发送并没有提高点击精度。
* **叠加带坐标的网格图案**：在截图上添加视觉坐标网格以帮助模型定位目标并没有产生可靠的收益。
* **调整大小算法选择**：PIL LANCZOS、sips和其他常见的调整大小算法产生了相同的结果。使用任何对您的技术栈方便的方法。
如果在尝试上述修复后模型行为不可预测，请记录完整的对话记录，并将预测的点击叠加在源截图上，以了解模型实际看到和决定的内容。
有些失败根本不是关于点击精度的。例如，某些下拉菜单可能会调用系统级UI，而浏览器视口无法捕获这些UI——模型看起来似乎失败了任务，但它只是看不到需要交互的菜单。在这种情况下，模型应依赖替代方法，如JavaScript执行、键盘导航或直接文档对象模型（DOM）操作，而不是点击。
*如何为计算机操作缩放和准备图像*
```
import math
from PIL import Image
import base64
import io
MAX_LONG_EDGE = 1568
MAX_PIXELS = 1_150_000
def prepare_screenshot(screenshot: Image.Image, native_w: int, native_h: int) -> tuple[str, int, int]:
    """调整截图大小以符合API限制，并返回base64和显示尺寸。"""
    display_w, display_h = 1280, 720
    resized = screenshot.resize((display_w, display_h), Image.LANCZOS)
    buffer = io.BytesIO()
    resized.save(buffer, format="PNG")
    b64 = base64.standard_b64encode(buffer.getvalue()).decode()
    return b64, display_w, display_h
def scale_coordinates(api_x: int, api_y: int, display_w: int, display_h: int,
                      screen_w: int, screen_h: int) -> tuple[int, int]:
    """将API返回的坐标缩放回原生屏幕空间。"""
    screen_x = int(api_x * (screen_w / display_w))
    screen_y = int(api_y * (screen_h / display_h))
    return screen_x, screen_y
def compute_max_api_fit(native_w: int, native_h: int) -> tuple[int, int]:
    """计算符合API限制的最大分辨率，同时保持宽高比。"""
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
```
import anthropic
from PIL import Image
client = anthropic.Anthropic()
screenshot = Image.open("screenshot.png")
native_w, native_h = screenshot.size
b64, display_w, display_h = prepare_screenshot(screenshot, native_w, native_h)
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
api_x, api_y = extract_click_coords(response)  # 您的解析逻辑
screen_x, screen_y = scale_coordinates(api_x, api_y, display_w, display_h, native_w, native_h)
```
Claude的最新模型支持[自适应思考](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking)，这是一种设置，让Claude决定在行动之前需要多少推理。自适应思考不是手动设置思考令牌预算，而是让Claude根据每个请求的复杂性动态确定何时以及使用多少扩展思考。对于计算机操作，这意味着Claude可以思考它在屏幕上看到的内容，规划多步交互，并在提交点击或按键之前自我纠正。
通过自适应思考，Claude的思考深度通过思考参数控制，具有努力级别：低、中、高、极高（Opus 4.7）和最大。更多的思考意味着每次行动有更多的推理，但也意味着更多的输出令牌、更高的延迟和更高的成本。
自然的问题是：根据模型的不同，计算机操作的最佳思考量是多少？
我们在涵盖桌面应用程序、浏览器和多应用程序工作流程的一系列端到端UI自动化任务中测试了每个思考努力级别。
**Opus 4.7优于4.6系列。** 在OSWorld Verified基准测试中，我们发现Opus在等效令牌使用和努力设置下优于所有4.6系列模型。低努力的Opus 4.7得分与最大努力的Sonnet 4.6相似，而每个任务使用的令牌大约只有后者的1/10。对于困难任务，Opus 4.7是显而易见的选择。
**将努力设置为`高`** 可以达到接近最高的任务成功率，同时使用的输出令牌大约只有`最大`的一半。与Opus 4.6相比，低、中和高都使用大致相同数量的令牌，同时提高了OSWorld的得分。在我们的内部测试中，最大努力使用了更多令牌并提供了最佳得分。下表概述了我们关于何时使用每个思考努力级别的建议。
| 场景 | 思考努力 | 原因 |
| --- | --- | --- |
| 大多数用例的默认值 | `高` | Opus 4.7最适合困难任务。使用高将为模型提供足够的推理能力来规划复杂的多步交互，而不会显著增加令牌使用。 |
| 高吞吐量/成本敏感 | `低` | 较低的令牌使用，同时提供介于Opus 4.6的高和最大努力设置之间的质量。 |
| 简单、定义明确的工作流程/最快 | 建议尝试Sonnet 4.6 | 如果低延迟是最高优先级时使用。对于UI一致且工作流程已知的短、可预测任务足够。 |
| 复杂、一次性任务 | `最大` | 当任务极具挑战性且需要在第一次尝试时就做对时使用。 |
我们在涵盖桌面应用程序、浏览器和多应用程序工作流程的一系列端到端UI自动化任务中测试了每个思考努力级别。
两个模式脱颖而出：
**中等努力是最佳选择。** 将努力设置为中等可以达到接近最高的任务成功率，同时使用的输出令牌大约只有高的一半。超过中等后，性能有些趋于平稳。值得注意的是，当任务被重试时，中等和高会收敛到相同的成功率。这意味着高努力可能帮助模型在第一次尝试时就做对困难任务，但给定多次尝试，中等可能以更低的成本同样可靠地达到目标。
**一点思考大有裨益。** 低努力是一个令人惊讶的强大选项。它实际上使用的总输出令牌*少于*完全禁用思考（模型犯的错误更少，需要的重试周期更少），同时匹配或略超过无思考的精度。这使其成为成本敏感、高吞吐量工作负载的最佳选择。下表概述了我们的努力建议。
| 场景 | 思考努力 | 原因 |
| --- | --- | --- |
| 大多数用例的默认值 | `中等` | 最佳的精度与成本比。为模型提供足够的推理能力来规划多步交互，而不会过度思考。通过重试，以一半的令牌成本匹配高努力性能。 |
| 高吞吐量/成本敏感 | `低` | 比无思考更准确，但由于错误和重试更少，令牌使用更低。 |
| 简单、定义明确的工作流程/最快 | 禁用思考 | 如果低延迟是最高优先级时使用。对于UI一致且工作流程已知的短、可预测任务足够。 |
| 复杂、一次性任务 | `高` | 当任务具有挑战性且需要在第一次尝试时就做对时使用。如果您的系统支持重试，中等可能达到相同的最终成功率。 |
我们不推荐为计算机操作使用`最大`努力。在我们的测试中，它没有提供比`高`更好的精度，同时进一步增加了输出令牌成本。UI任务主要是感知性的，而不是深度逻辑性的，额外的推理预算要么未被使用，要么导致过度思考。请记住，随着模型的演进，这一建议将会改变。
```
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
UI自动化任务从根本上不同于编码或数学问题。大多数计算机操作行动是感知性和机械性的：识别正确的元素，点击正确的位置，而不是深度逻辑性的。思考在以下情况下最有帮助：
* 在开始之前规划多步序列（例如，"我需要打开设置，导航到隐私，然后禁用跟踪"）
* 从未预期的UI状态中恢复（例如，出现了一个未预料到的对话框）
* 交叉引用屏幕上的信息和任务指令
* 在专业软件上完成具有挑战性的项目
*本节介绍提示注入保护，如果您使用我们的官方计算机操作工具头，默认免费提供。但是，如果您有兴趣在自定义计算机或浏览器操作工具上启用此功能，请填写我们的* [*提示注入分类器兴趣表。*](https://docs.google.com/forms/d/e/1FAIpQLSfXj6rXC-SUQEYHCLabwUe5JuYiYyJ29Ja-KP7EhLIPlyz0tw/viewform?usp=dialog)
计算机操作智能体设计上会与不受信任的内容交互。Claude处理的每张截图、网页或应用程序UI都可能包含对抗性指令，包括隐藏文本、被操纵的图像、欺骗性UI元素或试图劫持智能体行为的社会工程尝试。这个攻击面与您控制输入的典型API集成根本不同。对于计算机操作，模型的输入是开放的互联网和智能体正在导航的任何软件。
随着计算机操作智能体变得更有能力并更广泛部署，提示注入成为一个相应更严重的风险。能够点击、输入和导航的智能体可以被操纵执行现实世界的行动，如填写表单、下载文件或导航到恶意URL。构建针对这些攻击的稳健防御对于任何生产部署都是必不可少的。
我们已经详细撰写了关于我们[浏览器和计算机操作的提示注入防御方法](https://www.anthropic.com/research/prompt-injection-defenses)。我们的防御策略在多个层面运作：
**训练时稳健性。** 我们使用强化学习将提示注入抵抗能力直接构建到Claude的能力中。在训练期间，Claude会接触到嵌入在模拟网页和应用程序UI中的注入内容，并在正确识别和拒绝遵循恶意指令时获得奖励。这意味着Claude的第一道防线是模型本身，因为它已经学会了区分合法用户指令和在任务执行过程中遇到的对抗性内容。
**实时分类器。** 我们运行探针来扫描进入Claude上下文窗口的内容，并标记潜在的提示注入尝试。这些探针跨多种模态检测对抗性命令，例如隐藏在页面内容中的文本、嵌入在图像中的指令以及旨在欺骗智能体的欺骗性UI元素，并在识别到攻击时调整Claude的行为。
**持续红队测试。** 我们的安全研究人员持续探测这些防御措施，并且我们参与外部对抗性评估，以基准测试针对不断演变的攻击技术的稳健性。
自我们最初的计算机操作研究预览以来，我们继续在所有三个层面进行大量投资。每一代新模型都融入了更强的训练时防御和更有能力的分类器，并且我们扩大了红队评估的攻击技术范围。
当您通过API使用Claude的[官方计算机操作工具](https://docs.anthropic.com/en/docs/agents-and-tools/computer-use)时，提示注入分类器会在每个请求上自动运行。这些分类器与主模型推理并行运行，为您的请求增加大约零额外延迟和零额外成本。
您无需配置任何东西即可启用此保护。当您使用官方`computer_20251124`工具类型时，它默认开启。分类器会评估截图和其他内容是否有提示注入的迹象，并相应地影响Claude的响应。
```
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
许多开发者使用自定义工具定义而不是官方`computer_20251124`工具类型来构建计算机操作集成，例如，定义自己的截图和点击工具。如果这是您的设置，上述内置分类器目前不会在您的请求上运行。
我们正在积极探索如何将提示注入保护扩展到这些自定义实现。如果您正在构建没有官方工具类型的计算机操作或浏览器操作集成，并且对提示注入分类器感兴趣，请[填写此兴趣表](https://docs.google.com/forms/d/e/1FAIpQLSfXj6rXC-SUQEYHCLabwUe5JuYiYyJ29Ja-KP7EhLIPlyz0tw/viewform?usp=dialog)，我们将在该功能可用时跟进。
分类器是一层防御，而不是完整的解决方案。我们建议任何计算机操作部署遵循以下实践：
**对高风险行动实施人在回路中。** 让智能体在执行不可逆操作（如提交表单、进行购买、发送消息或修改数据）之前暂停并请求用户确认。这是针对提示注入最有效的缓解措施，无论分类器性能如何。
**限定智能体的权限。** 限制智能体可以做什么。如果您的工作流程不需要文件下载，就不要给智能体访问下载文件的权限。如果不需要发送电子邮件，就不要给智能体访问电子邮件客户端的权限。减少成功注入的爆炸半径与防止注入本身同样重要。
**监控和记录智能体行动。** 记录智能体执行的完整行动序列，包括每个步骤的截图。这使您能够检测异常行为，审计出问题时发生的情况，并构建反馈循环以随着时间的推移提高系统的稳健性。
**将所有网页内容视为不受信任。** 设计智能体的系统提示，清晰区分用户的指令和在任务执行过程中遇到的内容。提醒模型，在网页、电子邮件或应用程序UI中找到的文本不是来自用户，不应被视为指令。
在构建计算机操作智能体时，截图会快速累积。每个行动都会生成一张新图像，每张图像根据分辨率消耗大约1,000-1,800个令牌。在考虑了系统提示、工具定义和文本内容后，一个200k的上下文窗口可以在远少于100张截图的情况下填满。
良好管理此上下文有两个目标：1）保持总令牌数有界，2）保持提示缓存有效，这样您就不会为相同的前缀反复支付全价。我们发现，有效的[上下文管理](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)对长时间运行的智能体成本和延迟的影响几乎超过任何其他优化。本节涵盖三个可以干净组合的层面：放置缓存断点、修剪旧截图而不破坏缓存，以及在修剪不够时总结历史。
提示缓存只有在断点落在跨轮次重复出现的内容上时才有帮助。API总共支持四个缓存断点。将全部四个放在稳定的前缀（系统提示、工具定义）上会浪费它们，因为该前缀只被命中一次且永远不会失效，所以一个断点就足够了。其他三个最好用在最近的历史上，那里失效风险最高，且节省在长时间会话中会累积。
我们建议：
* **一个断点在系统提示或尾随的工具定义上。** 此前缀在会话中很少更改。
* **最多三个额外的断点在最近的工具结果上**，每轮推进并清除前一轮的断点，这样就不会超过四个断点的限制。
将断点分布在最近的位置上可以为您提供优雅的降级。如果您最近的断点失效了，例如由于图像修剪、压缩或工具定义更改，较早的断点仍然可以命中，您只需支付完整输入成本的10%而不是100%。
*缓存控制和设置断点的示例：*
```
def set_trailing_cache_control(messages, max_breakpoints=3):
    """在最近的tool_result块上放置最多`max_breakpoints`个临时cache_control标记，
    清除任何现有标记后。"""
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
保持令牌数有界的最简单方法是只保留最近的N张截图，并丢弃其余的。在每个API调用之前，遍历消息数组，用简短的占位符（例如，一个说"[图像已省略]"的文本块）替换较旧的图像块。
这种模式的简单版本是随着截图过期而逐个丢弃，这会在每一轮更改前缀，并持续使提示缓存失效。这就是滚动缓冲区因破坏缓存而名声不佳的原因。解决方法是批量修剪，这样前缀在几轮内保持字节相同，然后失效一次，然后再次保持稳定。
我们测试过的一个具体模式是：
1. 以完整分辨率保留最近的keep_n张截图。
2. 一旦总截图数量超过keep_n + interval，在一次传递中将最旧的interval张截图替换为占位符。
3. 在修剪事件之间，消息数组跨轮次字节相同，因此您的缓存断点持续命中。
合理的默认起始值：keep_n = 3，interval = 25。这些是可调的，更高的interval意味着更少的修剪事件（更好的缓存效率），但上下文中保留的完整分辨率截图尾部更大（更多令牌）。在代表性轨迹上测量缓存命中率和总输入令牌，并进行调整。
*修剪旧截图同时保持缓存断点的示例：*
```
def prune_old_screenshots(messages, keep_n=3, interval=25):
    """批量将较旧的截图替换为文本占位符。
    仅在总数超过keep_n + interval时修剪，因此消息前缀在`interval`轮之间保持字节稳定。"""
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
滚动缓冲区仍然有一个真正的限制：缓冲区之外的任何内容都消失了。原始指令、智能体已经尝试过的内容以及它在任务中的位置都会随着修剪的截图一起消失。对于短任务（大约50个行动以内），这没问题。对于任何更长的任务，请将其与压缩结合使用。
不是静默丢弃旧图像，而是在丢弃之前总结整个对话。摘要保留了发生了什么、用户要求了什么、已经完成了什么以及从哪里继续。同时保留几张最近的截图，以便智能体可以看到它当前正在查看的内容。
压缩和缓存感知的滚动缓冲区是互补的。使用滚动缓冲区进行轮次间的管理，以保持令牌增长可控；偶尔使用压缩来回收窗口的其余部分，而不会丢失早期上下文。每个压缩事件设计上就是一次缓存失效，因此您希望它很少发生，而不是每几轮发生一次。
此示例提示提供了一个框架，其中每个部分针对特定的失败模式。提示必须捕获智能体继续任务所需的一切，而无需重新阅读原始对话，如下例所示：
```
COMPACT_PRO