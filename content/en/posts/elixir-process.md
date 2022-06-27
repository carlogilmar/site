+++
title = "Process in Elixir: A Simple Example"
tags = ["elixir"]
categories = ["programming"]
date = "2018-04-02"
+++

In the last days I have been learning about Process in Elixir. After doing some exercises and understand some things about it, I can explain how to create a simple example about process.

## The Process

Work with process in Elixir is so common, so, it’s important know how to use it. For create a process you can use `spawn` (which takes a function).

> The process structure is very simple: you have a mailbox, created within a function and by the word `receive`, and you can manage different messages and do something.  When you create a process with `spawn`, you will get a Process ID, and also you can know if this process is alive.

When you have a process alive, you can send it messages, for do this you only have to send with the process ID and a message (a data structure, a tuple, an atom…) .

![](/blog/blog/process/duno.png)

For make this more clearly I will show how to create an example.

## Ping Pong Example With a Process
We have to create a simple function and create a `receiver`, within this, we are going to use the patter matching for receive different types of messages, in this case we are to receive two atoms: `:ping`and `:pong`. When we receive this, we will print a message.

![](/blog/blog/process/ddos.png)

``` elixir
defmodule Pingpong do

  def mailbox() do
    receive do
      {:ping} ->
        IO.puts " Ping !!"
      {:pong} ->
        IO.puts " Pong !!"
    end
  end

end
```

If we run the `iex`shell we can create a process with this function and send it a messages for print the `ping`and `pong`:

![](/blog/blog/process/uno.png)

You will see that only in the first message sent we can see the printed message

## Process Alive
This happens because the process created was killed after receive the first message. We can know if the process is alive in the `iex`shell:

![](/blog/blog/process/dos.png)

The solution for avoid this is call the same function in the receiver, with this the process won’t be killed after receive a message.

``` elixir
  def mailbox() do
    receive do
      {:ping} ->
        IO.puts " Ping !!"
	mailbox()
      {:pong} ->
        IO.puts " Pong !!"
	mailbox()
    end
  end
```

We are sending a message to a process alive. Now I want that this process send messages to the same process for do the `ping-pong`.

![](/blog/blog/process/tres.png)

## Ping Pong in the same Process

My first try is that when the process receive a message, send the other message to the same process.

![](/blog/blog/process/dtres.png)

For this we only have to include the process ID in the tuple, and make a send with the other message.

I’m going to print the process id in the function using `self()` .

```
  def mailbox() do
    IO.inspect self()
    receive do
      {:ping, pid} ->
        :timer.sleep(500)
        IO.puts " Ping !!"
        send(pid, {:pong, pid})
        mailbox()

      {:pong, pid} ->
        :timer.sleep(500)
        IO.puts " Pong !!"
        send(pid, {:ping, pid})
        mailbox()
    end
  end
```

![](/blog/blog/process/cuatro.png)

## Ping Pong with Two Process
Now we have to create the `ping-pong` example with two process.

![](/blog/blog/process/dcuatro.png)

For do this we can create two process of the same function.  Instead of get a single process id in the `receiver`we can receive two process Id.

```
  def mailbox() do
    IO.inspect self()
    receive do
      {:ping, process1, process2} ->
        :timer.sleep(500)
        IO.puts " Ping !!"
        send(process2, {:pong, process1, process2})
        mailbox()

      {:pong, process1, process2} ->
        :timer.sleep(500)
        IO.puts " Pong !!"
        send(process1, {:ping, process1, process2})
        mailbox()
    end
  end
```

We can see which process are print the `ping`or `pong` message.

![](/blog/blog/process/cinco.png)

#### With simple exercise I understand how to create process and send messages
##### It's important because Elixir use it a lot!
### Thanks for reading!
