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
*(image 1-1)*

first We need to undertsand how this chip works and what it does. In 2004 intel needed a new cpu to put into their servers that was able to do lots of background computation but wasnt heavily involved in GUI or graphic intenseive prcessing. This chip also needed to be highly scalabale,quick and reliable. They created NetBurst and Hyper-threading, NetBurst is a genereal term used to describe a lor of systems under one name, it gives the proccesor an L2 cache to be used for multithreading pof up to three proccesies at once, basic mathmatics can retuirte in one half of a clock cycle,with that ALU's run at double the proccesor frequency. We also have Hyper-Threading which works uder the NetBurst name. Hyper-threading allows us to run multiple task at once across the proccesor, specifficly up to three proccessies at once which when coupled with the L2 cache sub-system means we can have constant, very fast , stable, output's with just a single processor. Now thats only the surface of what this proccesor can do.

All proccesors are about the same everything from 8-bit to 64-bit, they all have a set of pins that when are sent a very speciffic electrical voltage will trigger some gate to open/close inside the proccsor which will than outpt something. The only problem is that on a modern proccesor like the XEON we have 604 pins not just the 40 that are on the 8086. This means there are A LOT more instructions that this chip can run but also A LOT more that nees to happen for those instructions to be carried out properlly.

The best part of a 64-bit architecture is that you can mimic everythign below it, Intel had this idea and impleimented MODES. MODES allow for a devolper to trigger a change in mode at any time (for most modes). The modes look like this.

## MODES
  * IA-32
    * Protected *(DEFUALT)*
      * This is the defualt mode and allows youn to use all the neccesary standard functions of the chip  
    * Real-Address *(8086 Emulation)*
      * This emultaes an 8086 proccesor so you really have two diffrent proccesors in this chip
    * System-Mangment-Mode *(ADMIN)*
      * WHen in SMM you have total control over the entire chip and are allowed to alter states that usaully are protected
### SUB-MODES
  * IA-32e (64-bit)
    * Compatabillity Mode *(For Legacy Software)*
        * a simple mode that allows for cross bit legacy software to run, it acts like 8086 mode but will not allow Virtual(emulated)-8086 software to run on it. Supports 16 and 32-bit software
    * 64-Bit Mode
      * The actuall 64-bit mode that runs on top of the IA-32 system. It is a full 64-bit address space,*(little saide note: the 8086 proccesor has and address space of 1 million with 2^20 addresses, the 2004 intel xeon has (2^64)-1 which is 18 QUINTILLION addresses...absolutly insane number)* But more importantly it adds 8 more GPR (genreal purpose registers) from the original 8, Each register is also expnded to a full 64-bits.

  When using these modes there a diffrent opCodes (just short pnuemonics used to represent a functions like CMP, looks at the data from one locatins and COMPARES it to the data in anbothe rlocation) we have to remeber, When using 64-bit mode since our address space and GPR our expanded so much we get certain instructions to access those otherwiose unnutillised areas. REX (register extensions) which allows us to go to those extended registers and address spaces within the chip.

Within a proccesor there are registers which are very fast and usaully very small ares for srtoring data. the xeon has four registers. XMM, MMX, YMM and GPR, MMX and XMM both are made for simple quiock math that does not take up much space and can occur all at one time. This is called SIMD (Simple-Instruction-Multiple-Data), Where a few instructions are put into an vector (list of data) and all operated on at the same time. MMX is only 64-bits total but XMM is a full 128-bits, Both of these registers are split into 8 bit sections and can do word,soubleword and quadword mathmatics, which is to say they can work with numbers that are 6-bits,12-bits and 64-bits. Only the XMM register though can do single and double point presciosn (the amount of accurate decimal points) floating integers (decimal values). The next one is YMM which is absolutly insane in how powerfull it is, it can do up to 256-bit operations on word,doubleword and quadword integers along with single and double point precisioon floating integers over the whole 8 sections of 256-bits WITH SIMD instructions as well. IT is an absilute beast of a register compared tot he others and should come in hand later down the line.

The stack also allows us to contnius store data at a slower but much more reliable and larger maner. The stack is just the RAM thats been put insode the chip. By pushing to the stack we can add data and pop (remove) data from the staclk for use. Although yes you can techiniquily just use the registers for storting data its best praftice to segemnt offf sections fo your registers for the most important data dna to make sure to never fills those portions sop that there always room  for more. The stacl llows us to keep theose very quick acting and very imortant registers empty wehile still having perfectly goood sotrage for more data that might jnot be needed immediatly or as fast as possible. The stac is alo much alrtger on this chip than the registers are which makes it even better for storing data that we could use later on. 
