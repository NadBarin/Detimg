
##### Оригинальное фото:
<center><img src="https://github.com/NadBarin/Detimg/blob/main/1/4.jpg?raw=true" alt="drawing" width="700"/><center>

Морфологические преобразования - это несколько простых операций, основанных на форме изображения. Обычно это выполняется с двоичными изображениями. Ему нужны два входных сигнала, один из которых - наше исходное изображение, второй называется структурирующим элементом или ядром, которое определяет характер операции. Двумя основными морфологическими операторами являются Эрозия и Расширение. Затем в игру также вступают его различные формы, такие как Открытие, Закрытие, Градиент и т.д. Мы увидим их один за другим с помощью следующего изображения:

<center><img src="https://github.com/NadBarin/Detimg/blob/main/1/j.png?raw=true" alt="drawing" width="100"/><center>

Основная идея эрозии аналогична только эрозии почвы, она размывает границы объекта переднего плана (всегда старайтесь, чтобы передний план был белым). Ядро проходит по изображению (как в 2D-свертке). Пиксель в исходном изображении (либо 1, либо 0) будет считаться 1 только в том случае, если все пиксели под ядром равны 1, в противном случае он размывается (обнуляется). Все пиксели вблизи границы будут отброшены в зависимости от размера ядра. Таким образом, уменьшается толщина или размер объекта переднего плана или просто уменьшается белая область на изображении.
    
<center><img src="https://github.com/NadBarin/Detimg/blob/main/1/erosion.png?raw=true" alt="drawing" width="100"/><center>
    
Расширение прямо противоположно эрозии. Здесь пиксельный элемент равен "1", если хотя бы один пиксель под ядром равен "1". Таким образом, увеличивается белая область на изображении или увеличивается размер объекта переднего плана. Обычно в таких случаях, как устранение шума, за эрозией следует расширение, потому что эрозия удаляет белые шумы, но она также уменьшает размер нашего объекта. Поэтому мы расширяем его. Поскольку шум исчез, он не вернётся, но площадь нашего объекта увеличивается. Это также полезно при соединении разорванных частей объекта.
    
<center><img src="https://github.com/NadBarin/Detimg/blob/main/1/dilation.png?raw=true" alt="drawing" width="100"/><center>
        
Открытие(cv.morphologyEx(img, cv.MORPH_OPEN, kernel)) - это просто другое название эрозии, за которой следует расширение. Это полезно для устранения шума, как было обьяснено выше.
    
<center><img src="https://github.com/NadBarin/Detimg/blob/main/1/opening.png?raw=true" alt="drawing" width="200"/><center>

Закрытие (cv.morphologyEx(img, cv.MORPH_CLOSE, kernel)) происходит в обратном направлении от Открытия, Расширение сопровождается Эрозией. Это полезно для закрытия небольших черных точек на объекте.
   
<center><img src="https://github.com/NadBarin/Detimg/blob/main/1/closing.png?raw=true" alt="drawing" width="200"/><center>

Морфологический градиент (cv.morphologyEx(img, cv.MORPH_GRADIENT, kernel)) - это разница между расширением и эрозией изображения. Результат будет выглядеть как контур объекта.
    
<center><img src="https://github.com/NadBarin/Detimg/blob/main/1/gradient.png?raw=true" alt="drawing" width="200"/><center>

После создания морфологического градиента разделяем градиентное изображение на каналы и применяем метод Оцу к каждому каналу. Потом обьединяем все каналы и сохраняем картинку.

##### Результат:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src1_without_blackout.jpg?raw=true" alt="drawing" width="700"/>

Белый фон залит в чёрный чтобы белыми остались только обведённые обьекты.

##### Результат, с заливкой в чёрный:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src1.jpg?raw=true" alt="drawing" width="700"/>
    
Время работы: 2 секунды

Образовавшиеся шумы после заливки можно убрать с помощью библиотеки skimage через morphology.remove_small_objects что позволяет убирать мелкие обьекты, состоящие из определённого количества пикселей и закрашивать их в цвет который их окружает. Я тестировала на разных размерах, основываясь на ширине, беря какой то процент:

##### Маска, с убранными шумами по размеру 1% от ширины:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src2_0.01.jpg?raw=true" alt="drawing" width="700"/>

##### Маска, с убранными шумами по размеру 5% от ширины:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src2_0.05.jpg?raw=true" alt="drawing" width="700"/>

##### Маска, с убранными шумами по размеру 10% от ширины:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src2_0%2C1.jpg?raw=true" alt="drawing" width="700"/>
 
Время работы для одного варианта: 3 секунды

Вычисляем количество связных компонент с помощью cv2.connectedComponents и их расположение. Потом сопоставляем метки компонентов со значениями оттенков цветов и переводим все цвета RGB. Фон окрашиваем в чёрный. Так получается изображение со связными компонентами, окрашенными в различные цвета.

##### Маска раскрашенная, с убранными шумами по размеру ширина*0,01:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src3_0%2C01.jpg?raw=true" alt="drawing" width="700"/>

##### Маска раскрашенная, с убранными шумами по размеру ширина*0,1:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src3_0%2C1.jpg?raw=true" alt="drawing" width="700"/>
    
Дальше будет рассматриваться пример с маской с убранными шумами по размеру ширина*0,1. 
    
Затем проводим подсчёт получившихся цветов и с помощью cv2.inRange(labeled_img, colours1[i], colours1[i]) выночим на отдельную картинку каждый элемент, цвет которого выше порогового значения 1% от ширины. В результате получится набор картинок, на каждой из которых будет маска отдельного обьекта. Пример некоторых результатов:
   

<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo1.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo5.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo16.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo30.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo49.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo51.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo54.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo57.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo59.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo60.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo64.jpg?raw=true" alt="drawing" width="700"/>
    
Время работы:41 секунда
    
Далее чистим от шумов с помощью библиотеки skimage через morphology.remove_small_objects убирая шумами по размеру 1% от ширины:
    
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__1.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__5.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__16.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__30.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__49.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__51.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__54.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__57.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__59.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__60.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo__64.jpg?raw=true" alt="drawing" width="700"/>
  
Время работы: 124 Секунды
    
Функция cv::bitwise_and вычисляет побитовое логическое соединение для каждого элемента для: Двух массивов, когда src1 и src2 имеют одинаковый размер:
    
    dst(I)=src1(I)∧src2(I)if mask(I)≠0
    
То есть берутся вся не чёрная область маски и побитово соединяется с битами оригинальной картинки. Пример как это выглядит:
    
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___1.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___5.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___16.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___30.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___49.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___51.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___54.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___57.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___59.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___60.jpg?raw=true" alt="drawing" width="700"/>
<img src="https://github.com/NadBarin/Detimg/blob/main/1/photo___64.jpg?raw=true" alt="drawing" width="700"/>
    
Время работы: 27 секунд
    
    
Если менять размер фото то можно сократить время работы программы, однако из-за этого могут потеряться части обьектов или обьекты могут слиться между собой из-за окрашивания. Пример на изменении размера фото до 1280x720:


<img src="https://github.com/NadBarin/Detimg/blob/main/1/src3__.jpg?raw=true" alt="drawing" width="700"/>
