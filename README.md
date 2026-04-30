# Лабораторная работа 2(изучение систем контроля версий на примере git)

Цель работы работа с репозиториями/ветками и управление файлами cpp обновление и изменение коммитов

## Подготовка

Для начала(мы это уже делали) нужно настроить имя и имейл гитхаба
```shell
git config --global user.name "qNetayS"
git config --global user.email "-"
```

## Part1

### 1.Создать пустой репозиторий с лицензий MIT
создадим через терминал линукса
```shell
gh auth login
gh repo create my-lab02 --public --clone --add-readme --license MIT
```
### 2.Вполнить инструкцию по созданию первого коммита
инструкция по созданию первого коммита сосотоит из нескольких комманд
```shell
echo "#lab02" >> README.md
git init 
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/qNetayS/lab02.git
git push -u origin master
```
### 3.Создание hello_world.cpp с плохим стилем 
напишем команду создания файла а после код внутри файла
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
```

### 4-5.Добавление файла в индекс и создание коммита
данный пункт выполняется простой командой 
```shell
git add hello_world_cpp
```
коммитим данное действие
```shell
git commit -m "add hello_world.cpp"
```

### 6.Замена кода
заменим код на код содержащий переменную имени и вывод её с осмысленным сообщением
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
### 7.коммит без git add
для этого используем коммит с флагом -am
```shell
git commit -am "add um input"
```

### 8.запушим изменения в удаленный репозиторий
```shell
git push
```

### 9.проверка истории
в данном пункте нужно проверить все ли действия отобразились на странице гитхаба 
Результат:все действия отобразились 

## Part2
В данной часте отображается работа с ветками 

### 1-2.Создайте локальную ветку patch1 и написание туда код без usingnamespace std


```shell
git checkout -b patch1
```
редактируем hello_world.cpp

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

### 3.commit push ветки patch1
```shell
git commit -am "fix codestyle"
git push -u origin patch1
```
### 4-5 проверкадоступа ветки в удаленном репозитории и создания пул реквеста
для первого пункта перключаем ветку master -> patch1
для второго пункта создаем new pull rewuest во вкладке pull request
при создании используем master <- patch1 и  create pull request
добавляем описание
результаты можно увидеть на моем гитхабе в публичном репозитории

### 6.В локальной ветке patch1 добавить комментарии 
для этого изменим hello_world.cpp добавить комментарии
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
### 7.commit ,push в ту же ветку 
для этго используем команды 
```shell
git commit -am "add comm"
git push
```

### 8-9.проверка и выполняем слияние
открываем pull request и файлы file changed и старый и новый код должны быть видны
на странице пул реквеста нажимаем merge pull request и confirm branch
delete branch

### 10.локально выполните pull  в ветке mster 
переходим в мастер и пишем pull

```shell
git checkout master
git pull
```

### 11.просмотреть историю git log
пишем в строку 
```shell
git log --oneline --graph --all
```
результат:
a751b-- (head ->master,origin/master_merge pull request #1 from qNetayS/patch1
и т.д

### 12.удалить локальную ветку patch1

удаление выполняется через команду:
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

### 5-6.Проверка наличия конфликтов в pul request и исправление конфликтов
переходим в ветку пул реквестов patch2 ->master и оно показывает надпись невозможности автоматического соединения
для решения данной проблемы мы используем связку pull + rebase 
```shell
git checkout patch2
git fetch origin
git rebase origin/master
```
далее после невозможности кофликта заходим в папку hello_world.cpp и убираем все ====
и >>>>

после правок 
```shell
git add hello_world.cpp
git rebase --continue
```

### 7-9 сделать force push в ветку patch2 проверка на наличие конфликто и слияние пулреквестов 
для первого пункта используем стандартную функцию push
```shell
git push --force-with-lease origin patch2
```
мы успешно пропушилив нашу ветку нужные обновления 
далее для 8 пункта мы проверим конфликты - конфликты пропали
Выполним слияние как мы это делали во второй части через merge pull request
результат:слияние выполнено

    

