использовать пакет "net/http"
[[Протокол HTTP]]
примеры кода + описание https://vk.com/@-205770088-nethttp-v-golang-rukovodstvo-dlya-novichkov?anchor=
### Запуск сервера: 
**ListenAndServe** – это точка входа на ваш сервер. Использует конфигурацию сервера по умолчанию, но можно задать и  свою конфигурацию (определить собственный объект _http.Server_).
```Go
func main () { 
	http. ListenAndServe (": 8080", nil) 
}
```

### Обработка запросов:
**HandleFunc** - обработчик
```Go
func defaultMuxHandler(writer http.ResponseWriter, request *http.Request) {
	json.NewDecoder(request).Decode("default");
	json.NewEncoder(writer).Encode("default")
} 
func main() { 
	http.HandleFunc("/default", defaultMuxHandler)
	http.ListenAndServe(":8080", nil) 
}
```