---
title: Разбираемся с регулярными выражениями в Python
layout: default
---

Здесь будет проводиться разбор модуля RE. Я пользовался в основном findall, поэтому начнем с неё 
(не понятно, что значит flags - параметр (я им никогда не пользовался))

def findall(pattern, string, flags=0):
    return _compile(pattern, flags).findall(string)
    
Значит самый ключевой метод это _compile. Начнем разбирать его. Он представляет собой простую обертку, а именно:

Имеется dictionary cache(pattern type, 'patern', flags) -> (compiled pattern, locale)
этот метод проверяет кэш на наличие данного паттерна, в случае удачи его возвращает, иначе компилирует и возращает экземпляр.
Значит основной проброс выполнения идет в следующей строке - sre_compile.compile(patern, flags) - основная цимес тут.

sre_compile.compile состоит из следующих этапов:

1)  Парсинг (разбор) паттерна при помощи sre_parse.parse(p, flags)
2)  Генерация кода при помощи code = _code(p, flags)
3)  Компиляция кода??? _sre.compile(???)


    
