---
title: Илон vs Марк  
author: rodkot
date: 2023-12-25 00:34:00 +0800
categories: [Проекты, IlonMarkFight]
tags: [dotnet, c#, aspnet, trueengineering, nuget]
image:
    path: /assets/img/ilonmarkfight/icon.jpg
    lqip: data:image/webp;base64
---

# 👋 Всем Привет !

На курсе ``"Введение в платформу .NET"`` ребята из [True Engineering](https://www.trueengineering.ru/) предложили
интересную задачу:

``` 
Илон Маск и Марк Цукерберг решили сразиться в поединке в римском Колизее. Боги разгневались на них, схватили и заперли в двух разных комнатах. Они согласны освободить Марка и Илона, если они решат одну задачку.

Есть колода из 36 карт. Боги перемешивают колоду, делят её напополам и отдают одну стопку Илону Маску, а вторую стопку Марку Цукербергу. Затем оба смотрят на свои карты не перемешивая их. Каждый из них должны назвать номер карты в стопке у партнера. Если цвета выбранных карт совпадают, им разрешают сразится иначе, не разрешают.

Илон и Марк, как придвинутые парни, заранее предвидели все эти обстоятельства и имели возможность договориться о стратегиях своего поведения (они могут быть как одинаковы, так и различны для Илона и Марка).
```

## 💪 Моя реализация

Solution `IlonMarkFight` состоит из 7 Projects:

| Projects        | Тип сборки               | Описание                                                                      |
|-----------------|--------------------------|-------------------------------------------------------------------------------|
| ConsoleApp      | Console Application      | Точка входа в приложение, работа песочницы                                    |
| Core            | Class Library            | Реализация библиотечных абстракций для задачи                                 |
| DataLib         | Class Library            | Абстракции библиотеки                                                         |
| DbStorage       | Class Library            | Описание контекста для Entity Framework и Entity для сохранения контекста игр |
| Models          | Class Library            | Модели библиотеки                                                             |
| OpponentsWebApp | ASP.NET Core Application | Web-приложения оппонентов                                                     |    
| Test            | Unit Test Project        | Unit-тестирование программы                                                   |    

### Абстракции
Объединяющая часть сервиса это песочница ``ISandBox``.  Единственный метод интерфейса возвращает выжили ли участники или нет.
```
public interface ISandBox
{
    bool Round(IShuffleableDesk desk);
}
```
Метод ``Round`` принимает на вход колоду, которая реализует ``IShuffleableDesk``

```
public interface IShuffleableDesk : IDesk
{
    void SwapCards(int i, int j)
    {
        if (i >= Cards.Count || j >= Cards.Count) return;
        (Cards[i], Cards[j]) = (Cards[j], Cards[i]);
    }
}
```

Методе ``Round`` колода перемешивается методом ``Shuffle`` из ``IDeskShuffler``
```
public interface IDeskShuffler
{
    void Shuffle(IShuffleableDesk desk);
}
```

Далее колода методом ``IDesk.Split`` разделяется на два листа и передается оппонентам для получения выбранных карт, которые реализуют ``IChooseCard``
```
public interface IChooseCard
{
    Card Choose(IEnumerable<Card> cards);
}
```

И последний этап боя это оценка результата богами, которые реализуют ``IDistributor``
```
public interface IDistributor
{
    bool Judge(Card cardFirstOpponent, Card cardSecondOpponent);
}
```
### Реализации 

В модуле ``Core`` содержаться реализации приведенных абстракций.

## 🚀 Как использовать

1. Посетите мой репозиторий на [GitHub](https://github.com/rodkot/IlonMarkFight).
2. Инструкция по эксплуатации.
3. Поделитесь своим опытом и предложениями в разделе Issues.

# ⚔️ Скоро выйдет пост, в котором я расскажу о самой выгодной стратегии для Илона и Марка