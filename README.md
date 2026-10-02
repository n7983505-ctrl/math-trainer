<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Интерактивный тренажёр: Задачи на пропорциональное деление</title>

<script>
  window.MathJax = {
    tex: {
      inlineMath: [['$', '$'], ['\\(', '\\)']],
      displayMath: [['$$', '$$'], ['\\[', '\\]']]
    },
    options: {
      skipHtmlTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
    }
  };
</script>
<script id="MathJax-script" async
        src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

<style>
  :root{
    --bg:#eef2f9;
    --card:#ffffff;
    --ink:#1f2937;
    --muted:#6b7280;
    --accent:#2563eb;
    --accent2:#0ea5e9;
    --ok:#16a34a;
    --ok-bg:#ecfdf5;
    --err:#dc2626;
    --err-bg:#fef2f2;
    --warn:#d97706;
    --line:#e5e7eb;
    --shadow:0 1px 3px rgba(15,23,42,.08), 0 8px 24px rgba(15,23,42,.06);
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    background:var(--bg);
    color:var(--ink);
    font-family:'Segoe UI', 'Inter', system-ui, -apple-system, Arial, sans-serif;
    line-height:1.55;
    font-size:16px;
  }
  a{color:var(--accent);}

  /* ---------- Шапка ---------- */
  .hero{
    background:linear-gradient(135deg,#1e3a8a 0%, #2563eb 55%, #0ea5e9 100%);
    color:#fff;
    padding:28px 20px 22px;
    box-shadow:0 6px 24px rgba(30,58,138,.25);
  }
  .hero-inner{max-width:1080px;margin:0 auto;}
  .hero h1{margin:0 0 4px;font-size:1.65rem;letter-spacing:.3px;}
  .hero .sub{margin:0 0 18px;opacity:.9;font-size:1rem;}
  .score-panel{
    display:flex;flex-wrap:wrap;gap:12px;align-items:center;
  }
  .score-item{
    background:rgba(255,255,255,.14);
    border:1px solid rgba(255,255,255,.25);
    border-radius:12px;
    padding:8px 16px;
    display:flex;flex-direction:column;align-items:center;
    min-width:110px;
    backdrop-filter:blur(4px);
  }
  .score-num{font-size:1.35rem;font-weight:700;line-height:1.1;}
  .score-lbl{font-size:.72rem;text-transform:uppercase;letter-spacing:.6px;opacity:.85;}
  .progress{
    margin-top:16px;height:10px;border-radius:999px;
    background:rgba(255,255,255,.22);overflow:hidden;
  }
  .progress-fill{
    height:100%;width:0%;border-radius:999px;
    background:linear-gradient(90deg,#a7f3d0,#34d399);
    transition:width .45s ease;
  }

  /* ---------- Кнопки ---------- */
  button{
    font-family:inherit;font-size:.9rem;font-weight:600;
    border-radius:10px;border:1px solid transparent;
    padding:9px 16px;cursor:pointer;transition:.18s;
    background:var(--accent);color:#fff;
  }
  button:hover{filter:brightness(1.08);transform:translateY(-1px);}
  button:active{transform:translateY(0);}
  button.ghost{
    background:transparent;color:var(--accent);
    border:1px solid var(--accent);
  }
  button.ghost:hover{background:rgba(37,99,235,.08);}
  .hero button.ghost{
    color:#fff;border-color:rgba(255,255,255,.7);
    background:rgba(255,255,255,.1);
  }
  .hero button.ghost:hover{background:rgba(255,255,255,.22);}

  /* ---------- Общая теория ---------- */
  .wrap{max-width:1080px;margin:0 auto;padding:26px 18px 70px;}
  .general-theory{
    background:var(--card);border-radius:16px;box-shadow:var(--shadow);
    padding:6px 20px;margin-bottom:26px;border-left:5px solid var(--accent2);
  }
  .general-theory summary{
    cursor:pointer;font-weight:700;font-size:1.05rem;padding:14px 0;
    color:#1e3a8a;list-style:none;
  }
  .general-theory summary::-webkit-details-marker{display:none;}
  .general-theory summary::before{content:"▸ ";color:var(--accent2);}
  .general-theory[open] summary::before{content:"▾ ";}
  .theory-grid{
    display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
    gap:16px;padding-bottom:18px;
  }
  .theory-card{
    background:#f8fafc;border:1px solid var(--line);border-radius:12px;padding:14px 16px;
  }
  .theory-card h4{margin:0 0 8px;font-size:.95rem;color:#1e3a8a;}
  .theory-card p{margin:0 0 8px;}
  .theory-card p:last-child{margin-bottom:0;}

  /* ---------- Секции типов ---------- */
  .type-section{
    background:var(--card);border-radius:16px;box-shadow:var(--shadow);
    padding:20px 22px 24px;margin-bottom:26px;
  }
  .type-section h2{
    margin:0 0 14px;font-size:1.2rem;color:#1e3a8a;
    padding-bottom:10px;border-bottom:2px solid var(--line);
  }
  .type-section details.theory{
    background:#f1f5fd;border:1px solid #dbe4f7;border-radius:12px;
    padding:0 16px;margin-bottom:18px;
  }
  .type-section details.theory summary{
    cursor:pointer;font-weight:600;color:#1d4ed8;padding:12px 0;
    list-style:none;font-size:.92rem;
  }
  .type-section details.theory summary::-webkit-details-marker{display:none;}
  .type-section details.theory summary::before{content:"💡 ";}
  .theory-body{padding:0 0 14px;font-size:.94rem;color:#334155;}
  .theory-body p{margin:0 0 9px;}

  /* ---------- Карточка задания ---------- */
  .tasks{display:grid;gap:14px;}
  .task{
    border:1px solid var(--line);border-radius:13px;padding:14px 16px 12px;
    background:#fff;transition:.2s;
  }
  .task:hover{border-color:#c7d2fe;}
  .task.solved{background:var(--ok-bg);border-color:#a7f3d0;}
  .task-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;}
  .num{
    display:inline-flex;align-items:center;justify-content:center;
    width:26px;height:26px;border-radius:50%;
    background:#e0e7ff;color:#3730a3;font-size:.82rem;font-weight:700;
  }
  .status{font-size:.75rem;text-transform:uppercase;letter-spacing:.5px;color:var(--muted);font-weight:600;}
  .status.ok{color:var(--ok);}
  .status.err{color:var(--err);}
  .task-text{margin-bottom:11px;}
  .controls{display:flex;flex-wrap:wrap;gap:9px;align-items:center;}
  .controls input{
    width:150px;padding:9px 12px;border-radius:10px;
    border:1.5px solid #cbd5e1;font-size:.95rem;font-family:inherit;
    background:#fff;transition:.18s;
  }
  .controls input:focus{
    outline:none;border-color:var(--accent);
    box-shadow:0 0 0 3px rgba(37,99,235,.15);
  }
  .controls input:disabled{background:#f1f5f9;color:var(--muted);}
  .unit{font-size:.88rem;color:var(--muted);font-weight:600;}
  .feedback{margin-top:9px;font-size:.88rem;font-weight:600;min-height:0;}
  .feedback.ok{color:var(--ok);}
  .feedback.err{color:var(--err);}
  .feedback.warn{color:var(--warn);}
  .hint{
    margin-top:10px;padding:11px 14px;border-radius:10px;
    background:#fffbeb;border-left:4px solid #fbbf24;
    font-size:.9rem;color:#78350f;
  }
  .hint .ans-line{display:block;margin-top:7px;font-weight:700;color:#b45309;}

  /* ---------- Подвал ---------- */
  footer{
    text-align:center;padding:22px 16px 34px;
    color:var(--muted);font-size:.88rem;
  }
  footer .author{
    display:inline-block;margin-top:6px;padding:8px 20px;border-radius:999px;
    background:#fff;border:1px solid var(--line);font-weight:600;color:#1e3a8a;
    box-shadow:var(--shadow);
  }

  @media (max-width:600px){
    .hero h1{font-size:1.3rem;}
    .score-item{min-width:88px;padding:7px 12px;}
    .score-num{font-size:1.1rem;}
    .controls input{width:120px;}
    .type-section{padding:16px 14px 18px;}
  }
</style>
</head>
<body>

<header class="hero">
  <div class="hero-inner">
    <h1>Интерактивный тренажёр по математике</h1>
    <p class="sub">Тема: «Задачи на пропорциональное деление»</p>

    <div class="score-panel">
      <div class="score-item">
        <span class="score-num" id="score">0</span>
        <span class="score-lbl">баллов</span>
      </div>
      <div class="score-item">
        <span class="score-num" id="total">0</span>
        <span class="score-lbl">всего заданий</span>
      </div>
      <div class="score-item">
        <span class="score-num" id="percent">0%</span>
        <span class="score-lbl">выполнено</span>
      </div>
      <button class="ghost" id="resetBtn">Сбросить всё</button>
    </div>

    <div class="progress"><div class="progress-fill" id="progressFill"></div></div>
  </div>
</header>

<main class="wrap">

  <!-- ======================= ОБЩАЯ ТЕОРИЯ ======================= -->
  <details class="general-theory" open>
    <summary>Основные теоретические сведения</summary>
    <div class="theory-grid">

      <div class="theory-card">
        <h4>Пропорция</h4>
        <p>Пропорцией называют равенство двух отношений:</p>
        <p>$a:b=c:d$ &nbsp;или&nbsp; $\dfrac{a}{b}=\dfrac{c}{d}=k$, где $k$ — коэффициент пропорциональности.</p>
        <p><b>Основное свойство:</b> $ad=bc$, откуда</p>
        <p>$a=\dfrac{bc}{d}$, &nbsp; $b=\dfrac{ad}{c}$, &nbsp; $c=\dfrac{ad}{b}$, &nbsp; $d=\dfrac{bc}{a}$.</p>
      </div>

      <div class="theory-card">
        <h4>Прямая пропорциональная зависимость</h4>
        <p>При увеличении (уменьшении) одной величины в $k$ раз другая увеличивается (уменьшается) во столько же раз:</p>
        <p>$a=k\cdot b$.</p>
        <p>Пропорция: $\dfrac{a_1}{a_2}=\dfrac{b_1}{b_2}$.</p>
      </div>

      <div class="theory-card">
        <h4>Обратная пропорциональная зависимость</h4>
        <p>При увеличении (уменьшении) одной величины в $k$ раз другая уменьшается (увеличивается) во столько же раз:</p>
        <p>$a=\dfrac{k}{b}$.</p>
        <p>Пропорция: $\dfrac{a_1}{a_2}=\dfrac{b_2}{b_1}$.</p>
      </div>

      <div class="theory-card">
        <h4>Деление числа в данном отношении</h4>
        <p>Чтобы разделить число $S$ в отношении $m:n:p$, находят общее число частей $m+n+p$, затем величину одной части $S:(m+n+p)$ и умножают её на каждое из чисел отношения.</p>
      </div>

      <div class="theory-card">
        <h4>Пропорциональное деление по произведению</h4>
        <p>Если плата (расход) пропорциональна произведению числа потребителей на их мощность, то сначала вычисляют «вклады» каждого участника, а затем делят сумму в полученном отношении.</p>
      </div>

      <div class="theory-card">
        <h4>Растворы, сплавы, смеси</h4>
        <p>Масса чистого вещества сохраняется:</p>
        <p>$m_1w_1+m_2w_2=(m_1+m_2)w$.</p>
        <p>Для двух сплавов с долями компонента $\dfrac{a}{a+b}$ и $\dfrac{c}{c+d}$, взятых массами $x$ и $S-x$, получаем уравнение относительно доли в новом сплаве.</p>
      </div>

    </div>
  </details>

  <!-- ======================= ТРЕНАЖЁР ======================= -->
  <div id="trainer"></div>

</main>

<footer>
  <div>Тренажёр по теме «Задачи на пропорциональное деление» · 6 типов заданий · 60 заданий</div>
  <div class="author">Разработчик: Котова Наталья Юрьевна, преподаватель математики</div>
</footer>

<script>
/* =========================================================================
   ДАННЫЕ ТРЕНАЖЁРА
   ========================================================================= */
const TYPES = [

/* ------------------------------------------------------------------ */
{
  key: 'direct',
  title: 'Тип I. Прямая пропорциональная зависимость',
  theory: `
    <p>Две величины <b>прямо пропорциональны</b>, если при увеличении (уменьшении) одной из них
    в $k$ раз другая увеличивается (уменьшается) во столько же раз.</p>
    <p><b>Метод решения.</b> Составляем пропорцию $\dfrac{a_1}{a_2}=\dfrac{b_1}{b_2}$,
    где $a_1,a_2$ — значения первой величины, $b_1,b_2$ — соответствующие значения второй.
    Неизвестный член находим по основному свойству пропорции: $ad=bc$.</p>
    <p><b>Важно:</b> величины должны быть однородными и записаны в одних и тех же единицах.</p>
  `,
  tasks: [
    { q: 'Кусок медного провода длиной 5 м имеет массу 400 г. Какова масса куска этого провода длиной 7 м?',
      a: 560, unit: 'г',
      hint: 'Длина и масса прямо пропорциональны. Пропорция: $\\dfrac{5}{7}=\\dfrac{400}{x}$, откуда $x=\\dfrac{7\\cdot 400}{5}$.' },

    { q: '3 кг конфет стоят 450 рублей. Сколько стоят 5 кг таких конфет?',
      a: 750, unit: 'руб.',
      hint: 'Масса и стоимость прямо пропорциональны. Пропорция: $\\dfrac{3}{5}=\\dfrac{450}{x}$.' },

    { q: 'Автомобиль за 4 ч проехал 320 км. Какое расстояние он проедет за 7 ч при той же скорости?',
      a: 560, unit: 'км',
      hint: 'Время и путь при постоянной скорости прямо пропорциональны. Пропорция: $\\dfrac{4}{7}=\\dfrac{320}{x}$.' },

    { q: 'Из 6 кг яблок получают 2,4 л сока. Сколько литров сока получат из 15 кг яблок?',
      a: 6, unit: 'л',
      hint: 'Масса яблок и объём сока прямо пропорциональны. Пропорция: $\\dfrac{6}{15}=\\dfrac{2{,}4}{x}$.' },

    { q: '8 одинаковых деталей имеют массу 12 кг. Какова масса 14 таких деталей?',
      a: 21, unit: 'кг',
      hint: 'Число деталей и их масса прямо пропорциональны. Пропорция: $\\dfrac{8}{14}=\\dfrac{12}{x}$.' },

    { q: 'За 5 одинаковых тетрадей заплатили 350 рублей. Сколько стоят 12 таких тетрадей?',
      a: 840, unit: 'руб.',
      hint: 'Количество тетрадей и стоимость прямо пропорциональны. Пропорция: $\\dfrac{5}{12}=\\dfrac{350}{x}$.' },

    { q: '4 м ткани стоят 1200 рублей. Сколько стоят 9 м этой ткани?',
      a: 2700, unit: 'руб.',
      hint: 'Метраж и стоимость прямо пропорциональны. Пропорция: $\\dfrac{4}{9}=\\dfrac{1200}{x}$.' },

    { q: 'Из 25 кг винограда получают 20 кг изюма. Сколько изюма получат из 40 кг винограда?',
      a: 32, unit: 'кг',
      hint: 'Масса винограда и масса изюма прямо пропорциональны. Пропорция: $\\dfrac{25}{40}=\\dfrac{20}{x}$.' },

    { q: 'Поезд за 3 ч проходит 240 км. За сколько часов он пройдёт 560 км при той же скорости?',
      a: 7, unit: 'ч',
      hint: 'Время и путь прямо пропорциональны. Пропорция: $\\dfrac{3}{x}=\\dfrac{240}{560}$.' },

    { q: '12 одинаковых труб имеют массу 84 кг. Какова масса 20 таких труб?',
      a: 140, unit: 'кг',
      hint: 'Число труб и масса прямо пропорциональны. Пропорция: $\\dfrac{12}{20}=\\dfrac{84}{x}$.' }
  ]
},

/* ------------------------------------------------------------------ */
{
  key: 'inverse',
  title: 'Тип II. Обратная пропорциональная зависимость',
  theory: `
    <p>Две величины <b>обратно пропорциональны</b>, если при увеличении (уменьшении) одной из них
    в $k$ раз другая уменьшается (увеличивается) во столько же раз.</p>
    <p><b>Метод решения.</b> Произведение соответствующих значений величин постоянно,
    поэтому пропорция записывается «перевёрнуто»: $\dfrac{a_1}{a_2}=\dfrac{b_2}{b_1}$.</p>
    <p>Типичные примеры: число рабочих и время работы; число насосов и время наполнения;
    скорость и время движения при постоянном пути.</p>
  `,
  tasks: [
    { q: '16 каменщиков вымостили улицу за 21 день. Сколько каменщиков нужно, чтобы вымостить ту же улицу за 14 дней при той же производительности труда?',
      a: 24, unit: 'каменщиков',
      hint: 'Число рабочих и время обратно пропорциональны: $\\dfrac{16}{x}=\\dfrac{14}{21}$, откуда $x=\\dfrac{16\\cdot 21}{14}$.' },

    { q: '8 насосов откачивают воду из котлована за 6 ч. Сколько насосов потребуется, чтобы откачать эту воду за 4 ч?',
      a: 12, unit: 'насосов',
      hint: 'Число насосов и время обратно пропорциональны: $\\dfrac{8}{x}=\\dfrac{4}{6}$.' },

    { q: '6 маляров покрасят забор за 10 дней. За сколько дней выполнят эту работу 5 маляров?',
      a: 12, unit: 'дней',
      hint: 'Число маляров и время обратно пропорциональны: $\\dfrac{6}{5}=\\dfrac{x}{10}$.' },

    { q: '10 грузовиков перевозят груз за 12 дней. Сколько грузовиков нужно, чтобы перевезти этот груз за 8 дней?',
      a: 15, unit: 'грузовиков',
      hint: 'Число грузовиков и время обратно пропорциональны: $\\dfrac{10}{x}=\\dfrac{8}{12}$.' },

    { q: '15 рабочих выполняют работу за 24 дня. За сколько дней выполнят эту работу 18 рабочих?',
      a: 20, unit: 'дней',
      hint: 'Число рабочих и время обратно пропорциональны: $\\dfrac{15}{18}=\\dfrac{x}{24}$.' },

    { q: 'При скорости 60 км/ч автомобиль проезжает путь за 5 ч. За сколько часов он проедет этот путь при скорости 75 км/ч?',
      a: 4, unit: 'ч',
      hint: 'Скорость и время при постоянном пути обратно пропорциональны: $\\dfrac{60}{75}=\\dfrac{x}{5}$.' },

    { q: '9 тракторов вспашут поле за 8 дней. Сколько тракторов нужно, чтобы вспахать это поле за 6 дней?',
      a: 12, unit: 'тракторов',
      hint: 'Число тракторов и время обратно пропорциональны: $\\dfrac{9}{x}=\\dfrac{6}{8}$.' },

    { q: '5 насосов наполняют бассейн за 18 мин. За сколько минут наполнят бассейн 9 таких насосов?',
      a: 10, unit: 'мин',
      hint: 'Число насосов и время обратно пропорциональны: $\\dfrac{5}{9}=\\dfrac{x}{18}$.' },

    { q: 'Запаса корма 20 коровам хватает на 30 дней. На сколько дней хватит этого запаса 24 коровам?',
      a: 25, unit: 'дней',
      hint: 'Число коров и число дней обратно пропорциональны: $\\dfrac{20}{24}=\\dfrac{x}{30}$.' },

    { q: '24 рабочих выполнят заказ за 15 дней. Сколько рабочих нужно, чтобы выполнить заказ за 10 дней?',
      a: 36, unit: 'рабочих',
      hint: 'Число рабочих и время обратно пропорциональны: $\\dfrac{24}{x}=\\dfrac{10}{15}$.' }
  ]
},

/* ------------------------------------------------------------------ */
{
  key: 'ratio',
  title: 'Тип III. Деление числа в заданном отношении',
  theory: `
    <p><b>Алгоритм.</b> Чтобы разделить число $S$ в отношении $m:n:p$:</p>
    <p>1) находят общее число частей: $m+n+p$;</p>
    <p>2) находят величину одной части: $S:(m+n+p)$;</p>
    <p>3) умножают величину одной части на каждое число отношения.</p>
    <p>Если отношение задано дробными числами, его предварительно упрощают (умножают все члены
    на одно и то же число).</p>
  `,
  tasks: [
    { q: 'На производство костюма израсходовано 2,8 м² ткани. Площади ткани, израсходованной на пиджак, брюки и жилет, относятся как $7:5:2$. Сколько квадратных метров ткани пошло на брюки?',
      a: 1, unit: 'м²',
      hint: 'Всего частей $7+5+2=14$. На одну часть приходится $2{,}8:14=0{,}2$ м². На брюки — $5\\cdot 0{,}2$.' },

    { q: 'Разделите 420 рублей в отношении $3:4$. Найдите меньшую часть.',
      a: 180, unit: 'руб.',
      hint: 'Всего частей $3+4=7$. Одна часть: $420:7=60$. Меньшая часть: $3\\cdot 60$.' },

    { q: 'Отрезок длиной 84 см разделён в отношении $5:7$. Найдите длину большей части.',
      a: 49, unit: 'см',
      hint: 'Всего частей $5+7=12$. Одна часть: $84:12=7$ см. Большая часть: $7\\cdot 7$.' },

    { q: '560 г конфет разделили в отношении $2:3:5$. Найдите массу наибольшей части.',
      a: 280, unit: 'г',
      hint: 'Всего частей $2+3+5=10$. Одна часть: $560:10=56$ г. Наибольшая часть: $5\\cdot 56$.' },

    { q: 'Проволоку длиной 4,5 м разрезали на части в отношении $4:5$. Найдите длину меньшей части.',
      a: 2, unit: 'м',
      hint: 'Всего частей $4+5=9$. Одна часть: $4{,}5:9=0{,}5$ м. Меньшая часть: $4\\cdot 0{,}5$.' },

    { q: 'Число 960 разделили в отношении $3:4:5$. Найдите среднюю часть.',
      a: 320, unit: '',
      hint: 'Всего частей $3+4+5=12$. Одна часть: $960:12=80$. Средняя часть: $4\\cdot 80$.' },

    { q: 'Периметр треугольника равен 48 см, а его стороны относятся как $3:4:5$. Найдите длину наибольшей стороны.',
      a: 20, unit: 'см',
      hint: 'Периметр — это сумма трёх сторон. Всего частей $3+4+5=12$; одна часть $48:12=4$ см.' },

    { q: '1,2 кг сплава разделили на части в отношении $7:5$. Найдите массу большей части.',
      a: 0.7, unit: 'кг',
      hint: 'Всего частей $7+5=12$. Одна часть: $1{,}2:12=0{,}1$ кг. Большая часть: $7\\cdot 0{,}1$.' },

    { q: 'Три магазина получили 6800 рублей прибыли, распределив её в отношении $2:3:5$. Сколько рублей получил третий магазин?',
      a: 3400, unit: 'руб.',
      hint: 'Всего частей $2+3+5=10$. Одна часть: $6800:10=680$ руб. Третий магазин: $5\\cdot 680$.' },

    { q: 'Смесь массой 3,6 кг состоит из двух компонентов, массы которых относятся как $2:7$. Найдите массу большего компонента.',
      a: 2.8, unit: 'кг',
      hint: 'Всего частей $2+7=9$. Одна часть: $3{,}6:9=0{,}4$ кг. Больший компонент: $7\\cdot 0{,}4$.' }
  ]
},

/* ------------------------------------------------------------------ */
{
  key: 'product',
  title: 'Тип IV. Пропорциональное деление по произведению величин',
  theory: `
    <p>Если сумма распределяется пропорционально <b>произведению</b> двух величин
    (например, числа приборов и их мощности), то:</p>
    <p>1) вычисляют «вклад» каждого участника как произведение $n_i\\cdot p_i$;</p>
    <p>2) составляют отношение полученных произведений и упрощают его;</p>
    <p>3) делят общую сумму в этом отношении по правилу пропорционального деления.</p>
    <p>Пример: $(3\\cdot 50):(4\\cdot 25):(2\\cdot 75)=150:100:150=3:2:3$.</p>
  `,
  tasks: [
    { q: 'Три офиса должны уплатить по одному счёту за электроэнергию 4080 рублей. У первого офиса 3 лампы по 50 Вт, у второго — 4 лампы по 25 Вт, у третьего — 2 лампы по 75 Вт. Сколько рублей должен уплатить второй офис?',
      a: 1020, unit: 'руб.',
      hint: '$(3\\cdot50):(4\\cdot25):(2\\cdot75)=150:100:150=3:2:3$; всего $3+2+3=8$ частей; $4080:8=510$ руб. на часть.' },

    { q: 'Три цеха пользуются электроэнергией: 5 станков по 2 кВт, 4 станка по 3 кВт и 3 станка по 5 кВт. Общий счёт равен 7400 рублей. Сколько рублей должен уплатить третий цех?',
      a: 3000, unit: 'руб.',
      hint: '$(5\\cdot2):(4\\cdot3):(3\\cdot5)=10:12:15$; всего $37$ частей; $7400:37=200$ руб. на часть.' },

    { q: 'Три квартиры оплачивают общий счёт 7000 рублей. В первой 4 лампы по 60 Вт, во второй — 3 лампы по 40 Вт, в третьей — 2 лампы по 100 Вт. Сколько рублей платит третья квартира?',
      a: 2500, unit: 'руб.',
      hint: '$(4\\cdot60):(3\\cdot40):(2\\cdot100)=240:120:200=6:3:5$; всего $14$ частей; $7000:14=500$ руб. на часть.' },

    { q: 'Три мастерские потребили электроэнергию: 2 станка по 6 кВт, 3 станка по 8 кВт и 5 станков по 4 кВт. Общая сумма к оплате 5600 рублей. Сколько рублей должна уплатить вторая мастерская?',
      a: 2400, unit: 'руб.',
      hint: '$(2\\cdot6):(3\\cdot8):(5\\cdot4)=12:24:20=3:6:5$; всего $14$ частей; $5600:14=400$ руб. на часть.' },

    { q: 'Три магазина имеют холодильники: 2 по 300 Вт, 5 по 200 Вт и 3 по 100 Вт. Общий счёт 5700 рублей. Сколько рублей должен уплатить третий магазин?',
      a: 900, unit: 'руб.',
      hint: '$(2\\cdot300):(5\\cdot200):(3\\cdot100)=600:1000:300=6:10:3$; всего $19$ частей; $5700:19=300$ руб. на часть.' },

    { q: 'Три склада освещаются: 4 лампы по 100 Вт, 6 ламп по 50 Вт и 5 ламп по 80 Вт. Общая сумма 4400 рублей. Сколько рублей платит второй склад?',
      a: 1200, unit: 'руб.',
      hint: '$(4\\cdot100):(6\\cdot50):(5\\cdot80)=400:300:400=4:3:4$; всего $11$ частей; $4400:11=400$ руб. на часть.' },

    { q: 'Три дома потребляют электроэнергию: 6 ламп по 40 Вт, 5 ламп по 60 Вт и 4 лампы по 100 Вт. Общий счёт 4700 рублей. Сколько рублей платит третий дом?',
      a: 2000, unit: 'руб.',
      hint: '$(6\\cdot40):(5\\cdot60):(4\\cdot100)=240:300:400=12:15:20$; всего $47$ частей; $4700:47=100$ руб. на часть.' },

    { q: 'Три участка используют насосы: 3 насоса по 4 кВт, 2 насоса по 9 кВт и 5 насосов по 2 кВт. Общий счёт 6000 рублей. Сколько рублей платит второй участок?',
      a: 2700, unit: 'руб.',
      hint: '$(3\\cdot4):(2\\cdot9):(5\\cdot2)=12:18:10=6:9:5$; всего $20$ частей; $6000:20=300$ руб. на часть.' },

    { q: 'Три киоска обогреваются: 2 обогревателя по 1,5 кВт, 4 обогревателя по 1 кВт и 3 обогревателя по 0,5 кВт. Общая сумма 5100 рублей. Сколько рублей платит второй киоск?',
      a: 2400, unit: 'руб.',
      hint: '$(2\\cdot1{,}5):(4\\cdot1):(3\\cdot0{,}5)=3:4:1{,}5=6:8:3$; всего $17$ частей; $5100:17=300$ руб. на часть.' },

    { q: 'Три лаборатории используют приборы: 5 приборов по 200 Вт, 3 прибора по 500 Вт и 6 приборов по 100 Вт. Общий счёт 6200 рублей. Сколько рублей платит третья лаборатория?',
      a: 1200, unit: 'руб.',
      hint: '$(5\\cdot200):(3\\cdot500):(6\\cdot100)=1000:1500:600=10:15:6$; всего $31$ часть; $6200:31=200$ руб. на часть.' }
  ]
},

/* ------------------------------------------------------------------ */
{
  key: 'solution',
  title: 'Тип V. Задачи на растворы и концентрацию',
  theory: `
    <p><b>Концентрация</b> — отношение массы чистого вещества к массе всего раствора,
    выраженное в процентах: $w=\\dfrac{m_{\\text{в-ва}}}{m_{\\text{р-ра}}}\\cdot 100\\%$.</p>
    <p><b>Ключевая идея:</b> при разбавлении, смешивании или выпаривании
    <i>масса чистого вещества сохраняется</i> (меняется только масса раствора):</p>
    <p>$m_1w_1+m_2w_2=(m_1+m_2)w$.</p>
    <p>Для задач «сколько добавить» удобно составлять уравнение относительно массы чистого вещества.</p>
  `,
  tasks: [
    { q: 'Восемнадцатипроцентный раствор соли массой 2 кг разбавили стаканом воды (0,25 кг). Какой концентрации (в процентах) раствор был получен?',
      a: 16, unit: '%',
      hint: 'Соли: $2\\cdot0{,}18=0{,}36$ кг. Новая масса раствора: $2+0{,}25=2{,}25$ кг. Концентрация: $0{,}36:2{,}25\\cdot100\\%$.' },

    { q: '20%-й раствор соли массой 3 кг разбавили 2 кг воды. Какова концентрация полученного раствора (в процентах)?',
      a: 12, unit: '%',
      hint: 'Соли: $3\\cdot0{,}2=0{,}6$ кг. Новая масса раствора: $5$ кг.' },

    { q: '25%-й раствор соли массой 2 кг разбавили 3 кг воды. Какова концентрация полученного раствора (в процентах)?',
      a: 10, unit: '%',
      hint: 'Соли: $2\\cdot0{,}25=0{,}5$ кг. Новая масса раствора: $5$ кг.' },

    { q: 'Смешали 3 кг 10%-го раствора соли и 2 кг 15%-го раствора соли. Какова концентрация полученной смеси (в процентах)?',
      a: 12, unit: '%',
      hint: 'Соли: $3\\cdot0{,}1+2\\cdot0{,}15=0{,}6$ кг. Общая масса: $5$ кг.' },

    { q: 'Смешали 4 кг 20%-го раствора соли и 6 кг 10%-го раствора соли. Какова концентрация полученной смеси (в процентах)?',
      a: 14, unit: '%',
      hint: 'Соли: $4\\cdot0{,}2+6\\cdot0{,}1=1{,}4$ кг. Общая масса: $10$ кг.' },

    { q: 'Смешали 5 кг 30%-го раствора соли и 15 кг 10%-го раствора соли. Какова концентрация полученной смеси (в процентах)?',
      a: 15, unit: '%',
      hint: 'Соли: $5\\cdot0{,}3+15\\cdot0{,}1=3$ кг. Общая масса: $20$ кг.' },

    { q: 'К 3 кг 20%-го раствора соли добавили воду и получили 12%-й раствор. Сколько килограммов воды добавили?',
      a: 2, unit: 'кг',
      hint: 'Соли: $3\\cdot0{,}2=0{,}6$ кг — это $12\\%$ новой массы, значит новая масса равна $0{,}6:0{,}12=5$ кг. Добавили $5-3$ кг.' },

    { q: 'Сколько килограммов 10%-го раствора соли нужно смешать с 2 кг 30%-го раствора, чтобы получить 18%-й раствор?',
      a: 3, unit: 'кг',
      hint: 'Пусть $x$ кг — масса 10%-го раствора. Тогда $0{,}1x+2\\cdot0{,}3=0{,}18(x+2)$.' },

    { q: 'Из 10 кг 20%-го раствора соли выпарили 2 кг воды. Какова концентрация оставшегося раствора (в процентах)?',
      a: 25, unit: '%',
      hint: 'Соли: $10\\cdot0{,}2=2$ кг. Масса после выпаривания: $8$ кг.' },

    { q: 'Смешали 2 кг 25%-го раствора соли и 3 кг 5%-го раствора соли. Какова концентрация полученной смеси (в процентах)?',
      a: 13, unit: '%',
      hint: 'Соли: $2\\cdot0{,}25+3\\cdot0{,}05=0{,}65$ кг. Общая масса: $5$ кг.' }
  ]
},

/* ------------------------------------------------------------------ */
{
  key: 'alloy',
  title: 'Тип VI. Задачи на сплавы и смеси (уравнивание отношений)',
  theory: `
    <p>Пусть первый сплав содержит компонент в отношении $a:b$, второй — в отношении $c:d$.
    Обозначим массу первого сплава через $x$, тогда масса второго равна $S-x$
    ($S$ — масса нового сплава).</p>
    <p>Доля компонента в первом сплаве равна $\\dfrac{a}{a+b}$, во втором — $\\dfrac{c}{c+d}$,
    в новом сплаве — $\\dfrac{m}{m+n}$. Получаем уравнение:</p>
    <p>$\\dfrac{a}{a+b}\\,x+\\dfrac{c}{c+d}\\,(S-x)=S\\cdot\\dfrac{m}{m+n}$.</p>
    <p>Решая его, находим массу первого сплава, затем массу второго.</p>
  `,
  tasks: [
    { q: 'Имеется два сплава золота и серебра: в одном количество этих металлов относится как $2:3$, в другом — как $3:7$. Сколько килограммов первого сплава нужно взять, чтобы получить 8 кг нового сплава с отношением золота и серебра $5:11$?',
      a: 1, unit: 'кг',
      hint: 'Доли золота: $\\dfrac{2}{5}$, $\\dfrac{3}{10}$, в новом сплаве $\\dfrac{5}{16}$. Уравнение: $\\dfrac{2}{5}x+\\dfrac{3}{10}(8-x)=2{,}5$.' },

    { q: 'Имеется два сплава меди и цинка: в одном металлы относятся как $1:1$, в другом — как $1:3$. Сколько килограммов первого сплава нужно взять, чтобы получить 10 кг нового сплава с отношением меди и цинка $2:3$?',
      a: 6, unit: 'кг',
      hint: 'Доли меди: $\\dfrac{1}{2}$, $\\dfrac{1}{4}$; в новом сплаве $\\dfrac{2}{5}$. Уравнение: $\\dfrac{1}{2}x+\\dfrac{1}{4}(10-x)=4$.' },

    { q: 'Имеется два сплава золота и серебра: в одном металлы относятся как $3:2$, в другом — как $1:4$. Сколько килограммов первого сплава нужно взять, чтобы получить 20 кг нового сплава с отношением золота и серебра $1:1$?',
      a: 15, unit: 'кг',
      hint: 'Доли золота: $\\dfrac{3}{5}$, $\\dfrac{1}{5}$; в новом сплаве $\\dfrac{1}{2}$. Уравнение: $\\dfrac{3}{5}x+\\dfrac{1}{5}(20-x)=10$.' },

    { q: 'Имеется два сплава олова и свинца: в одном металлы относятся как $2:3$, в другом — как $4:1$. Сколько килограммов первого сплава нужно взять, чтобы получить 10 кг нового сплава с отношением олова и свинца $3:2$?',
      a: 5, unit: 'кг',
      hint: 'Доли олова: $\\dfrac{2}{5}$, $\\dfrac{4}{5}$; в новом сплаве $\\dfrac{3}{5}$. Уравнение: $\\dfrac{2}{5}x+\\dfrac{4}{5}(10-x)=6$.' },

    { q: 'Имеется два сплава золота и серебра: в одном металлы относятся как $1:3$, в другом — как $3:1$. Сколько килограммов первого сплава нужно взять, чтобы получить 12 кг нового сплава с отношением золота и серебра $1:1$?',
      a: 6, unit: 'кг',
      hint: 'Доли золота: $\\dfrac{1}{4}$, $\\dfrac{3}{4}$; в новом сплаве $\\dfrac{1}{2}$. Уравнение: $\\dfrac{1}{4}x+\\dfrac{3}{4}(12-x)=6$.' },

    { q: 'Имеется два сплава меди и цинка: в одном металлы относятся как $2:5$, в другом — как $5:2$. Сколько килограммов первого сплава нужно взять, чтобы получить 14 кг нового сплава с отношением меди и цинка $1:1$?',
      a: 7, unit: 'кг',
      hint: 'Доли меди: $\\dfrac{2}{7}$, $\\dfrac{5}{7}$; в новом сплаве $\\dfrac{1}{2}$. Уравнение: $\\dfrac{2}{7}x+\\dfrac{5}{7}(14-x)=7$.' },

    { q: 'Имеется два сплава золота и серебра: в одном металлы относятся как $1:4$, в другом — как $4:1$. Сколько килограммов первого сплава нужно взять, чтобы получить 15 кг нового сплава с отношением золота и серебра $2:3$?',
      a: 10, unit: 'кг',
      hint: 'Доли золота: $\\dfrac{1}{5}$, $\\dfrac{4}{5}$; в новом сплаве $\\dfrac{2}{5}$. Уравнение: $\\dfrac{1}{5}x+\\dfrac{4}{5}(15-x)=6$.' },

    { q: 'Имеется два сплава олова и свинца: в одном металлы относятся как $3:7$, в другом — как $7:3$. Сколько килограммов первого сплава нужно взять, чтобы получить 20 кг нового сплава с отношением олова и свинца $2:3$?',
      a: 15, unit: 'кг',
      hint: 'Доли олова: $\\dfrac{3}{10}$, $\\dfrac{7}{10}$; в новом сплаве $\\dfrac{2}{5}$. Уравнение: $\\dfrac{3}{10}x+\\dfrac{7}{10}(20-x)=8$.' },

    { q: 'Имеется два сплава меди и цинка: в одном металлы относятся как $1:2$, в другом — как $2:1$. Сколько килограммов первого сплава нужно взять, чтобы получить 24 кг нового сплава с отношением меди и цинка $3:5$?',
      a: 21, unit: 'кг',
      hint: 'Доли меди: $\\dfrac{1}{3}$, $\\dfrac{2}{3}$; в новом сплаве $\\dfrac{3}{8}$. Уравнение: $\\dfrac{1}{3}x+\\dfrac{2}{3}(24-x)=9$.' },

    { q: 'Имеется два сплава золота и серебра: в одном металлы относятся как $5:3$, в другом — как $1:7$. Сколько килограммов первого сплава нужно взять, чтобы получить 16 кг нового сплава с отношением золота и серебра $1:1$?',
      a: 12, unit: 'кг',
      hint: 'Доли золота: $\\dfrac{5}{8}$, $\\dfrac{1}{8}$; в новом сплаве $\\dfrac{1}{2}$. Уравнение: $\\dfrac{5}{8}x+\\dfrac{1}{8}(16-x)=8$.' }
  ]
}

];

/* =========================================================================
   СОСТОЯНИЕ И ЛОГИКА
   ========================================================================= */
const state = {};
const taskMap = {};
let TOTAL = 0;
let mjTries = 0;

/* ---------- Рендер тренажёра ---------- */
function buildTrainer(){
  const root = document.getElementById('trainer');
  root.innerHTML = '';

  TYPES.forEach(type => {
    const sec = document.createElement('section');
    sec.className = 'type-section';
    sec.id = 'type-' + type.key;

    const h2 = document.createElement('h2');
    h2.textContent = type.title;
    sec.appendChild(h2);

    const det = document.createElement('details');
    det.className = 'theory';
    det.innerHTML =
      '<summary>Теоретическая справка и метод решения</summary>' +
      '<div class="theory-body">' + type.theory + '</div>';
    sec.appendChild(det);

    const wrap = document.createElement('div');
    wrap.className = 'tasks';

    type.tasks.forEach((t, qi) => {
      TOTAL++;
      const id = type.key + '-' + qi;
      state[id] = { solved: false, attempts: 0 };
      taskMap[id] = t;

      const card = document.createElement('article');
      card.className = 'task';
      card.dataset.id = id;

      card.innerHTML =
        '<div class="task-top">' +
          '<span class="num">' + (qi + 1) + '</span>' +
          '<span class="status" data-status>не решено</span>' +
        '</div>' +
        '<div class="task-text">' + t.q + '</div>' +
        '<div class="controls">' +
          '<input type="text" inputmode="decimal" autocomplete="off" placeholder="Ваш ответ" data-input>' +
          (t.unit ? '<span class="unit">' + t.unit + '</span>' : '') +
          '<button data-check>Проверить</button>' +
          '<button class="ghost" data-hint>Подсказка</button>' +
        '</div>' +
        '<div class="feedback" data-feedback></div>' +
        '<div class="hint" data-hintbox hidden>' + t.hint + '</div>';

      wrap.appendChild(card);
    });

    sec.appendChild(wrap);
    root.appendChild(sec);
  });

  document.getElementById('total').textContent = TOTAL;
  updateScore();
  typeset(root);
}

/* ---------- Отрисовка формул ---------- */
function typeset(el){
  if (window.MathJax && MathJax.typesetPromise) {
    MathJax.typesetPromise([el]).catch(() => {});
    return;
  }
  if (mjTries++ < 40) {
    setTimeout(() => typeset(el), 250);
  }
}

/* ---------- Проверка ответа ---------- */
function parseAnswer(str){
  const cleaned = String(str).trim()
    .replace(/\s+/g, '')
    .replace(/,/g, '.')
    .replace(/[^0-9.\-]/g, '');
  if (cleaned === '' || cleaned === '-' || cleaned === '.') return NaN;
  return parseFloat(cleaned);
}

function checkAnswer(card, id){
  const task = taskMap[id];
  const st = state[id];
  const input = card.querySelector('[data-input]');
  const fb = card.querySelector('[data-feedback]');
  const status = card.querySelector('[data-status]');

  if (st.solved) return;

  const val = parseAnswer(input.value);

  if (isNaN(val)) {
    fb.textContent = 'Введите числовой ответ.';
    fb.className = 'feedback warn';
    return;
  }

  st.attempts++;

  const eps = 1e-6 * Math.max(1, Math.abs(task.a));
  if (Math.abs(val - task.a) < eps) {
    st.solved = true;
    card.classList.add('solved');
    fb.textContent = 'Верно! Задание зачтено (+1 балл).';
    fb.className = 'feedback ok';
    status.textContent = 'решено';
    status.className = 'status ok';
    input.disabled = true;
  } else {
    fb.textContent = 'Неверно. Проверьте пропорцию или вычисления и попробуйте снова.';
    fb.className = 'feedback err';
    status.textContent = 'ошибка';
    status.className = 'status err';
  }

  updateScore();
}

/* ---------- Подсказка ---------- */
function showHint(card, id){
  const box = card.querySelector('[data-hintbox]');
  const st = state[id];
  const task = taskMap[id];

  if (!box.hidden) {
    box.hidden = true;
    return;
  }

  box.hidden = false;

  if (st.attempts >= 3 && !box.querySelector('.ans-line')) {
    const line = document.createElement('span');
    line.className = 'ans-line';
    line.textContent = 'Правильный ответ: ' + String(task.a).replace('.', ',') +
                       (task.unit ? ' ' + task.unit : '');
    box.appendChild(line);
  }

  typeset(box);
}

/* ---------- Подсчёт баллов ---------- */
function updateScore(){
  let solved = 0;
  Object.keys(state).forEach(id => { if (state[id].solved) solved++; });

  document.getElementById('score').textContent = solved;
  const percent = TOTAL ? Math.round(solved / TOTAL * 100) : 0;
  document.getElementById('percent').textContent = percent + '%';
  document.getElementById('progressFill').style.width = percent + '%';
}

/* ---------- Сброс ---------- */
function resetAll(){
  Object.keys(state).forEach(id => {
    state[id] = { solved: false, attempts: 0 };
  });

  document.querySelectorAll('.task').forEach(card => {
    card.classList.remove('solved');
    const input = card.querySelector('[data-input]');
    input.value = '';
    input.disabled = false;
    const fb = card.querySelector('[data-feedback]');
    fb.textContent = '';
    fb.className = 'feedback';
    const status = card.querySelector('[data-status]');
    status.textContent = 'не решено';
    status.className = 'status';
    const box = card.querySelector('[data-hintbox]');
    box.hidden = true;
    const line = box.querySelector('.ans-line');
    if (line) line.remove();
  });

  updateScore();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

/* ---------- Инициализация ---------- */
document.addEventListener('DOMContentLoaded', () => {
  buildTrainer();

  const root = document.getElementById('trainer');

  root.addEventListener('click', e => {
    const card = e.target.closest('.task');
    if (!card) return;
    const id = card.dataset.id;

    if (e.target.matches('[data-check]')) checkAnswer(card, id);
    if (e.target.matches('[data-hint]'))  showHint(card, id);
  });

  root.addEventListener('keydown', e => {
    if (e.key === 'Enter' && e.target.matches('[data-input]')) {
      const card = e.target.closest('.task');
      checkAnswer(card, card.dataset.id);
    }
  });

  document.getElementById('resetBtn').addEventListener('click', resetAll);

  if (window.MathJax && MathJax.startup && MathJax.startup.promise) {
    MathJax.startup.promise.then(() => typeset(document.body));
  }
});
</script>
</body>
</html>
