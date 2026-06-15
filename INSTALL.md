# think-jump 插件 - Composer 安装指南

## 📦 通过 Composer 安装

### 方式一：直接安装（推荐）

在项目根目录执行：

```bash
composer require bleeld/think-jump:@dev
```

### 方式二：修改 composer.json 后安装

1. 在 `composer.json` 的 `require` 部分添加：

```json
{
    "require": {
        "bleeld/think-jump": "@dev"
    }
}
```

2. 在 `composer.json` 的 `repositories` 部分添加本地仓库配置：

```json
{
    "repositories": [
        {
            "type": "path",
            "url": "./vendor/bleeld/think-jump",
            "options": {
                "symlink": true
            }
        }
    ]
}
```

3. 运行安装命令：

```bash
composer install
```

或更新依赖：

```bash
composer update bleeld/think-jump
```

## ✅ 验证安装

安装完成后，可以通过以下命令验证：

```bash
composer show bleeld/think-jump
```

应该看到类似输出：

```
name     : bleeld/think-jump
descrip. : ThinkPHP 8.0 跳转扩展，提供 success/error/redirect/result 方法
versions : * dev-main
type     : library
license  : Apache License 2.0 (Apache-2.0)
```

## 🔧 自动配置

安装后，Composer 会自动：

1. ✅ 注册 PSR-4 自动加载：`think\` → `vendor/bleeld/think-jump/src/`
2. ✅ 发布配置文件到：`config/jump.php`
3. ✅ 生成自动加载文件

## 📝 使用方法

### 在 BaseController 中使用（已集成）

插件已在 `app/BaseController.php` 中集成，所有继承 BaseController 的控制器都可以直接使用：

```php
namespace app\admin\controller;

class Index extends \app\BaseController
{
    public function index()
    {
        // 成功跳转
        return $this->success('操作成功', 'index/index');
        
        // 错误跳转
        return $this->error('操作失败');
        
        // URL重定向
        return $this->redirect('/admin/index/index');
        
        // API返回
        return $this->result(['data' => 'value']);
    }
}
```

### 直接在控制器中使用

```php
namespace app\controller;

class Index
{
    use \think\Jump;
    
    public function index()
    {
        return $this->success('操作成功');
    }
}
```

## 🗑️ 卸载插件

如果需要卸载插件，执行：

```bash
composer remove bleeld/think-jump
```

这会：
- 从 `composer.json` 中移除依赖
- 删除 `vendor/bleeld/think-jump` 目录
- 更新自动加载文件

**注意**：卸载后需要手动：
1. 从 `app/BaseController.php` 中移除 `use \think\Jump;` 和 `use Jump;`
2. 删除 `config/jump.php` 配置文件（可选）

## 📂 安装后的文件结构

```
vendor/bleeld/think-jump/
├── src/
│   ├── Jump.php                    # Trait 核心类
│   └── tpl/
│       └── dispatch_jump.tpl       # 跳转模板
├── config/
│   └── jump.php                    # 配置文件（源文件）
├── composer.json                   # Composer 配置
└── README.md                       # 使用文档

config/
└── jump.php                        # 发布的配置文件（自动生成）
```

## 🔍 常见问题

### Q1: 为什么使用 @dev 版本？

因为这是本地开发的插件，还没有发布正式版本。使用 `@dev` 可以安装开发版本。

### Q2: 如何发布正式版本？

1. 在 `composer.json` 中设置版本号：
```json
{
    "version": "1.0.0"
}
```

2. 创建 Git 标签：
```bash
git tag v1.0.0
git push origin v1.0.0
```

3. 发布到 Packagist.org（可选）

### Q3: 配置文件没有自动生成怎么办？

手动复制配置文件：

```bash
cp vendor/bleeld/think-jump/config/jump.php config/jump.php
```

然后运行：

```bash
php think vendor:publish
```

### Q4: 如何更新插件？

```bash
composer update bleeld/think-jump
```

## 📖 更多信息

- **使用文档**: `vendor/bleeld/think-jump/README.md`
- **测试页面**: http://www.crm.com/test_jump.html
- **开发报告**: `tests/THINK_JUMP_DEVELOPMENT_REPORT.md`

## ✨ 特性

- ✅ 支持 Composer 安装/卸载
- ✅ 自动配置和发布
- ✅ PSR-4 自动加载
- ✅ 本地 symlink 链接（开发友好）
- ✅ 与 ThinkPHP 8.0 完美集成

---

**安装完成时间**: 2026-04-20  
**插件版本**: dev-main  
**状态**: ✅ 已通过 Composer 安装
