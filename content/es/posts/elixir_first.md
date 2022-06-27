+++
title = "Hello Elixir!"
description = "Primer acercamiento a Elixir"
date = "2018-03-03T09:13:50-06:00"
categories = ['elixir']
tags = ['programming']
+++

Recientemente he estado conociendo el lenguaje Elixir, un lenguaje que aprovecha la máquina virtual de Erlang, una plataforma conocida por soportar sistemas en baja latencia, sistemas distribuidos y tolerantes a fallas.

Para familiarizarme con el lenguaje he optado por resolver algunos ejercicios, un par de ellos son el Fizz Buzz y Guess My Number.

## Fizz Buzz
``` swift
// My Fizzbuzz Solution in Swift

for i in 1...50 {
  if i%5==0 && i%3==0 {
    print("Fizzbuzz 🍻 ")
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

Normalmente suelo resolver este ejercicio con una iteración, y el siempre confiable y útil *IF*, sin embargo, Elixir al ser un lenguaje funcional obliga a la implementación de otra solución bajo términos funcionales, aquí el primer reto.

La forma natural en la pienso el cómo resolver esto es:

 1. Obtener una lista de números para iterar
 2. Evaluar cada número en los posibles casos del FizzBuzz

## Primer Acercamiento con Elixir
El primer alcance que pude lograr fue el obtener una lista de números a partir de un número, el número mayor en esta lista.

``` elixir
defmodule Learning do
  def makeListUntil(1), do: [1]
  def makeListUntil(n), do: makeListUntil(n-1) ++ [ n ]
end
```

![](/blog/blog/elixir/fizzbuzz.png)

La principal dificultad de esto fue el concebir la idea del tener más de una función que se llama a sí misma, y la idea de que tener casos específicos de esta función como el caso de `def makeListUntil(1), do: [1]` necesario para tener un tope en las llamadas recursivas al invocar la función. En lo personal resultó complicado e interesante el hecho de ir armando una lista de esta forma.

Esto me ayudó para implementar la distinción de casos del fizz buzz, para lo cuál realice varios casos de una misma función:

``` elixir

def eval(0), do: [0]

def eval(elem) when rem(elem,5)==0 and rem(elem,3)==0, do: :fizzbuzz

def eval(elem) when rem(elem,3)==0, do: [:fizz]

def eval(elem) when rem(elem,5)==0, do: [:buzz]

def eval(elem), do: elem
```


Al pasar un solo número con esto podía distinguir si era un fizz, un buzz, un fizz buzz, o cero.

El siguiente pasó fue implementar la lista, de modo que al pasar un número N, obtuviera una lista desde 0 hasta N, con la implementación de fizz buzz.

Este paso resultó también el que más tiempo me llevó, probablemente debido a ser uno de mis primeros acercamientos al lenguaje.

``` elixir
defmodule Learning do

  def eval(0), do: [0]

  def eval(elem) when rem(elem,5)==0 and rem(elem,3)==0, do: eval(elem-1) ++[:fizzbuzz]

  def eval(elem) when rem(elem,3)==0, do: eval(elem-1) ++ [:fizz]

  def eval(elem) when rem(elem,5)==0, do: eval(elem-1) ++ [:buzz]

  def eval(elem), do: eval(elem-1) ++ [elem]

end
```


Esta única función recibe un número, el cuál es evaluado por casos según el fizz buzz, e  inicia una lista, enviando el número menos uno a la misma función.  En la ejecución podemos ver que el número pasa de forma descendente por las funciones escritas.

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
Ya con ayuda de @neodevelop, pudimos encontrar una segunda solución a este ejercicio usando más elementos del lenguaje.

Este acercamiento trataba directamente a una lista de números, a los cuales se aplicaba una función, que debería retornar una función con la solución correcta. Aquí hicimos uso de las tuplas, para llamar a una función `fb_categorize`  que nos regresaría un elemento con el que iríamos formando la lista con la solución.

``` elixir

def applyFizzbuzz([], fb_list), do: fb_list

def applyFizzbuzz([head|tail], fb_list) do
    item = fb_categorize({ rem(head,3)==0, rem(head,5)==0, {head} })
    applyFizzbuzz( tail, fb_list ++ [item] )
end
```


Para ello nuestra función `fb_categorize` al recibir una tupla de elementos booleanos podríamos implementarla tantas veces como combinaciones necesarias.

```
  def fb_categorize({true, false, _}), do: :fizz
  def fb_categorize({false, true, _}), do: :buzz
  def fb_categorize({true, true, _}), do: :fizzbuzz
  def fb_categorize({_, _, number}), do: number
```


Finalmente para concluir nuestro ejercicio hicimos uso del pipe de elixir para primero crear una lista de número a partir de un número, y luego aplicar el fizz buzz, y hacer nuestro código más legible:

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

Al comprender estos elementos del lenguaje pude resolver otro ejercicio “Guess my number”, del cuál platicaré en otro post.

Gracias por leer.
