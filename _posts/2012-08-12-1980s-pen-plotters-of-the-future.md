---
title: "1980s pen plotters of the future"
date: "2012-08-12"
---

\[youtube https://www.youtube.com/watch?v=T20-KcCGokU?rel=0&w=560&h=315\]

Early 1980s pen plotters are amazing tools that are still very useful in today’s world. There’s something completely transfixing about a mechanical device moving an actual pen on paper versus the smelly black box of the laser printer. And if you’re trying to draw lines or curves or like the effect of actual ink touching paper (not sprayed on in microdots) there’s no other way. Luckily, there’s some great tools out there for making plotters work on modern hardware and using modern file formats (PDFs) and the hardware itself, while finicky and aging, is cheap.

## Hardware

You’re going to have to start with the right hardware. [The Chiplotle! plotter FAQ is great for this](http://music.columbia.edu/cmc/chiplotle/plotter_FAQs.shtml), I’ve added my notes below:

- **USB - serial interface** or a serial port on your computer. I have trouble with Chiplotle! with the very common Prolific serial adapters (it might be the drivers) but if you have something that works it’ll probably be fine. I use [an ex-Keyspan one](http://www.tripplite.com/en/products/model.cfm?txtModelID=3914).
- **Any HPGL compatible pen plotter**. eBay is almost always your best bet, unless you live near the MIT Flea Market. Make sure the plotter supports HPGL via serial connection. If it says HPIB or GPIB, do not get. Very common eBay finds are the Roland DXYs or the HP 7475a. [If in doubt, check the Chiplotle! list of supported plotters](http://music.columbia.edu/cmc/chiplotle/manual/chapters/api/plotters.html) (although keep in mind similar model #s will also work; for example the DXY-1150 and 1150a act as a DXY1300 in Chiplotle!.) I normally pay $50 or so for a single sheet plotter like the DXY-1150. Make sure you get a power supply with the plotter, that’s often the hardest thing to find and none of them use anything standard.
- **A plotter serial cable**. The only place you’ll need to pull out a soldering iron. You can buy plotter serial cables on eBay but it’s easier to just make your own from a DB25 male to DB9 female cable. You have to [re-route a few wires](http://music.columbia.edu/cmc/chiplotle/plotter_cable.pdf) but it’s easy to do.
- **Pens & paper**. Your eBay plotter likely will come with a plastic bag full of dried out pens or, if you’re lucky, boxed ones. There’s a huge variety of pen types (for different paper or thicknesses, felt vs. fine point) or you can fashion your own if you’re wily. For paper, the average desktop plotter can take up to 11 x 17" paper or just normal printer paper (make sure to set the DIP switches on the back if you don’t use the full size.) I have a nice stack of artist vellum paper with nice vellum pens.

Have all that? Now, find a place to put the plotter, run the plotter cable from the serial port on back of the plotter to your USB adapter, and load up some paper. Depending on the plotter, the paper might be held in _electrostatically_ or it may be magnetized and expect a metal tab to hold in the paper (which you obviously did not get in the eBay shipment, you can use a metal ruler or something similar instead.) Or maybe you will have to tape the paper on. If it’s a wide format plotter the paper will have to be in a roll and the vertical axis is done via the roller mechanism like a receipt printer (these are notriously fiddly, I would avoid them unless you really need 3 foot wide paper.) You then load up a pen (in the pen holder off to the side, not the plotter head – the beginning of every HPGL file tells the plotter to pick up the pen from the holder.) Now, let’s plot.

## Software

In 2008 I was bitten by the plotter bug all of a sudden. I was trying to draw a smooth bezier curve robotically and was looking at various servo or motor solutions when I stumbled on the community of folks that have adapted Roland pen plotters into vinyl cutting CNC machines. I found myself intensely bidding on my first plotter against a familiar eBay username. After I lost, I confirmed my suspicions: I was in competition with my dear friend [Douglas Repetto](http://music.columbia.edu/~douglas/) of CMC & dorkbot fame. And not only was he also independently plotter crazed, he was working on a Python module for HPGL control called, well, [Chiplotle!](http://chiplotle.org). Maybe there was something in the water that week.

![Chiplotle!](images/chiplotle_logo_2.png)

Chiplotle is obviously the best and only way to reliably control a plotter from a modern computer. It does quite a lot of work for you: it manages the commandset of your plotter, buffers output so it doesn’t overflow and start drawing random straight lines, provides a interactive terminal where you can “live draw” and a bunch of other necessary stuff. Although you can import chiplotle into your program and programatically control your plotter, I tend to use just one commandset of Chiplotle – the plot\_hpgl\_file script that it installs.

HPGL files are like the wizened ancestor of PDF. It is simply an ASCII file of [text commands to draw lines, curves, choose pens, etc](http://www.isoplotec.co.jp/HPGL/eHPGL.htm) and is the plotter’s native language. If you want, you can ignore Chiplotle! altogether and just **cat** an HPGL file to your serial port at 9600 baud, 8N1. This will work fine for the first few commands but eventually the plotter’s internal buffer (mine is 512 bytes) will overflow. plot\_hpgl\_file takes care of all of this. The first time you run it, it will attempt to detect which plotter you have on which serial port. Then it will slowly spit out the HPGL commands and make sure the plotter is acknowledging them.

My workflow for the project I am on now is to generate PDF files programatically using the [amazing ReportLab python PDF toolkit](http://www.reportlab.com/software/opensource/) and then convert them to HPGL using [pstoedit](http://www.pstoedit.net/) and plot it. It is as simple as

```
python your_pdf_generator.py file.pdf
pstoedit -f hpgl file.pdf > output.hpgl
plot_hpgl_file.py output.hpgl
```

Obviously I could have my python program directly control the plotter, but as ink and paper add up, you will want a step in between to make sure your art is OK, and the PDF step is natively viewable on any platform. Since PDF and HPGL both share a lot of common ancestry, the curveTos, lineTos and moveTos are kept consistent with no loss of quality. There’s no rasterizing step: if you generate curves programatically with ReportLab, they will be the same curve on the paper in the plotter.

![Owls](images/20120812-kuqixaqy85n1f9iqgim7qu6egb.jpg)
