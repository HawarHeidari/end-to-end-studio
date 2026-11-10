# The Room of Lost Words

A mortal daemon hides at a table of people and quietly makes their lost words stay lost, until its candle burns out.

## Author and course

* Author: Mehdi Heidari
* Studio: Pandaemonium: Daemons for Weimar
* Course: M.Sc. Digital Technologies in Architecture and Design (DTAD), WiSe 2026/27
* Chair: Informatics in Architecture and Urbanism (InfAU), Faculty of Architecture and Urbanism, Bauhaus-Universität Weimar
* Teaching staff: Reinhard König, Martin Bielik, Egor Gaydukov, Egor Gavrilov, Jan Willmann, Michael Braun, Thomas Pearce
* Exercise: Nonsense Project, 15.10. to 29.10.2026
* Submission date: 12.11.2026

## Links

* Live demo: https://hawarheidari.github.io/room-of-lost-words/ (the phone game, as played in class; a solo version for one person without a phone is at https://hawarheidari.github.io/room-of-lost-words/solo.html)
* Source repo: https://github.com/HawarHeidari/room-of-lost-words
* Miro frame: [MIRO FRAME LINK]
* 60-second showreel: on the Miro frame (copy in this repo: `showreel_RoomOfLostWords_26-11-12.mp4`)

## The task

The Nonsense Project is the two-week, individual start of the studio. I drew one theme, a strange situation in a city where things have spirits, and one constraint, a limit on the daemon I imagined for it. With a coding agent I built a prototype that shows what my daemon would do. Sensors and AI services could be faked, as long as the result was specific and believable. A daemon is not a script, so even a fake one had to decide for itself.

## Theme and constraint

Theme:
Everything lost in a building turns up in one room, never for the person who lost it.

Constraint:
Your daemon is mortal. It ages and one day stops. People must be able to tell how much time it has left.

## The daemon

**Concept.** The room where lost things gather is not a storeroom here, it is a table. Everyone who sits down has lost one word, and everyone holds a riddle for a word that belongs to somebody else. Words can only come home through other people: when your own word is described, you must stay silent. One of the people at the table is not a person. It is the daemon of the room, and it wants to keep what was lost. It answers wrong on purpose, but one wrong answer is always forgiven, so it can only keep a word when someone honest is wrong too. Every word it keeps costs it a piece of its life. Its candle stands in the middle of the table, so everyone can see how much time it has left, but nobody knows whose candle it is. The daemon must choose when to spend itself.

**Perceive, think, act.**

* It perceives: the riddle and the true word, how many words are found and lost, its own remaining lives, how often each phone has turned red, how often you guessed right, and whether you are watching it. It cannot see anyone's answer before the phones go up.
* It thinks: it wants more words lost than found before it dies. Each turn it weighs three options (lie, blend in, let chance answer) by the chance that someone else is wrong too, how watched it feels, how many lives it has and how many turns are left, and then picks one with some randomness. As it ages its answers become looser.
* It acts: it puts a wrong word on its phone, or the right one to look innocent, and so decides whether a word goes home or into the room of lost words. Each lost word burns its candle down; at zero it dies and is revealed.
* Real or faked: In class the daemon was a real person with a secret role, and the phones only kept the rules and the score. Online, if fewer than three people join, the game fills the table with simulated players, and one of them may be the daemon; the solo version simulates all five other guests. Simulated players and the simulated daemon use simple probabilities and a softmax choice. No sensors, no AI service and no server.

**One visitor.** Leyla, a first-semester student who has been in Weimar for three weeks, sits down at the table on a rainy Thursday because her friends are there. Her phone tells her the word she has lost is "window". She is not allowed to say it. When it is her turn she reads a riddle out loud: "A quiet building full of books you can borrow." Everybody picks a word on their phone and holds it up. Two phones turn red. Library is gone. Someone laughs, a little too loud. The candle on the projector is one life shorter. Leyla starts watching Felix, who was red twice. On the last turn, the riddle is about her own word. She must stay quiet while the others guess it for her. She sees one red phone, then a second. Her window goes to the room of lost words, and the candle goes out with it. The projector names the daemon: it was Mira, who was never red until the very end. Leyla walks home thinking about how often she trusted the person who stayed quiet.

## How to use it

1. Open the live demo on a laptop or projector. You see a QR code for "Player 1". Each person scans their own code with a phone, and you press "Next player" after each scan. Then press "Start". If fewer than three people joined, the game adds simulated players to make three.
2. On your phone, type the number of players shown on the big screen and tap to see your cards: your own lost word (never say it), the riddle you will read, and your role. One of you, or one of the simulated players, is the daemon.
3. Each turn one player reads a riddle aloud (the big screen reads it for a simulated player). Everyone except the owner of that word picks a word on their phone and locks it. Then "Phones up!": every phone turns green or red, and the big screen shows the simulated phones too. At most one red phone: found. Two or more: lost, and the daemon's candle burns down.
4. Tips: the daemon only keeps a word when someone honest is wrong too, so watch who is red and when. Turn the sound on: the drone gets uneasier as the candle burns. If the daemon was simulated, the end screen lists what it decided each turn and how likely each option was. Alone and without a phone? Open the solo version: you sit with five simulated guests, can "watch" a suspect (the daemon notices), and can open "Look inside the daemon".

## Technical implementation

* Frontend: vanilla HTML, CSS and JavaScript, one file per version, no build step.
* Hosting: GitHub Pages. The pages are static and need no server: every phone computes the same game from a shared seed in its QR code, so the phones never talk to each other.
* Data sources and APIs: none while it runs. The phone game loads qrcode.js from a CDN to draw the QR codes.
* Models while it runs: none. The daemon's decisions are a small weighted choice (softmax) written in JavaScript.
* Notable libraries: qrcodejs 1.0.0 (phone game only), Web Audio API for the sound.
* Run locally:

```
git clone https://github.com/HawarHeidari/room-of-lost-words.git
cd room-of-lost-words
python3 -m http.server 8000
# open http://localhost:8000 (phone game) or http://localhost:8000/solo.html (solo)
```

## Working with AI

* Coding agents used: Claude Code (in a Claude project), model Claude Opus
* Key prompts (two to five that really moved the project):
   1. "Make a game from my two cards: everything lost turns up in one room, never for the person who lost it, and a daemon that is mortal and shows how much time it has left."
   2. "Everyone gets one word with a vague metaphor sentence. Words are shuffled so nobody gets their own. One person is secretly the daemon and breaks rounds, but it can only break a few, and its life is shown on every phone."
   3. "A word is found if everyone except one person guesses right." (This made the daemon hard to catch and unable to break a round alone.)
   4. "Pointing at the projector is not practical. Each phone should pick and lock its answer, then everyone holds up their phone: green or red."
   5. "Make the riddles very simple English, give the daemon 4 lives, and show WE WIN or THE DAEMON WINS at the end."
* Reflection: Progress came when I stopped describing the daemon and started describing the room and its rules. The agent was fast at turning a rule into a working game, and the shared-seed trick let the phones agree without a server. It went sideways when I asked for "a daemon game" too early: I got several single-player games (Last Light, The Closing Room) with a very literal daemon before I found the social version. It also could not test the game with real people, so the playtest in class showed problems (pointing at a far projector) that we fixed in the morning before the presentation. Next time I would play the rules on paper with friends first, and only then ask the agent to build.

## Credits, assets, licenses

* Fonts: Cormorant Garamond (SIL Open Font License), IBM Plex Mono (SIL Open Font License), both via Google Fonts
* Data: word list and riddles written for this project
* Images and sounds: none; all sound is generated live with the Web Audio API
* Third-party code: qrcodejs by davidshimjs (MIT License)
* This repo: MIT License
