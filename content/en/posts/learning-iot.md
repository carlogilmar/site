+++
title = "Learning IoT: First Steps with Elixir"
tags = ["elixir"]
categories = ["programming", "phoenix", "iot"]
date = "2018-09-12"
+++

![](/blog/blog/iot/uno.png)

Recently I have the opportunity to give a conference in the [Startup Week in Mexico City](https://techstarsstartupweekmexicoc2018.sched.com/event/G1Kh/internet-of-things-primeros-pasos). So I decided talk about Internet of things, because I was learning this using an **Arduino** and a **Raspberry Pi**.

### [DEMO](https://www.youtube.com/watch?v=me1TFfHmpPM&feature=youtu.be)

## Connect Everything!

The project is create a simple web application for create distribuid music. I was inspired in the **Launchpad App** and the [Smartphone Symphony.](https://www.youtube.com/watch?v=lEMG7ZayU48)

I planned make this using **Phoenix Framework**, and connect an **Arduino** for play the music.

## The Arduino

I use the arduino for built a circuit based on receipt and send signals using the serial port. This means that, for example, I send a message when I press a key from the membrane switch, or when the photo-resistance validate the light intensity.

And when the circuit receives a signal, they could do something: turn on leds, etc. I used the **Arduino IDE** for test this circuit, for send and receive signals.

![](/blog/blog/iot/dos.png)

## The Raspberry-Pi

Although the **Raspberry Pi** have a **GPIO**, I think that is more convenient use with the **Arduino**, because It has more usability (analog pins, more digital pins). The greatest part about Raspberry is that you could have an entire SO!

So I used the **Raspbian SO** for get a **Linux SO**. When you have a Linux you know that impossible is nothing.

## Elixir

I decided to choose **elixir** for programming over the Raspberry Pi because I was learning elixir, and this could be a great opportunity for improve my skills using an embedded systems.

### Installing Elixir

Raspbian SO have a simple way to install elixir, although you will have the version 1.3 I used other package installer ASD for install the most recent version.

[ASD GitHub Repository](https://github.com/asdf-vm/asdf)

## Phoenix App

#### GenServer implementation for get a sound

The web app has planned for assign a different sound per user. For this I used the Channels for create sockets connections and I created a simple gen server for get the sound.

``` Ruby
defmodule Chatter.Director do

  use GenServer

	# The sounds
  @songs [
    %{"song" => "http://carlogilmar.me/uno.m4a", "category" => "A"},
    %{"song" => "http://carlogilmar.me/dos.m4a", "category" => "B"},
    %{"song" => "http://carlogilmar.me/tres.m4a", "category" => "C"},
    %{"song" => "http://carlogilmar.me/cuatro.m4a", "category" => "D"},
    %{"song" => "http://carlogilmar.me/cinco.m4a", "category" => "E"},
    %{"song" => "http://carlogilmar.me/seis.m4a", "category" => "F"},
    %{"song" => "http://carlogilmar.me/siete.m4a", "category" => "G"},
    %{"song" => "http://carlogilmar.me/ocho.m4a", "category" => "H"}
  ]

  def start_link() do
    GenServer.start_link(__MODULE__, [], [name: __MODULE__])
  end

	# I have to call this function for get a sound
  def suscribe() do
    GenServer.call( __MODULE__, :suscribe)
  end

  def init(_) do
		# Start the counter in 0 for serve the first sound
    {:ok, {:counter, 0} }
  end

  def handle_call( :suscribe, _, {:counter, counter} ) do
		# I serve the song and update the counter
    { counter, song, next_counter } = suscribe_song( counter )
    {:reply, {counter, song}, {:counter, next_counter}}
  end

	# The counter cannot be more greater than 7

  def suscribe_song( 8 ) do
    [ first_song | _ ] = @songs
    { 0, first_song, 1 }
  end

  def suscribe_song( current_counter ) do
    song = Enum.at( @songs, current_counter)
    { current_counter, song, current_counter + 1 }
  end

end
```

![](/blog/blog/iot/tres.png)

#### Getting a sound per user connected

I used the channels because It has a function handle_join when somebody create a new connection, so it’s easy identify this moment.

```
  def join("room:lobby", _, socket) do
    Logger.info ":: Join to room:lobby ::", ansi_color: :green
    { _, current_user} = Director.suscribe() # I get the sound and send to the client
    {:ok, current_user, socket}
  end
```

For know more about Sockets in Phoenix you could visit my [last post](http://carlogilmar.me/post/phoenix-sockets/)

#### Client side

![](/blog/blog/iot/seis.png)

For play and stop music I implement the Howler JS library in the client side from Phoenix. When a user is getting connected, the app assign a sound.

Using the broadcast messaging I could send strings, and in the client side I play different actions:

  - **Start**: Start to play every sound in every user connection
  - **Play**: Put the volume in 1.
  - **Stop**: Put the volume in 0.
  - **Reload**: Reload the browser.
  - **Current Letter**: Play only a sound.

##### NOTE: I have one trouble using Phoenix in the Raspberry Pi, that was installing Node JS.

## Connect the Arduino and the Raspberry Pi: Nerves UART

So, at this point I have a simple circuit running on the **Arduino**, and a **Phoenix Web App**. The next step is connect both. For this purpose I used [Nerves UART Projet](https://github.com/nerves-project/nerves_uart).

![](/blog/blog/iot/cuatro.png)

For understand how it works, read the documentation is enough.

#### Nerves UART
```
iex> Nerves.UART.enumerate
%{"COM14" => %{description: "USB Serial Port", manufacturer: "FTDI", product_id: 24577,
    vendor_id: 1027},
  "COM5" => %{description: "Prolific USB-to-Serial Comm Port",
    manufacturer: "Prolific", product_id: 8963, vendor_id: 1659},
  "COM16" => %{description: "Arduino Uno",
    manufacturer: "Arduino LLC (www.arduino.cc)", product_id: 67, vendor_id: 9025}}

iex> {:ok, pid} = Nerves.UART.start_link
{:ok, #PID<0.132.0>}

iex> Nerves.UART.open(pid, "COM14", speed: 115200, active: false)
:ok

iex> Nerves.UART.write(pid, "Hello there\r\n")
:ok

iex> Nerves.UART.read(pid, 60000)
{:ok, "Hi"}
```

**Nerves UART** help to build the communication bridge between the arduino and the elixir app.

##### NOTE: I have some troubles connecting the arduino with my MacOS. So I tested it directly in the raspberry pi and It works.

So, the interesting part about this was implement the **UART** with the **Phoenix app**. For this I implemented a **GenServer** for get the port pidm.

#### UART GenServer

I created a GenServer for get the pidm from the opened port, so in the **init** function I open the port and keep the port as the state. With this pidm I could send messages to this port using the UART write function.

```
def init(_) do
	{:ok, uart_pid} = Nerves.UART.start_link
	port = Nerves.UART.open( uart_pid, "ttyACM0", speed: 9600, active: false)
	Nerves.UART.configure(uart_pid, framing: {Nerves.UART.Framing.Line, separator: "\r\n"})
	loop()
	{:ok, uart_pid}
end


def handle_call( :get_uart, _, state ) do
	# state is the uart_pid
	{:reply, state, state}
end
```

#### Read the Serial Port with UART

The main idea is send messages, as strings, from the arduino to the phoenix app. For make this possible I had to read the serial port for receive the signals, and send as broadcast to every socket connection.

For this I created a **loop function** for read every second the serial port, and send the signal as broadcast to the channels.

```
 defp loop() do
   send self(), :loop
 end

def handle_info(:loop, state) do
  {:ok, message_from_arduino} = Nerves.UART.read( state, 1000 )
  RoomChannel.send_broadcast( message_from_arduino )
  loop()
  {:noreply, state}
end
```

With all of this I could connect everything and make the little demo. I built a simple device connecting all of this as a example of how IoT works.

![](/blog/blog/iot/cinco.png)

## Bonus: Telegram

I used [Telegram Bot API](https://github.com/rockneurotiko/ex_gram) for connect a Telegram Bot for send messages to the sockets connections.


## Finally

At the end of the day I want to use the **Arduino** for send signals to the web application built with Phoenix. Installing this framework in the Raspberry Pi is so great, because you could implement everything that you know, as an example, I connected Telegram as other device for send messages.

The web app is simple: create distribuited music melody based on 8 sounds from Launchpad.

### Features:

#### Start

- Every user has a different sound.
- When a user enter to the webapp, the arduino start to play the leds.

#### Photo-resistance

- When turn-off the light, all devices start to play their sound.
- When turn-off the light, start to turn-on the leds.
- When turn-on the light, all devices stop their sound.

#### Membrance module

- When touch a letter in the membrance module, all devices with this letter start to play their sound.
- When touch the play button in the membrance module, all devices start to play their sound.
- When touch the stop button in the membrance module, all devices stop their sound.
- When touch the reload button in the membrance module, all devices reload their connection and get a new letter.

#### Telegram

- Send a message for start to play all sounds, stop, reload, and play every sound.

## Thanks for reading!
### Don't forget see the demo!!

### [DEMO](https://www.youtube.com/watch?v=me1TFfHmpPM&feature=youtu.be)
