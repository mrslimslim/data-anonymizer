# Anonymizer Tool

[![GitHub](https://img.shields.io/github/license/mashape/apistatus.svg)](LICENSE)
[![npm version](https://badge.fury.io/js/anonymizer-tool.svg)](https://badge.fury.io/js/anonymizer-tool)

## 介绍

Anonymizer Tool 是一款用于对文本中的敏感信息进行匿名化处理的工具。它可以在不透露具体内容的情况下，保护你的数据免受泄露风险。此工具特别适合于那些需要使用OpenAI能力，而又担心数据安全的场景。

## 安装

```bash
npm install anonymizer-tool
```

或

```bash
yarn add anonymizer-tool
```

## 使用说明

以下是如何使用 `anonymizer-tool` 的示例代码：

```javascript
import Anonymizer from 'anonymizer-tool';

const sampleText = '我的名字叫JameBruce,我的github地址是：github.com/mrslimslim, 我的邮箱地址是jamebruce89@gmail.com，我的博客地址是https://mmblog.com。';

const anonymizer = new Anonymizer({
    input: sampleText,
    rules: [], // rules 默认包含 email, IP, domain, phoneNumber
});

const { result, decoder } = anonymizer.anonymize();

console.log('Anonymized text:', result);
console.log('Original text:', decoder(result));

// 输出结果应该显示原始文本与脱敏后文本之间的转换效果
```

## 功能特性

- **自动识别**：默认规则可以自动检测常见的敏感信息，如电子邮件、域名和电话号码。
- **自定义规则**：用户可以定义自己的规则，以适应更复杂的脱敏需求。
- **可逆操作**：提供的 `decoder` 方法，可以将混淆后的文本还原成原始状态。

## API 文档

### 构造函数

#### `new Anonymizer(options)`

**参数**

- `options` (Object) - 包含配置信息的对象。
  - `input` (String) - 需要进行脱敏处理的原始文本。
  - `rules` (Array) - 可选的自定义规则列表，默认为空列表，此时使用默认规则。

**返回值**

- (Anonymizer) - 创建的 Anonymizer 实例。

### 方法

#### `anonymize()`

执行文本的脱敏处理。

**返回值**

- (Object) - 包含脱敏后的结果和解码器对象的结果。

  - `result` (String) - 经过混淆处理后的文本。
  - `decoder` (Function) - 用于还原混淆文本的函数。

#### `decoder(anonymizedText)`

将混淆后的文本还原为原始文本。

**参数**

- `anonymizedText` (String) - 需要还原的混淆文本。

**返回值**

- (String) - 还原后的原始文本。

## 自定义规则

自定义规则可以通过传递一个规则数组给构造函数来实现。每个规则应该是一个对象，包含以下属性：

- `type` (String) - 规则的类型，仅用于内部跟踪。
- `replace` (Function) - 接收原始匹配项作为参数的替换函数。
- `reg` (RegExp) - 用于在文本中查找匹配项的正则表达式。

例如：

```javascript
const customRules = [
    {
        type: 'creditCard',
        replace: () => 'CREDIT_CARD_REPLACED',
        reg: /\b(?:(?:\d(?:[- ])?){13,16}\d)\b/g,
    },
];

const anonymizer = new Anonymizer({
    input: sampleText,
    rules: customRules,
});
```

## 贡献

欢迎社区成员贡献代码和反馈。对于任何问题或改进意见，请先提交 issue。

## 许可证

本项目基于 MIT 许可证发布。更多信息请参阅 [LICENSE](LICENSE) 文件。