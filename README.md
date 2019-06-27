# JpgToWebp class PHP
**Установка**<br>
Поместите сам класс в перед вызовом изображений. Желательно ближе к `<head></head>`.<br> Можно инициализировать в начале после открывающегося тега `<body>`. Код самого класса приведен ниже:
```
class Webp{
	public $prefix = "thumb"; //Переменная подпапки куда класть webp
	public $access_webp = Null;
	public $prefix_format = ".webp";

	function getHeaders(){
		return $_SERVER['HTTP_ACCEPT'];
	}

	function access(){
		return preg_match("(image\/webp)", $this->getheaders()); 
	}

	function concatWebp($string){
		$filename = explode(".", $string);
		unset($filename[count($filename)-1]);
		$filename = implode(".", $filename) . $this->prefix_format;
		return $filename;
	}

	function absoluteUrl($url){
		return $_SERVER['DOCUMENT_ROOT'].$url;
	}

	function getImage($url){
		if ($this->access_webp){
			$mass = explode("/", $url);
			$result = "";

			if (count($mass) > 0){
				for ($i = 0; $i < count($mass); $i++){
					if ($i !== (count($mass)-1)){
						$result .= $mass[$i] . "/";
					}else{
						$result .= ($this->prefix)? $this->prefix . "/" : "";
						$result .= $this->concatWebp($mass[$i]);
					};
				}
			};

			if(is_file($this->absoluteUrl($result))){
				return $result;

			}else return $url;

		} return $url;
	}

	function __construct(){
		$this->access_webp = $this->access();
	}
}
```
**Вызов изображения**<br>
В качестве аргумента в getImage передаешь урл на JPG изображение если найдет webp во вложенной папке thumb то возвращает webp<br>
* Можно передать в public `$prefix = "";`, тогда он будет искать в том же каталоге.
```echo $webp->getImage("/project/2/330x247/d2.jpg"); ```

