# -02
# Лабораторная работа 2

## Цель работы 
Изучение систем контроля версий на примере Git

## Подготовка

Сначала нужно настроить login и email гитхаба
```shell
git config --global user.name "Tagir-iu"
git config --global user.email "-"
```

## Part1

### 1.Создать пустой репозиторий с лицензий MIT
Cоздадим через терминал линукса
```shell
gh auth login
gh repo create -02 --public --clone --add-readme --license MIT
```
### 2.Выполнить инструкцию по созданию первого коммита
Инструкция по созданию первого коммита сосотоит из нескольких комманд
```shell
echo "#lab02" >> README.md
git init 
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin "//github.com/Tagir-iu/lab02.git"
git push -u origin master
```
### 3.Создание hello_world.cpp с плохим стилем 
Напишем команду создания файла, а после код внутри файла
```shell
cat > hello_world << "EOF"
#include <iostream>
using namespace std;
int main(){
    cout << "Hello world" << endl;
    return 0;
}
EOF
```
### 4-5.Добавление файла в индекс и создание коммита
Данный пункт выполняется простой командой 
```shell
git add hello_world_cpp
```
Коммитим данное действие
```shell
git commit -m "add hello_world.cpp"
```
### 6.Замена кода
заменим код на код содержащий, переменную имени и вывод её с осмысленным сообщением
```shell
cat > hello_world.cpp << "EOF"
#include <isotream>
#include <string>
using namespace std;
int main(){
    string name;
    cout << " your name";
    cin >> name;
    cout << "hello world from" << name << endl;
    return 0;
}
EOF
```
### 7.Коммит без git add
Для этого используем коммит с флагом -am
```shell
git commit -am "add um input"
```
### 8.Запушим изменения в удаленный репозиторий
```shell
git push
```
### 9.Проверка истории
В данном пункте нужно проверить все ли действия отобразились на странице гитхаба 
Результат: все действия отобразились 

## Part2
В данной части отображается работа с ветками 
### 1-2.Создание локальной ветки patch1 и написание туда кода без usingnamespace std
```shell
git checkout -b patch1
```
Редактируем hello_world.cpp
```shell
cat > hello_world.cpp << "EOF"
#include <iostream>
#include <string>
int main(){
    std::string name;
    std::cout << "your name";
    std::cin >> name;
    std::cout << "hello world from" << name << std::endl;
    return 0;
}
EOF
```
### 3.Commit push ветки patch1
```shell
git commit -am "fix codestyle"
git push -u origin patch1
```
### 4-5 Проверка доступа ветки в удаленном репозитории и создания пул реквеста
Для первого пункта перключаем ветку master -> patch1
Для второго пункта создаем new pull request во вкладке pull request
При создании используем master <- patch1 и  create pull request
Добавляем описание
Результаты можно увидеть на моем гитхабе в публичном репозитории

### 6.В локальной ветке patch1 добавить комментарии 
Для этого изменим hello_world.cpp добавить комментарии
```shell
cat > hello_world.cpp << "EOF"
#include <iostream>
#include <string>
//main func
int main(){
    std::string name; // my name
    std::cout << "your name";
    std::cin >> name;
    //output name with another info
    std::cout << "hello world from " << name << std::endl;
    return 0;
}
EOF
```
### 7.commit, push в ту же ветку 
Для этого используем команды 
```shell
git commit -am "add comm"
git push
```
### 8-9.Проверяем и выполняем слияние
Открываем pull request и файлы file changed, старый и новый код должны быть видны
на странице пул реквеста. Нажимаем merge pull request и confirm branch
delete branch
### 10.Локально выполните pull  в ветке mster 
переходим в мастер и пишем pull

```shell
git checkout master
git pull
```

### 11.Просмотреть историю git log
пишем в строку 
```shell
git log --oneline --graph --all
```
результат:
a751b-- (head ->master,origin/master_merge pull request #1 from Tagir-iu/patch1
и т.д
### 12.Удалить локальную ветку patch1
Удаление выполняется через команду:
```shell
git branch -d patch1
```
## Part3
### 1.Создаем новую локальную ветку patch2 от master
```shell
git checkout master
git checkout -b patch2
```
### 2.изменение кодстайла с помощью clang-format -style=Mozilla
для начала скачаем clang-format
```shell
sudo apt install clang-format
```
после этого меняем кодстал через данную библиотеку
```shell
clang-format -style=Mozilla -i hello_world.cpp
```
### 3.commit,push создайте pull-request patch2 ->master
```shell
git commit -am "apply mozilla code style
git push -u origin patch2
```
### 4.изменение в удаленной ветке комментариев
через терминал это можно выполнить командой commit -am
```shell
git checkout master
git commit -am "change language to rus"
git push
```
### 5-6.Проверка наличия конфликтов в pull request и исправление конфликтов
Переходим в ветку пул реквестов patch2 ->master и оно показывает надпись невозможности автоматического соединения
Для решения данной проблемы мы используем связку pull + rebase 
```shell
git checkout patch2
git fetch origin
git rebase origin/master
```
Далее после невозможности кофликта заходим в папку hello_world.cpp и убираем все ====
и >>>>
После правок 
```shell
git add hello_world.cpp
git rebase --continue
```
### 7-9 сделать force push в ветку patch2 проверка на наличие конфликтов и слияние пулреквестов 
Для первого пункта используем стандартную функцию push
```shell
git push --force-with-lease origin patch2
```
Мы успешно пропушили в нашу ветку нужные обновления 
далее для 8 пункта мы проверим конфликты:Конфликтов нет
Выполним слияние как мы это делали во второй части через merge pull request
Результат: слияние выполнено
