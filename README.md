# think-jump

ThinkPHP 8.0 跳转扩展，提供 success/error/redirect/result 方法。

## 安装

```bash
composer require bleeld/think-jump
```

## 配置

安装后会在 `config` 目录生成 `jump.php` 配置文件：

```php
return [
    // 默认跳转页面对应的模板文件
    'dispatch_success_tmpl' => app()->getRootPath() . '/vendor/bleeld/think-jump/src/tpl/dispatch_jump.tpl',
    'dispatch_error_tmpl'   => app()->getRootPath() . '/vendor/bleeld/think-jump/src/tpl/dispatch_jump.tpl',
];
```

你可以自定义模板文件路径。

## 使用方法

### 方式一：在 BaseController 中集成（推荐）

在 `app/BaseController.php` 底部添加：

```php
use \think\Jump;
```

然后所有继承 BaseController 的控制器都可以直接使用跳转方法。

### 方式二：直接在控制器中使用

```php
namespace app\controller;

class Index
{
    use \think\Jump;
    
    public function index()
    {
        return $this->success('操作成功', 'index/index');
    }
}
```

## API 说明

### 1. success - 成功跳转

```php
/**
 * 操作成功跳转的快捷方法
 * @param  mixed $msg 提示信息
 * @param  string $url 跳转的URL地址
 * @param  mixed $data 返回的数据
 * @param  integer $wait 跳转等待时间
 * @param  array $header 发送的Header信息
 */

// 简单用法
return $this->success('登录成功', 'index/index');

// 完整用法
return $this->success(
    $msg = '登录成功',
    $url = 'index/index',
    $data = '',
    $wait = 3,
    $header = []
);
```

### 2. error - 错误跳转

```php
/**
 * 操作错误跳转的快捷方法
 * @param  mixed $msg 提示信息
 * @param  string $url 跳转的URL地址
 * @param  mixed $data 返回的数据
 * @param  integer $wait 跳转等待时间
 * @param  array $header 发送的Header信息
 */

// 简单用法
return $this->error('登录失败');

// 指定跳转地址
return $this->error('登录失败', 'login/index');

// 完整用法
return $this->error(
    $msg = '登录失败',
    $url = 'login/index',
    $data = '',
    $wait = 3,
    $header = []
);
```

### 3. redirect - URL重定向

```php
/**
 * URL重定向
 * @param  string $url 跳转的URL表达式
 * @param  integer $code http code
 * @param  array $with 隐式传参
 */

// 跳转到完整地址
return $this->redirect('/admin/index/index');

// 使用 url 函数生成地址
return $this->redirect(url('index/index', ['name' => 'think']));

// 跳转到外部网址
return $this->redirect('http://www.thinkphp.cn');

// 完整用法
return $this->redirect(
    $url = '/admin/index/index',
    $code = 302,
    $with = ['data' => 'hello']
);
```

### 4. result - 返回API数据

```php
/**
 * 返回封装后的API数据到客户端
 * @param  mixed $data 要返回的数据
 * @param  integer $code 返回的code
 * @param  mixed $msg 提示信息
 * @param  string $type 返回数据格式
 * @param  array $header 发送的Header信息
 */

// 简单用法
return $this->result(['username' => 'admin', 'role' => 'administrator']);

// 完整用法
return $this->result(
    $data = ['username' => 'admin'],
    $code = 0,
    $msg = '',
    $type = '',
    $header = []
);
```

## 完整示例

```php
<?php
namespace app\admin\controller;

class Index extends \app\BaseController
{
    /**
     * 示例1：成功跳转
     */
    public function demo1()
    {
        return $this->success('操作成功', 'index/index');
    }

    /**
     * 示例2：错误跳转
     */
    public function demo2()
    {
        return $this->error('操作失败');
    }

    /**
     * 示例3：URL重定向
     */
    public function demo3()
    {
        return $this->redirect('/admin/index/index');
    }

    /**
     * 示例4：返回API数据
     */
    public function demo4()
    {
        return $this->result([
            'username' => 'admin',
            'role' => 'administrator'
        ]);
    }
}
```

## 特性

- ✅ 自动判断 AJAX/JSON 请求，返回 JSON 格式
- ✅ HTML 请求使用模板渲染，显示友好的跳转页面
- ✅ 支持倒计时自动跳转
- ✅ 支持手动点击跳转链接
- ✅ 响应式设计，适配移动端
- ✅ 与 ThinkPHP 5.x 的跳转方法保持兼容

## 许可证

Apache-2.0
