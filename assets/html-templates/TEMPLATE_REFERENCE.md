# HTML简历模板参考

本文档提供生成HTML简历所需的模板结构和代码参考。

## 文件结构

```
{输出目录}/
├── {姓名}_简历.html           # 主HTML文件
└── templates/                  # 模板目录
    ├── resume_modern.html
    ├── resume_classic.html
    ├── resume_creative.html
    ├── resume_minimal.html
    ├── resume_elegant.html
    ├── resume_tech.html
    ├── resume_formal.html
    └── resume_colorful.html
```

---

## 主HTML文件结构

主HTML文件使用现代简约（modern）风格，模板路径指向 `./templates/`。

### 完整HTML模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{姓名}} - {{职位}}简历 | 现代简约风</title>
    <style>
        /* 样式代码见下方CSS部分 */
    </style>
</head>
<body>
    <!-- 工具栏 -->
    <div class="toolbar">
        <button class="btn-template" onclick="openModal()">切换模板</button>
        <button class="btn-pdf" onclick="exportPDF()">导出 PDF</button>
        <button class="btn-png" onclick="exportPNG()">导出 PNG</button>
    </div>

    <!-- 模板选择弹窗 -->
    <div class="modal" id="templateModal">
        <div class="modal-content">
            <h3>选择简历模板</h3>
            <div class="template-list">
                <!-- 8个模板选项 -->
            </div>
            <button class="modal-close" onclick="closeModal()">关闭</button>
        </div>
    </div>

    <!-- 简历内容 -->
    <div class="container">
        <header class="header">
            <h1 class="name">{{姓名}}</h1>
            <p class="title">{{职位}} | {{工作年限}}年经验 | {{期望薪资}}</p>
            <div class="contact-line">
                {{手机}} | {{邮箱}} | {{微信}} | 期望：{{城市}}
            </div>
        </header>

        <main class="main">
            <!-- 亮点 -->
            <div class="highlights">
                {{#each 核心亮点}}
                <div class="highlight-item">{{this}}</div>
                {{/each}}
            </div>

            <!-- 工作经历 -->
            <section class="section">
                <h2 class="section-title">工作经历</h2>
                {{#each 工作经历}}
                <div class="job">
                    <div class="job-header">
                        <div>
                            <span class="company">{{公司名称}}</span>
                            <span class="position"> / {{职位}}</span>
                        </div>
                        <span class="job-time">{{时间}}</span>
                    </div>
                    <div class="job-content">
                        <ul>
                            {{#each 工作内容}}
                            <li>{{{this}}}</li>
                            {{/each}}
                        </ul>
                    </div>
                </div>
                {{/each}}
            </section>

            <!-- 更多区块... -->
        </main>

        <footer class="footer">
            RESUME · {{日期}}
        </footer>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
    <script>
        // JavaScript代码见下方
    </script>
</body>
</html>
```

---

## 必要的JavaScript代码

每个HTML文件都需要包含以下JavaScript：

```javascript
// 模板映射（主文件用./templates/，模板文件用./）
const templates = {
    modern: './templates/resume_modern.html',  // 模板文件中用 './resume_modern.html'
    classic: './templates/resume_classic.html',
    creative: './templates/resume_creative.html',
    minimal: './templates/resume_minimal.html',
    elegant: './templates/resume_elegant.html',
    tech: './templates/resume_tech.html',
    formal: './templates/resume_formal.html',
    colorful: './templates/resume_colorful.html'
};

function openModal() {
    document.getElementById('templateModal').classList.add('active');
}

function closeModal() {
    document.getElementById('templateModal').classList.remove('active');
}

function selectTemplate(name) {
    document.querySelectorAll('.template-item').forEach(item => item.classList.remove('active'));
    event.currentTarget.classList.add('active');
    if (templates[name]) {
        window.location.href = templates[name];
    }
}

function exportPDF() {
    document.querySelector('.toolbar').style.display = 'none';
    window.print();
    setTimeout(() => {
        document.querySelector('.toolbar').style.display = 'flex';
    }, 100);
}

async function exportPNG() {
    const btn = document.querySelector('.btn-png');
    const originalText = btn.textContent;
    btn.textContent = '生成中...';
    btn.disabled = true;

    document.querySelector('.toolbar').style.display = 'none';

    try {
        const container = document.querySelector('.container');
        const canvas = await html2canvas(container, {
            scale: 2,
            useCORS: true,
            backgroundColor: '#ffffff',
            logging: false
        });

        const link = document.createElement('a');
        const templateName = window.location.pathname.split('/').pop().replace('.html', '');
        link.download = `{{姓名}}_简历_${templateName}.png`;
        link.href = canvas.toDataURL('image/png');
        link.click();

        btn.textContent = '已保存!';
        setTimeout(() => { btn.textContent = originalText; }, 2000);
    } catch (error) {
        console.error('导出PNG失败:', error);
        alert('导出PNG失败，请重试');
        btn.textContent = originalText;
    }

    document.querySelector('.toolbar').style.display = 'flex';
    btn.disabled = false;
}

document.getElementById('templateModal').addEventListener('click', function(e) {
    if (e.target === this) closeModal();
});
```

---

## 模板选择弹窗HTML

```html
<div class="template-list">
    <div class="template-item {{#if active_modern}}active{{/if}}" onclick="selectTemplate('modern')">
        <div class="icon">🎨</div>
        <div class="name">现代简约</div>
        <div class="desc">蓝紫渐变</div>
    </div>
    <div class="template-item" onclick="selectTemplate('classic')">
        <div class="icon">💼</div>
        <div class="name">经典商务</div>
        <div class="desc">深色稳重</div>
    </div>
    <div class="template-item" onclick="selectTemplate('creative')">
        <div class="icon">✨</div>
        <div class="name">创意活力</div>
        <div class="desc">多彩配色</div>
    </div>
    <div class="template-item" onclick="selectTemplate('minimal')">
        <div class="icon">📄</div>
        <div class="name">极简纯净</div>
        <div class="desc">黑白灰调</div>
    </div>
    <div class="template-item" onclick="selectTemplate('elegant')">
        <div class="icon">🌷</div>
        <div class="name">优雅格调</div>
        <div class="desc">粉金配色</div>
    </div>
    <div class="template-item" onclick="selectTemplate('tech')">
        <div class="icon">💻</div>
        <div class="name">科技极客</div>
        <div class="desc">暗黑科技</div>
    </div>
    <div class="template-item" onclick="selectTemplate('formal')">
        <div class="icon">📋</div>
        <div class="name">正式传统</div>
        <div class="desc">稳重专业</div>
    </div>
    <div class="template-item" onclick="selectTemplate('colorful')">
        <div class="icon">🌈</div>
        <div class="name">活力多彩</div>
        <div class="desc">年轻动感</div>
    </div>
</div>
```

---

## 8种模板风格特点

| 模板 | 主色调 | 特点 | 适用场景 |
|------|--------|------|----------|
| modern | 蓝紫渐变 | 清新专业，圆角设计 | 互联网、科技公司 |
| classic | 深蓝+金色 | 稳重大气，衬线字体 | 传统企业、金融 |
| creative | 紫粉渐变 | 活力动感，卡片设计 | 创意、设计行业 |
| minimal | 黑白灰 | 极简至上，留白多 | 任何行业 |
| elegant | 粉金配色 | 优雅温柔，圆角柔和 | 女性、教育、文化 |
| tech | 暗黑+绿色 | 代码风格，终端感 | 程序员、技术岗 |
| formal | 深蓝 | 正式专业，传统布局 | 国企、政府、银行 |
| colorful | 彩虹渐变 | 年轻动感，多彩配色 | 年轻人、初创公司 |

---

## 生成注意事项

1. **路径区分**：
   - 主HTML文件（`{姓名}_简历.html`）：模板路径用 `./templates/xxx.html`
   - templates目录下的模板：模板路径用 `./xxx.html`

2. **当前模板标记**：
   - 每个模板文件中，对应的模板项需要添加 `active` 类

3. **PNG文件名**：
   - 导出PNG时文件名格式：`{姓名}_简历_{模板名}.png`

4. **打印样式**：
   - 所有模板都需要包含 `@media print` 样式，隐藏工具栏

5. **响应式**：
   - 模板选择弹窗在移动端显示2列，桌面端显示4列
