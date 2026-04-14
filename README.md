# 📡 minitalk

## 📌 Overview

`minitalk` is a simple client-server communication program built entirely using UNIX signals.  

The goal of this project is to implement a **reliable data transmission system between processes** using only `SIGUSR1` and `SIGUSR2`, without sockets or pipes.  

It explores how binary data can be encoded and reconstructed through low-level process signaling.  

---

## ⚙️ Architecture

The project is composed of two executables:  

* 🖥️ `server` → receives and reconstructs messages  
* 📤 `client` → encodes and sends strings to the server  

---

## 🚀 Usage

### 1. Start the server

```bash
./server
```

The server will display its PID:  

```text
Server PID: 12345
```

---

### 2. Send a message from the client

```bash
./client <server_pid> "Hello world"
```

Example:  

```bash
./client 12345 "42 is awesome"
```

---

## 📡 Communication Method

All communication is done using **UNIX signals only**:  

* `SIGUSR1` → represents binary `0`  
* `SIGUSR2` → represents binary `1`  

### Encoding process (client):

* Each character is converted into its binary representation  
* Bits are sent one by one using signals  
* A small delay ensures synchronization between signals  

### Decoding process (server):

* Signals are received via `sigaction`  
* Bits are reconstructed into characters  
* Characters are printed immediately once completed  

---

## 🧠 Key Design Concepts

### 🔁 Bit-level communication

Instead of sending strings directly, data is transmitted bit by bit using signals, requiring manual reconstruction on the server side.  

---

### ⏱️ Synchronization handling

Because UNIX signals are not reliably queued, the system relies on:  

* controlled timing (`usleep`)  
* acknowledgment logic (optional improvement pattern)  
* careful sequencing of signal dispatch  

---

### 🧵 Server robustness

The server is designed to:  

* continuously listen for messages  
* handle multiple clients sequentially  
* never require restart between transmissions  

---

## ⚠️ Constraints

* Only `SIGUSR1` and `SIGUSR2` are allowed for communication  
* No sockets, pipes, or shared memory  
* Must not crash under any input  
* Must be leak-free  
* One global variable allowed per program (justified use only)  

---

## 🧪 Error Handling

The program must safely handle:  

* invalid PID  
* interrupted transmission  
* unexpected signal behavior  
* client/server desynchronization  

No segmentation faults or undefined behavior are allowed.  

---

## 📬 Contact

GitHub: [https://github.com/wanyingcodes](https://github.com/wanyingcodes)  
