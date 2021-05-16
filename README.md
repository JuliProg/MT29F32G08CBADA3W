![Create new chip](https://github.com/JuliProg/MT29F32G08CBADA3W/workflows/Create%20new%20chip/badge.svg?event=repository_dispatch)
![ChipUpdate](https://github.com/JuliProg/MT29F32G08CBADA3W/workflows/ChipUpdate/badge.svg)
# Join the development of the project ([list of tasks](https://github.com/users/JuliProg/projects/1))


# MT29F32G08CBADA3W
Implementation of the MT29F32G08CBADA3W chip for the JuliProg programmer

Dependency injection, DI based on MEF framework is used to connect the chip to the programmer.

<section class = "listing">

# Chip parameters
```c#


        //--------------------Vendor Specific Pin configuration---------------------------

        //  VSP1(38pin) - NC    
        //  VSP2(35pin) - NC
        //  VSP3(20pin) - NC 

        ChipAssembly()
        {
            myChip.devManuf = "MICRON";
            myChip.name = "MT29F32G08CBADA3W";
            myChip.chipID = "2C-44-44-4B-A9-00-00-00";               // device ID 

            myChip.width = Organization.x8;                          // chip width (x8 or x16)
            myChip.bytesPP = 8192;                                   // page size in bytes
            myChip.spareBytesPP = 744;                                // size Spare Area in bytes
            myChip.pagesPB = 256;                                     // the number of pages per block 
            myChip.bloksPLUN = 2128;                                 // number of blocks in CE 
            myChip.LUNs = 1;                                         // the amount of CE in the chip
            myChip.colAdrCycles = 2;                                 // cycles for column addressing
            myChip.rowAdrCycles = 3;                                 // cycles for row addressing 
            myChip.vcc = Vcc.v3_3;                                   // supply voltage
            (myChip as ChipPrototype_v1).EccBits = 19;               // Number of bits ECC correctability for each 512 bytes

```
# Chip operations
```c#


            //------- Add chip operations    https://github.com/JuliProg/Wiki#command-set----------------------------------------------------

            myChip.Operations("Reset_FFh").
                   Operations("Erase_60h_D0h").
                   Operations("Read_00h_30h").
                   Operations("PageProgram_80h_10h");

```
# Initial Invalid Block (s)
```c#

            
            //------- Select the Initial Invalid Block (s) algorithm    https://github.com/JuliProg/Wiki/wiki/Initiate-Invalid-Block-----------
                
            myChip.InitialInvalidBlock = "InitInvalidBlock_v1";
                
```
# Chip registers (optional)
```c#


            //------- Add chip registers (optional)----------------------------------------------------

            myChip.registers.Add(                   // https://github.com/JuliProg/Wiki/wiki/StatusRegister
                "Status Register").
                Size(1).
                Operations("ReadStatus_70h").
                Interpretation("SR_Interpreted").
                UseAsStatusRegister();



            myChip.registers.Add(                  // https://github.com/JuliProg/Wiki/wiki/ID-Register
                "Id Register").     
                Size(8).
                Operations("ReadId_90h");
            

            myChip.registers.Add(
              "Parameter Page (ONFI parameter)").
              Size(768).
              Operations("ReadParameterPage_ECh");

            myChip.registers.Add(                  // https://github.com/JuliProg/Wiki/wiki/OTP
               "OTP memory area").
               Size((8192 + 744) * 30).           // Number of OTP pages = 30
               Operations("OTP_Mode_On_v1").      // set chip to OTP mode then Read or Programm block[0].page[02]...block[0].page[32]
               Operations("OTP_Mode_Off_v1");     // set chip to normall mode

            myChip.registers.Add(
               "Unique Id").
               Size(32).
               Operations("ReadUniqueId_EDh");

```
</section>





















footer
