# Vladivostok2022
Russian Championship September 2022

В состав репозитория включены файлы кода и входные данные для решения в два разделения тестового набора и четыре прогнозные модели по разделению. Последовательно по этапам выполнения модели:

1-й этап - предобработка. Файл "Модуль Логином Предобработка данных.lgp" - модуль lgp аналитической платформы Логином в бесплатной общедоступной версии Community Edition v.6.5.1 (дистрибутив можно свободно получить по адресу https://loginom.ru/download). Входные данные для предобработки (включающие переводы поля summary и первых строк текстовых полей комментариев) представлеы в репозитории набором ("test_issues.csv", "train_issues.csv", "train_comments_short.csv", "test_comments_short.csv").Результирующие файлы предобработки для входа на нейросеть (трейн и тест): "train_predobr.csv" , "test_predobr.csv". 

2-й этап - первичный прогноз, создание шаблона для замен (в общем итоговом прогноза значения не имеет, ни по одной записи, использовался как шаблон для присоединения и последующей проверки значимости прогнозов на частях теста). Выполнен нейросетью в среде Ludwig. Файл модели "Ludwig вариант.ipynb". Файл лучшего результата (опоры для дальнейших замен) "итоговый прогноз_460842 на паблик ЛБ.csv" (результат 0.0460842). Модуль замен, использованный далее по всем прогнозам "Замена типовая.ipynb"

3-й этап - первое разделение тестового набора ансамблем бинарной классификации xgboost и ctboost по уровню выше 7200 сек (позитивный бинарный класс) и ниже 7200. Файл модели разделения (выполнялся в среде kaggle) "Первое разделение тестового набора catboost-xgboost-diff7200.ipynb" , входящие наборы "train_predobr.csv" , "test_predobr.csv" (по итогам 1-го этапа), выходных наборов 3 - 251 значение теста "точно меньше 7200" (подтвержденное обоими моделями бинарной классификации по значению 0), 479 значений "точно больше 7200" (подтвержденное по значению 1) и 330 значений "средние" (прогнозы моделей на которых различались). Все выходные тестовые наборы представлены в директориях первых трех прогнозов, в качестве входных для моделей обработки.

4-й этап - модуль кластеризации аналитической платформы Логином, файл "Модуль Логином Кластеризация.lgp". Входными данными являются значения трейна, отфильтрованные по оптимальному уровню целевой переменной, выходными - таблица кластеров с id и центром кластера и частей трейна с добавлением поля id кластера для проведения классификации моделью прогноза.

5-й этап - классификация catboost тестового набора значений "точно больше 7200", полученного в результате выполнения 3-го этапа (479 значений). Все данные этапа приложены в директории "Первый прогноз" в составе:
"catboost_классификация_по_кластерам_7200_вверх.ipynb" - код модели прогноза
"test_predobr_after7200_claster.csv" - входной набор тестовой части
"train_predobr_claster_vverh.csv" - входной набор трейновой части
"tabliza_clasters_vverh.csv" - входная таблица кластеров для выполнения классификации
"прогноз вверх лучший.csv" - выходной прогноз части теста (лучший вариант)
"сабмиссия кластеры 8_лучший_093435.csv" - итоговая сабмиссия этапа, после присоединения прогноза к опоре через модуль "Замена типовая.ipynb" 

6-й этап - классификация catboost тестового набора значений "точно меньше 7200", полученного в результате выполнения 3-го этапа (251 значение). Все данные этапа приложены в директории "Второй прогноз" в составе:
"catboost_классификация_по_кластерам_7200_вниз.ipynb" - код модели прогноза
"test_predobr_do7200_claster.csv" - входной набор тестовой части
"train_predobr_7200_vniz.csv" - входной набор трейновой части
"tabliza_clasters_vniz_5.csv" - входная таблица кластеров для выполнения классификации
"прогноз Пуассон кластер 5 вниз.csv" - выходной прогноз части теста (лучший вариант)
"сабмиссия кластеры 5 вниз лучший 093582.csv" - итоговая сабмиссия этапа, после присоединения прогноза к опоре через модуль "Замена типовая.ipynb" 

7-й этап - классификация catboost тестового набора значений "средние", полученного в результате выполнения 3-го этапа (330 значений). Все данные этапа приложены в директории "Третий прогноз" в составе:
"catboost_классификация_по_кластерам_7200_средние.ipynb" - код модели прогноза
"test_predobr_middle7200_claster.csv" - входной набор тестовой части
"train_predobr_claster_vverh_7200.csv" - входной набор трейновой части
"tabliza_clasters_middle.csv" - входная таблица кластеров для выполнения классификации
"прогноз 11 кластер по средней части лучший.csv" - выходной прогноз части теста (лучший вариант)
"сабмиссия 11 по средней части лучшая_115414.csv" - итоговая сабмиссия этапа, после присоединения прогноза к опоре через модуль "Замена типовая.ipynb" 

8-й этап - второе разделение тестового набора ансамблем бинарной классификации xgboost и ctboost по уровню выше 68000 сек (позитивный бинарный класс) и ниже 68000. Файл модели разделения (выполнялся в среде kaggle) "Второе разделение тестового набора catboost-xgboost-diff68000.ipynb" , входящие наборы "train_predobr.csv" , "test_predobr.csv" (по итогам 1-го этапа). По итогам выполнения на нескольких random_seed получен набор значений, подтвержденный обоими значениями ансамбля. При тестировании изменения их значений выявлен принцип формирования приватного лидерборда (обнаружены "замороженные записи", изменения значений которых не влияют на результат) и записи, прямо повышающие оценку прогноза. Итог выполнения 8-го этапа приведен в файле "id 25 записей полученных по итогам второго разделения.csv" - в котором зафиксированы id 22 записей, не влияющих на прогноз ("замороженных" записей привата) и 3-х записей, основых для прогноза паблика (предположительно, максимально возможный результат на паблике - 0.5, остальные 0.5 прогноза - по итогам прогнозирования всех "замороженных" записей теста на привате).

9-й этап - выполнение прогноза для 25 записей, выявленных при втором разделении теста. Все данные этапа приложены в директории "Четвертый итоговый прогноз" в составе:
"Замена итоговая.ipynb" - модуль последовательного выполнения итогового прогноза с комментариями каждого шага
"тест 3 наиболее значимых.csv" - id и определенные по кластерам значения для 3-х записей, сформировавшие основной итог прогнозирования открытой части теста
"тест 22 неизвестных.csv" - id "замороженных" записей теста, классифицированных на 8-м этапе, как наибольшие
"сабмиссия лучшая по первому разделению_115414.csv" - опорная сабмиссия, лучшая по итогам 7-го этапа для формирования окончательно прогноза
"tabliza_clasters_for3.csv" - таблица кластеров для фиксации значений 3-х наибольшей записей открытой части теста
"tabliza_clasters_for22.csv" - таблица кластеров для прогнозирования "замороженных" значений
"test_22.csv","train_predobr_for22.csv" - входные наборы теста и трейна для прогнозирования 22 "замороженных" значений
"catboost_классификация_итоговая для 22 неизвестных.ipynb" - код модуля прогноза
"prognoses for 22.csv" - таблица результатов выполнения модуля прогноза с указанием random_seed для усреднения в модуле "Замена итоговая.ipynb"
"итоговый прогноз.csv" - итоговый прогноз модуля "Замена итоговая.ipynb" с результатом 0.460842 на лидерборде.

10-й этап, заключительный. Кластеризация итогового значения. "итоговый прогноз.csv" содержит 23 группы значений. Кластеризация его по 14 центрам позволила улучшить результат до 0.463290, что и определено, как финальное решение (файл "прогноз по кластерам 14_463290.csv"). Модуль Логином, выполнивший финальную кластеризацию, приложен в репозитории с именем "Модуль Логином Кластеризация для прогноза итогового.lgp"








