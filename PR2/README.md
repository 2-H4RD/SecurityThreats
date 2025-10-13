# Основы обработки данных с помощью R и Dplyr
vszub24@yandex.ru

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить практические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  Программное обеспечение ОС Windows 11 Pro
2.  RStudio
3.  Интерпретатор языка R 4.5.1

## План

1.  Установть пакет dplyr.

2.  Проанализировать встроенный в пакет dplyr набор данных starwars с
    помощью языка R и ответить на вопросы:

    1\. Сколько строк в датафрейме?

    2\. Сколько столбцов в датафрейме?

    3\. Как просмотреть примерный вид датафрейма?

    4\. Сколько уникальных рас персонажей (species) представлено в
    данных?

    5\. Найти самого высокого персонажа.

    6\. Найти всех персонажей ниже 170

    7\. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ
    подсчитать по формулe$I=\frac{m}{h^2}$, где 𝑚 – масса (weight), а ℎ
    – рост (height).

    8\. Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по
    отношению массы (mass) к росту (height) персонажей.

    9\. Найти средний возраст персонажей каждой расы вселенной Звездных
    войн.

    10\. Найти самый распространенный цвет глаз персонажей вселенной
    Звездных войн.

    11\. Подсчитать среднюю длину имени в каждой расе вселенной Звездных
    войн.

## Шаги:

### Установка пакета dplyr, загрузка датасета.

``` r
install.packages("dplyr")
```

> WARNING: Rtools is required to build R packages but is not currently
> installed. Please download and install the appropriate version of
> Rtools before proceeding:

https://cran.rstudio.com/bin/windows/Rtools/ Устанавливаю пакет в
‘D:/Rlib’ (потому что ‘lib’ не определено) пробую URL
‘https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/dplyr_1.1.4.zip’
Content type ‘application/zip’ length 1593077 bytes (1.5 MB) downloaded
1.5 MB

пакет ‘dplyr’ успешно распакован, MD5-суммы проверены

Скачанные бинарные пакеты находятся в D:\_packages \>  
library(dplyr)

Присоединяю пакет: ‘dplyr’

Следующие объекты скрыты от ‘package:stats’:

filter, lag

Следующие объекты скрыты от ‘package:base’:

intersect, setdiff, setequal, union

``` r
data(starwars)
```

### Анализ датасета и ответы на вопросы

#### Сколько строк в датафрейме?

``` r
nrow(starwars)
```

> \[1\] 87

#### Сколько столбцов в датафрейме?

``` r
ncol(starwars)
```

> \[1\] 14

#### Как просмотреть примерный вид датафрейма?

``` r
glimpse(starwars)
```

> Rows: 87 Columns: 14 $ name <chr> “Luke Skywalker”, “C-3PO”, “R2-D2”,
> “Darth Vader”, “Leia Organa”, “Owen Lars”, “Beru Whitesun … $ height
> <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 228,
> 180, 173, 175, 170, 180, 66, 17… $ mass <dbl> 77.0, 75.0, 32.0, 136.0,
> 49.0, 120.0, 75.0, 32.0, 84.0, 77.0, 84.0, NA, 112.0, 80.0, 74.0, 135…
> $ hair_color <chr>”blond”, NA, NA, “none”, “brown”, “brown, grey”,
> “brown”, NA, “black”, “auburn, white”, “blond… $ skin_color
> <chr>”fair”, “gold”, “white, blue”, “white”, “light”, “light”,
> “light”, “white, red”, “light”, “fai… $ eye_color <chr>”blue”,
> “yellow”, “red”, “yellow”, “brown”, “blue”, “blue”, “red”, “brown”,
> “blue-gray”, “blue… $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0,
> 52.0, 47.0, NA, 24.0, 57.0, 41.9, 64.0, 200.0, 29.0, 44.0, 600.… $ sex
> <chr>”male”, “none”, “none”, “male”, “female”, “male”, “female”,
> “none”, “male”, “male”, “male”, “m… $ gender <chr>”masculine”,
> “masculine”, “masculine”, “masculine”, “feminine”, “masculine”,
> “feminine”, “masc… $ homeworld <chr>”Tatooine”, “Tatooine”, “Naboo”,
> “Tatooine”, “Alderaan”, “Tatooine”, “Tatooine”, “Tatooine”, “… $
> species <chr>”Human”, “Droid”, “Droid”, “Human”, “Human”, “Human”,
> “Human”, “Droid”, “Human”, “Human”, “Hum… $ films <list> \<”A New
> Hope”, “The Empire Strikes Back”, “Return of the Jedi”, “Revenge of
> the Sith”, “The F… $ vehicles <list> \<”Snowspeeder”, “Imperial
> Speeder Bike”\>, \<\>, \<\>, \<\>, “Imperial Speeder Bike”, \<\>,
> \<\>, \<\>, \<\>… $ starships <list> \<“X-wing”, “Imperial shuttle”\>,
> \<\>, \<\>, “TIE Advanced x1”, \<\>, \<\>, \<\>, \<\>, “X-wing”,
> \<“Jedi s…

#### Сколько уникальных рас персонажей (species) представлено в данных?

``` r
starwars %\>% distinct(species) %\>% nrow()
```

> \[1\] 38

#### Найти самого высокого персонажа.

``` r
starwars %\>%
  filter(height == max(height,na.rm = TRUE)) %\>%
  pull(name)
```

> \[1\] “Yarael Poof”

#### Найти всех персонажей ниже 170

``` r
starwars %\>%
  filter(height \< 170) %\>%
  select(name, height) %\>%
  mutate(row_number = row_number())%\>%
  select(row_number, name, height) %\>%
  as.matrix() %\>%
  head(10)
```

> row_number name height  
> \[1,\] ” 1” “C-3PO” “167”  
> \[2,\] ” 2” “R2-D2” ” 96”  
> \[3,\] ” 3” “Leia Organa” “150”  
> \[4,\] ” 4” “Beru Whitesun Lars” “165”  
> \[5,\] ” 5” “R5-D4” ” 97”  
> \[6,\] ” 6” “Yoda” ” 66”  
> \[7,\] ” 7” “Mon Mothma” “150”  
> \[8,\] ” 8” “Wicket Systri Warrick” ” 88”  
> \[9,\] ” 9” “Nien Nunb” “160”  
> \[10,\] “10” “Watto” “137”

#### Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать по формулe$I=\frac{m}{h^2}$, где 𝑚 – масса (weight), а ℎ – рост (height).

``` r
starwars %\>%
  mutate(height_m = height / 100,bmi = mass / (height_m)\^2) %\>%
  select(name, mass, height, bmi) %\>%\
  as.matrix() %\>%\
  head(10)
```

> name mass height bmi   
> \[1,\] “Luke Skywalker” ” 77.0” “172” ” 26.02758”  
> \[2,\] “C-3PO” ” 75.0” “167” ” 26.89232”  
> \[3,\] “R2-D2” ” 32.0” ” 96” ” 34.72222”  
> \[4,\] “Darth Vader” ” 136.0” “202” ” 33.33007”  
> \[5,\] “Leia Organa” ” 49.0” “150” ” 21.77778”  
> \[6,\] “Owen Lars” ” 120.0” “178” ” 37.87401”  
> \[7,\] “Beru Whitesun Lars” ” 75.0” “165” ” 27.54821”  
> \[8,\] “R5-D4” ” 32.0” ” 97” ” 34.00999”  
> \[9,\] “Biggs Darklighter” ” 84.0” “183” ” 25.08286”  
> \[10,\] “Obi-Wan Kenobi” ” 77.0” “182” ” 23.24598”  

#### Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по отношению массы (mass) к росту (height) персонажей.

``` r
starwars %\>% 
 mutate(stretch_ratio = mass / height) %\>%
 filter(!is.na(stretch_ratio)) %\>%
 arrange(desc(stretch_ratio)) %\>%
 head(10) %\>%
 select(name, mass, height, stretch_ratio) %\>%
 mutate(row_number = row_number()) %\>%
 select(row_number, name, mass, height, stretch_ratio) %\>%
 as.matrix()
```

> row_number name mass height stretch_ratio  
> \[1,\] ” 1” “Jabba Desilijic Tiure” “1358” “175” “7.7600000”   
> \[2,\] ” 2” “Grievous” ” 159” “216” “0.7361111”   
> \[3,\] ” 3” “IG-88” ” 140” “200” “0.7000000”  
> \[4,\] ” 4” “Owen Lars” ” 120” “178” “0.6741573”   
> \[5,\] ” 5” “Darth Vader” ” 136” “202” “0.6732673”   
> \[6,\] ” 6” “Jek Tono Porkins” ” 110” “180” “0.6111111”   
> \[7,\] ” 7” “Bossk” ” 113” “190” “0.5947368”   
> \[8,\] ” 8” “Tarfful” ” 136” “234” “0.5811966”   
> \[9,\] ” 9” “Dexter Jettster” ” 102” “198” “0.5151515”   
> \[10,\] “10” “Chewbacca” ” 112” “228” “0.4912281” 

#### Найти средний возраст персонажей каждой расы вселенной Звездных войн.

``` r
starwars %\>%
 mutate(current_year = 100,age = current_year + birth_year) %\>%
 filter(!is.na(age) & !is.na(species)) %\>%
 group_by(species) %\>%
 summarise(average_age = mean(age),count = n()) %\>%  + arrange(desc(average_age)) %\>%
 mutate(row_number = row_number()) %\>%
 select(row_number, species, average_age, count) %\>%
 as.matrix()
```

> row_number species average_age count  
> \[1,\] ” 1” “Yoda’s species” “996.0000” ” 1”  
> \[2,\] ” 2” “Hutt” “700.0000” ” 1”   
> \[3,\] ” 3” “Wookiee” “300.0000” ” 1”   
> \[4,\] ” 4” “Cerean” “192.0000” ” 1”   
> \[5,\] ” 5” “Zabrak” “154.0000” ” 1”   
> \[6,\] ” 6” “Human” “153.7423” “26”   
> \[7,\] ” 7” “Droid” “153.3333” ” 3”   
> \[8,\] ” 8” “Trandoshan” “153.0000” ” 1”   
> \[9,\] ” 9” “Gungan” “152.0000” ” 1”   
> \[10,\] “10” “Mirialan” “149.0000” ” 2”  
> \[11,\] “11” “Twi’lek” “148.0000” ” 1”   
> \[12,\] “12” “Rodian” “144.0000” ” 1”   
> \[13,\] “13” “Mon Calamari” “141.0000” ” 1”   
> \[14,\] “14” “Kel Dor” “122.0000” ” 1”   
> \[15,\] “15” “Ewok” “108.0000” ” 1” 

#### Найти самый распространенный цвет глаз персонажей вселенной Звездных войн.

``` r
starwars %\>%
 count(eye_color, sort = TRUE) %\>%
 filter(!is.na(eye_color)) %\>%
 slice(1) %\>%
 pull(eye_color)
```

> \[1\] “brown”

#### Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн.

``` r
starwars %\>%
  mutate(name_length = nchar(name)) %\>%
  filter(!is.na(species)) %\>%
  group_by(species) %\>%
  summarise(avg_name_length = mean(name_length, na.rm = TRUE),count = n()) %\>%
  arrange(desc(avg_name_length)) %\>%
  mutate(row_number = row_number()) %\>%
  select(row_number, species, avg_name_length, count) %\>%
  as.matrix()
```

> row_number species avg_name_length count  
> \[1,\] ” 1” “Ewok” “21.000000” ” 1”   
> \[2,\] ” 2” “Hutt” “21.000000” ” 1”   
> \[3,\] ” 3” “Geonosian” “17.000000” ” 1”   
> \[4,\] ” 4” “Besalisk” “15.000000” ” 1”   
> \[5,\] ” 5” “Mirialan” “14.000000” ” 2”  
> \[6,\] ” 6” “Toong” “14.000000” ” 1”   
> \[7,\] ” 7” “Aleena” “12.000000” ” 1”   
> \[8,\] ” 8” “Cerean” “12.000000” ” 1”   
> \[9,\] ” 9” “Gungan” “11.666667” ” 3”   
> \[10,\] “10” “Human” “11.342857” “35”   
> \[11,\] “11” “Iktotchi” “11.000000” ” 1”   
> \[12,\] “12” “Neimodian” “11.000000” ” 1”   
> \[13,\] “13” “Quermian” “11.000000” ” 1”   
> \[14,\] “14” “Twi’lek” “11.000000” ” 2”   
> \[15,\] “15” “Chagrian” “10.000000” ” 1”   
> \[11,\] “11” “Iktotchi” “11.000000” ” 1”   
> \[12,\] “12” “Neimodian” “11.000000” ” 1”   
> \[13,\] “13” “Quermian” “11.000000” ” 1”   
> \[14,\] “14” “Twi’lek” “11.000000” ” 2”   
> \[15,\] “15” “Chagrian” “10.000000” ” 1”   
> \[16,\] “16” “Clawdite” “10.000000” ” 1”   
> \[17,\] “17” “Pau’an” “10.000000” ” 1”   
> \[18,\] “18” “Skakoan” “10.000000” ” 1”   
> \[19,\] “19” “Tholothian” “10.000000” ” 1”   
> \[20,\] “20” “Zabrak” ” 9.500000” ” 2”   
> \[21,\] “21” “Nautolan” ” 9.000000” ” 1”   
> \[22,\] “22” “Sullustan” ” 9.000000” ” 1”   
> \[23,\] “23” “Kaleesh” ” 8.000000” ” 1”   
> \[24,\] “24” “Kel Dor” ” 8.000000” ” 1”   
> \[25,\] “25” “Muun” ” 8.000000” ” 1”   
> \[26,\] “26” “Togruta” ” 8.000000” ” 1”   
> \[27,\] “27” “Vulptereen” ” 8.000000” ” 1”   
> \[28,\] “28” “Wookiee” ” 8.000000” ” 2”   
> \[29,\] “29” “Dug” ” 7.000000” ” 1”   
> \[30,\] “30” “Kaminoan” ” 7.000000” ” 2”   
> \[31,\] “31” “Xexto” ” 7.000000” ” 1”   
> \[32,\] “32” “Mon Calamari” ” 6.000000” ” 1”   
> \[33,\] “33” “Rodian” ” 6.000000” ” 1”   
> \[34,\] “34” “Toydarian” ” 5.000000” ” 1”   
> \[35,\] “35” “Trandoshan” ” 5.000000” ” 1”   
> \[36,\] “36” “Droid” ” 4.833333” ” 6”   
> \[37,\] “37” “Yoda’s species” ” 4.000000” ” 1”   

## Оценка результатов

В данной практической работе мы применили знания полученные в результате
первой практической работы для обработки массива данных.

## Вывод

Таким образом, мы развили практические навыки использования функций
обработки данных пакета dplyr – функции select(), filter(), mutate(),
arrange(), group_by()
