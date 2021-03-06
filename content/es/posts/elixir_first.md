+++
title = "Hello Elixir!"
description = "Primer acercamiento a Elixir"
date = "2018-03-03T09:13:50-06:00"
categories = ['elixir']
tags = ['programming']
+++

Recientemente he estado conociendo el lenguaje Elixir, un lenguaje que aprovecha la m谩quina virtual de Erlang, una plataforma conocida por soportar sistemas en baja latencia, sistemas distribuidos y tolerantes a fallas.

Para familiarizarme con el lenguaje he optado por resolver algunos ejercicios, un par de ellos son el Fizz Buzz y Guess My Number.

## Fizz Buzz
``` swift
// My Fizzbuzz Solution in Swift

for i in 1...50 {
  if i%5==0 && i%3==0 {
    print("Fizzbuzz 馃嵒 ")
  }
  else if i%5 == 0{
    print("Buzz")
  }
  else if i%3==0{
    print("Fizz")
  }
  else{
    print("\(i)")
  }
}
```

Normalmente suelo resolver este ejercicio con una iteraci贸n, y el siempre confiable y 煤til *IF*, sin embargo, Elixir al ser un lenguaje funcional obliga a la implementaci贸n de otra soluci贸n bajo t茅rminos funcionales, aqu铆 el primer reto.

La forma natural en la pienso el c贸mo resolver esto es:

 1. Obtener una lista de n煤meros para iterar
 2. Evaluar cada n煤mero en los posibles casos del FizzBuzz

## Primer Acercamiento con Elixir
El primer alcance que pude lograr fue el obtener una lista de n煤meros a partir de un n煤mero, el n煤mero mayor en esta lista.

``` elixir
defmodule Learning do
  def makeListUntil(1), do: [1]
  def makeListUntil(n), do: makeListUntil(n-1) ++ [ n ]
end
```

![](/blog/blog/elixir/fizzbuzz.png)

La principal dificultad de esto fue el concebir la idea del tener m谩s de una funci贸n que se llama a s铆 misma, y la idea de que tener casos espec铆ficos de esta funci贸n como el caso de `def makeListUntil(1), do: [1]` necesario para tener un tope en las llamadas recursivas al invocar la funci贸n. En lo personal result贸 complicado e interesante el hecho de ir armando una lista de esta forma.

Esto me ayud贸 para implementar la distinci贸n de casos del fizz buzz, para lo cu谩l realice varios casos de una misma funci贸n:

``` elixir

def eval(0), do: [0]

def eval(elem) when rem(elem,5)==0 and rem(elem,3)==0, do: :fizzbuzz

def eval(elem) when rem(elem,3)==0, do: [:fizz]

def eval(elem) when rem(elem,5)==0, do: [:buzz]

def eval(elem), do: elem
```


Al pasar un solo n煤mero con esto pod铆a distinguir si era un fizz, un buzz, un fizz buzz, o cero.

El siguiente pas贸 fue implementar la lista, de modo que al pasar un n煤mero N, obtuviera una lista desde 0 hasta N, con la implementaci贸n de fizz buzz.

Este paso result贸 tambi茅n el que m谩s tiempo me llev贸, probablemente debido a ser uno de mis primeros acercamientos al lenguaje.

``` elixir
defmodule Learning do

  def eval(0), do: [0]

  def eval(elem) when rem(elem,5)==0 and rem(elem,3)==0, do: eval(elem-1) ++[:fizzbuzz]

  def eval(elem) when rem(elem,3)==0, do: eval(elem-1) ++ [:fizz]

  def eval(elem) when rem(elem,5)==0, do: eval(elem-1) ++ [:buzz]

  def eval(elem), do: eval(elem-1) ++ [elem]

end
```


Esta 煤nica funci贸n recibe un n煤mero, el cu谩l es evaluado por casos seg煤n el fizz buzz, e  inicia una lista, enviando el n煤mero menos uno a la misma funci贸n.  En la ejecuci贸n podemos ver que el n煤mero pasa de forma descendente por las funciones escritas.

```
Erlang/OTP 20 [erts-9.2.1] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

Compiling 2 files (.ex)
Interactive Elixir (1.6.1) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> Learning.eval 10
[0, 1, 2, :fizz, 4, :buzz, :fizz, 7, 8, :fizz, :buzz]
iex(2)> Learning.eval 11
[0, 1, 2, :fizz, 4, :buzz, :fizz, 7, 8, :fizz, :buzz, 11]
iex(3)> Learning.eval 20
[0, 1, 2, :fizz, 4, :buzz, :fizz, 7, 8, :fizz, :buzz, 11, :fizz, 13, 14,
 :fizzbuzz, 16, 17, :fizz, 19, :buzz]
iex(4)> Learning.eval 1
[0, 1]
iex(5)>
```


## Segundo Acercamiento
Ya con ayuda de @neodevelop, pudimos encontrar una segunda soluci贸n a este ejercicio usando m谩s elementos del lenguaje.

Este acercamiento trataba directamente a una lista de n煤meros, a los cuales se aplicaba una funci贸n, que deber铆a retornar una funci贸n con la soluci贸n correcta. Aqu铆 hicimos uso de las tuplas, para llamar a una funci贸n `fb_categorize`  que nos regresar铆a un elemento con el que ir铆amos formando la lista con la soluci贸n.

``` elixir

def applyFizzbuzz([], fb_list), do: fb_list

def applyFizzbuzz([head|tail], fb_list) do
    item = fb_categorize({ rem(head,3)==0, rem(head,5)==0, {head} })
    applyFizzbuzz( tail, fb_list ++ [item] )
end
```


Para ello nuestra funci贸n `fb_categorize` al recibir una tupla de elementos booleanos podr铆amos implementarla tantas veces como combinaciones necesarias.

```
  def fb_categorize({true, false, _}), do: :fizz
  def fb_categorize({false, true, _}), do: :buzz
  def fb_categorize({true, true, _}), do: :fizzbuzz
  def fb_categorize({_, _, number}), do: number
```


Finalmente para concluir nuestro ejercicio hicimos uso del pipe de elixir para primero crear una lista de n煤mero a partir de un n煤mero, y luego aplicar el fizz buzz, y hacer nuestro c贸digo m谩s legible:

```
  def fizzbuzz(number) do
    number
      |> makeListUntil
      |> applyFizzbuzz([])
  end
```

```
Erlang/OTP 20 [erts-9.2.1] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

Interactive Elixir (1.6.1) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> Learning.fizzbuzz 20
[
  {1},
  {2},
  :fizz,
  {4},
  :buzz,
  :fizz,
  {7},
  {8},
  :fizz,
  :buzz,
  {11},
  :fizz,
  {13},
  {14},
  :fizzbuzz,
  {16},
  {17},
  :fizz,
  {19},
  :buzz
]
iex(2)>
```

Al comprender estos elementos del lenguaje pude resolver otro ejercicio 鈥淕uess my number鈥?, del cu谩l platicar茅 en otro post.

Gracias por leer.
