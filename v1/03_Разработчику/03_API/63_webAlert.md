###Отображает сообщение alert с помощью JavaScript

void webAlert(string $msg[, $url]);

**$msg** - сообщение показываемое в alert
**$url** - определяет действие, которое после alert в зависимости от значения
пусто - ничего не делать
значение начинается на 'javascript:' - выполнится код, помещенный после
указан адрес - произойдет редирект

***

####Пример

	$modx->webAlert('Hello!');
	выведет alert с сообщением «Hello!»

	$modx->webAlert('Hello!','http://modx.im');
	//выведет alert с сообщением «Hello!», а затем произойдет редирект на http://modx.im

Смотрите также: regClientScript(), sendAlert()