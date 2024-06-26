#### 05. Обработка текстовых данных
Поскольку в Linux все есть файл, работать с текстовыми данными разного формата вам придется много и часто и в Linux для этого существует богатый функционал. Начнем с самого простого и постепенно пойдем глубже
1) утилита cat - выводит содержимое файла на экран
```
cd 05_processing_text
cat access.log
```
2) утилиты head и tail без опций выводят первые и последние 10 строк файла соответственно
```
head access.log
tail access.log
```
ключом -n можно переопределить дефолтное количество строк
```
head -n 3 access.log
tail -n 15 access.log
```
у tail есть отличный ключик -f, который позволяет "следовать" за файлом, то есть отслеживать появление записей в режиме реального времени
```
tail -f access_ex.log
echo "53.60.34.242 - - [26/Jan/2024:16:39:44 +0000] "GET /Synchronised/hierarchy/Seamless.js HTTP/1.1" 200 1340 "-" "Opera/10.39 (Windows NT 6.1; en-US) Presto/2.9.227 Version/12.00"
" >> access_ex.log
```
3) wc - подсчет срок, слов, символов
```
wc -l access.log
wc -c access.log
wc -w access.log
```
4) утилита cut позволяет обрезать строки
```
head info.txt
cut -d: -f1 info.txt
cut -d: -f4 info.txt
cut -d: -f '1 2' info.txt
cut -d'.' -f '1 2' info.csv
```
5) утилита grep предназначена для поиска строк, соответствующих шаблону. Фишка грепа в возможности поиска совпадений внутри множества файлов. Для этого используется параметр -r — рекурсивный поиск в каталогах
```
grep '57.177.171.24' access.log
grep -r 'www-data' /etc/
grep -n '57.177.171.24' access.log
grep -rn 'www-data' /etc/
grep -rni --include="*.log" "57.177.171.24" ./
```
Grep позволяет искать и более сложные вещи, чем одно вхождение, например, мы можем найти несколько разных вхождений
```
grep -rn --include="*.log" "57.177.171.24\|133.6.196.192" ./
```
помимо всего этого греп поддерживает регулярные выражения (базовые и расширенные) - шаблон(маска) поиска.
Для чего применяют регулярные выражения?
- Удалить все файлы, начинающиеся на test (чистим за собой тестовые данные)
- Найти все логи
- grep-нуть логи
- Найти все даты

Основы regex
```
^ -  начало строки
$ - конец строки
. - любой символ
[abc] - набор символов (соответствует любому одному)
* - любое число повторений символа перед
```
давайте рассмотрим примеры
```
ls /bin | grep '.zip'
ls /bin | grep '^zip'
ls /bin | grep 'zip$'
ls /bin | grep -e '^zip$'
ls /bin | grep '[bg]zip'
ls /bin | grep -E '^(bz|gz|zip)'

grep -Ev '^\([0-9]{3}\) [0-9]{3}-[0-9]{4}$' phones.txt
```
6) awk - в некотором смысле язык программирования для обработки текстовых данных; умеет:
- Объявлять переменные для хранения данных.
- Использовать арифметические и строковые операторы для работы с данными.
- Использовать структурные элементы и управляющие конструкции языка, такие, как оператор if-then и циклы, что позволяет реализовать сложные алгоритмы обработки данных.
- Создавать форматированные отчёты.
```
awk '{print $1}' access.log

awk -F: '{print $1}' /etc/passwd

awk 'BEGIN {print "The File Contents IPs:"}
{print $1}
END {print "End of File"}'
access.log

awk  -f awk_script /etc/passwd
```
7) sed
```
echo "This is a test" | sed 's/test/another test/'
```
