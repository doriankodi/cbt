<!DOCTYPE html>
<html lang="ru" >
<head>
  <meta charset="UTF-8">
  <title>Колесо удачи</title>
  <!-- используем всю ширину экрана -->
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <!-- подключаем стили -->
  <style type="text/css">
  /* делаем везде так, чтобы свойства width и height задавали не размеры контента, а размеры блока */
* {
  box-sizing: border-box;
}

/* общие настройки страницы */
body {
  /* подключаем сетку */
  display: grid;
  /* ставим всё по центру */
  place-items: center;
  /* если что-то не помещается на своё место — скрываем то, что не поместилось */
  overflow: hidden;
}

/* общий блок для всех элементов */
.deal-wheel {
  /* задаём переменные блока */
  /* размеры колеса */
  --size: clamp(250px, 80vmin, 700px);
  /* настройки яркости и заливки фона секторов */
  --lg-hs: 0 3%;
  --lg-stop: 50%;
  --lg: linear-gradient(
      hsl(var(--lg-hs) 0%) 0 var(--lg-stop),
      hsl(var(--lg-hs) 20%) var(--lg-stop) 100%
    );
  /* добавляем позиционирование относительно других элементов */
  position: relative;
  /* подключаем сетку */
  display: grid;
  grid-gap: calc(var(--size) / 20);
  /* выравниваем содержимое блока по центру */
  align-items: center;
  /* задаём имена областей внутри сетки */
  grid-template-areas:
    "spinner"
    "trigger";
  /* устанавливаем размер шрифта */
  font-size: calc(var(--size) / 30);
}

/* всё, что относится ко внутренним элементам главного блока, будет находиться в области сетки с названием spinner */
.deal-wheel > * {
  grid-area: spinner;
}

/* сам блок и кнопка будут находиться в области сетки с названием trigger и будут выровнены по центру */
.deal-wheel .btn-spin {
  grid-area: trigger;
  justify-self: center;
}

/* сектор колеса */
.spinner {
  /* добавляем относительное позиционирование */
  position: relative;
  /* подключаем сетку */
  display: grid;
  /* выравниваем всё по центру */
  align-items: center;
  /* добавляем элемент в сетку */
  grid-template-areas: "spinner";
  /* устанавливаем размеры */
  width: var(--size);
  height: var(--size);
  /* поворачиваем элемент  */
  transform: rotate(calc(var(--rotate, 25) * 1deg));
  /* рисуем круглую обводку, а всё, что не поместится, — будет скрыто за кругом */
  border-radius: 50%;
}

/* всё, что внутри этого блока, будет находиться в области сетки с названием spinner */
.spinner * {
  grid-area: spinner;
}

/* текст на секторах */
.prize {
  /* включаем «гибкую» вёрстку */
  display: flex;
  align-items: center;
  /* задаём отступы от краёв блока */
  padding: 0 calc(var(--size) / 6) 0 calc(var(--size) / 40);
  /* устанавливаем размеры */
  width: 50%;
  height: 50%;
  /* устанавливаем координаты, относительно которых будем вращать текст */
  transform-origin: center right;
  /* поворачиваем текст */
  transform: rotate(var(--rotate));
  /* запрещаем пользователю выделять мышкой текст на секторах */
  user-select: none;
}

/* язычок */
.ticker {
  /* добавляем относительное позиционирование */
  position: relative;
  /* устанавливаем размеры */
  left: calc(var(--size) / -30);
  width: calc(var(--size) / 10);
  height: calc(var(--size) / 20);
  /* фон язычка */
  background: var(--lg);
  /* делаем так, чтобы язычок был выше колеса */
  z-index: 1;
  /* форма язычка */
  clip-path: polygon(20% 0, 100% 50%, 20% 100%, 0% 50%);
  /* устанавливаем точку, относительно которой будет вращаться язычок при движении колеса */
  transform-origin: center left;
}

/* кнопка запуска колеса */
.btn-spin {
  color: white;
  background: black;
  border: none;
  /* берём размер шрифта такой же, как в колесе */
  font-size: inherit;
  /* добавляем отступы от текста внутри кнопки */
  padding: 0.9rem 2rem 1rem;
  /* скругляем углы */
  border-radius: 0.5rem;
  /* меняем внешний вид курсора над кнопкой на руку*/
  cursor: pointer;
}

/* если кнопка нажата и неактивна */
.btn-spin:disabled {
  /* меняем внешний вид курсора */
  cursor: progress;
  /* делаем кнопку полупрозрачной */
  opacity: 0.25;
}

/* анимация вращения */
.is-spinning .spinner {
  transition: transform 8s cubic-bezier(0.1, -0.01, 0, 1);
}

/* анимация движения язычка */
.is-spinning .ticker {
          animation: tick 700ms cubic-bezier(0.34, 1.56, 0.64, 1);
}


/* эффект, когда колесо задевает язычок при вращении */
@keyframes tick {
  40% {
    /* чуть поворачиваем язычок наверх в середине анимации */
    transform: rotate(-12deg);
  }
}

/* анимируем выпавший сектор */
.prize.selected .text {
  /* делаем текст белым */
  color: white;
  /* настраиваем длительность анимации */
  animation: selected 800ms ease;
}

/* настраиваем анимацию текста на выпавшем секторе по кадрам */
@keyframes selected {
  /* что происходит на 25% от начала анимации */
  25% {
    /* увеличиваем текст в 1,25 раза */
    transform: scale(1.25);
    /* добавляем тексту тень */
    text-shadow: 1vmin 1vmin 0 hsla(0 0% 0% / 0.1);
  }
  40% {
    transform: scale(0.92);
    text-shadow: 0 0 0 hsla(0 0% 0% / 0.2);
  }
  60% {
    transform: scale(1.02);
    text-shadow: 0.5vmin 0.5vmin 0 hsla(0 0% 0% / 0.1);
  }
  75% {
    transform: scale(0.98);
  }
  85% {
    transform: scale(1);
  }
}
  </style>

</head>
<body>
<!-- главный блок -->
<div class="deal-wheel">
  <!-- блок с призами -->
  <ul class="spinner"></ul>
  <!-- язычок барабана -->
  <div class="ticker"></div>
  <!-- кнопка -->
  <button class="btn-spin">Выбрать пытку</button>
</div>
  <!-- подключаем скрипт -->
  <script>
  // надписи и цвета на секторах
const prizes = [
  {
    text: "Удары по яйцам 10 раз",
    color: "hsl(360 100% 50%)",
  },
  { 
    text: "Подвесить бутылку на яйца",
    color: "hsl(323 100% 50%)",
  },
  { 
    text: "Укол физ раствора в правое яичко",
    color: "hsl(286 100% 50%)",
  },
  {
    text: "Укол в левое яичко физ раствора",
    color: "hsl(249 100% 50%)",
  },
  {
    text: "Проколоть иглой уздечку",
    color: "hsl(212 100% 50%)",
  },
  {
    text: "Перетянуть яички на 10 минут",
    color: "hsl(175 100% 50%)",
  },
  {
    text: "Проколоть головку тонкой иглой",
    color: "hsl(138 100% 50%)",
  },
  {
    text: "Перетянуть член на 10 минут",
    color: "hsl(101 100% 50%)",
  },
  {
    text: "Записать в чат пошлое голосовое о себе",
    color: "hsl(64 100% 50%)",
  },
  {
    text: "Выпить кружку своей мочи",
    color: "hsl(27 100% 50%)",
  },
  { 
    text: "Кастрация бинго",
    color: "hsl(212 100% 50%)",
  },
  {
    text: "Одеть пв на три дня",
    color: "hsl(175 100% 50%)",
  }
];

// создаём переменные для быстрого доступа ко всем объектам на странице — блоку в целом, колесу, кнопке и язычку
const wheel = document.querySelector(".deal-wheel");
const spinner = wheel.querySelector(".spinner");
const trigger = wheel.querySelector(".btn-spin");
const ticker = wheel.querySelector(".ticker");

// на сколько секторов нарезаем круг
const prizeSlice = 360 / prizes.length;
// на какое расстояние смещаем сектора друг относительно друга
const prizeOffset = Math.floor(180 / prizes.length);
// прописываем CSS-классы, которые будем добавлять и убирать из стилей
const spinClass = "is-spinning";
const selectedClass = "selected";
// получаем все значения параметров стилей у секторов
const spinnerStyles = window.getComputedStyle(spinner);

// переменная для анимации
let tickerAnim;
// угол вращения
let rotation = 0;
// текущий сектор
let currentSlice = 0;
// переменная для текстовых подписей
let prizeNodes;

// расставляем текст по секторам
const createPrizeNodes = () => {
  // обрабатываем каждую подпись
  prizes.forEach(({ text, color, reaction }, i) => {
    // каждой из них назначаем свой угол поворота
    const rotation = ((prizeSlice * i) * -1) - prizeOffset;
    // добавляем код с размещением текста на страницу в конец блока spinner
    spinner.insertAdjacentHTML(
      "beforeend",
      // текст при этом уже оформлен нужными стилями
      `<li class="prize" data-reaction=${reaction} style="--rotate: ${rotation}deg">
        <span class="text">${text}</span>
      </li>`
    );
  });
};

// рисуем разноцветные секторы
const createConicGradient = () => {
  // устанавливаем нужное значение стиля у элемента spinner
  spinner.setAttribute(
    "style",
    `background: conic-gradient(
      from -90deg,
      ${prizes
        // получаем цвет текущего сектора
        .map(({ color }, i) => `${color} 0 ${(100 / prizes.length) * (prizes.length - i)}%`)
        .reverse()
      }
    );`
  );
};

// создаём функцию, которая нарисует колесо в сборе
const setupWheel = () => {
  // сначала секторы
  createConicGradient();
  // потом текст
  createPrizeNodes();
  // а потом мы получим список всех призов на странице, чтобы работать с ними как с объектами
  prizeNodes = wheel.querySelectorAll(".prize");
};

// определяем количество оборотов, которое сделает наше колесо
const spinertia = (min, max) => {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min;
};

// функция запуска вращения с плавной остановкой
const runTickerAnimation = () => {
  // взяли код анимации отсюда: https://css-tricks.com/get-value-of-css-rotation-through-javascript/
  const values = spinnerStyles.transform.split("(")[1].split(")")[0].split(",");
  const a = values[0];
  const b = values[1];  
  let rad = Math.atan2(b, a);
  
  if (rad < 0) rad += (2 * Math.PI);
  
  const angle = Math.round(rad * (180 / Math.PI));
  const slice = Math.floor(angle / prizeSlice);

  // анимация язычка, когда его задевает колесо при вращении
  // если появился новый сектор
  if (currentSlice !== slice) {
    // убираем анимацию язычка
    ticker.style.animation = "none";
    // и через 10 миллисекунд отменяем это, чтобы он вернулся в первоначальное положение
    setTimeout(() => ticker.style.animation = null, 10);
    // после того, как язычок прошёл сектор - делаем его текущим 
    currentSlice = slice;
  }
  // запускаем анимацию
  tickerAnim = requestAnimationFrame(runTickerAnimation);
};

// функция выбора призового сектора
const selectPrize = () => {
  const selected = Math.floor(rotation / prizeSlice);
  prizeNodes[selected].classList.add(selectedClass);
};

// отслеживаем нажатие на кнопку
trigger.addEventListener("click", () => {
  // делаем её недоступной для нажатия
  trigger.disabled = true;
  // задаём начальное вращение колеса
  rotation = Math.floor(Math.random() * 360 + spinertia(2000, 5000));
  // убираем прошлый приз
  prizeNodes.forEach((prize) => prize.classList.remove(selectedClass));
  // добавляем колесу класс is-spinning, с помощью которого реализуем нужную отрисовку
  wheel.classList.add(spinClass);
  // через CSS говорим секторам, как им повернуться
  spinner.style.setProperty("--rotate", rotation);
  // возвращаем язычок в горизонтальную позицию
  ticker.style.animation = "none";
  // запускаем анимацию вращение
  runTickerAnimation();
});

// отслеживаем, когда закончилась анимация вращения колеса
spinner.addEventListener("transitionend", () => {
  // останавливаем отрисовку вращения
  cancelAnimationFrame(tickerAnim);
  // получаем текущее значение поворота колеса
  rotation %= 360;
  // выбираем приз
  selectPrize();
  // убираем класс, который отвечает за вращение
  wheel.classList.remove(spinClass);
  // отправляем в CSS новое положение поворота колеса
  spinner.style.setProperty("--rotate", rotation);
  // делаем кнопку снова активной
  trigger.disabled = false;
});

// подготавливаем всё к первому запуску
setupWheel();
  
  </script>

</body>
</html>
