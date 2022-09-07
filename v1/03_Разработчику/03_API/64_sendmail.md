Функция отправки почты через API Evolution CMS. 

Работает в связке с настройками, то есть отправляет почту способом, указанным в панели управления сайта. 
```
$modx->sendmail()
```

## Пример использования:

### Простой вариант 
```
   		$param = array();
		$param['from']    = "{$site_name}<{$emailsender}>";
		$param['subject'] = $emailsubject;
		$param['body']    = $message;
		$param['to']      = $email;
		$rs = $modx->sendmail($param);
```

### Вариант с расширенными настройками  

```
		$modx->loadExtension('MODxMailer');
		$modx->mail->IsHTML($isHtml);
		$modx->mail->From	= $from;
		$modx->mail->FromName	= $fromname;
		$modx->mail->Subject	= $subject;
		$modx->mail->Body	= $report;
		AddAddressToMailer($modx->mail,"replyto",$replyto);
		AddAddressToMailer($modx->mail,"to",$to);
		AddAddressToMailer($modx->mail,"cc",$cc);
		AddAddressToMailer($modx->mail,"bcc",$bcc);
		AttachFilesToMailer($modx->mail,$attachments);
		if(!$modx->mail->send()) return 'Main mail: ' . $_lang['ef_mail_error'] . $modx->mail->ErrorInfo;
 ```
Значения некоторых из полей (From и Fromname, допустим) при при инициализации ModxMailer подставляются из конфигурации сайта. Так что, если нет необходимости их заменять, число параметров можно сократить.

### Вариант использования \Helpers\Mailer

Этот способ доступен начиная с версии 2.х, описание использования на странице [Helpers](/03_develop/06_di_v_evolution_cms/04_helper.html).
