### curl是php的一个扩展，当php安装了curl扩展后就可以使用curl函数了

使用curl函数基本思想：

​    1、使用curl_init()初始化一个curl回话。

​    2、通过curl_setopt()设置你需要的全部选项

​    3、通过curl_exec()执行回话

​    4、通过curl_close()关闭回话

常用的get/post请求的封装

```php
class MyCurl
{
	public static function get($url)
	{
		//创建curl资源
		$ch = curl_init() ;
		//设置参数
		curl_setopt($ch,CURLOPT_URL,$url);//设置请求地址--如果init($url)这步可以不写
		curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);//设置是否返回数据
		curl_setopt($ch,CURLOPT_HEADER,false);//是否显示请求头
		curl_setopt($ch,CURLOPT_TIMEOUT,10);//设置超时时间，秒
		curl_setopt($ch,CURLOPT_SSL_VERIFYPEER,false);//是否启用ssl验证
		curl_setopt($ch,CURLOPT_SSL_VERIFYHOST,false);//是否验证主机
		//发起请求
		$content = curl_exec($ch);
		//关闭
		curl_close($ch);
		return $content;
	}
	public static function post($url,$data)
	{
		//创建curl资源
		$ch = curl_init() ;
		//设置参数
		curl_setopt($ch,CURLOPT_URL,$url);//设置请求地址
		curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);//设置是否返回数据
		curl_setopt($ch,CURLOPT_HEADER,false);//是否显示请求头
		curl_setopt($ch,CURLOPT_TIMEOUT,10);//设置超时时间，秒
		curl_setopt($ch,CURLOPT_POST,true);//设置post请求
		curl_setopt($ch,CURLOPT_POSTFIELDS,$data);//设置post请求参数
		curl_setopt($ch,CURLOPT_SSL_VERIFYPEER,false);//是否启用ssl验证
		curl_setopt($ch,CURLOPT_SSL_VERIFYHOST,false);//是否验证主机
		//发起请求
		$content = curl_exec($ch);
		//关闭
		curl_close($ch);
		return $content;
	}
}

RESTFUL接口（增加post	删除delete   更新put	   查询get）
namespace Curl;

class Curl
{
	private $ch;

	public function __construct()
	{
		$this->ch = curl_init();
		$this->ready();
	}
	public function get($url, $params = [])
	{
		if(count($params) > 0)
		{
			$url .= '?'.http_build_query($params);//用于生成请求字符串
		}
		$this->setUrl($url);
		return $this->exec();
	}

	public function post($url, $params)
	{
		$this->setOpt(CURLOPT_POST, 1);//设置post请求（0返回页面 1返回页面代码）
		$this->setOpt(CURLOPT_POSTFIELDS, $params);//设置post请求字段【数组】
		$this->setUrl($url);
		return $this->exec();
	}	

	public function put()
	{
		$this->setOpt(CURLOPT_CUSTOMREQUEST, 'PUT');
		$this->setOpt(CURLOPT_POSTFIELDS, $params);

		// $_SERVER['REQUEST_METHOD'];

		$this->setUrl($url);
		return $this->exec();
	}

	public function delete()
	{
		$this->setOpt(CURLOPT_CUSTOMREQUEST, 'DELETE');
		$this->setOpt(CURLOPT_POSTFIELDS, $params);
		$this->setUrl($url);
		return $this->exec();
	}

	private function ready()
	{
		$this->setOpt(CURLOPT_RETURNTRANSFER);
		$this->setOpt(CURLOPT_HEADER, 0);
	}


	private function setOpt($option, $value = 1)
	{
		curl_setopt($this->ch, $option, $value);
	}

	private function setUrl($url)
	{
		$this->setOpt(CURLOPT_URL, $url);
	}

	private function exec()
	{
		$result = curl_exec($this->ch);
		if($result)
		{
			return $result;
		}else{
			return [
				'errno' => curl_errno($this->ch),
				'error' => curl_error($this->ch)
			];
		}
	}
}

$curl = new Curl();
// $params['appid'] = '1000000001';
// $params['secret'] = '123456aaaa';
// var_dump($curl->post('http://localhost/code/1705/0525/caoliu/public/index.php?mod=ApiAuth&act=token', $params));
$result = $curl->delete('http://localhost/caoliu/public/index.php?mod=video&act=info', ['token' => '0399d7e79f9b512a3e30c51b807cb2de']);
//var_dump($result);
```

