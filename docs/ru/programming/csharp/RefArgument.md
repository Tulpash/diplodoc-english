# Передача аргумента по ссылке
Можно использовать ключевое слово **ref** для  передачи аргумента в метод по ссылке.

## как работает с Value Type
Может показаться что происходит боксинг, но это не так. В метод просто передается указатель на адрес этой перемнной в памяти (стеке).

## как работает с Reference Type
Кажется что это и так ссылочныый тип так зачем нам тут **ref**, но по умолчанию в метод передается копия ссылки и как следствие если в методе попытаться заменить объект на новый, то мы сделаем это только в рамкх метода, но елси передать аргумент как **ref**, то мы передадим ссылку на ссылку и при переопределении заменим объект везде, а не только в рамках метода.