<!-- комментарий-->
<!--ссылка на файл <a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/NNBayes.R'>NNBayes.R</a>-->
<!--вставка картинки <img src="img/omega.jpg" alt="вероятность_собятия">-->
<base href="https://github.com/PavlyukovVladimir/SMPR2/blob/master/" ></base>

<a href='Jupyter-notebook-notes/Metricheskiye_algoritmy_klassifikatcii.ipynb'>Metricheskiye_algoritmy_klassifikatcii.ipynb</a>
## Навигация
  <p><a href="#ak"><h1>1. Алгоритмы классификации</h1></a></p>
  <p><a href="#mak"><h2>1.1. Метрические алгоритмы классификации</h2></a></p>
  
  
  
  <p><a href="#Metricheskiye_algoritmy">2. Метрические алгоритмы</a></p>
  <p><a href="#a1NN">2.1. 1NN</a></p>
  <p><a href="#akNN">2.2. kNN</a></p>
  <p><a href="#Loo_akNN">2.3. LOO для kNN</a></p>
  <p><a href="#Bayesophskiye_klassiphikatory">3. Байесовские классификаторы</a></p>
  <p><a href="#Naivnyy_bayesophskiy_klassiphikator">3.1. &#171Наивный&#187 байесовский классификатор</a></p>
  <p><a href="#plug_in">3.2. Классификатор plug-in</a></p>
  <p><a href="#fisher">3.3. Линейный дискриминант Фишера</a></p>
  <p><a href="#Lineynyye_klassiphikatory">4. Линейные алгоритмы классификации</a></p>
  <p><a href="#SG">4.1. Метод стохастического градиента.</a></p>
  <p><a href="#ADALINE">4.1.1 ADALINE.</a></p>
  <p><a href="#Perseptron">4.1.2 Персептрон Розенблатта.</a></p>
  <h1 align="center">Алгоритмы классификации</h1><a name="ak"></a>
  <h2>1.1. Метрические алгоритмы классификации</h2><a name="mak"></a>
<li>Используя готовые методы из библиотеки sklearn, применить алгоритмы классификации, основанные на методе ближайших соседей и методе парзеновского окна (использовать различные ядра) для классификации исходных данных.</li>
<li>Оптимальные параметры выбирать по критерию скользящего контроля.</li>
<li>Оценить качество построенных алгоритмов.</li>
  ## 1. sklearn.neighbors.KNeighborsClassifier(Класс реализующий классификацию методом k ближайших соседей) <a name="knn"></a>
  
  <p>class sklearn.neighbors.KNeighborsClassifier(n_neighbors=5, weights=’uniform’, algorithm=’auto’, leaf_size=30, p=2, metric=’minkowski’, metric_params=None, n_jobs=1, **kwargs)</p>
  <p>class KNeighborsClassifier(NeighborsBase, KNeighborsMixin,
                           SupervisedIntegerMixin, ClassifierMixin):
    """Classifier implementing the k-nearest neighbors vote.
    Read more in the :ref:`User Guide <classification>`.
    Parameters
    ----------
    n_neighbors : int, optional (default = 5)
        Number of neighbors to use by default for :meth:`kneighbors` queries.
    weights : str or callable, optional (default = 'uniform')
        weight function used in prediction.  Possible values:
        - 'uniform' : uniform weights.  All points in each neighborhood
          are weighted equally.
        - 'distance' : weight points by the inverse of their distance.
          in this case, closer neighbors of a query point will have a
          greater influence than neighbors which are further away.
        - [callable] : a user-defined function which accepts an
          array of distances, and returns an array of the same shape
          containing the weights.
    algorithm : {'auto', 'ball_tree', 'kd_tree', 'brute'}, optional
        Algorithm used to compute the nearest neighbors:
        - 'ball_tree' will use :class:`BallTree`
        - 'kd_tree' will use :class:`KDTree`
        - 'brute' will use a brute-force search.
        - 'auto' will attempt to decide the most appropriate algorithm
          based on the values passed to :meth:`fit` method.
        Note: fitting on sparse input will override the setting of
        this parameter, using brute force.
    leaf_size : int, optional (default = 30)
        Leaf size passed to BallTree or KDTree.  This can affect the
        speed of the construction and query, as well as the memory
        required to store the tree.  The optimal value depends on the
        nature of the problem.
    p : integer, optional (default = 2)
        Power parameter for the Minkowski metric. When p = 1, this is
        equivalent to using manhattan_distance (l1), and euclidean_distance
        (l2) for p = 2. For arbitrary p, minkowski_distance (l_p) is used.
    metric : string or callable, default 'minkowski'
        the distance metric to use for the tree.  The default metric is
        minkowski, and with p=2 is equivalent to the standard Euclidean
        metric. See the documentation of the DistanceMetric class for a
        list of available metrics.
    metric_params : dict, optional (default = None)
        Additional keyword arguments for the metric function.
    n_jobs : int, optional (default = 1)
        The number of parallel jobs to run for neighbors search.
        If ``-1``, then the number of jobs is set to the number of CPU cores.
        Doesn't affect :meth:`fit` method.
    Examples
    --------
    >>> X = [[0], [1], [2], [3]]
    >>> y = [0, 0, 1, 1]
    >>> from sklearn.neighbors import KNeighborsClassifier
    >>> neigh = KNeighborsClassifier(n_neighbors=3)
    >>> neigh.fit(X, y) # doctest: +ELLIPSIS
    KNeighborsClassifier(...)
    >>> print(neigh.predict([[1.1]]))
    [0]
    >>> print(neigh.predict_proba([[0.9]]))
    [[ 0.66666667  0.33333333]]
    See also
    --------
    RadiusNeighborsClassifier
    KNeighborsRegressor
    RadiusNeighborsRegressor
    NearestNeighbors
    Notes
    -----
    See :ref:`Nearest Neighbors <neighbors>` in the online documentation
    for a discussion of the choice of ``algorithm`` and ``leaf_size``.
    .. warning::
       Regarding the Nearest Neighbors algorithms, if it is found that two
       neighbors, neighbor `k+1` and `k`, have identical distances
       but different labels, the results will depend on the ordering of the
       training data.
    https://en.wikipedia.org/wiki/K-nearest_neighbor_algorithm
    """</p>
  <p>Задача <i>обучения по прецедентам</i> заключается в том, чтобы по выборке 𝑋<sup>ℓ</sup> <i>восстановить зависимость</i> y*, то есть построить <i>решающую функцию</i> a: X -> Y, которая приближала бы целевую функцию y*(x), причем не только на объектах обучающей выборки, но и на всем множестве X.</p>
  <p>Решающая функция a должна допускать эффективную компьютерную реализацию; по этой причине её называют <i>классифицирующим алгоритмом</i> .</p>
  <p><i>Признак f</i> объекта x – это результат измерения некоторой характеристики объекта. Формально признаком называется отображение <i>f</i>: X -> D<i>f</i>, где D<i>f</i> – множество допустимых значений признака. В частности, любой алгоритм a: X -> Y также можно рассматривать как признак.</p>
  <p>Набор признаков <i>f</i><sub>1</sub>,…, <i>f</i><sub>n</sub>. Вектор (<i>f</i><sub>1</sub>(x),…, <i>f</i><sub>n</sub>(x)) называют <i>признаковым описанием</i> объекта x ∈ X. В дальнейшем будем полагать, что X = 𝐷<sub><i>𝑓</i><sub>1</sub></sub> × … × 𝐷<sub><i>𝑓</i><sub>𝑛</sub></sub>.</p>
  <p>Совокупность признаковых описаний всех объектов выборки записанная в виде таблицы размера ℓ × n, называют <i>матрицей объектов-признаков</i>:</p>
  <p><img src="img/matrix.jpg" alt="матрица объектов-признаков"></p>
  <p><i>Моделью алгоритмов</i> называется параметрическое семейство отображений A = {g(x, <i>θ</i>) | <i>θ</i> ∈ <i>Θ</i>}, где g: X × <i>Θ</i> -> Y – некоторая фиксированная функция, <i>Θ</i> – множество допустимых значений параметра <i>θ</i>, называемое <i>пространством параметров</i> или <i>пространством поиска</i>.</p>
  <p>Процесс подбора оптимального параметра модели <i>θ</i> по обучающей выборке 𝑋<sup>ℓ</sup> называют <i>настройкой</i> или <i>обучением</i> алгоритма a ∈ A.</p>
  <p><i>Метод обучения</i> – это отображение μ: (X × Y)<sup>ℓ</sup> -> A, которое произвольной конечной выборке 𝑋<sup>ℓ</sup>=(𝑥<sub>𝑖</sub>,𝑦<sub>𝑖</sub>)<sup>ℓ</sup><sub>𝑖=1</sub> ставит в соответствие некоторый алгоритм a ∈ A. Говорят также, что метод <i>строит</i> алгоритм a по выборке X<sup>ℓ</sup>.</p>
  <p><i>Функция потерь</i> – это неотрицательная функция ℒ(a, x), характеризующая величину ошибки алгоритма a на объекте x. Если ℒ(a, x) = 0, то ответ a(x) называется <i>корректным</i>.</p>
  <p><i>Функционал качества</i> алгоритма a на выборке 𝑋<sup>ℓ</sup>:</p>
  <p><img src="img/funktcional.jpg" alt="формула функционала качества"></p>
  <p>Другие названия функционала качества – <i>функционал средних потерь</i> и <i>эмпирический риск</i>.</p>
  <p>Если функция потерь принимает значения 1 – ошибочная классификация или 0 – корректная классификация, то функция потерь называется <i>бинарной</i> или <i>индикатором ошибки</i>, а функционал качества называется <i>частотой ошибок</i> алгоритма на выборке.</p>
  <p>Часто используют:</p>
  <p>ℒ(a, x) = |a(x) – y*(x)| - отклонение от правильного ответа; функционал качества тогда зовут – <i>средней ошибкой</i>.</p>
  <p>ℒ(a, x) =(a(x) – y*(x))2 – квадратичная функция потерь; функционал качества – <i>среднеквадратичной ошибкой</i>.</p>
  <p>Классический метод обучения, называемый <i>минимизацией эмпирического риска</i>, заключается в том, чтобы найти в заданной модели A алгоритм a, доставляющий минимальное значение функционалу качества <i>Q</i> на заданной обучающей выборке 𝑋<sup>ℓ</sup>:</p>
  <p><img src="img/klass_obuch.jpg" alt="классический метод обучения"></p>
  
  ## 2. Метрические алгоритмы  <a name="Metricheskiye_algoritmy"></a>
  
  ### 2.1. 1NN  <a name="a1NN"></a>
  
  <p><ol>
    <li>Подбирается метрика. В данной работе это декартово расстояние между векторами.</li>
    <li>Обучающая выборка сортируется в порядке увеличения расстояния от классифицируемого элемента.</li>
    <li>Элемент относят к тому классу к которому принадлежит ближайший (первый в отсортированной выборке) элемент.</li>
  </ol></p>
  
  ### 2.2. kNN  <a name="akNN"></a>
  
  <p>Алгоритм 1NN чувствителен к <i>выбросам</i>-случаям, когда 1 или несколько элементов одного класса оказываются среди элементов другого, устранить эти ситуации может алгоритм kNN.</p>
  <p>Алгоритм kNN отличается от 1NN тем что он относит классифицируемый элемент не к классу ближайшего к нему элемента, а к классу чаще всего встречающемуся среди k ближайших элементов.</p>
  <p>Посмотрим как они работают на выборке <a href="https://ru.wikipedia.org/wiki/Ирисы_Фишера"; target="_blank">ирисов фишера</a>.</p>
  <p>Для иллюстрации методов, достаточно производить обучение и классификацию используя лишь 2 признака: «длинна лепестка» и «ширина лепестка».</p>
  <p>Листинг скрипта на R:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/script_1NN_kNN.R'>script_1NN_kNN.R</a></p>
  <p>Иллюстрация резултатов работы алгоритмов 1NN и kNN(k=4):</p>
  <p><img src="img/klassif.jpg" alt="результат работы 1NN и kNN"></p>
  <p>Как видно из 1 графика алгоритм 1NN допускает при классификации ошибку в точке(6.3, 4.9),что казалось бы невозможно при использовании алгоритма 1NN,но на самом деле это обьясняется тем что 2 объекта разных класов имеют одни и те же координаты, при классификации они отнесены к классу первого встретившегося обьекта, а при построении графика второй обьект закрасил первый. Отсюда следует сделать вывод, что нужно строить классификацию на большем количестве признаков, или подобрать другие признаки не допускающие такого задвоения, или исключить один из элементов из выборки, или смириться с такой неточностью в классификации.</p>
  <p>Алгоритм kNN допускает больше ошибок, но визуально он выглядит более качествеено. Для того чтобы привести более точное обоснование чем kNN лучше в этом случае, чем 1NN, следует прибегнуть к скользящему контролю.</p>
  
  ### 2.3. LOO для kNN  <a name="Loo_akNN"></a>
  
  <p>Посмотрим, как часто будет ошибаться алгоритм если по очереди извлекать по 1 элементу, обучать алгоритм на оставшихся, классифицировать извлеченный, возвращать его, извлекать следующий и так со всеми элементами выборки.(Это называется скользящим контролем LOO). Таким образом проверим все возможные классифицирующие алгоритмы от 1NN до kNN с k = 150(150 - число элементов в выборке)</p>
<p>Листинг скрипта на R:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/Loo_kNN.r'>script_Loo_kNN.R</a></p>
<p><img src="img/ris_loo_knn.jpg" alt="Зависимость частоты ошибок классификации от k в алгоритме kNN по скользящему контролю Loo"></p>
<p>Как видно из графика, минимальная частота ошибок классификации достигается при k равном 3 или 4.
Скрипт выполнялся более 26 минут. Можно значительно его ускорить если заметить, что для классификации извлеченного элемента методами kNN при разных k , при обучении на оставшейся выборке, достаточно произвести не 150 сортировок, а всего одну.
Такой подход ускорит процесс во много раз, но скрипт потеряет наглядность показывающую принцип работы скользящего контроля LOO.</p>

  ## 3. Байесовские классификаторы  <a name="Bayesophskiye_klassiphikatory"></a>
  
  <p>Пусть <i>X</i> &#8212 множество объектов, <i>Y</i> &#8212 конечное множество имён классов, множество <i>X × Y</i> является вероятностным пространством с плотностью распределения p(x, y) = P(y)p(x|y). Вероятности появления объектов каждого из классов
P<sub>y</sub> = P(y) называются априорными вероятностями классов.</p>
  <p.Плотности распределения p<sub>y</sub>(x) = p(x|y) называются функциями правдоподобия классов.</p>
  <p>Знание функций правдоподобия позволяет находить вероятности событий вида &#171x ∈ Ω при условии, что x принадлежит классу y&#187:</p>
  <p><img src="img/omega.jpg" alt="вероятность_собятия"></p>
  <p>Вероятностная постановка задачи классификации разделяется на две независимые подзадачи.</p>
  <p><b>Задача 1.</b> Имеется простая выборка X<sup>ℓ</sup> = (x<sub>i</sub>, y<sub>i</sub>)<sup>ℓ</sup><sub>i=1</sub> из неизвестного распределения p(x, y) = P<sub>y</sub>p<sub>y</sub>(x). Требуется построить эмпирические оценки априорных вероятностей P&#770<sub>y</sub> и функций правдоподобия p&#770<sub>y</sub>(x) для каждого из классов y ∈ Y .<p>
  <p><b>Задача 2.</b> По известным плотностям распределения p<sub>y</sub>(x) и априорным вероятностям P<sub>y</sub> всех классов y ∈ Y построить алгоритм a(x), минимизирующий вероятность ошибочной классификации.</p>
  <p><i>Функционалом среднего риска</i> называется ожидаемая величина потери при классификации объектов алгоритмом a:</p>
  <p><img src="img/funktcyonal.jpg" alt="Функционал_среднего_риска"></p>
  <p>Если известны априорные вероятности P<sub>y</sub> и функции правдоподобия p<sub>y</sub>(x), то минимум среднего риска R(a) достигается алгоритмом</p>
  <p><img src="img/reshayusheye_pravilo1.jpg" alt="Оптимальное байесовское решающее правило"></p>
  <p>Если P<sub>y</sub> и p<sub>y</sub>(x) известны, λ<sub>yy</sub> = 0 и λ<sub>ys</sub> ≡ λ<sub>y</sub> для всех y, s ∈ Y ,
то минимум среднего риска достигается алгоритмом</p>
  <p><img src="img/reshayusheye_pravilo2.jpg" alt="Оптимальное байесовское решающее правило"></p>
  <p><i>Разделяющая поверхность</i> между классами s и t &#8212 это геометрическое место точек x ∈ X таких, что минимум среднего риска достигается одновременно при y = s и y = t: λ<sub>t</sub>P<sub>t</sub>p<sub>t</sub>(x) = λ<sub>s</sub>P<sub>s</sub>p<sub>s</sub>(x). Объекты x, удовлетворяющие этому уравнению, можно относить к любому из двух классов s, t, что не повлияет на средний риск R(a).</p>
  <p><i>Апостериорная вероятность</i> класса y для объекта x &#8212 это условная вероятность P(y|x). Она может быть вычислена по формуле Байеса, если известны p<sub>y</sub>(x) и Py:</p>
  <p><img src="img/bayes.jpg" alt="Формула_байеса"></p>
  <p>Оптимальный алгоритм классификации можно переписать через апостериорные вероятности:</p>
  <p><img src="img/printc_max_aposteri.jpg" alt="Принцип_максимума_апостериорной_вероятности"></p>
  <p>Минимальное значение среднего риска R(a), достигаемое байесовским решающим правилом, называется <i>байесовским риском</i> или <i>байесовским уровнем ошибки</i>.</p>
  <p>Если классы равнозначны (λ<sub>y</sub> ≡ 1), то байесовское правило называется также <i>принципом максимума апостериорной вероятности</i>.</p>
  <p>Если классы ещё и равновероятны (P<sub>y</sub> ≡ 1 |Y |), то объект x просто относится к классу y с наибольшим значением плотности распределения p<sub>y</sub>(x) в точке x</p>

  <H3>3.1. &#171Наивный&#187 байесовский классификатор</H3>  <a name="Naivnyy_bayesophskiy_klassiphikator"></a>
  
  <p>Если признаки элементов независимые случайные величины, то функции правдоподобия классов примут вид</p>
  <p>p<sub>y</sub>(x) = p<sub>y1</sub>(ξ<sub>1</sub>)⋅⋅⋅p<sub>yn</sub>(ξ<sub>n</sub>)</p>
  <p>где p<sub>yj</sub>(ξ<sub>j</sub>) плотность распределения значений j-го признака для класса y.</p>
  <p>Подставив в байесовское решающее правило эмпирические оценки одномерных плотностей признаков получим алгоритм</p>
  <p><img src="img/naivnyy_bayes.jpg" alt="Наивный байесовский алгоритм"></p>
  <p>Листинг скрипта на R реализующий &#171Наивный&#187 байесовский классификатор на любимых всеми студентами информатиками ирисах фишера:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/NNBayes.R'>NNBayes.R</a></p>
  <p><img src="img/vizualizatciya_naivnogo_bayesa.jpg" alt="Визуализация Наивного байеса на ирисах фишера"></p>
  <p>Как видно из рисунка предположение, что признаки были независимыми нормально распределенными было действительно &#171наивным&#187 потому что ошибка классификации составила почти 9%.</p>
  <p>Зато алгоритм достаточно прост и скор в реализации и дает гладкие разделяющие линии.</p>

  <H3>3.2. Классификатор plug-in</H3>  <a name="plug_in"></a>
  
  <p>Листинг скрипта на R реализующий plag-in классификатор на любимых всеми студентами информатиками ирисах фишера:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/plug_in.R'>plug_in.R</a></p>
  <p><img src="img/plugin.jpg" alt="Визуализация plug-in"></p>
  
  <H3>3.3. Линейный дискриминант Фишера</H3>  <a name="fisher"></a>
  
  <p>Листинг скрипта на R реализующий Линейный дискриминант Фишера на ирисах фишера:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/fisher.R'>fisher.R</a></p>
  <p><img src="img/fisher.jpg" alt="Визуализация LDF"></p>
  
  ## 4. Линейные алгоритмы классификации.  <a name="Lineynyye_klassiphikatory"></a>
  
  ### 4.1. Метод стохастического градиента.  <a name="SG"></a>
  
  ### 4.1.1 ADALINE.  <a name="ADALINE"></a>
  
<p>Вынесенные в отдельный файл функции для ADALINE:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/SG_funktcii.R'>SG_funktcii.R</a></p>
<p>Листинг скрипта на R реализующий алгоритм классификации ADALINE:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/SG.R'>SG.R</a></p>
<p><img src="img/ADALINE.jpg" alt="Визуализация ADALINE"></p>

### 4.1.2 Персептрон Розенблатта.  <a name="Perseptron"></a>

<p>Вынесенные в отдельный файл функции для Персептрона:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/PRozenblata_functions.R'>PRozenblata_functions.R</a></p>
<p>Листинг скрипта на R реализующий алгоритм классификации Персептрона Розенблата на основе правила Хэбба:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/PRozenblata.R'>PRozenblata.R</a></p>
<p><img src="img/Perseptron.jpg" alt="Визуализация Персептрона"></p>
<p>Листинг скрипта на R с помощью которгого можно протестировать работу алгоритма:<a href='https://github.com/PavlyukovVladimir/SMPR/blob/master/scripts/test_PRozenblata.R'>test_PRozenblata.R</a></p>
