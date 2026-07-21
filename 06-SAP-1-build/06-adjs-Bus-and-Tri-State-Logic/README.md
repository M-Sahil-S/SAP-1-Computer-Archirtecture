# BUS ARCHITECTURE

In the next upcoming module we're going to focus on building the registers for this computer.  the registers are a fundamental building block of any computer and we've got a couple of them in this computer. A egister just stores Data. it stores 8 Bits of data in this case this an 8-bit computer and then it interfaces to the bus. so to really understand the function of a register you've got to understand how the bus Works within a computer and and the importance of the bus in the computer.

most computers are organized around a bus and many computers have multiple buses but in this case
we're going to keep things simple we have a single bus 
a bus is basically just a a collection of data points from different modules of the computer

think of it like a long straigh pipe lien of water and our modules as different inputs and out puts from pipeline. we can either draw the water iDATA from the pipelin or we can feed the water into main poieline to be late rpicked up by some other module. essentially a bus uis like highway pa=th for different modules to communcate the dta awith each other.

lets take a simple example. assume module A outputs some data D1 onto the bus. now any othe rmodule can see "ohh there is data D1 on the bus. i need it" so by some logic and circuitry which we will build as its unique to eqach module, it pulls the dat into itself. not that this doesnt mean dat disappears from the bus. it stays there. all the input and out puts on bus are read only. thats how this archictecturei s build. sio D1 data stays on bus and anyone other module can read it and take it in for themselvbes. same with out put. any module can ouyt put its data onto the bus and any oither module can read it.

# SAP bus architecture #

for our build we have a single bus but the bus in itself contain 8 bus lines as were building an 8 bit arcjhitecture. so think like and individual bus line for each bit. for example on bus line BIT3 : all the inputs an dfoutputs for BIT 3 will be fed. thus modukle A can feed bit 3 of its data onto line 3 2hich can be later read by by any module in its bit 3 location and so on. we have bit lines 0-7 since/ thus 8 buis lines for each. 

this process of outputing data from one module into bus and then feeding into different moduel ius known as a bus transfer.. an example of a 4 bit bus tranfer is shown below :

<!- DLS video of a 4 line bus transfer -->

# Tri state logic

now that we know a bus works. we need to solve an important issue while dealing with bus.

assume 8 bit binary data by module A as 1100 1100. and and data B by module B as 1100 1101. and we wanna out put both of them into the bus.

notice a conflict? tyhe last bit has data 0 for module A and 1 for data B. this messes up the computer. it gets confused wether to read in a 0 or a 1 at bit 7? (remeber bits stsrat from 0 thus last bit is 7). to solve such dat conflicts. we use a tri state buffer.

A tri state buffer basically have 3 out put types. 1 , 0 and disconnected. say we feed a 0 into tri state buffer and activate its out put enable pin , it will pass the incoming 0 normally. same for a 1. but when we deactivaet its output enable pin. it eill disonnecet the input comming. essential a floating statre which is neither 1 nor 0.

its easire to undertsand from an example expaline below/

asumme data A is 1100 1100 which is first fed into a buffer who's output is then fed into bus
assume data B is 1010 1010 which is again fed into a buffer whos output is then fed into bus.

now if i activate OE of A : the data on the bus will be 1100 1100 and module B will be disconnected since bufdfer is on a floating stat / disconnected state for B. as if the module didnt even exist tehre in fuisrt place. hecne no errors or conflicts.

then any moduole C can read the data 1100 1100 from A normally.

once were don. w can deatcivate OE pin of A, essentially disconnecting module A from bus an dactive OE of module B, thus new data on bus is 1010 1010. and any modue D can read that data normally. 

thi sremoves errors and conflicts and helps su control which dat dgoes ouyt on the bus. thus triu state buffersd will be used wuith every module except for some special cases. this concludes the bus archiutecture and Trui state buffers module 