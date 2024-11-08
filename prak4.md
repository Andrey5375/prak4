### Задача 1
На сайте https://onlywei.github.io/explain-git-with-d3 или http://git-school.github.io/visualizing-git/ (цвета могут отличаться, есть команды undo/redo) с помощью команд эмулятора git получить следующее состояние проекта (сливаем master с first, перебазируем second на master): см. картинку ниже. Прислать свою картинку.
![alt text](image.png)

```bash
git commit
git tag in
git branch first
git branch second
git commit
git commit
git checkout first
git commit
git commit
git checkout master
git merge first
git checkout second
git commit
git commit
git rebase master
git checkout master
git merge second
git checkout in
```


### Задача 2
```bash

User@DESKTOP-OBLBJ4I MINGW64 ~
$ git init my_project
Reinitialized existing Git repository in C:/Users/Ксения/my_project/.git/

User@DESKTOP-OBLBJ4I MINGW64 ~
$ git init my_pr
Initialized empty Git repository in C:/Users/Ксения/my_pr/.git/

User@DESKTOP-OBLBJ4I MINGW64 ~
$ cd my_pr

User@DESKTOP-OBLBJ4I MINGW64 ~/my_pr (master)
$ nano prog.py

User@DESKTOP-OBLBJ4I MINGW64 ~/my_pr (master)
$ git add prog.py
warning: in the working copy of 'prog.py', LF will be replaced by CRLF the next time Git touches it

User@DESKTOP-OBLBJ4I MINGW64 ~/my_pr (master)
$ git commit -m "new: добавлен файл"
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'User@DESKTOP-OBLBJ4I.(none)')

User@DESKTOP-OBLBJ4I MINGW64 ~/my_pr (master)
$ git config user.name "Coder 1"

User@DESKTOP-OBLBJ4I MINGW64 ~/my_pr (master)
$ git config user.email "coder1@corp.com"

User@DESKTOP-OBLBJ4I MINGW64 ~/my_pr (master)
$ git add prog.py

User@DESKTOP-OBLBJ4I MINGW64 ~/my_pr (master)
$ git commit -m "new: добавлен файл"
[master (root-commit) 4528e96] new: добавлен файл
 1 file changed, 1 insertion(+)
 create mode 100644 prog.py

User@DESKTOP-OBLBJ4I MINGW64 ~/my_pr (master)
$ git log
commit 4528e96a05116aa33e67a306f6268859a57a55d2 (HEAD -> master)
Author: Coder 1 <coder1@corp.com>
Date:   Fri Nov 8 10:44:31 2024 +0300

    new: добавлен файл

```

![image](https://github.com/user-attachments/assets/5dab3729-9b20-45fd-9c5e-ad2a82fcb144)



## Задача 3


```bash
git init
git config user.name "coder1"
git config user.email "coder1@mirea.ru"
echo print("Hello, Mirea!") > prog.py
git add prog.py
git commit -m "first commit"
git status
git log

cd D:\mygitrepo
git init --bare server

git remote add server D:\mygitrepo\server
git remote -v

git push server master

git clone D:\mygitrepo\server D:\mygitrepo\client
cd D:\mygitrepo\client
git config user.name "coder2"
git config user.email "coder2@mirea.ru"

echo "Author Information:" > readme.md
git add readme.md
git commit -m "docs"

git remote rename origin server

git push server master

cd D:\mygitrepo
git pull server master

echo "Author: coder1" >> readme.md
git add readme.md
git commit -m "coder1 info"
git push server master

cd D:\mygitrepo\client
echo "Author: coder2" >> readme.md
git add readme.md
git commit -m "coder2 info"
git push server master

git pull server master

git add readme.md
git commit -m "readme fix"
git push server master

cd ..
cd server
git log -n 5 --graph --decorate --all
```

## Результат:

![image](https://github.com/user-attachments/assets/8427230a-a91f-40d7-8292-d6450180827d)

![image](https://github.com/user-attachments/assets/11b0ad91-a9d6-4885-a3e1-efa510addc7c)

```bash
C:\mygitrepo\server>git log -n 5 --graph --decorate --all
*   commit 0144a7a9873b5dbbe1c1116d094b3a8ee53fb81b (HEAD -> master)
|\  Merge: 1c6d5a4 65523b9
| | Author: coder2 <coder2@mirea.ru>
| | Date:   Thu Nov 7 15:24:20 2024 +0300
| |
| |     readme fix
| |
| * commit 65523b9d3feb31031eba117fd82076704db8cc60
| | Author: coder1 <coder1@mirea.ru>
| | Date:   Thu Nov 7 15:20:47 2024 +0300
| |
| |     coder1 info
| |
* | commit 1c6d5a465c39ad35407482b7759235cff6885902
|/  Author: coder2 <coder2@mirea.ru>
|   Date:   Thu Nov 7 15:22:36 2024 +0300
|
|       coder2 info
|
* commit 3f778482c576ef449cfac571c21ddeb620af5817
| Author: coder2 <coder2@mirea.ru>
| Date:   Thu Nov 7 15:17:50 2024 +0300
|
|     docs
|
* commit 44ef3379e63cd30e4f05b6023b2e07a6a15d7fa3
  Author: coder1 <coder1@mirea.ru>
  Date:   Thu Nov 7 15:04:14 2024 +0300

      first commit

D:\mygitrepo\server>
```

## Задача 4

Написать программу на Питоне (или другом ЯП), которая выводит список содержимого всех объектов репозитория. Воспользоваться командой "git cat-file -p". Идеальное решение – не использовать иных сторонних команд и библиотек для работы с git.

## Решение:

```python
import subprocess


def get_git_objects():
    # Получаем список всех объектов в репозитории
    try:
        # Выполняем команду 'git rev-list --all' для получения всех хешей коммитов
        commits = subprocess.check_output(['git', 'rev-list', '--all']).decode('utf-8').splitlines()

        # Для каждого коммита получаем содержимое объекта
        for commit in commits:
            print(f'Contents of commit {commit}:')
            try:
                # Используем 'git cat-file -p' для получения содержимого
                content = subprocess.check_output(['git', 'cat-file', '-p', commit]).decode('utf-8')
                print(content)
            except subprocess.CalledProcessError as e:
                print(f'Error retrieving object {commit}: {e}')
            print('-' * 40)
    except subprocess.CalledProcessError as e:
        print(f'Error retrieving commits: {e}')


if __name__ == '__main__':
    get_git_objects()
```

## Результат:

![image](https://github.com/user-attachments/assets/0853f136-068b-4181-933d-d1d646f5b453)

