
##### Оригинальное фото:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/4.jpg?raw=true" alt="drawing" width="700"/>

Морфологические преобразования - это несколько простых операций, основанных на форме изображения. Обычно это выполняется с двоичными изображениями. Ему нужны два входных сигнала, один из которых - наше исходное изображение, второй называется структурирующим элементом или ядром, которое определяет характер операции. Двумя основными морфологическими операторами являются Эрозия и Расширение. Затем в игру также вступают его различные формы, такие как Открытие, Закрытие, Градиент и т.д.

Раскрытие(cv.morphologyEx(img, cv.MORPH_OPEN, kernel)) - это просто другое название эрозии, за которой следует расширение. Основная идея эрозии аналогична только эрозии почвы, она размывает границы объекта переднего плана (всегда старайтесь, чтобы передний план был белым). Ядро проходит по изображению (как в 2D-свертке). Пиксель в исходном изображении (либо 1, либо 0) будет считаться 1 только в том случае, если все пиксели под ядром равны 1, в противном случае он размывается (обнуляется). Все пиксели вблизи границы будут отброшены в зависимости от размера ядра. Таким образом, уменьшается толщина или размер объекта переднего плана или просто уменьшается белая область на изображении. Расширение прямо противоположно эрозии. Здесь пиксельный элемент равен "1", если хотя бы один пиксель под ядром равен "1". Таким образом, увеличивается белая область на изображении или увеличивается размер объекта переднего плана. Обычно в таких случаях, как устранение шума, за эрозией следует расширение.

Закрытие (cv.morphologyEx(img, cv.MORPH_CLOSE, kernel)) происходит в обратном направлении от Открытия, Расширение сопровождается Эрозией. Это полезно для закрытия небольших отверстий внутри объектов переднего плана или небольших черных точек на объекте.

Морфологический градиент (cv.morphologyEx(img, cv.MORPH_GRADIENT, kernel)) - это разница между расширением и размыванием изображения. Результат будет выглядеть как контур объекта.

После создания морфологического градиента разделяем градиентное изображение на каналы и применяем метод Оцу к каждому каналу. Потом обьединяем все каналы и сохраняем картинку.

##### Результат:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src1_without_blackout.jpg?raw=true" alt="drawing" width="700"/>

Белый фон залит в чёрный чтобы белыми остались только обведённые обьекты.

##### Результат, с заливкой в чёрный:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src1.jpg?raw=true" alt="drawing" width="700"/>

Образовавшиеся шумы после заливки можно убрать с помощью библиотеки skimage через morphology.remove_small_objects что позволяет убирать мелкие обьекты, состоящие из определённого количества пикселей и закрашивать их в цвет который их окружает. Я тестировала на разных размерах, основываясь на ширине, беря какой то процент:

##### Маска, с убранными шумами по размеру 1% от ширины:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src2_0.01.jpg?raw=true" alt="drawing" width="700"/>

##### Маска, с убранными шумами по размеру 5% от ширины:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src2_0.05.jpg?raw=true" alt="drawing" width="700"/>

##### Маска, с убранными шумами по размеру 10% от ширины:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src2_0%2C1.jpg?raw=true" alt="drawing" width="700"/>

Вычисляем количество связных компонент с помощью cv2.connectedComponents и их расположение. Потом сопоставляем метки компонентов со значениями оттенков цветов и переводим все цвета в HSV. Фон окрашиваем в чёрный. Та

##### Маска раскрашенная, с убранными шумами по размеру ширина*0,01:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src3_0%2C01.jpg?raw=true" alt="drawing" width="700"/>

##### Маска раскрашенная, с убранными шумами по размеру ширина*0,1:
<img src="https://github.com/NadBarin/Detimg/blob/main/1/src3_0%2C1.jpg?raw=true" alt="drawing" width="700"/>