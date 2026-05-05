# MARP

https://github.com/marp-team/marp-cli


marp slide-deck.md -o output.html


## PDF
marp --pdf slide-deck.md
marp slide-deck.md -o converted.pdf


## PPT

marp --pptx slide-deck.md
marp slide-deck.md -o converted.pptx

A created PPTX includes rendered Marp slide pages and the support of Marpit presenter notes. It can open with PowerPoint, Keynote, Google Slides, LibreOffice Impress, and so on...



[EXPERIMENTAL] Generate editable pptx (--pptx-editable)
A converted PPTX usually consists of pre-rendered background images, that is meaning contents cannot to modify or re-use in PowerPoint. If you want to generate editable PPTX for modifying texts, shapes, and images in GUI, you can pass --pptx-editable option together with --pptx option.

marp --pptx --pptx-editable slide-deck.md



## PNG/JPEG image(s) 🌐
Multiple images (--images)
You can convert the slide deck into multiple images when specified --images [png|jpeg] option.

Convert into multiple JPEG image files

marp --images jpeg slide-deck.md