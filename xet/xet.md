---
title: xet
author: dawn
author_url: dawnvoid.neocities.org
---
# xet
*xet, short for xettelkasten, an intentional mispelling of zettelkasten*
- public zettelkasten
    - yes, the internet is one, but it's shit
    - link to / interface with other public zettelkastens
- a handful of simple formatting conventions to adhere to
    - [notes should be atomic](https://notes.andymatuschak.org/z4Rrmh17vMBbauEGnFPTZSK3UmdsGExLRfZz1)
        - Note filenames should be snake_cased versions of their titles.
            - `Minimalism is practical` becomes `minimal_is_practical.md`
            - ignore punctuation
    - [notes should be densely linked](https://notes.andymatuschak.org/z2HUE4ABbQjUNjrNemvkTCsLa1LPDRuwh1tXC)
    - [note titles should be complete phrases](https://notes.andymatuschak.org/z3KmNj3oKKSTJfqdfSEBzTQiCVGoC4GfK3rYW)
        - But not complete sentences, because then you have to deal with caps and punctuation.
    - The title of each note is given in the ~~H1~~ frontmatter `title:` field.
    - The author of each note is optionally given in the frontmatter `author:` field
- simple and minimal
    - just a pile of markdown files in a folder
    - converted to html with `mori`
    - frontend is a static webpage with nothing fancy
- can by edited by anyone
    - public GitHub repo
    - pull requests welcome
- free
    - creative commons / open source license of some sort (undecided)

![[xet.excalidraw]]

### Digital garden design & implementation
- [A Brief History & Ethos of the Digital Garden](https://maggieappleton.com/garden-history)
- [Digital Garden Terms of Service](https://www.swyx.io/digital-garden-tos)
- [learn in public](https://www.swyx.io/learn-in-public)
- [tiddlywiki](https://nesslabs.com/tiddlywiki-beginner-tutorial)

How to structure?
- Inline links should be `[text](link)` format, as that means renaming the file won't break the text around it.
- Standalone links can use any format, as long as it's easy to read.
- Wiki-style `[[link]]` links are convenient but:
    - can't have multiple files with the same name
    - probably avoid? not sure tbh