class: middle, center, title-slide
# Нейронні мережі

Лекція 7: Комп'ютерний зір ІІ

<br><br>
Кочура Юрій Петрович<br>
[iuriy.kochura@gmail.com](mailto:iuriy.kochura@gmail.com) <br>
<a href="https://t.me/y_kochura">@y_kochura</a> <br>

---

class:  black-slide,
background-image: url(./figures/lec1/nn.jpg)
background-size: cover

<br>
# Сьогодні
.larger-x[<p class="shadow" style="line-height: 250%;">Як створювати нейронні мережі для деяких просунутих задач комп'ютерного зору: <br>

🎙️ Класифікація <br>
🎙️ Детекція об'єктів <br>
🎙️ Сегментація <br>
</p>]

---


class: middle

.width-90.center[![](figures/lec7/tasks.jpg)]

.footnote[Джерело: [Aurélien Géron](https://www.oreilly.com/content/introducing-capsule-networks/), 2018.]

???

Для кожної з цих задач потрібна своя архітектура нейронної мережі.

---

class: blue-slide, middle, center
count: false

.larger-xx[Класифікація]

Поради з використання ConvNet для класифікації зображень

---


class: middle

# Згорткові нейронні мережі

- Згорткові нейронні мережі поєднують згорткові шари, шари агрегації (pooling) та повнозв’язні шари.
- Забезпечують передові результати для .bold[просторово структурованих даних], таких як зображення, звук або текст. 

.center.width-110[![](figures/lec5/lenet.svg)]

.footnote[Джерело: [Dive Into Deep Learning](https://d2l.ai/), 2020.]

???
Просторово структуровані дані — це такі дані, в яких важливе розташування елементів у просторі, тобто є структура або взаємозв’язки між сусідніми елементами.

Приклади:

*Зображення*

- Кожен піксель має своє місце, і значення пікселів поруч часто пов’язані.

- Наприклад, краї об'єктів, форми — це все просторові ознаки.

*Текст*

 - Розташування слів у реченні має значення.

 - Хоч текст не є фізичним простором, але порядок — це структурована послідовність.

---

class: middle

# Класифікація
- Активація у вихідному шарі &mdash; це .bold[Softmax]-активація, яка формує вектор ймовірнісних оцінок $$\mathbf{\hat y} = [\hat y\_1, \hat y\_2, \ldots, \hat y\_C ] \in \bigtriangleup^C$$
$$\sum_{i = 1}^C \hat y_i = 1$$
$$\hat y\_i = P(Y=i|\mathbf{x}) = \text{softmax}(z\_i) = \frac{\exp^{z\_i}}{\sum^C\_j \exp^{z\_j}},$$ де $C$ &mdash; кількість класів;
- Функція втрат &mdash; перехресна ентропія: $\mathcal{L} = -\sum_{i=1}^{C} y_i \log(\hat{y}_i)$

---

class: middle

# Три способи покращити дані

.bold[Нестача даних] &mdash; найбільше обмеження для ефективності моделей глибокого навчання.
1. Збір та анотація додаткових даних зазвичай є дорогим і трудомістким завданням.

.center.width-50[![](figures/lec7/google-street-view-car.jpg)]

---

class: middle
count: false

# Три способи покращити дані

.bold[Нестача даних] &mdash; найбільше обмеження для ефективності моделей глибокого навчання.
1. .inactive[Збір додаткових даних зазвичай є дорогим і трудомістким завданням.]
1. Створення синтетичних даних може бути складним завдання і не відображати реальний розподіл.

.center.width-90[![](figures/lec7/marilyn_ethnicity_hu186fc8fda88363407e2c23753c57080f_560556_1200x1200_fit_lanczos_3.png)]
Мерилін Монро в азійському, оригінальному та африканському стилях

.footnote[Джерело: [Generative models and bias](https://blog.demaupeou.com/post/generative-models-bias/), 2021.]

---

class: middle
count: false

# Три способи покращити дані

.bold[Нестача даних] &mdash; найбільше обмеження для ефективності моделей глибокого навчання.
1. .inactive[Збір додаткових даних зазвичай є дорогим і трудомістким завданням.]
1. .inactive[Створення синтетичних даних може бути складним завдання і не відображати реальний розподіл.]
1. .bold[Доповнення] даних за допомогою базових перетворень &mdash; простий та ефективний підхід, але пошук хорошої стратегії для доповнення даних може вимагати численних експериментів. 

.center.width-40[![](figures/lec7/augment.png)]

.footnote[Джерело: [DeepAugment](https://github.com/barisozmen/deepaugment), 2020.]

---


class: middle
background-size: contain

background-image: url(./figures/lec7/augmentation-ua.png)

.footnote[Джерело: [DeepAugment](https://github.com/barisozmen/deepaugment), 2020.]

---

class: middle

.center.width-100[![](figures/lec7/deepaugment-ua.png)]

$$\text{Error Reduction} = \frac{E\_\text{old} - E\_\text{new}}{E\_\text{old}}\cdot 100\% = \frac{0.143 - 0.058}{0.143}\cdot 100\% = 59.4\% $$

.footnote[Джерело: [DeepAugment](https://github.com/barisozmen/deepaugment), 2020.]

---


class: middle

# Попередньо навчені моделі

- Навчання моделі «з нуля» може тривати .bold[дні] або навіть .bold[тижні].
- Багато моделей, .bold[навчених на великих наборах даних], доступні у відкритому доступі. Їх можна використовувати як: **екстрактори ознак** або як основу для розумної *ініціалізації* при донавчанні.
- Такі моделі слід розглядати як .bold[універсальні], .bold[багаторазові] активи.

???

Слід підкреслити, що це вже стало стандартною практикою в глибокому навчанні. Лише незначна кількість фахівців усе ще навчає моделі з нуля.
З поширенням foundation models таких випадків стає ще менше.

---

class: middle

# Передавальне навчання

- Беремо попередньо навчену мережу, видаляємо останній (або кілька останніх) шарів і використовуємо решту як .bold[фіксований] екстрактор ознак.
- На основі цих ознак навчаємо .bold[нову модель] для .bold[нової задачі].
- Часто показує кращі результати, ніж ручне створення ознак або навчання моделі лише на нових даних.

<br>
.center.width-100[![](figures/lec7/feature-extractor.png)]

.footnote[Джерело: Mormont et al, [Comparison of deep transfer learning strategies for digital pathology](http://hdl.handle.net/2268/222511), 2018.]

---

class: middle

.center.width-65[![](figures/lec7/finetune.svg)]

## Тонке налаштування

Ми не лише використовуємо попередньо навчену модель для нової задачі, але й дозволяємо моделі адаптуватися до нових даних, змінюючи її ваги під час навчання.

.footnote[Джерело: [Dive Into Deep Learning](https://d2l.ai/), 2020.]

???
Тонке налаштування — це популярний підхід у глибокому навчанні, особливо коли ми маємо обмежену кількість даних для нової задачі, але можемо використати знання, отримані на інших великих наборах даних.

---

class: middle

Моделі, які попередньо навчені на **ImageNet**: передані/тонко налаштовані мережі зазвичай працюють краще навіть тоді, коли вхідні зображення належать до іншої доменної області (наприклад, біомедичні зображення, супутникові зображення або картини).

.center.width-75[![](figures/lec7/fine-tuning-results.png)]

.footnote[Джерело: Matthia Sabatelli et al, [Deep Transfer Learning for Art Classification Problems](http://openaccess.thecvf.com/content_ECCVW_2018/papers/11130/Sabatelli_Deep_Transfer_Learning_for_Art_Classification_Problems_ECCVW_2018_paper.pdf), 2018.]

---

class: blue-slide, middle, center
count: false

.larger-xx[Детекція об'єктів]


---

class: middle

Найпростіша стратегія для переходу від класифікації зображень до детекції об'єктів &mdash; це класифікація локальних ділянок на різних масштабах і в різних місцях.

.center.width-80[![](figures/lec7/sliding.gif)]

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---

exclude: true
class: middle

## Intersection over Union (IoU)

Стандартним показником ефективності для виявлення об'єктів є (Intersection over Union, IoU) між передбаченою прямокутною рамкою $\hat{B}$ та анотованою прямокутною рамкою $B$.
$$\text{IoU}(B,\hat{B}) = \frac{\text{area}(B \cap \hat{B})}{\text{area}(B \cup \hat{B})}.$$

.center.width-45[![](figures/lec7/iou.png)]

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---


class: middle 

Підхід зі слайдінгом вікна оцінює класифікатор на великій кількості позицій та масштабів.

Цей підхід зазвичай є .bold[обчислювально витратним], оскільки ефективність безпосередньо залежить від роздільної здатності та кількості вікон, поданих до класифікатора (чим більше вікон, тим краще, але й тим дорожче).

---

# OverFeat 

.grid[
.kol-2-3[

Складність підходу зі слайдінгом вікна була знижена в піонерській мережі OverFeat (Sermanet et al, 2013) шляхом додавання блоку регресії для передбачення обмежувальної рамки об'єкта $(x, y, w, h)$.

]
.kol-1-3[.center.width-100[![](figures/lec7/overfeat.png)]]
]

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

???
У цій роботі автори запропонували метод OverFeat, який поєднує в собі задачі розпізнавання, локалізації та виявлення об'єктів за допомогою конволюційних нейронних мереж (CNN). Ключовою ідеєю було додавання  блоку регресії до мережі для передбачення координат прямокутної рамки об'єкта (замість традиційного методу слайдінга вікна), що значно покращило ефективність та швидкість виявлення об'єктів на зображеннях.

Це була одна з перших робіт, яка демонструвала використання CNN для локалізації та детекції об'єктів в реальних умовах.

---

class: middle

.center[.width-60[![](figures/lec7/overfeat-grid.png)] 
.width-60[![](figures/lec7/overfeat-predictions.png)]]

Для кожної позиції та масштабу:
- блок класифікатора виводить клас та ймовірність (верхній рис.);
- блок регресії передбачає місцезнаходження об'єкта (нижній рис.).

.footnote[Джерело: Sermanet et al, 2013.]

---

class: middle

.center.width-60[![](figures/lec7/overfeat-merge.png)]

Ці прямокутні рамки в кінцевому підсумку об'єднуються за допомогою .bold[Non-Maximum Suppression], щоб отримати остаточні передбачення для меншої кількості об'єктів.

.footnote[Джерело: Sermanet et al, 2013.]

???

Non-Maximum Suppression (NMS) — це метод, який допомагає усунути зайві та дублюючі прямокутні рамки, залишаючи лише найкращі (з найбільшими ймовірностями) для кожного виявленого об'єкта.

NMS:
1. Почати з усіх прямокутних рамок виявлення, кожна з яких має бал впевненості.
2. Відсортувати всі рамки за балом впевненості (від найвищого до найнижчого).
3. Вибрати рамку з найвищим балом впевненості та додати її до фінального списку виявлених об'єктів.
4. Обчислити IoU між цією рамкою та всіма іншими залишковими рамками.
5. Видалити рамки з IoU менше за попередньо визначений поріг (зазвичай 0.5-0.7).
6. Повторювати кроки 3-5, поки не залишиться одна рамка.

IoU = Area of Intersection / Area of Union
(range from 0 (no overlap) to 1 (perfect overlap))

---

class: middle

Архітектура OverFeat має кілька .bold[недоліків]:
- це роз'єднана система (2 окремі блоки з відповідними втратами, спеціальний процес об'єднання);
- орієнтована на точне визначення місця (локалізації) об'єкта на зображенні, а не на виявленні самого об'єкта;
- не враховує загальний контекст зображення при прийнятті рішень, через це потрібно виконувати значне постоброблення для того, щоб зробити детекцію об'єктів більш узгодженою та точною.

???

Архітектура OverFeat спочатку фокусується на локалізації об'єкта: вона визначає координати прямокутної рамки, де, ймовірно, знаходиться об'єкт. Однак вона не оптимізується на комплексному виявленні об'єкта (включаючи правильну класифікацію та точне об'єднання результатів). Це означає, що в OverFeat більше уваги приділяється визначенню точного місця розташування об'єкта, ніж безпосередньо його виявленню та класифікації.


---

# YOLO (You Only Look Once)

.center.width-65[![](figures/lec7/yolo-model.png)]

YOLO (Redmon et al., 2015) розглядає задачу детекції об'єктів як задачу регресії.

Зображення розбивається на сітку розміру $S \times S$. Для кожної клітинки сітки передбачаються такі значення: $B$ обмежувальні рамки, ймовірність для кожної з цих рамок та $C$ класова ймовірність. Ці прогнози кодуються як тензор розміру $S \times S \times (5B + C)$. 


.footnote[Джерело: Redmon et al, 2015.]

---

class: middle

Для $S=7$, $B=2$, $C=20$,  мережа передбачає вектор розміру $30$ для кожної клітинки. 

.center.width-100[![](figures/lec7/yolo-architecture.png)]

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---

class: middle

Мережа виконує передбачення оцінок класів та регресій обмежувальних рамок, .bold[і незважаючи на те, що вихід формується повнозв’язними шарами, він зберігає 2D структуру].

- На відміну від методів ковзаючого вікна, YOLO аналізує зображення як єдине ціле.
- Модель обробляє усе зображення як на етапі навчання, так і під час тестування, тому неявно враховує контекст про класи та їхній зовнішній вигляд.

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---

class: middle

Під час навчання YOLO припускає, що в кожній клітинці сітки $S \times S$ може бути щонайбільше один об’єкт (а точніше &mdash; центр об’єкта). Для кожного зображення визначаються: індекс клітинки $i = 1, ..., S\times S$, індекс передбаченої рамки $j = 1, ..., B$ та індекс класу $c = 1, ..., C$:

- $\mathbb{1}_i^\text{obj}$ дорівнює $1$, якщо в клітинці $i$ є об’єкт, і $0$ — інакше;
- $\mathbb{1}_{i,j}^\text{obj}$ дорівнює $1$, якщо в клітинці $i$ є об’єкт і передбачена рамка $j$ є тією, що найкраще його описує (має найбільше перекриття), і $- 0$ — інакше;
- $p_{i,c}$ дорівнює $1$, якщо в клітинці $i$ є об’єкт класу $c$, і $0$ — інакше;
- $x_i, y_i, w_i, h_i$ &mdash; координати анотованої обмежувальної рамки навколо об’єкта (визначаються тільки якщо $\mathbb{1}_i^\text{obj}=1$; координати та розміри - задані відносно меж клітинки);
- $c_{i,j}$ &mdash; це IoU між передбаченою рамкою та реальним об’єктом.

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---

class: middle

Навчання починається з обчислення $\mathbb{1}\_{i,j}^\text{obj}$ та $c\_{i,j}$ для зображення, після чого виконується один крок мінімізації функції втрат:
.smaller-x[
$$
\begin{aligned}
& \lambda\_\text{coord} \sum\_{i=1}^{S \times S} \sum\_{j=1}^B \mathbb{1}\_{i,j}^\text{obj} \left( (x\_i - \hat{x}\_{i,j})^2 + (y\_i - \hat{y}\_{i,j})^2 + (\sqrt{w\_i} - \sqrt{\hat{w}\_{i,j}})^2 + (\sqrt{h\_i} - \sqrt{\hat{h}\_{i,j}})^2\right)\\\\
& + \lambda\_\text{obj} \sum\_{i=1}^{S \times S} \sum\_{j=1}^B \mathbb{1}\_{i,j}^\text{obj} (c\_{i,j} - \hat{c}\_{i,j})^2 + \lambda\_\text{noobj} \sum\_{i=1}^{S \times S} \sum\_{j=1}^B (1-\mathbb{1}\_{i,j}^\text{obj}) \hat{c}\_{i,j}^2  \\\\
& + \lambda\_\text{classes} \sum\_{i=1}^{S \times S} \mathbb{1}\_i^\text{obj} \sum\_{c=1}^C (p\_{i,c} - \hat{p}\_{i,c})^2 
\end{aligned}
$$
]

де $\hat{p}\_{i,c}$, $\hat{x}\_{i,j}$, $\hat{y}\_{i,j}$, $\hat{w}\_{i,j}$, $\hat{h}\_{i,j}$ та $\hat{c}\_{i,j}$ &mdash; це значення, які генерує нейромережа як свій вихід.

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---

class: middle 

Навчання YOLO залежить від .bold[багатьох інженерних рішень], що добре ілюструє складність практичного застосування глибокого навчання:
- попереднє навчання перших 20 згорткових шарів на датасеті ImageNet для задачі класифікації;
- використання вхідних зображень розміром $448 \times 448$ для детекції замість $224 \times 224$;
- застосування Leaky ReLU як функцій активації для всіх шарів;
- використання dropout після першого згорткового шару;
- нормалізація параметрів обмежувальних рамок до діапазону $[0,1]$;
- квадратична функція втрат не лише для координат прямокутників, але й для довіри (confidence) та класових оцінок;
- зменшення ваги великих обмежувальних рамок шляхом використання квадратного кореня з їх розміру у функції втрат;
- зменшення впливу порожніх комірок (які не містять об'єктів) шляхом меншого вагового коефіцієнта у функції втрат для довіри;
- аугментація даних за допомогою масштабування, трансляції та перетворень у просторі HSV.

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---

class: middle, center, black-slide

<iframe width="600" height="450" src="https://www.youtube.com/embed/YmbhRxQkLMg" frameborder="0" allowfullscreen></iframe>

YOLO (Redmon, 2017).

---

exclude: true
class: middle

## SSD

The Single Shot Multi-box Detector (SSD; Liu et al, 2015) improves upon YOLO by using a fully-convolutional architecture and multi-scale maps.

.center.width-80[![](figures/lec7/ssd.png)]

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---

# Region-based CNNs (R-CNNs)

Альтернатива великій кількості заздалегідь визначених боксів &mdash; використання *пропозицій регіонів*, які спочатку виділяються на зображенні.

Головні архітектури цього підходу &mdash; R-CNNs:
- (Slow) R-CNN (Girshick et al, 2014)
- Fast R-CNN ([Girshick et al, 2015](https://openaccess.thecvf.com/content_iccv_2015/papers/Girshick_Fast_R-CNN_ICCV_2015_paper.pdf))
- Faster R-CNN ([Ren et al, 2015](https://arxiv.org/pdf/1506.01497))
- Mask R-CNN ([He et al, 2017](https://arxiv.org/pdf/1703.06870))


.center.width-80[![](figures/lec7/new_splash-method_NaA95zW.png)]

.footnote[Джерело: [Rich feature hierarchies for accurate object detection and semantic segmentation
Tech report (v5)](https://arxiv.org/pdf/1311.2524v5), 2014.]

???
R-CNN — архітектура нейронної мережі для детекції об’єктів, яка поєднує:

 - виділення регіонів-кандидатів (region proposals) за допомогою selective search;

 - витяг ознак з кожного регіону окремо за допомогою глибокої CNN;

 - класифікацію кожного регіону незалежно.

  ✅ Локалізує та сегментує об’єкти
  ❌ Повільна: CNN запускається для кожного регіону окремо

---

class: middle

## R-CNN

Архітектура R-CNN складається з 4 частин:
1. Селективний пошук застосовується до вхідного зображення для генерації якісних регіонів-кандидатів.
2. Попередньо навчена згорткова нейронна мережа (опорна мережа, backbone) вставляється перед вихідним шаром. Вона масштабує кожен запропонований регіон до потрібного розміру та виконує прямий прохід, щоб отримати вектор ознак для кожного регіону.
3. Отримані ознаки подаються на SVM-класифікатор для визначення класу об’єкта.
4. Ті ж ознаки використовуються в лінійній регресії для прогнозування координат обмежувальної рамки (bounding box).

.center.width-90[![](figures/lec7/r-cnn.svg)]

.footnote[Джерело: [Dive Into Deep Learning](https://d2l.ai/), 2020.]

???
Під "якісними" маються на увазі регіони з високим перекриттям (IoU) з реальними об’єктами.

---

class: middle

.center.width-90[![](figures/lec7/selective-search.png)]

Селективний пошук (Uijlings et al, 2013) групує сусідні пікселі зі схожою текстурою, кольором або яскравістю,
аналізуючи вікна різних розмірів на зображенні.

---

class: middle

.grid[
.kol-3-5[
<br><br>

## Fast R-CNN

- R-CNN повільна, оскільки ознаки витягуються окремо для кожного регіону-кандидата.
- Fast R-CNN використовує все зображення як вхід для CNN і витягує спільну карту ознак, замість обробки кожного регіону окремо.
- Fast R-CNN вводить RoI pooling, який дозволяє отримувати вектори ознак фіксованого розміру для регіонів-кандидатів різного розміру.

]
.kol-2-5[.width-100[![](figures/lec7/fast-rcnn.svg)]

.width-100[![](figures/lec7/fast-rcnn.png)]]
]

.footnote[Джерело: [Dive Into Deep Learning](https://d2l.ai/), 2020.]

---

class: middle

.center.width-70[![](figures/lec7/faster-rcnn.svg)]

## Faster R-CNN

- Продуктивність як R-CNN, так і Fast R-CNN залежить від якості регіонів-кандидатів, отриманих за допомогою селективного пошуку.
- Faster R-CNN замінює селективний пошук на мережу пропозицій регіонів (Region Proposal Network, RPN).
- Ця мережа зменшує кількість запропонованих регіонів, зберігаючи високу точність виявлення об’єктів.і

.footnote[Джерело: [Dive Into Deep Learning](https://d2l.ai/), 2020.]

---

class: middle, center, black-slide

<iframe width="600" height="450" src="https://www.youtube.com/embed/V4P_ptn2FF4" frameborder="0" allowfullscreen></iframe>

YOLO (v2) vs YOLO 9000 vs SSD vs Faster RCNN

---

class: middle

# Основні висновки

- Одноетапні детектори (YOLO, SSD, RetinaNet тощо) &mdash; швидкі для інференсу, але зазвичай не найточніші для виявлення об'єктів.
- Двоетапні детектори (Fast R-CNN, Faster R-CNN, R-FCN, Light head R-CNN, тощо) &mdash; зазвичай повільніші, але часто є точнішими.
- Вибір мережі залежать від багатьох інженерних рішень.

---

class: middle, center

.larger-xxx[🧿[Демо](https://colab.research.google.com/gist/kirisakow/325a557d89262e8d6a4f2918917e82b4/real-time-object-detection-in-webcam-video-stream-using-ultralytics-yolov8.ipynb)]

???

Use Lucie's kitchen set.
- Far vs. near detections
- Individual vs. packed detections
- Rotation, flip, etc

---

class: blue-slide, middle, center
count: false

.larger-xx[Сегментація]

---

class: middle

.center.width-90[![](figures/lec7/segmentation.png)]

Segmentation is the task of partitioning an image, at the pixel level, into regions:
- .bold[Semantic segmentation]: All pixels in an image are labeled with their class (e.g., car, pedestrian, road).
- .bold[Instance segmentation]: Pixels of detected objects are labeled with an instance ID (e.g., car 1, car 2, pedestrian 1).
- Panoptic segmentation: Combines semantic and instance segmentation. All pixels in an image are labeled with a class and an instance ID (if applicable).

.footnote[Джерело: [Dive Into Deep Learning](https://d2l.ai/), 2020.]

---

class: middle

The deep learning approach casts semantic segmentation as pixel classification. Convolutional networks can be used for that purpose, but with a few adaptations.

---

class: middle

.center.width-100[![](figures/lec7/fcn-1.png)]

.footnote[Джерело: [CS231n, Lecture 11](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture11.pdf), 2018.]

---

class: middle

.center.width-100[![](figures/lec7/fcn-2.png)]

.footnote[Джерело: [CS231n, Lecture 11](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture11.pdf), 2018.]

???

Convolution and pooling layers reduce the input width and height, or keep them unchanged.

Semantic segmentation requires to predict values for each pixel, and therefore needs to increase input width and height.

Fully connected layers could be used for that purpose but would face the same limitations as before (spatial specialization, too many parameters).

Ideally, we would like layers that implement the inverse of convolutional
 and pooling layers.

---

class: middle

## Transposed convolution

A **transposed convolution** is a convolution where the implementation of the forward and backward passes
are swapped.

Given a convolutional kernel $\mathbf{u}$,
- the forward pass is implemented as $v(\mathbf{h}) = \mathbf{U}^T v(\mathbf{x})$ with appropriate reshaping, thereby effectively up-sampling an input $v(\mathbf{x})$ into a larger one;
- the backward pass is computed by multiplying the loss by $\mathbf{U}$ instead of $\mathbf{U}^T$.

???

In a regular convolution,
- the forward pass is equivalent to $v(\mathbf{h}) = \mathbf{U} v(\mathbf{x})$;
- the backward pass is computed by multiplying the loss by $\mathbf{U}^T$.

Transposed convolutions are also referred to as fractionally-strided convolutions or deconvolutions (mistakenly).

---

class: middle

.pull-right[<br><br>![](figures/lec7/no_padding_no_strides_transposed.gif)]

$$
\begin{aligned}
\mathbf{U}^T v(\mathbf{x}) &= v(\mathbf{h}) \\\\
\begin{pmatrix}
1 & 0 & 0 & 0 \\\\
4 & 1 & 0 & 0 \\\\
1 & 4 & 0 & 0 \\\\
0 & 1 & 0 & 0 \\\\
1 & 0 & 1 & 0 \\\\
4 & 1 & 4 & 1 \\\\
3 & 4 & 1 & 4 \\\\
0 & 3 & 0 & 1 \\\\
3 & 0 & 1 & 0 \\\\
3 & 3 & 4 & 1 \\\\
1 & 3 & 3 & 4 \\\\
0 & 1 & 0 & 3 \\\\
0 & 0 & 3 & 0 \\\\
0 & 0 & 3 & 3 \\\\
0 & 0 & 1 & 3 \\\\
0 & 0 & 0 & 1
\end{pmatrix}
\begin{pmatrix}
2 \\\\
1 \\\\
4 \\\\
4
\end{pmatrix} &=
\begin{pmatrix}
2 \\\\
9 \\\\
6 \\\\
1 \\\\
6 \\\\
29 \\\\
30 \\\\
7 \\\\
10 \\\\
29 \\\\
33 \\\\
13 \\\\
12 \\\\
24 \\\\
16 \\\\
4
\end{pmatrix}
\end{aligned}$$

.footnote[Джерело: Dumoulin and Visin, [A guide to convolution arithmetic for deep learning](https://arxiv.org/abs/1603.07285), 2016.]

---

class: middle

## Fully convolutional networks (FCNs)

.grid[
.kol-3-4[

A fully convolutional network (FCN) is a convolutional network that replaces the fully connected layers with convolutional layers and transposed convolutional layers. 

For semantic segmentation, the simplest design of a fully convolutional network consists in:
- using a (pre-trained) convolutional network for downsampling and extracting image features;
- replacing the dense layers with a  $1 \times 1$ convolution layer to  transform the number of channels into the number of categories;
- upsampling the feature map to the size of the input image by using one (or several) transposed convolution layer(s).
]
.kol-1-4[.center.width-90[![](figures/lec7/fcn.svg)]]
]

---

class: middle

Contrary to fully connected networks, the dimensions of the output of a fully convolutional network is not fixed. It directly depends on the dimensions of the input, which can be images of arbitrary sizes.

---

exclude: true
class: middle

.center.width-100[![](figures/lec7/fcn-convdeconv.png)]

.footnote[Джерело: [Noh et al](https://arxiv.org/abs/1505.04366), 2015.]

---

class: middle

.center.width-100[![](figures/lec7/ConvSemSeg.svg)]

The previous .bold[encoder-decoder architecture] is a simple and effective way to perform semantic segmentation. 

However, the low-resolution representation in the middle of the network can be a bottleneck for the segmentation performance, as it must retain enough information to reconstruct the high-resolution segmentation map.

.footnote[Джерело: Simon J.D. Prince, [Understanding Deep Learning](https://udlbook.github.io/udlbook/), 2023.]

---

class: middle

## UNet

The .bold[UNet] architecture is an encoder-decoder architecture with skip connections (usually concatenations) that directly connect the encoder and decoder layers at the same resolution. In this way, the decoder can use both
- the corresponding high-resolution features from the encoder, and
- the lower-resolution features from the previous layers.

.center.width-80[![](figures/lec7/ResidualUNet.svg)]

.footnote[Джерело: Simon J.D. Prince, [Understanding Deep Learning](https://udlbook.github.io/udlbook/), 2023.]

???

Take the time to explain that that same architecture can be used for image to image mappings, as in some of their projects.

Insist once again on the increasing number of kernels (=out_channels) in the encoder and the decreasing number of kernels in the decoder.

Mention the final 1x1 convolution to reduce the number of channels to the number of classes.

---

class: middle

.center.width-100[![](figures/lec7/ResidualUNetResults.svg)]

.center[3d segmentation results using a UNet architecture.<br> (a) Slices of a 3d volume of a mouse cortex, (b) A UNet is used to classify voxels as either inside or outside neutrites. Connected regions are shown with different colors, (c) 5-member ensemble of UNets.]


.footnote[Джерело: Simon J.D. Prince, [Understanding Deep Learning](https://udlbook.github.io/udlbook/), 2023.]

---

class: middle

.center[(demo)]

---

class: middle

## Mask R-CNN

.grid[
.kol-1-2[

Segmentation is a natural extension of object detection. For example, Mask R-CNN extends the Faster R-CNN model for semantic segmentation: 
- The RoI pooling layer is replaced with an RoI alignment layer. 
- It branches off to an FCN for predicting a semantic segmentation mask.
- Object detection combined with mask prediction enables *instance segmentation*.


]
.kol-1-2[.center.width-95[![](figures/lec7/mask-rcnn.svg)]]
]

.footnote[Джерело: [Dive Into Deep Learning](https://d2l.ai/), 2020.]

---

class: middle

.center.width-100[![](figures/lec7/mask-rcnn-results.png)]

.footnote[Джерело: [He et al](https://arxiv.org/abs/1703.06870), 2017.]

---

class: middle, center, black-slide

<iframe width="600" height="450" src="https://www.youtube.com/embed/OOT3UIXZztE" frameborder="0" allowfullscreen></iframe>

---

class: middle

It is noteworthy that for detection and segmentation, there is an heavy
re-use of large networks trained for classification.

.bold[The models themselves, as much as the source code of the algorithm that
produced them, or the training data, are generic and re-usable assets.]

.footnote[Джерело: Francois Fleuret, [EE559 Deep Learning](https://fleuret.org/ee559/), EPFL.]

---

class: end-slide, center
count: false

.larger-xxxx[🏁]

???

Quiz:
- What architecture would you use on images?
- Would you train from scratch?
- What is the difference between object detection and segmentation?
- Name one architecture for object detection.
- Name one architecture for semantic segmentation.
- What kind of layer can you use to upscale a feature map?