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

1)  Парсинг (разбор) паттерна при помощи sre_parse.parse(p, flags). На данном этапе генерируется последовательность вида
    [(Operand, (data, data, data, ...))]
    
2)  Генерация кода при помощи code = _code(p, flags). Здесь обрабатывается последовательность полученная на предыдущем шаге 
    В итоге получаем просто последовательность опкодов и параметров -> [opcode, param*, opcode, param*, ...]

3)  Компиляция _sre.compile(???). Обработанная последовательность сохраняется *опкод за опкодом в специальный объект,
который представляет собой скомпилированный Pattern. Самая вкуснота состоит в том, что данный модуль _sre расширяет язык Питон
(модуль написан на языке программирования С).

Далее, опишем интерфейс скомпилированного объекта. он имеет тип PatternObject

typedef struct {
    PyObject_VAR_HEAD
    Py_ssize_t groups; /* must be first! */
    PyObject* groupindex; /* dict */
    PyObject* indexgroup; /* tuple */
    /* compatibility */
    PyObject* pattern; /* pattern source (or None) */
    int flags; /* flags used when compiling pattern source */
    PyObject *weakreflist; /* List of weak references */
    int isbytes; /* pattern type (1 - bytes, 0 - string, -1 - None) */
    /* pattern code */
    Py_ssize_t codesize;
    SRE_CODE code[1];
} PatternObject;

static PyMemberDef pattern_members[] = {
    {"pattern",    T_OBJECT,    PAT_OFF(pattern),       READONLY},
    {"flags",      T_INT,       PAT_OFF(flags),         READONLY},
    {"groups",     T_PYSSIZET,  PAT_OFF(groups),        READONLY},
    {NULL}  /* Sentinel */
};


    
