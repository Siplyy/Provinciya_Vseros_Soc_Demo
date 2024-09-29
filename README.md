# Решение команды "Provinciya" для Всероссийского хакатона ИИ (27-29 сентября, г.Москва)
## Участники команды
Сорочайкин Александр   
Родин Владислав  
Прокошев Тимур  
Ольховский Феликс  
Косинов Михаил  
## Краткое описание задачи
Нам предоставлена информация о событиях на платформе rutube, содержащие информацию о просмотре пользователем видео. 
Платформе важно знать возраст и пол всех пользователей, но, к сожалению, многие пользователи работают на платформе
не авторизируясь, и мы теряем информацию о них. Наша задача, разработать систему, позволяющую предсказывать пол и 
возрастную категорию пользователей, основываясь на данных об их просмотрах.
## Описание решения
### Признаки, на основе колонок из датасета
- Количество различных жанров, просматриваемых пользователем
- Самый любимый жанр пользователя
- Отдельно поработали со столбцами Браузер, ОС и устройство. Разбили на осмысленные категории, объединив минорные категории в одно целое
### Признаки на основе геолокации
Зная регион пользователя, мы добавили следующую статистическую информацию:
- Средние месячные расходы на семью, в рамках региона 
- Средняя скорость интернета в регионе (обычного и мобильного)
- Средний возраст населения в регионе
- Соотношение мужчин и женщин в регионе
### Признаки, завязанные на времени
Благодаря информации о МСК времени просмотра видео и региону пользователя мы получили точную информацию о местном времени просмотра видео.
На основе этого были получены признаки о том, когда пользователь предпочитает смотреть видео: будни/выходные, после/во время школы/работы и т.п.
### Создание датасета с информацией по видео
Важной деталью для определения социально-географических признаков пользователя является информация о том, что именно он смотрит.
Основываясь на этом, нами было принято решение создать датасет, в котором храниться информация о всех видео. Мы получали статистику
о возрасте и поле людей, которые просматривали каждое видео. Для тех видео, о которых у нас не достает информации, мы находим похожие по названию
(используя эмбеддинги названий видео) и заполняем статистику на их основе.
### Признаки на основе просматриваемых видео
- Находим среди всех просмотренных пользователем видео самые популярные, и берем по ним средние значения вычесленных статистик. Тем самым, мы учитываем информацию о пользователях, которые смотрят те же видео, что и наш пользователь. Однако, данный признак никак не учитывает индивидуальность пользователя (просто рассматривает самые популярные видео из тех, что он смотрел)
- Берем видео из любимой категории пользователя, которое он досмотрел до конца и ищем для него самое похожее среди популярных видео внутри данного жанра. Из этого также получаем признаки о пользователях, которые его смотрели
## Описание файлов в репозитории
- title_embeddings_file.ipynb - Вспомогательный ноутбук, в котором генерируются эмбеддинги для названий всех видео
