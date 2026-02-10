---
# try also 'default' to start simple
theme: bricks
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS (experimental)
wakeLock: "build"
# aspect ratio for the slides
aspectRatio: 16/9
css: unocss
---

# WebTigerPython

## edu-i-day 

Clemens Bachmann

---
layout: two-cols-header
---

# Who am I

::left::
- Clemens Bachmann
- Doctoral / Teachers Diploma Student
- clemens.bachmann@inf.ethz.ch
- Algorithms and **Didactics** Group
- Working in the Group of Dennis Komm

::right::
<img src="./images/Clemens_Flaechen.svg" style="filter: brightness(0) saturate(100%) invert(80%);" width=300/>

[Illustration by Anna Staub](https://www.instagram.com/anna.staub.illustration/?hl=en)

---
layout: two-cols-header
---

# What is WebTigerPython

::left::
- Educational IDE
- Web Based
- Modern Python (3.13)
- Frontend code execution
    - No internet connection required
- Small to medium sized school projects
- NOT VSCode

::right::
<img src="./images/tigerjython-icon.svg" width=300/>

---
layout: wtp-2-cols
code: |
  # Koch.py

  from gturtle import *

  def koch(s, n):
      if n == 0:
          forward(s)
          return
      koch(s / 3, n - 1)
      left(45)
      koch(s / 3, n - 1)
      right(90)
      koch(s / 3, n - 1)
      left(45)
      koch(s / 3, n - 1)

  setPenColor("blue")
  speed(-1)
  setPos( -100, 100)
  for i in range(4):
      right(90)
      koch(100, 4)
---

# WebTigerPython🐯🐍 - Programming in the browser

- New Features
- Hands-on Examples

---
layout: two-cols-header
---

# Features of WTP 🛠️

::left::

<img src="./images/turtle.jpg" width=150/>
Visual Computing (turtle, gpanel, pygame)

<img src="./images/maqueelplusv3.jpg" width=150/>
Robotics (WebUSB, simulation)

[python-online.ch/](https://python-online.ch/)

::right::

<img src="./images/databases.jpg" width=300/>
Databases (sqlite3, database1)

<img src="./images/debugger.jpg" width=200/>

---
layout: wtp-2-cols
wtpLayout: '["Editor", "Console"]'
leftWidth: 30%
code: |
    def calculate_list_sum(numbers):
        sum = 0
        for i in numbers:
            sum += i
        return sum

    my_list = [1, 2, 3, 4, 5]
    result = calculate_list_sum(my_list)
    print(result)
---
# Debugger

- Inspired by Python Tutor

---
layout: wtp-2-cols
code: |
  import pygame

  pygame.examples.chimp.main()
---

<img src="./images/pygame_ce_logo.svg" width=380/>

- Popular library for programming games
- Sounds, images, animations
- Event handling (keyboard, mouse)
- Can be used in the browser*

*asynchronity issue

---
layout: two-cols-header
---

# Asynchronity Problem

<!-- ::left::

- Render Updates are done in the Webloop
- It is not possible to synchronously give control to webloop
- asyncio.sleep
  - only in asynchronous contexts
  - async Programming is not easy

::right:: -->

::left::
<img src="./images/runtime-environment-diagram.svg" width=380/>

::right::
### Synchronous Execution in Browser

```mermaid
flowchart LR
    A[Python Execution Pt. 1 & 2] --> C[Render 1]
    C --> D[Render 2]

    style A fill:#ffcc00, color:#000000
    style C fill:#66ccff, color:#000000
    style D fill:#66ccff, color:#000000
```

### Desired Execution Flow

```mermaid
flowchart LR
    A[Python Execution Pt. 1] --> B[Render 1]
    B --> C[Python Execution Pt. 2]
    C --> D[Render 2]

    style A fill:#ffcc00, color:#000000
    style B fill:#66ccff, color:#000000
    style C fill:#ffcc00, color:#000000
    style D fill:#66ccff, color:#000000
```

---

# Asynchronity Example

````md magic-move
```py
from pygame import *

def render():
     #...
    display.flip()

def updateActors():
    #...
    render()

def main():
    while True:
        #...
        updateActors()

main()
```
```py
from pygame import *
import asyncio

async def render():
     #...
    display.flip()
    await asyncio.sleep(0)

def updateActors():
    #...
    render()

def main():
    while True:
        #...
        updateActors()

main()
```
```py
from pygame import *
import asyncio

async def render():
     #...
    display.flip()
    await asyncio.sleep(0)

async def updateActors():
    #...
    await render()

def main():
    while True:
        #...
        updateActors()

main()
```
```py
from pygame import *
import asyncio

async def render():
     #...
    display.flip()
    await asyncio.sleep(0)

async def updateActors():
    #...
    await render()

async def main():
    while True:
        #...
        await updateActors()

await main()
```
````
<v-click>

-> This is done by WTP

</v-click>


---
layout: wtp-2-cols
code: |
  import sys, pygame, time

  W,H = 400, 600
  pygame.init()
  s = pygame.display.set_mode((W,H))

  # simple objects as rects
  player = pygame.Rect(W//2-15, H-50, 30, 10)
  aliens = [pygame.Rect(50 + i*60, 50, 32, 20) for i in range(5)]
  bullets = []

  # gameloop
  while True:
      for e in pygame.event.get():
          if e.type == pygame.QUIT:
              pygame.quit(); sys.exit()
          if e.type == pygame.KEYDOWN:
              if e.key == pygame.K_SPACE:
                  bullets.append(pygame.Rect(player.centerx-2, player.top-8, 4, 8))

      # keyboard interaction
      keys = pygame.key.get_pressed()
      if keys[pygame.K_LEFT]:
          player.x -= 4
      if keys[pygame.K_RIGHT]:
          player.x += 4
      player.x = max(0, min(W-player.w, player.x))

      # TODO move bullets and simple collision
      # use x.colliderect(y) to check if there is a collision


      # draw
      s.fill((0,0,0))
      pygame.draw.rect(s, (0,200,255), player)
      for a in aliens: pygame.draw.rect(s, (200,50,50), a)
      for b in bullets: pygame.draw.rect(s, (255,255,0), b)
      pygame.display.flip()
      time.sleep(0.006)
---

# Pygame - Hands on

- Check out the code example
- Try to implement bullet movement
- Implement simple collision detection
  - use x.colliderect(y) to check if there is a collision
- Implement alien movement
- Codeskeleton -> Moodle
- Have fun!

---
layout: wtp-2-cols
code: |
    import pygame
    pygame.examples.aliens.main()
---
# Pygame Aliens Example
---
layout: two-cols-header
---

# Maqueen Plus V3 Lidar

::left::
<img src="./images/lidar1.jpg" width=300/>

::right::
<img src="./images/lidar2.jpg" width=300/>

---
layout: wtp-2-cols
device: micro:bit
leftWidth: 30%
code: |
    from mbrobot_plusV3 import *

    def showMap(matrix):
        for row in matrix:
            line = ""
            for val in row:
                if val < 30:
                    line += " X "
                elif val < 60:
                    line += " x "
                elif val < 100:
                    line += " - "
                elif val < 150:
                    line += " . "
                else:
                    line += " "
            print(line)
        print("")    

    while True:
        matrix = getDistanceGrid()
        showMap(matrix)
        delay(3000)

---
# Lidar Simulator Example
---
base: https://exp.webtigerpython.ethz.ch
layout: wtp-2-cols
leftWidth: 30%
code: |
    import pygame
    pygame.examples.aliens.main()
---

# Multifile Mode

<v-clicks>

- Try it out
- Groups of 2 - create a small multifile task and implement it
- exp.webtigerpython.ethz.ch (experimental)

</v-clicks>

---

# WebTigerPython - The Future 🚀

- REPL?
- Robots?
