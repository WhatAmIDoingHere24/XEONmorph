# XEONmorph
I have seen only a small handfull of peolple on the internet try and build a HoeBrew x86 computer. Now there are some amazing videos out there of peolple builing 64-bit proccesors but not full systems like Ben Eaters. I plan on following the ideas and rough builds of ben eater for this series becuase he was the inspiration. I will be using an '2004 Intel Xeon 3.4' proccesor to do all the hard work. So throughout this project i will write in this README the progress of this project, I also want to explain a lot of major parts of computing and how this chip does some really cool stuff. So follow me on this project as i try and build my FIRST homebrew computer, attempt to explain all of this and hopefully inspire some others along the way!


# And god said...Let there be...A Data Sheet!
So of course before we can even begin this project we have to undertand WHAT where tryig to do, HOW where going to do it and WHAT where going to do it with. So in the RESOURCES tab ive linked to the pdf im using to create this whole project. So here are some guidelines im using to do this:

* What am I trying to do?
   * i want to be able to do simple math with a 64-bit proccesor
   * i want it to resemble Ben Eaters 8-bit computer architecture
   * learn and explain what im doing/going to do effectivly

* What resources do i need to do this
  * COMPUTER PARTS
      *(most taken from Ben Eater)*
      * 2004 Intel Xeon CPU
      *(Photo 1-1)*
      * BreadBoards (830 point)
      * 22-Guage Wire
      * AT28C256-15PU EEPROM
      * 62256 256K SRAM
  * RESOURCES
      * Intel Xeon Datasheet (Assembly/instructions)
      * Intel Xeon Spec Sheet (electrical specifications,limits,desgin)
      * Ben Eaters 8-bit computer tutorials

Now that we have arough idea of all the parts that need to come together for this project lets start taking some notes. Now dont worry im not going to explain eveysingle piece of this data sheet so ill just give a breif syummary of the most important parts and of course give a refrecne to where theyare in the datasheet just in case you really DO want to read it. 

# Understanding the Xeon
![Our XEON chip](https://github.com/user-attachments/assets/a920a8f7-f9ce-4b88-86d6-4f47718f16c2)

first We need to undertsand how this chip works and what it does. In 2004 intel needed a new cpu to put into their servers that was able to do lots of background computation but wasnt heavily involved in GUI or graphic intenseive prcessing. This chip also needed to be highly scalabale,quick and reliable. They created NetBurst and Hyper-threading, NetBurst is a genereal term used to describe a lor of systems under one name, it gives the proccesor an L2 cache to be used for multithreading pof up to three proccesies at once, basic mathmatics can retuirte in one half of a clock cycle,with that ALU's run at double the proccesor frequency. We also have Hyper-Threading which works uder the NetBurst name. Hyper-threading allows us to run multiple task at once across the proccesor, specifficly up to three proccessies at once which when coupled with the L2 cache sub-system means we can have constant, very fast , stable, output's with just a single processor. Now thats only the surface of what this proccesor can do.

All proccesors are about the same everything from 8-bit to 64-bit, they all have a set of pins that when are sent a very speciffic electrical voltage will trigger some gate to open/close inside the proccsor which will than outpt something. The only problem is that on a modern proccesor like the XEON we have 604 pins not just the 40 that are on the 8086. This means there are A LOT more instructions that this chip can run but also A LOT more that nees to happen for those instructions to be carried out properlly. 

# How are we going to put this all together? 
Much like other HomeBew style computers this whole project will take place on 830 pin breadboards. These allow us to simply connect all of our componets without having to solder and can easily be re-arrranged. To best model what this is goiong to look like ive re-created the project in a CAD-like software for computer logic. 
