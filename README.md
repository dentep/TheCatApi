# TheCatAPI

Общее количество времени: 20-30 часов

<ul>
  <li>1 - 2 часа: создание приложения на js</li>
  <li>3 часа: документация TypeScript и transition js/jsx => ts/tsx</li>
  <li>1 час: Проверки</li>
  <li>5 часов (ужас): пытаться сделать кнопки навигации как в примере Figma (см. ниже)</li>
  <li>10-20 часов: обучение и применение Redux</li>
</ul>

# Alpha 1.0.3 prod

С 27.07.2021 этот проект считается завершенным. Я буду продолжать данный проект (дополнять функционал / делать boilerplate) под другим именем проекта (TheCatAPI).

`Установка (можно любой из способов):`

<ol>
  <li>Заходим на <a href="https://expo.io/@denistepp/Cats-YakovlevStudio?release-channel=staging">Проект в Expo</a>. Сканим QR и смотрим приложение.</li>
  <li>Клонируем репу -> npm install -> expo start</li>
</ol>

`Что нового:`

`1.0.3`

-   Добавлен Redux Persist
-   Добавлен Redux Saga

`1.0.2`

Я думал добавить redux-persist и redux-saga, но за имением всего 6 часов свободного времени за эту неделю я решил посвятить нашей задаче по ТЕНЯМ (shadows). Оказалось, что сделать их РЕАЛЬНО!

После всех моих мучений и вопросов на SO, я все так выполнил решение как было описано в Figma. Результат в [Дополнительно](#Дополнительно).

`1.0.1`

Я добавил контекст приложения используя два метода (мне было очень интересно какая будет разница между hooks и redux). Так же пофиксил `StyleProps` и `children` (`ReactNode`).

В итоге:

<ul>
<li>В приложении теперь есть контекст Redux который контролирует favourites картинки. Так же добавлены actions (удобно на самом деле) но я делал dispatch напрямую так как были простые типы данных</li>
<li>Так же, я добавил контекст другим способом: Context API из useReducer() и useContext() хуков</li>
<li>Картинки в FavouritesScreen загружается из redux контекста. Это можно легко поменять в рендере на state.favourites.</li>
<li>Функция удаления картинки из favourites пока еще не добавлена, но я планирую сделать или Swipeable или onLongPress() (не думаю что сложно так как action я уже сделал)</li>
<li>Так же есть функция рефреша - она синхронизирует данные из апи и переписывает контекст. Не знаю, real case это или нет, но я посчитал лишним выкидывать готовые методы апи из проекта. Косяк только в том что надо теперь сохранять контекст когда выходишь из приложения и загружать его снова когда заходишь чтобы не было нестыковок с апи и контекстом.</li>
</ul>

Для себя я выделил что Redux неприхотлив только когда создаешь boilerplate. Как только основа готова, redux становится крутым инструментом в приложении.

Я бы поспорил что использование Redux нужно всегда, так как Context API решает (почти) все те же самые проблемы, но более легким путем. Нужно правильно расчитывать ресурсы и выделить для себя нужную архитектуру.

`Стек`:

<ol>
  <li>Typescript (90%) / Javascript (10%) - не считая Shadow Components</li>
  <li>Redux & useContext() + useReducer()</li>
  <li>Lottie</li>
  <li>React Navigation</li>
  <li>Expo</li>
</ol>

`Реализованные задачи:`

<ol>
  <li>Экран BreedsScreen для вывода всех пород</li>
  <li>Экран FavouritesScreen для вывода избранных пород</li>
  <li>Экран BreedScreen для вывода определенной породы (фото, название породы, описание)</li>
  <li>Экран BreedScreen для вывода определенной породы (фото, название породы, описание)
    <ol>
      <li> Функция "Другое Фото" которая загружает рандомное фото по определенной породе</li>
      <li> Функция "Добавить в Избранное" добавляет фото в избранные фото (api /favourites)</li>
    </ol>
  </li>
  <li>Lottie анимации (на загрузке методов и отдельных фото)</li>
</ol>

`Проверки:`

<ol>
  <li>Провека на наличие интернет-соединения</li>
  <li>Проверка на ответы API (result.ok)</li>
  <li>Проверка на пустые фотографии (может быть что нет image поля, а может быть есть image, но нет image.url => выводим дефолт)</li>
  <li>Проверка на пустой список "избранных" фото => выводим NoDataIndicator</li>
  <li>Проверка на добавление в избранное (если фото пустое)</li>
  <li>Проверка на существующее фото в избранных (если есть, то вывести сообщение пользователю)</li>
</ol>

`TODO:`

<ol>
  <li>обновление storage на запуске приложения</li>
  <li>локализация</li>
  <li>login/logout</li>
  <li>смена темы</li>
  <li>Добавить проверки на "другое фото" (анимации, и state по обновлению фото)</li>
  <li>Удаление фото из избранных (но это не точно...)</li>
</ol>

# Дополнительно

## <a name="curious" href="https://cleancoders.com/episode/clean-code-episode-43">Как не надо тратить время, или тратить его с умом</a>

Пример Figma:
<img alt="Image" title="icon" src="figma.png" />

Обратите внимание на кнопки навигации (дом и сердце). Тут есть эффект "нажатия", в React Navigation это обычно используется с пропом `focused`, но эффект не совсем обычный...

Я потратил на разбор данного дизайна более 5 часов и так не(!) пришел к единому хорошему (быстрому и качественному) решению (если интересно: Боб Мартин, Clean Code, ep. 43: Productivity).

Вот мой разбор путей:

1. (Степень сложности - 0.1 из 100 ): Попробовать добавить shadow. Получилось для кнопки без фокуса. А как добавить inset shadow?
2. (Степень сложности - 4 из 100 ): 4 за 4 минуты после которых я понял что inset shadow вообще никак не встроен в ViewProps и React.CSSProperties. Ищем где можно использовать inset shadow.
3. (Степень сложности - 20 из 100): Пробуем xml svg от разных сторонних библиотек. Создаем кружок и накидываем shadow....все бы хорошо но css в svg на react native тоже не поддерживает inset!!!
4. (Степень сложности - 60 из 100 ): Переходим к более сложным и креативным (нет) путям. Я попробовал `expo-linear-gradient` и добился неплохого эффекта но все равно не получилось получить идеальный inset. Так же, рендер двух градиентов для разного фокуса - такое себе решение
5. (Степень сложности GIMP из GIMP): Создать две png картинки (эффект с нажатием и без). Я пользуюсь GIMP поэтому полез туда. После битых 1.5 часа я не смог сделать нормальную картинку и бросил это дело (нет). Кстати косяк с conditional rendering тут все равно остается, две картинки для кнопок навигации - не очень.
6. (Степень сложности - это реально?) После всех методов я так и не смог сделать такие же кнопки как на прототипе -> следовательно, не справился со своей задачей.

Я так много времени потратил на это, и теперь хочу знать, реально ли вообще сделать такой дизайн? Если да, то подскажите пожалуйста как это сделать или намекните на метод чтобы я смог хотя бы попробовать! Лично мне он понравился и я хотел бы его использовать его в последующих проектах.

v2: я все-таки нашел немного время чтобы вернуться за это проблемой и провел небольшой research. Вот что я для себя нашел:

<ul>
  <li>Такой дизайн кнопок называется neomorphism. Он был очень популярен в 2020 как стиль UI/UX. Почти все дизайнеры старались его использовать в своих новейших приложениях, но, как говорится, что-то пошло не так.</li>
  <li>Проблема этого дизайна заключалась а том, что его lifespan был очень коротким, и немногие моб-разработчики библиотек смогли к нему как следует прикоснутся (веб в счет не берем, там другой подход)</li>
  <li>Для себя я нашел несколько библиотек:
    <ol>
      <li><a href"https://github.com/tokkozhin/react-native-neomorph-shadows">react-native-neomorph-shadows</a></li>
      <li><a href"https://github.com/shaneboyar/react-native-neumorphic">react-native-neumorphic</a></li>
      <li><a href"https://github.com/Jamyth/react-native-neu-element">react-native-neu-element</a></li>
      <li><a href"https://github.com/mattperls-code/shadows-rn">shadows-rn</a></li>
    </ol>
  </li>
</ul>

Neumorphic design это просто двойное наложение shadow на определнный svg элемент который выдает эффект "рельефности". Его не сложно собрать, но подобрать нужную библиотеку было достаточно проблематично.

Мне больше всего понравилась <a href="https://github.com/tokkozhin/react-native-neomorph-shadows">react-native-neomorph-shadows</a>, но с версии 1.0.0 он не может работать с expo: библиотека была построена с `ReactNativeART`, а Expo решил убрать ART из последних SDK.

После многочисленных попыток я все таки смог установить версию 0.0.8 (скопировал просто себе все файлы в проект - 37КВ) а настроил тени. Разрешил js (переписывать на ts уже не было сил) и все! Результат ниже (работает на iOS и Android).

<img src="cats.gif" width="300" height="600"/>

Отдельное спасибо за помощь <a href="https://stackoverflow.com/users/7965547/ahmed-gaber">Ahmed Gaber</a> так как он был тем, кто пришел и спас меня с этой библиотекой, и помог установить ее до релиза 1.0.0. Вопрос на stackoverflow: <a href="https://stackoverflow.com/questions/68446420/react-native-inset-shadow-effect">здесь</a>.
