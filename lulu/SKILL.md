---
name: lulu
description: 基于 Lulu Design System 生成 Flutter 页面或弹窗代码。当用户提供 PRD、页面需求描述、或提到 LuluButton、LuluCard、LuluScaffold 等组件时自动使用。支持 /lulu 命令直接触发。
argument-hint: [粘贴你的 PRD 或页面需求描述]
---

# Lulu Design System — 代码生成规范

基于用户提供的 PRD 或需求描述，生成符合 Lulu Design System 规范的 Flutter 页面（Page）或弹窗（Dialog）代码。

## 用户请求

$ARGUMENTS

---

## 核心原则

1. **颜色必须从 Theme 读取**，禁止硬编码 hex 或 `Color(0xFF...)`
2. **间距必须用 `LuluSpacing` 常量**，禁止硬编码数字
3. **所有组件从 `LuluTheme.of(context)` 获取颜色**
4. **页面骨架统一用 `LuluScaffold`**

---

## 安装

```yaml
dependencies:
  lulu:
    path: ../lulu  # 或发布后的包名
```

```dart
import 'package:lulu/lulu.dart';
```

---

## 主题接入

在应用根部注入主题（必须，否则所有组件报错）：

```dart
LuluThemeScope(
  theme: LuluThemeData.dark(),  // 或 LuluThemeData.light()
  child: MyApp(),
)
```

跟随系统深浅色：

```dart
LuluThemeScope(
  theme: MediaQuery.platformBrightnessOf(context) == Brightness.dark
      ? LuluThemeData.dark()
      : LuluThemeData.light(),
  child: MyApp(),
)
```

---

## 组件 API 速查

### LuluScaffold — 页面骨架

```dart
LuluScaffold(
  title: '页面标题',          // 显示标准 AppBar
  // appBar: MyCustomAppBar(), // 自定义 AppBar（优先于 title）
  body: MyContent(),
  bottomBar: LuluButton(      // 底部固定区域
    label: '确认',
    onPressed: () {},
    expanded: true,
  ),
  scrollable: true,           // 内容可滚动
  applyScreenPadding: true,   // 应用左右屏幕边距（默认 true）
  floatingActionButton: ...,
)
```

### LuluButton — 按钮

```dart
// 主按钮
LuluButton(label: '提交', onPressed: () {})

// 次按钮（描边）
LuluButton(label: '取消', variant: LuluButtonVariant.outline, onPressed: () {})

// 幽灵按钮
LuluButton(label: '查看详情', variant: LuluButtonVariant.ghost, onPressed: () {})

// 危险按钮
LuluButton(label: '删除', variant: LuluButtonVariant.danger, onPressed: () {})

// 尺寸
LuluButton(label: '小', size: LuluButtonSize.small, onPressed: () {})
LuluButton(label: '大', size: LuluButtonSize.large, onPressed: () {})

// 带图标
LuluButton(
  label: '上传',
  leading: Icon(Icons.upload, size: 16),
  onPressed: () {},
)

// 撑满宽度
LuluButton(label: '登录', expanded: true, onPressed: () {})

// 加载中
LuluButton(label: '提交', loading: true)

// 禁用（onPressed 为 null）
LuluButton(label: '不可用', onPressed: null)
```

### LuluText — 文字

```dart
LuluText.displayLg('大标题')       // 32sp bold
LuluText.displayMd('页面标题')      // 24sp bold
LuluText.displaySm('Section 标题') // 20sp semibold
LuluText.bodyLg('正文大')          // 17sp regular
LuluText.bodyMd('正文')            // 15sp regular（默认）
LuluText.bodySm('正文小')          // 13sp regular，textSecondary 色
LuluText.label('标签文字')         // 13sp semibold
LuluText.caption('说明文字')       // 12sp regular，textTertiary 色

// 自定义颜色/对齐/截断
LuluText.bodyMd('内容', color: LuluColors.primary, maxLines: 2, overflow: TextOverflow.ellipsis)
```

### LuluInput — 输入框

```dart
LuluInput(
  label: '手机号',
  placeholder: '请输入手机号',
  keyboardType: TextInputType.phone,
  controller: _phoneController,
  onChanged: (v) {},
)

// 带图标
LuluInput(
  placeholder: '搜索',
  leading: Icon(Icons.search, size: 18),
)

// 错误状态
LuluInput(
  label: '密码',
  placeholder: '请输入密码',
  obscureText: true,
  errorText: '密码不能少于6位',
)

// 多行文本
LuluInput(
  placeholder: '请输入备注',
  maxLines: 4,
)
```

### LuluCard — 卡片

```dart
// 基础卡片
LuluCard(
  child: Column(children: [...]),
)

// 可点击
LuluCard(
  onTap: () {},
  child: ...,
)

// 带边框
LuluCard(bordered: true, child: ...)

// 自定义背景色和 padding
LuluCard(
  color: theme.backgroundSecondary,
  padding: EdgeInsets.all(LuluSpacing.sm),
  child: ...,
)
```

### LuluAvatar — 头像

```dart
// 网络图片
LuluAvatar(imageUrl: 'https://...', size: LuluAvatarSize.lg)

// 姓名首字母 fallback
LuluAvatar(name: 'Zhang Yucheng', size: LuluAvatarSize.md)

// 尺寸：xs(24) / sm(32) / md(40) / lg(56) / xl(80)
```

### LuluTag / LuluBadge — 标签/角标

```dart
// 标签
LuluTag(label: '进行中')
LuluTag(label: '待审核', variant: LuluTagVariant.outline)
LuluTag(label: '已完成', color: LuluColors.green)
LuluTag(label: '警告', color: LuluColors.orange, leading: Icon(Icons.warning, size: 12))

// 角标
LuluBadge(count: 5, child: Icon(Icons.notifications))   // 数字角标
LuluBadge(count: null, child: Icon(Icons.message))       // 小红点
LuluBadge(count: 0, child: Icon(Icons.bell))             // 隐藏角标
```

### LuluDivider — 分割线

```dart
LuluDivider()                                       // 水平
LuluDivider.vertical(length: 20)                    // 垂直
LuluDivider(margin: EdgeInsets.symmetric(vertical: LuluSpacing.md))
```

---

## 颜色 Token 速查

```dart
// 主色
LuluColors.primary        // 荧光绿 #ACFF4E

// 功能色
LuluColors.green          // 成功
LuluColors.red            // 错误
LuluColors.blue           // 信息
LuluColors.yellow         // 警告
LuluColors.orange         // 警告2

// Light 调色板（用于彩色 Tag、高亮等）
LuluColors.lightBlue
LuluColors.lightYellow
LuluColors.lightCyan
LuluColors.lightPink
LuluColors.lightPurple
```

从 Theme 读取语义化颜色（**推荐**）：

```dart
final theme = LuluTheme.of(context);
theme.primary           // 主色
theme.background        // 页面背景
theme.surface           // 卡片/输入框背景
theme.textPrimary       // 主文字
theme.textSecondary     // 次文字
theme.textTertiary      // 辅助文字
theme.textDisabled      // 禁用文字
theme.success / .warning / .error / .info
theme.border            // 边框色
theme.divider           // 分割线色
```

---

## 间距 Token 速查

```dart
LuluSpacing.xs    // 4
LuluSpacing.sm    // 8
LuluSpacing.md    // 16（基准）
LuluSpacing.lg    // 24
LuluSpacing.xl    // 32
LuluSpacing.xxl   // 48

LuluSpacing.sectionGap       // 32，Section 间距
LuluSpacing.componentGap     // 16，组件间距
LuluSpacing.itemGap          // 12，列表项间距
LuluSpacing.innerPadding     // 16，组件内 padding
LuluSpacing.innerPaddingSmall // 8，紧凑 padding
LuluSpacing.screenHorizontal // 16，屏幕左右边距

LuluSpacing.radiusSm   // 8
LuluSpacing.radiusMd   // 12
LuluSpacing.radiusLg   // 16
LuluSpacing.radiusXl   // 24
LuluSpacing.radiusFull // 999（胶囊形）
```

---

## 完整页面示例

### 登录页

```dart
class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  State<LoginPage> createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
  final _phoneCtrl = TextEditingController();
  final _passwordCtrl = TextEditingController();
  bool _loading = false;

  @override
  Widget build(BuildContext context) {
    final theme = LuluTheme.of(context);

    return LuluScaffold(
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          SizedBox(height: LuluSpacing.xxl),
          LuluText.displayMd('欢迎回来'),
          SizedBox(height: LuluSpacing.xs),
          LuluText.bodyMd('登录以继续', color: theme.textSecondary),
          SizedBox(height: LuluSpacing.xl),
          LuluInput(
            label: '手机号',
            placeholder: '请输入手机号',
            keyboardType: TextInputType.phone,
            controller: _phoneCtrl,
          ),
          SizedBox(height: LuluSpacing.componentGap),
          LuluInput(
            label: '密码',
            placeholder: '请输入密码',
            obscureText: true,
            controller: _passwordCtrl,
          ),
          SizedBox(height: LuluSpacing.xl),
          LuluButton(
            label: '登录',
            expanded: true,
            loading: _loading,
            onPressed: _onLogin,
          ),
          SizedBox(height: LuluSpacing.md),
          Center(
            child: LuluButton(
              label: '还没有账号？立即注册',
              variant: LuluButtonVariant.ghost,
              onPressed: () {},
            ),
          ),
        ],
      ),
    );
  }

  void _onLogin() {
    setState(() => _loading = true);
    // 业务逻辑...
  }
}
```

### 底部确认弹窗

```dart
void showConfirmSheet(BuildContext context, {
  required String title,
  required String content,
  required VoidCallback onConfirm,
}) {
  showModalBottomSheet(
    context: context,
    backgroundColor: Colors.transparent,
    builder: (_) {
      final theme = LuluTheme.of(context);
      return Container(
        padding: EdgeInsets.fromLTRB(
          LuluSpacing.screenHorizontal,
          LuluSpacing.lg,
          LuluSpacing.screenHorizontal,
          LuluSpacing.screenBottom,
        ),
        decoration: BoxDecoration(
          color: theme.surface,
          borderRadius: BorderRadius.vertical(
            top: Radius.circular(LuluSpacing.radiusXl),
          ),
        ),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Center(
              child: Container(
                width: 40,
                height: 4,
                decoration: BoxDecoration(
                  color: theme.border,
                  borderRadius: LuluSpacing.borderRadiusFull,
                ),
              ),
            ),
            SizedBox(height: LuluSpacing.lg),
            LuluText.displaySm(title),
            SizedBox(height: LuluSpacing.sm),
            LuluText.bodyMd(content, color: theme.textSecondary),
            SizedBox(height: LuluSpacing.xl),
            Row(
              children: [
                Expanded(
                  child: LuluButton(
                    label: '取消',
                    variant: LuluButtonVariant.outline,
                    onPressed: () => Navigator.pop(context),
                  ),
                ),
                SizedBox(width: LuluSpacing.componentGap),
                Expanded(
                  child: LuluButton(
                    label: '确认',
                    onPressed: () {
                      Navigator.pop(context);
                      onConfirm();
                    },
                  ),
                ),
              ],
            ),
          ],
        ),
      );
    },
  );
}
```

---

## 代码生成要求

根据用户的 PRD，输出完整可运行的 Flutter 代码，要求：

1. **页面用 `LuluScaffold` 包裹**，统一骨架
2. **文字用 `LuluText` 语义层级**，不直接用 `Text` + 手写 `TextStyle`
3. **颜色从 `LuluTheme.of(context)` 读取**，需要静态色才用 `LuluColors` 常量
4. **间距全用 `LuluSpacing` 常量**，不写裸数字
5. **交互按钮用 `LuluButton`**，根据操作重要性选择合适的 variant
6. **卡片内容用 `LuluCard` 包裹**
7. **输入字段统一用 `LuluInput`**
8. **弹窗/Sheet 的颜色、圆角、padding 均从 theme 和 spacing 读取**
9. **代码完整可运行**，包含必要的 import 和 StatefulWidget/StatelessWidget 声明
