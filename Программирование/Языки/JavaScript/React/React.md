**React** — это js библиотека для создания пользовательских интерфейсов (UI). Его главная задача — обеспечение вывода на экран того, что можно видеть на веб-страницах.
	React значительно облегчает создание интерфейсов благодаря разбиению каждой страницы на небольшие фрагменты. Мы называем эти фрагменты компонентами.

документация: https://react.dev/learn

[[Создание проекта]]

**Компонент React** — это, если по-простому, участок кода, который представляет часть веб-страницы. Каждый компонент — это JavaScript-функция, которая возвращает кусок кода, представляющего фрагмент страницы. Для формирования страницы мы вызываем эти функции в определённом порядке, собираем вместе результаты вызовов и показываем их пользователю.

React использует язык программирования, называемый JSX, который похож на HTML, но работает внутри JavaScript, что отличает его от HTML.
# Функциональные компоненты:
### Вызов компонентов как из HTML:

```html
<link rel="stylesheet" href="app.css">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Open+Sans:400,800">
<div id="container">
	<div id="hook"></div>
	<h1>Play Music</h1>
	<input type="file" id="files" name="files[]" multiple />
</div>

<script src="app.js"></script>
<script type="text/babel">
	const OurFirstComponent = () => {
		return (
			<h1>Hello, I am a React Component!</h1>
		);
	}
	const placeWeWantToPutComponent = document.getElementById('hook');
	ReactDOM.render(OurFirstComponent(), placeWeWantToPutComponent);
</script>
```
В этом коде вставлен фрагмент с функцией `OurFirstComponent`, которая выводит надпись на страницу. тег `<h1>` запихивается внутрь элемента с `id="hook"` с помощью ReactDOM:
```JavaScript
	const placeWeWantToPutComponent = document.getElementById('hook');
	ReactDOM.render(OurFirstComponent(), placeWeWantToPutComponent);

или
	const placeWeWantToPutComponent = document.getElementById('hook');
	ReactDOM.render(<OurFirstComponent />, placeWeWantToPutComponent);
```

### Вызов компонентов из компонентов:
```HTML
<link rel="stylesheet" href="app.css">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Open+Sans:400,800">
<div id="container">
	<div id="hook"></div>
	<h1>Play Music</h1>
	<input type="file" id="files" name="files[]" multiple />
</div>

<script src="app.js"></script>
<script type="text/babel">
	function OurFirstComponent() {
		return (
			<h1>I am the child!</h1>
		);
	}
	
	function Container() {
		return (
			<div>
				<h1>I am the parent!</h1>
				<OurFirstComponent />
			</div>
		);
	}
	
	const placeWeWantToPutComponent = document.getElementById('hook');
	ReactDOM.render(Container(), placeWeWantToPutComponent);
</script>
```


# Классы компонентов:
Классы компонентов должны содержать функцию, называемую `render()`. Эта функция возвращает JSX-код компонента. Их можно использовать так же, как функциональные компоненты.
В том случае, если вас интересуют компоненты без состояния, предпочтение следует отдать функциональным компонентам, их, в частности, легче читать.
```html
<link rel="stylesheet" href="app.css">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Open+Sans:400,800">
<div id="container">
	<div id="hook"></div>
	<h1>Play Music</h1>
	<input type="file" id="files" name="files[]" multiple />
</div>

<script src="app.js"></script>
<script type="text/babel">
	function OurFirstComponent() {
		return (
			<h1>I am the child!</h1>
		);
	}
	class Container extends React.Component {
		render() {
			return (
				<div>
					<h1>I am the parent!</h1>
					<OurFirstComponent />
				</div>
			);
		}
	}
const placeWeWantToPutComponent = document.getElementById('hook');
ReactDOM.render(<Container />, placeWeWantToPutComponent);
```

# JS код в JSX
В JSX-код можно помещать переменные JavaScript:
```JavaScript
class Container extends React.Component {
	render() {
		const greeting = 'I am a stfffring!';
		return (
			<div>
				<h1>{ greeting }</h1>
				<OurFirstComponent />
			</div>
		);
	}
}
```
Можно помещать вызов функций:
```JavaScript
class Container extends React.Component {
  render() {
    const addNumbers = (num1, num2) => {
      return num1 + num2;
    };
    return (
      <div>
        <h1>The sum is: { addNumbers(1, 2) }</h1>
        <OurFirstComponent />
      </div>
    );
  }
}
```

в коде нельзя использовать строку `<a href="#" title="Play video" class="play" />`
можно заменить слово `class` на `className`: `<a href="#" title="Play video" className="play" />`

