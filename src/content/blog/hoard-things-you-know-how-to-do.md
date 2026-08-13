---
title: "Hoard things you know how to do"
description: "Hoard things you know how to do"
pubDate: "2026-02-26"
heroImage: "/post_img.png"
tags: ["simon-guides"]
originalLink: "https://simonwillison.net/guides/agentic-engineering-patterns/hoard-things-you-know-how-to-do/"
---

我与编码代理高效合作的许多技巧，都源于我在职业生涯中（即使没有它们）发现有用的建议。这里有一个很好的例子：囤积你知道如何做的事情。

构建软件技能的一个重要部分是理解什么是可能的，什么是不可能的，并且至少对这些事情如何实现有一个大致的想法。

这些问题可能很宽泛，也可能相当冷门。一个网页能仅靠JavaScript运行OCR操作吗？一款iPhone应用能在未运行时与蓝牙设备配对吗？我们能在不先将整个100GB JSON文件加载到内存的情况下，用Python处理它吗？

你掌握的这类问题的答案越多，就越有可能发现机会，用别人可能还没想到的方式来部署技术解决问题。

对这些问题答案充满信心的最佳方式，是亲眼看到它们通过运行代码得到验证。知道某件事在理论上是可能的，与亲眼看到它被实现是不一样的。作为软件专业人士，要培养的一项关键资产，就是积累大量这类问题的答案，并附上这些答案的证明。

我以多种方式囤积这类解决方案。我的博客和TIL博客里塞满了关于我已弄清楚如何做某事的笔记。我在GitHub上有一千多个仓库，收集了我为不同项目编写的代码，其中许多是演示关键想法的小型概念验证。

最近，我使用LLM来帮助扩展我针对有趣问题的代码解决方案集合。

tools.simonwillison.net是我最大的LLM辅助工具和原型集合。我用它来收集我所谓的HTML工具——嵌入JavaScript和CSS、解决特定问题的单个HTML页面。

我的simonw/research仓库包含更大、更复杂的示例，在这些示例中，我挑战编码代理去研究一个问题，并带回可工作的代码和一份详细说明其发现的书面报告。

## 从你的囤积中重组内容

为什么要收集所有这些？除了帮助你建立和扩展自己的能力之外，你在此过程中生成的资产会成为你编码代理的强大输入。

我最喜欢的提示模式之一，是告诉代理通过组合两个或多个现有的工作示例来构建新东西。

一个帮助我认识到这有多有效率的项目，是我添加到工具集合中的第一个东西——一个基于浏览器的OCR工具，在此处有更详细的描述。

我想要一个简单的、基于浏览器的工具，用于对PDF文件中的页面进行OCR——特别是那些完全由扫描图像组成、根本不提供文本版本的PDF。

我之前曾尝试在浏览器中运行Tesseract.js OCR库，并发现它非常强大。该库提供了成熟的Tesseract OCR引擎的WebAssembly构建，并允许你从JavaScript调用它来从图像中提取文本。

但我不想处理图像，我想处理PDF。然后我记起我还用过Mozilla的PDF.js库，除其他功能外，它可以将PDF的单个页面转换为渲染图像。

我的笔记中有这两个库的JavaScript代码片段。

以下是我输入给模型（当时是Claude 3 Opus）的完整提示，结合了我的两个示例并描述了我正在寻找的解决方案：

这段代码展示了如何打开PDF并将其转换为每页一张图片：
```html
<!DOCTYPE html>
<html>
<head>
  <title>PDF to Images</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.9.359/pdf.min.js"></script>
  <style>
    .image-container img {
      margin-bottom: 10px;
    }
    .image-container p {
      margin: 0;
      font-size: 14px;
      color: #888;
    }
  </style>
</head>
<body>
  <input type="file" id="fileInput" accept=".pdf" />
  <div class="image-container"></div>

  <script>
  const desiredWidth = 800;
    const fileInput = document.getElementById('fileInput');
    const imageContainer = document.querySelector('.image-container');

    fileInput.addEventListener('change', handleFileUpload);

    pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.9.359/pdf.worker.min.js';

    async function handleFileUpload(event) {
      const file = event.target.files[0];
      const imageIterator = convertPDFToImages(file);

      for await (const { imageURL, size } of imageIterator) {
        const imgElement = document.createElement('img');
        imgElement.src = imageURL;
        imageContainer.appendChild(imgElement);

        const sizeElement = document.createElement('p');
        sizeElement.textContent = `Size: ${formatSize(size)}`;
        imageContainer.appendChild(sizeElement);
      }
    }

    async function* convertPDFToImages(file) {
      try {
        const pdf = await pdfjsLib.getDocument(URL.createObjectURL(file)).promise;
        const numPages = pdf.numPages;

        for (let i = 1; i <= numPages; i++) {
          const page = await pdf.getPage(i);
          const viewport = page.getViewport({ scale: 1 });
          const canvas = document.createElement('canvas');
          const context = canvas.getContext('2d');
          canvas.width = desiredWidth;
          canvas.height = (desiredWidth / viewport.width) * viewport.height;
          const renderContext = {
            canvasContext: context,
            viewport: page.getViewport({ scale: desiredWidth / viewport.width }),
          };
          await page.render(renderContext).promise;
          const imageURL = canvas.toDataURL('image/jpeg', 0.8);
          const size = calculateSize(imageURL);
          yield { imageURL, size };
        }
      } catch (error) {
        console.error('Error:', error);
      }
    }

    function calculateSize(imageURL) {
      const base64Length = imageURL.length - 'data:image/jpeg;base64,'.length;
      const sizeInBytes = Math.ceil(base64Length * 0.75);
      return sizeInBytes;
    }

    function formatSize(size) {
      const sizeInKB = (size / 1024).toFixed(2);
      return `${sizeInKB} KB`;
    }
  </script>
</body>
</html>
```
这段代码展示了如何对图像进行OCR：
```javascript
async function ocrMissingAltText() {
    // 加载Tesseract
    var s = document.createElement("script");
    s.src = "https://unpkg.com/tesseract.js@v2.1.0/dist/tesseract.min.js";
    document.head.appendChild(s);

    s.onload = async () => {
      const images = document.getElementsByTagName("img");
      const worker = Tesseract.createWorker();
      await worker.load();
      await worker.loadLanguage("eng");
      await worker.initialize("eng");
      ocrButton.innerText = "Running OCR...";

      // 遍历输出div中的所有图像
      for (const img of images) {
        const altTextarea = img.parentNode.querySelector(".textarea-alt");
        // 检查alt文本区域是否为空
        if (altTextarea.value === "") {
          const imageUrl = img.src;
          var {
            data: { text },
          } = await worker.recognize(imageUrl);
          altTextarea.value = text; // 将OCR结果设置到alt文本区域
          progressBar.value += 1;
        }
      }

      await worker.terminate();
      ocrButton.innerText = "OCR complete";
    };
  }
```
使用这些示例，组合成一个单独的HTML页面，其中嵌入HTML、CSS和JavaScript，提供一个大的方形区域，用户可以拖放PDF文件到上面，当用户这样做时，PDF的每一页都会被转换为JPEG并显示在页面下方，然后使用tesseract运行OCR，结果会显示在每个图像下方的文本区域块中。

这完美地工作了！模型输出一个概念验证页面，完全符合我的需求。

我最终与它迭代了几次才得到最终结果，但只花了几分钟就构建了一个真正有用的工具，我从此一直受益。

## 编码代理让这变得更强大

我是在2024年3月构建那个OCR示例的，比Claude Code的第一个版本发布早了将近一年。编码代理使得囤积工作示例变得更加有价值。

如果你的编码代理有互联网访问权限，你可以告诉它做类似这样的事情：

使用curl获取`https://tools.simonwillison.net/ocr`和`https://tools.simonwillison.net/gemini-bbox`的源代码，并构建一个新工具，允许你从PDF中选择一页，并将其传递给Gemini以返回该页上插图的边界框。（我在这里指定了curl，因为Claude Code默认使用WebFetch工具，该工具会总结页面内容，而不是返回原始HTML。）

编码代理非常擅长搜索，这意味着你可以在你自己的机器上运行它们，并告诉它们在哪里找到你想要它们做的事情的示例：向`~/dev/ecosystem/datasette-oauth`项目添加模拟HTTP测试，灵感来自`~/dev/ecosystem/llm-mistral`的实现方式。通常这就足够了——代理会启动一个搜索子代理来调查并提取完成任务所需的细节。

由于我的大部分研究代码都是公开的，我经常告诉编码代理将我的仓库克隆到/tmp，并将其用作输入：从GitHub克隆`simonw/research`到`/tmp`，并找到将Rust编译为WebAssembly的示例，然后用它来为这个项目构建一个演示HTML页面。这里的关键思想是，编码代理意味着我们只需要**一次**弄清楚一个有用的技巧。如果这个技巧随后被记录在某个地方，并附有可工作的代码示例，我们的代理就可以参考该示例，并在未来用它来解决任何类似形状的项目。
