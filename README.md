# Minitalk

> 🛰️ A simple message transmitter using only UNIX signals — because printf was too easy.

## 📌 Project Overview

**Minitalk** is a 42 Common Core project that challenges you to implement inter-process communication in C using only **UNIX signals**. You create a `server` and a `client` where the client sends a string to the server — bit by bit — and the server prints it to the terminal.

The main limitation? You’re only allowed to use `SIGUSR1` and `SIGUSR2`. No sockets, no pipes, no shared memory — just signals.

## 🛠️ How It Works

* The **server** starts first and displays its **PID**.
* The **client** takes the **server PID** and a **string** as arguments.
* The client encodes each character into binary and sends it to the server, one bit at a time, using `SIGUSR1` and `SIGUSR2`.
* The server reconstructs the characters and prints them as they arrive.

## ✨ Allowed Functions

* `write`, `malloc`, `free`, `getpid`, `kill`, `pause`, `usleep`, `signal`, `sigaction`, `exit`, `ft_printf` (or your own)

---

## 🧠 What I Learned

### Bit Manipulation

This project helped me understand how data is represented in binary. I practiced bitwise operations to extract and reconstruct characters bit by bit.

### UNIX Signals

I learned how UNIX signals work, especially:

* How to use `kill()` to send a signal to a specific PID.
* How to define a signal handler using `sigaction()`.
* The limitations of signal queuing on Linux (especially when sending signals very quickly).

### Process Communication

I experienced the challenges of synchronizing two processes that don’t share memory or data structures. Timing and signal delivery order became very important.

### Signal Safety & Limits

I had to implement basic flow control to ensure no bits were lost or misread — especially since UNIX signals aren’t queued reliably.

---

## 😓 Challenges I Faced

### Timing and Speed

* If signals were sent too quickly, the server would miss some bits. I had to tune the timing using `usleep()` carefully to slow down the client just enough without being inefficient.

### Signal Handling

* Setting up `sigaction` with proper `sigemptyset` and `SA_SIGINFO` was tricky at first.
* Making the handler non-blocking and safe was essential to avoid undefined behavior.

### Multi-client Handling

* Ensuring the server could process multiple client messages in a row — without restarting — required me to manage state carefully and reset buffers correctly.

---

## 🚀 Bonus Features

* ✅ Acknowledgement signals from server to client for every received character.
* ✅ Unicode support (sending multibyte characters).
* ✅ Cleaner handling of multiple clients simultaneously.

---

## 🧪 How to Run

1. Compile the project:

```bash
make
```

2. Run the server:

```bash
./server
```

It will print something like:

```
Server PID: 12345
```

3. In a separate terminal, run the client:

```bash
./client 12345 "Hello, signals!"
```

The server should print:

```
Hello, signals!
```

---

## 🏁 Final Thoughts

Minitalk was a great low-level project. It felt simple on paper but forced me to think deeply about process communication, signal safety, and bit-level data handling. Debugging signal-based bugs was also a great learning experience in patience and precision.

> It’s kind of amazing to realize you can send a message using just `kill()`.

