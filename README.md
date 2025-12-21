<p align="center">
  <img src="https://img.shields.io/badge/CS:Source-Source_Engine-orange?style=flat-square&logo=steam" />
  <img src="https://img.shields.io/badge/Reverse_Engineering-Game_Hacking-red?style=flat-square&logo=hackthebox" />
  <img src="https://img.shields.io/badge/Python-PyMem-yellow?style=flat-square&logo=python" />
  <img src="https://img.shields.io/badge/Platform-Windows-blue?style=flat-square&logo=windows" />
  <img src="https://img.shields.io/badge/Status-Educational-lightgrey?style=flat-square" />
</p>

<h1 align="center">CS:Source — Triggerbot (Team Based)</h1>

<p align="center">
Triggerbot developed <b>exclusively for educational purposes</b>,<br>
focused on the study of <b>Game Hacking</b> and <b>Reverse Engineering</b><br>
on the <b>Source Engine</b>.
</p>

<p align="center">
<i>The goal here is to understand how the game works internally —<br>
not to hide anything, not to bypass anything, not to use it in real environments.</i>
</p>

---

<h2 align="center">Legal Disclaimer</h2>

<p align="center">
<b>This code MUST NOT be used on online servers.</b><br>
<b>There is no protection against VAC or any Anti-Cheat.</b><br>
<b>Using it outside offline / LAN / -insecure environments will result in a ban.</b>
</p>

<p align="center">
This behavior is expected.<br>
The project does not attempt — and must not attempt — to avoid it.
</p>

---

<h2 align="center">About the Project</h2>

<p align="center">
This repository demonstrates, in a practical way, how to:
</p>

<p align="center">
• open an external process (CS:Source)<br>
• locate loaded modules (<code>client.dll</code>)<br>
• navigate internal structures using fixed offsets<br>
• interpret in-game entity data<br>
• implement minimal team-based logic
</p>

<p align="center">
The code is intentionally simple,<br>
but <b>technically correct</b>.
</p>

---

<h2 align="center">General Concept</h2>

<p align="center">
The triggerbot is based on three core pillars:
</p>

<p align="center">
<b>1.</b> LocalPlayer (who you are)<br>
<b>2.</b> Crosshair ID (who is under your crosshair)<br>
<b>3.</b> Entity List (target data)
</p>

<p align="center">
Based on this information, the code decides whether or not to shoot.
</p>

---

<h2 align="center">LocalPlayer</h2>

<p align="center">
The game keeps a global pointer to the local player:
</p>

<p align="center">
<code>client.dll + 0x0068EEC8</code>
</p>

<p align="center">
From the LocalPlayer, it is possible to read:
</p>

<p align="center">
• player team (T = 2 / CT = 3)<br>
• health, position, flags, etc.
</p>

---

<h2 align="center">Crosshair ID</h2>

<p align="center">
The Crosshair ID indicates whether the crosshair is pointing at a valid entity.
</p>

<p align="center">
It is obtained through the following pointer chain:
</p>

<p align="center">
<code>
client.dll + 0x00649910<br>
↳ +0x18<br>
↳ +0x10<br>
↳ +0x50 → crosshair_id
</code>
</p>

<p align="center">
The returned value is <b>1-based</b>:
</p>

<p align="center">
<code>1 = first entity</code><br>
<code>0 = nothing under the crosshair</code>
</p>

---

<h2 align="center">Entity List</h2>

<p align="center">
The Entity List in the Source Engine is a <b>continuous table</b>,<br>
not a linked pointer structure.
</p>

<p align="center">
Base:
</p>

<p align="center">
<code>client.dll + 0x006098C8</code>
</p>

<p align="center">
Each entity occupies a fixed slot:
</p>

<p align="center">
<code>ENTITY_STRIDE = 0x20</code>
</p>

<p align="center">
Since the Crosshair ID is 1-based, it must be converted:
</p>

<p align="center">
<code>entity_index = crosshair_id - 1</code>
</p>

---

<h2 align="center">Team Check</h2>

<p align="center">
Unlike a simple triggerbot, this project implements
<b>team verification</b>.
</p>

<p align="center">
Logical flow:
</p>

<p align="center">
<b>1.</b> Read local player team<br>
<b>2.</b> Read target entity team<br>
<b>3.</b> Compare values
</p>

<p align="center">
It only shoots if:
</p>

<p align="center">
<code>target_team != local_team</code>
</p>

<p align="center">
This prevents shooting teammates and makes the example
closer to real in-game logic.
</p>

---

<h2 align="center">Shooting</h2>

<p align="center">
The shot is simulated via <b>WinAPI</b> (<code>user32.dll</code>).
</p>

<p align="center">
There are no drivers, hooks, or concealment techniques.
</p>

<p align="center">
The small delay exists only to avoid click spamming
and to make the behavior observable during study.
</p>

---

<h2 align="center">What This Project Teaches</h2>

<p align="center">
• memory reading in real games<br>
• practical use of Source Engine offsets<br>
• entity list navigation<br>
• interpretation of internal structures<br>
• transforming analysis into functional code
</p>

<p align="center">
This code serves as a solid foundation for more advanced studies,
such as ESP, radar, or dynamic offset analysis.
</p>

---

<h2 align="center">Conclusion</h2>

<p align="center">
This project does not exist to be used.<br>
It exists to be <b>understood</b>.
</p>

<p align="center">
If the goal is to learn Reverse Engineering applied to games,<br>
it fulfills that role precisely.
</p>

<h2 align="center">Authorship</h2>

<p align="center">
This project was developed by<br>
<b><a href="https://github.com/nicolaspoersch" target="_blank">nicolaspoersch</a></b>.
</p>

<p align="center">
All code was written from scratch,<br>
with no use of pre-made bases or existing cheats.
</p>

<p align="center">
The offsets used were obtained manually,<br>
through direct memory analysis and reverse engineering<br>
of the Source Engine.
</p>

<p align="center">
Development took several hours of continuous work,<br>
including study, testing, offset validation, and implementation<br>
of the complete logic (localplayer, entity list, and team check).
</p>

<p align="center">
<i>
This repository reflects a real learning process,<br>
not copied or automated code.
</i>
</p>
