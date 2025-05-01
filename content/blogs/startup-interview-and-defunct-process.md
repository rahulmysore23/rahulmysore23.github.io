---
title: "Startup Interview and Defunct Process"
date: 2025-02-07T01:23:26-06:00
draft: false 
categories: ["interview"]
tags: ["tech", "linux", "Go", "CLI", "defunct", "concurrency"]
---

Alright, folks, gather 'round. Let me tell you the story of my interview with a blockchain company (which shall remain nameless, because, well, NDAs are a thing). It was Round 1, and the focus was on OS/System Programming. The interview was scheduled for 1 hour and 30 minutes, and I was told to have a Linux environment ready. Sounds simple enough, right? Wrong. This is where the adventure begins.

---

## The Setup: Windows, WSL2, and a Dash of Desperation

So, here’s the thing: I was using Windows at the time. *Gasp!* I know, I know. But hear me out—I was in the process of switching to **Hyprland** (a fancy tiling window manager for Linux), and I didn’t want to risk breaking my system right before an interview. So, I did what any sane person would do: I fired up **WSL2** (Windows Subsystem for Linux). It’s like having a Linux terminal inside Windows, which is great until you realize you’re essentially living in two operating systems at once. Schrödinger’s developer, if you will.

---

## The Task: Forever Programs and the Watchful Monitor

The interview began, and I was given a task to write two programs:

1. **Forever Program**: A program that takes an argument and runs forever. Simple, right?
2. **Monitor Program**: A program that monitors the Forever Program and spawns a new instance if it gets killed. Think of it as a helicopter parent for processes.

I decided to use **Go** for this task. Why? Because Go is my comfort language. It’s like that one hoodie you always reach for when you’re feeling lazy. Plus, Go has excellent concurrency support and built-in packages for OS-related stuff, which made it perfect for this task.

---

## The Forever Program

Let’s start with the Forever Program. Here’s the code:

```go
package main

import (
 "flag"
 "fmt"
 "os"
 "os/signal"
 "syscall"
)

func main() {
 fmt.Println("InfStones, interview 1 - Forever")

 // Parse command line arg
 numbPtr := flag.Int("numb", 42, "an int")
 flag.Parse()

 fmt.Println("Number entered is:", *numbPtr)

 // Wait till kill signal is passed
 c := make(chan os.Signal, 1) // we need to reserve to buffer size 1, so the notifier are not blocked
 signal.Notify(c, os.Interrupt, syscall.SIGTERM)

 for {
  select {
  case <-c:
   fmt.Println("Process Killed")
   return
  }
 }
}
```

This program is pretty straightforward. It takes an integer argument, prints it, and then waits for a kill signal. When it receives the signal, it prints "Process Killed" and exits. It’s like that friend who says, “I’ll stay as long as you want me to,” but leaves the moment you hint at it.

---

## The Monitor Program: Helicopter Parent Mode Activated

Now, onto the **Monitor Program**. This one was a bit more involved. The goal was to keep an eye on the Forever Program and restart it if it got killed. Here’s the code:

```go
package main

import (
 "fmt"
 "log"
 "os"
 "os/exec"
 "os/signal"
 "reflect"
 "strings"
 "syscall"
 "time"

 "github.com/shirou/gopsutil/process"
)

const FOREVER_PROGRAM = "forever"
const FOREVER_PROGRAM_PATH = "/home/rmysore/interviews/infstones/forever/"

func main() {
 fmt.Println("InfStones, interview 1 - Monitor")

 initMap := make(map[int]string)
 init := false

 // Poll and Check for forever processes and store them - Poll and events?

 // User ticker (Optional - and kill event?)
 ticker := time.NewTicker(1 * time.Second)
 done := make(chan bool)

 c := make(chan os.Signal, 1) // we need to reserve to buffer size 1, so the notifier are not blocked
 signal.Notify(c, os.Interrupt, syscall.SIGTERM)

 go func() {
  for {
   select {
   case <-done:
    return
   case t := <-ticker.C:
    fmt.Println("Tick at", t)

    // If the last poll and new poll has changes, Find the killed process and start it again

    compareMap := make(map[int]string)
    processes, _ := process.Processes()
    for _, process := range processes {
     name, _ := process.Name()
     if strings.Compare(name, FOREVER_PROGRAM) == 0 {
      cmdL, _ := process.Cmdline()
      if !init {
       initMap[int(process.Pid)] = cmdL
      } else {
       compareMap[int(process.Pid)] = cmdL
      }
     }
    }

    // compare maps
    if !reflect.DeepEqual(initMap, compareMap) && init {
     for k, v := range initMap {
      if _, ok := compareMap[k]; !ok {
       // Start process again
       args := strings.Split(v, " ")
       if args[len(args)-1] != "&" {
        args = append(args, "&")
       }

       fmt.Println("Starting process:", args)
       cmd := exec.Command("nohup", args...)
       err := cmd.Start()
       if err != nil {
        log.Fatal(err)
       }

       if cmd.Wait() != nil {
        log.Fatal(err)
       }
      }
     }
    }

    init = true

    // replace init with compare
    initMap = compareMap

   }
  }
 }()

 for {
  select {
  case <-c:
   fmt.Println("Process Killed")
   ticker.Stop()
   done <- true
   return
  }
 }
}
```

This program is like a helicopter parent. It checks on the Forever Program every second (using a ticker) to make sure it’s still alive. If it finds that the Forever Program has been killed, it starts a new instance. It’s basically saying, “Oh, you thought you could escape? Think again.”

---

## The Plot Twist: Zombie Processes

Now, here’s where things got interesting. When I killed and respawned the Forever Program multiple times, I noticed something strange: **defunct processes** were being created. A defunct process, also known as a **zombie process**, is a process that has completed execution but still has an entry in the process table. It’s like a ghost that haunts your system, refusing to leave until you properly deal with it.

### Why Does This Happen?

In Linux, when a process finishes execution, it sends a signal to its parent process to let it know it’s done. The parent process is supposed to read the child’s exit status (a process called “reaping”). If the parent doesn’t do this, the child process becomes a zombie. It’s not using any resources, but it’s still taking up space in the process table.

### The Fix: Reaping the Zombies

The issue in my Monitor Program was that it wasn’t properly waiting for the child process to exit. To fix this, I modified the Monitor Program to wait for the process while initializing it in a goroutine. Here’s the relevant part of the code:

```go
go func() {
    cmd := exec.Command("nohup", args...)
    err := cmd.Start()
    if err != nil {
        log.Fatal(err)
    }

    if cmd.Wait() != nil {
        log.Fatal(err)
    }
}()
```

By waiting for the process to exit in a goroutine, we ensure that the child process is properly reaped, preventing it from becoming a zombie. It’s like giving the ghost a proper burial so it can finally rest in peace.

---

## Lessons Learned: The Interview Was a Win, Regardless

This interview was a rollercoaster of emotions. I went from feeling confident to panicking about zombie processes to finally fixing the issue. But you know what? It was a fantastic learning experience.

---

**TL;DR:**  

- Used WSL2 because I was too scared to break my Linux setup.  
- Wrote a Forever Program and a Monitor Program in Go.  
- Accidentally created zombie processes.  
- Fixed it by properly reaping child processes.  
- Had fun.  
