Alipay
======

支付宝SDK在Laravel5封装包。

该拓展包想要达到在Laravel5框架下，便捷使用支付宝的目的。

## 安装

```
composer require latrell/alipay dev-master
```

更新你的依赖包 ```composer update``` 或者全新安装 ```composer install```。


## 使用

要使用支付宝SDK服务提供者，你必须自己注册服务提供者到Laravel服务提供者列表中。
基本上有两种方法可以做到这一点。

找到 `config/app.php` 配置文件中，key为 `providers` 的数组，在数组中添加服务提供者。

```php
    'providers' => [
        // ...
        'Latrell\Alipay\AlipayServiceProvider',
    ]
```

运行 `php artisan vendor:publish` 命令，发布配置文件到你的项目中。

配置文件 `config/latrell-alipay.php` 为公共配置信息文件， `config/latrell-alipay-web.php` 为Web版支付宝SDK配置， `config/latrell-alipay-mobile.php` 为手机端支付宝SDK配置。

## 例子

```php
	/**
	 * 异步通知
	 */
	public function webNotify()
	{
		if (! app('alipay.web')->verify()) {
			Log::notice('Alipay notify post data verification fail.', [
				'data' => Request::instance()->getContent()
			]);
			return 'fail';
		}
	
		switch (Input::get('trade_status')) {
			case 'TRADE_SUCCESS':
			case 'TRADE_FINISHED':
				Log::debug('Alipay notify post data verification success.', [
					'out_trade_no' => Input::get('out_trade_no'),
					'trade_no' => Input::get('trade_no')
				]);
				break;
		}
	
		return 'success';
	}
	
	/**
	 * 同步通知
	 */
	public function webReturn()
	{
		if (! app('alipay.web')->verify()) {
			Log::notice('Alipay return query data verification fail.', [
				'data' => Request::getQueryString()
			]);
			return view('alipay.fail');
		}

		switch (Input::get('trade_status')) {
			case 'TRADE_SUCCESS':
			case 'TRADE_FINISHED':
				Log::debug('Alipay notify get data verification success.', [
					'out_trade_no' => Input::get('out_trade_no'),
					'trade_no' => Input::get('trade_no')
				]);
				break;
		}

		return view('alipay.success');
	}
```

