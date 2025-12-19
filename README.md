# Neo6502
Notes on the Olimex Neo6502

## C Programming

### Installing the CC65 Compiler on Microsoft Windows

The CC65 compiler is used to cross compile C to 6502 assembly on Windows.

This forum entry describes how to download a CC65 64 bit binary for Microsoft Windows: https://cx16forum.com/forum/viewtopic.php?t=6630

> Navigate to CC65's Github, click "Actions", click "Snapshot Build", click the most recent build with a green checkmark, scroll down to "Artifacts", then download cc65-snapshot-win64. This is a ZIP file.

Unzip the zip file. It contains another cc65-snapshot-win64 folder which in turn contains the compiler. Take the inner cc65-snapshot-win64 folder out of the zip and place it on your harddrive. This is the folder that you will point build systems to so that they can find the CC65 compiler.

### Use an application Template

The git repository https://github.com/PeteGollan/Neo6502-programs contains a template and build files for a CC65 sample project.

```
git clone https://github.com/PeteGollan/Neo6502-programs.git
```

Next, adjust the HelloNeo6502-CC65\build.bat file to your local system.

The CC65_HOME variable is set to the folder that the CC65 compiler has been placed/unzipped into.

NEO_HOME is a variable that I do not know what to set it to. It points to some firmware which I have not figured out yet where to get that firmware files from. For now this is not necessary to set correctly.

As a tip for the rest of the variables: In Visual Studio code, you can select "Copy Path" from the context menu on a folder. Use this feature to easily copy and past correct paths for the variables NEO_NONELIB, NEO_APILIB and NEO_INC in the following.

NEO_NONELIB, set the path to the Neo6502_nonelib folder that came with the https://github.com/PeteGollan/Neo6502-programs.git repository.

NEO_APILIB, set the path to the Neo6502_libs folder that came with the https://github.com/PeteGollan/Neo6502-programs.git repository.

NEO_INC, set the path to the Neo6502inc folder that came with the https://github.com/PeteGollan/Neo6502-programs.git repository.

### Modify hello.c

As a first example, create a backup copy of hello.c and edit hello.c.

hello.c is contained in HelloNeo6502-CC65\hello.c.

The simpler version of hello.c looks like this:

```
#include <stdio.h>
#include <stdint.h>
#include <stdlib.h>
#include <stdbool.h>
#include <ctype.h>
#include <string.h>

// Neo6502 Kernel API convenience macros
#include <neo6502.h>
#include "neo6502apilib.h"

int main()
{
    return 1;
}
```

Then build the binary:

```
cd C:\Users\lapto\dev\neo6502\Neo6502-programs\HelloNeo6502-CC65
build.bat
```

The build.bat file creates a file called hello.neo. hello.neo initially contains the 6502 assembly file.

In a second step, the build.bat batch file tries to insert the .neo file format header before the data already contained in the .neo file. Adding the neo file format header would make the .neo file a real .neo file. The .neo file format is documented here: https://github.com/OLIMEX/Neo6502/blob/main/DOCUMENTS/Neo6502_User_Handbook.pdf in the section D) File Formats > NEO Load file format.

If you do not have Python 3 installed, this step will fail and instead of a real .neo file, you end up with the raw assembler bytes in the .neo file.

### Analysing the 6502 Assembly

For me, this assembly has been created by CC65:

```
D8 A9 00 85 00 A9 F6 85 01 20 6F 08 20 42 08 20 25 08 20 31 08 20 36 08 A9 03 8D 01 FF A9 01 8D 00 FF 6C 00 00 A0 00 F0 07 A9 31 A2 08 4C 92 08 60 A2 00 A9 01 60 A0 00 F0 07 A9 92 A2 08 4C 92 08 60 A9 92 85 08 A9 08 85 09 A9 92 85 0A A9 08 85 0B A2 DA A9 FF 85 10 A0 00 E8 F0 0D B1 08 91 0A C8 D0 F6 E6 09 E6 0B D0 F0 E6 10 D0 EF 60 A9 B7 85 08 A9 08 85 09 A9 00 A8 A2 00 F0 0A 91 08 C8 D0 FB E6 09 CA D0 F6 C0 00 F0 05 91 08 C8 D0 F7 60 8D A0 08 8E A1 08 8D A7 08 8E A8 08 88 B9 FF FF 8D B1 08 88 B9 FF FF 8D B0 08 8C B3 08 20 FF FF A0 FF D0 E8 60
```

Or formatted by teh HxD hex editor:

```
Offset(h) 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F

00000000  D8 A9 00 85 00 A9 F6 85 01 20 6F 08 20 42 08 20  Ø©.….©ö…. o. B.
00000010  25 08 20 31 08 20 36 08 A9 03 8D 01 FF A9 01 8D  %. 1. 6.©...ÿ©..
00000020  00 FF 6C 00 00 A0 00 F0 07 A9 31 A2 08 4C 92 08  .ÿl.. .ð.©1¢.L’.
00000030  60 A2 00 A9 01 60 A0 00 F0 07 A9 92 A2 08 4C 92  `¢.©.` .ð.©’¢.L’
00000040  08 60 A9 92 85 08 A9 08 85 09 A9 92 85 0A A9 08  .`©’….©.….©’….©.
00000050  85 0B A2 DA A9 FF 85 10 A0 00 E8 F0 0D B1 08 91  ….¢Ú©ÿ…. .èð.±.‘
00000060  0A C8 D0 F6 E6 09 E6 0B D0 F0 E6 10 D0 EF 60 A9  .ÈÐöæ.æ.Ððæ.Ðï`©
00000070  B7 85 08 A9 08 85 09 A9 00 A8 A2 00 F0 0A 91 08  ·….©.….©.¨¢.ð.‘.
00000080  C8 D0 FB E6 09 CA D0 F6 C0 00 F0 05 91 08 C8 D0  ÈÐûæ.ÊÐöÀ.ð.‘.ÈÐ
00000090  F7 60 8D A0 08 8E A1 08 8D A7 08 8E A8 08 88 B9  ÷`. .Ž¡..§.Ž¨.ˆ¹
000000A0  FF FF 8D B1 08 88 B9 FF FF 8D B0 08 8C B3 08 20  ÿÿ.±.ˆ¹ÿÿ.°.Œ³.
000000B0  FF FF A0 FF D0 E8 60                             ÿÿ ÿÐè`
```

You can copy those bytes into the online disassembler https://www.masswerk.at/6502/disassembler.html.

Then, in order to dissasemble the entire file look for the "to (hex):" edit field and type FFFF into that field, then click the "Disassemble" button.

The output is:

```
                            * = $0000
0000   D8         L0000     CLD
0001   A9 00                LDA #$00
0003   85 00                STA L0000
0005   A9 F6                LDA #$F6
0007   85 01                STA $01
0009   20 6F 08             JSR $086F
000C   20 42 08             JSR $0842
000F   20 25 08             JSR $0825
0012   20 31 08             JSR $0831
0015   20 36 08             JSR $0836
0018   A9 03                LDA #$03
001A   8D 01 FF             STA $FF01
001D   A9 01                LDA #$01
001F   8D 00 FF             STA $FF00
0022   6C 00 00             JMP (L0000)
0025   A0 00                LDY #$00
0027   F0 07                BEQ L0030
0029   A9 31                LDA #$31
002B   A2 08                LDX #$08
002D   4C 92 08             JMP $0892
0030   60         L0030     RTS
0031   A2 00                LDX #$00
0033   A9 01                LDA #$01
0035   60                   RTS
0036   A0 00                LDY #$00
0038   F0 07                BEQ L0041
003A   A9 92                LDA #$92
003C   A2 08                LDX #$08
003E   4C 92 08             JMP $0892
0041   60         L0041     RTS
0042   A9 92                LDA #$92
0044   85 08                STA $08
0046   A9 08                LDA #$08
0048   85 09                STA $09
004A   A9 92                LDA #$92
004C   85 0A                STA $0A
004E   A9 08                LDA #$08
0050   85 0B                STA $0B
0052   A2 DA                LDX #$DA
0054   A9 FF                LDA #$FF
0056   85 10                STA $10
0058   A0 00                LDY #$00
005A   E8         L005A     INX
005B   F0 0D                BEQ L006A
005D   B1 08      L005D     LDA ($08),Y
005F   91 0A                STA ($0A),Y
0061   C8                   INY
0062   D0 F6                BNE L005A
0064   E6 09                INC $09
0066   E6 0B                INC $0B
0068   D0 F0                BNE L005A
006A   E6 10      L006A     INC $10
006C   D0 EF                BNE L005D
006E   60                   RTS
006F   A9 B7                LDA #$B7
0071   85 08                STA $08
0073   A9 08                LDA #$08
0075   85 09                STA $09
0077   A9 00                LDA #$00
0079   A8                   TAY
007A   A2 00                LDX #$00
007C   F0 0A                BEQ L0088
007E   91 08      L007E     STA ($08),Y
0080   C8                   INY
0081   D0 FB                BNE L007E
0083   E6 09                INC $09
0085   CA                   DEX
0086   D0 F6                BNE L007E
0088   C0 00      L0088     CPY #$00
008A   F0 05                BEQ L0091
008C   91 08                STA ($08),Y
008E   C8                   INY
008F   D0 F7                BNE L0088
0091   60         L0091     RTS
0092   8D A0 08             STA $08A0
0095   8E A1 08             STX $08A1
0098   8D A7 08             STA $08A7
009B   8E A8 08             STX $08A8
009E   88         L009E     DEY
009F   B9 FF FF             LDA $FFFF,Y
00A2   8D B1 08             STA $08B1
00A5   88                   DEY
00A6   B9 FF FF             LDA $FFFF,Y
00A9   8D B0 08             STA $08B0
00AC   8C B3 08             STY $08B3
00AF   20 FF FF             JSR $FFFF
00B2   A0 FF                LDY #$FF
00B4   D0 E8                BNE L009E
00B6   60                   RTS
                            .END

;auto-generated symbols and labels
 L0000        $00
 L0030        $30
 L0041        $41
 L006A        $6A
 L005A        $5A
 L005D        $5D
 L0088        $88
 L007E        $7E
 L0091        $91
 L009E        $9E

```

Next, look at the first few lines:

```
0000   D8         L0000     CLD
0001   A9 00                LDA #$00
0003   85 00                STA L0000
0005   A9 F6                LDA #$F6
0007   85 01                STA $01
0009   20 6F 08             JSR $086F
000C   20 42 08             JSR $0842
000F   20 25 08             JSR $0825
```

These are the exact instructions as contained in the file Neo6502_nonelib\crt0.s

The Neo6502_nonelib\crt0.s assembly file is used to set up the CRT (C Runtime).

```
; ---------------------------------------------------------------------------
; A little light 6502 housekeeping
; ---------------------------------------------------------------------------
_init:
    cld      ; Clear decimal mode

; ---------------------------------------------------------------------------
; Set cc65 parameter stack pointer
; This is different from the 65C02 stack

  lda #<(__RAM_START__ + __RAM_SIZE__)
  sta c_sp
  lda #>(__RAM_START__ + __RAM_SIZE__)
  sta c_sp+1

  ; ---------------------------------------------------------------------------
  ; Initialize memory storage

  jsr zerobss  ; Clear BSS segment
  jsr copydata ; initialise data segment
  jsr initlib  ; Run constructors


  ; ---------------------------------------------------------------------------
  ; Call main()

  jsr _main
```

Before jumping to main, it executes cld, lda, sta, lda sta, jsr, jsr, jsr.

These exact sequence of instructions has been inserted by the compiler at the start of the hello.neo file.

After that, the application jumps into main().

There are a lot more instructions and I do not know yet where they come from. I am a bit baffled since the main() function just returns 1 and there should be way less code necessary for returning a 1. I assume the rest of the code are standard functions from the libc and the C-Runtime. I make this assumption since the HelloNeo6502-CC65\build.bat file links api.lib to the hello world executable.


### Test with nop

```
#include <stdio.h>
#include <stdint.h>
#include <stdlib.h>
#include <stdbool.h>
#include <ctype.h>
#include <string.h>

// Neo6502 Kernel API convenience macros
#include <neo6502.h>
#include "neo6502apilib.h"

int main()
{
    asm("nop");
    asm("nop");
    asm("nop");
    asm("nop");
    asm("nop");
    asm("nop");
    asm("nop");
    asm("nop");

    return 1;
}
```

```
Offset(h) 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F

00000000  D8 A9 00 85 00 A9 F6 85 01 20 77 08 20 4A 08 20  Ø©.….©ö…. w. J.
00000010  25 08 20 31 08 20 3E 08 A9 03 8D 01 FF A9 01 8D  %. 1. >.©...ÿ©..
00000020  00 FF 6C 00 00 A0 00 F0 07 A9 31 A2 08 4C 9A 08  .ÿl.. .ð.©1¢.Lš.
00000030  60 EA EA EA EA EA EA EA EA A2 00 A9 01 60 A0 00  `êêêêêêêê¢.©.` .
00000040  F0 07 A9 9A A2 08 4C 9A 08 60 A9 9A 85 08 A9 08  ð.©š¢.Lš.`©š….©.
00000050  85 09 A9 9A 85 0A A9 08 85 0B A2 DA A9 FF 85 10  ….©š….©.….¢Ú©ÿ….
00000060  A0 00 E8 F0 0D B1 08 91 0A C8 D0 F6 E6 09 E6 0B   .èð.±.‘.ÈÐöæ.æ.
00000070  D0 F0 E6 10 D0 EF 60 A9 BF 85 08 A9 08 85 09 A9  Ððæ.Ðï`©¿….©.….©
00000080  00 A8 A2 00 F0 0A 91 08 C8 D0 FB E6 09 CA D0 F6  .¨¢.ð.‘.ÈÐûæ.ÊÐö
00000090  C0 00 F0 05 91 08 C8 D0 F7 60 8D A8 08 8E A9 08  À.ð.‘.ÈÐ÷`.¨.Ž©.
000000A0  8D AF 08 8E B0 08 88 B9 FF FF 8D B9 08 88 B9 FF  .¯.Ž°.ˆ¹ÿÿ.¹.ˆ¹ÿ
000000B0  FF 8D B8 08 8C BB 08 20 FF FF A0 FF D0 E8 60     ÿ.¸.Œ». ÿÿ ÿÐè`
```

```
                            * = $0000
0000   D8         L0000     CLD
0001   A9 00                LDA #$00
0003   85 00                STA L0000
0005   A9 F6                LDA #$F6
0007   85 01                STA $01
0009   20 77 08             JSR $0877
000C   20 4A 08             JSR $084A
000F   20 25 08             JSR $0825
0012   20 31 08             JSR $0831
0015   20 3E 08             JSR $083E
0018   A9 03                LDA #$03
001A   8D 01 FF             STA $FF01
001D   A9 01                LDA #$01
001F   8D 00 FF             STA $FF00
0022   6C 00 00             JMP (L0000)
0025   A0 00                LDY #$00
0027   F0 07                BEQ L0030
0029   A9 31                LDA #$31
002B   A2 08                LDX #$08
002D   4C 9A 08             JMP $089A
0030   60         L0030     RTS
0031   EA                   NOP
0032   EA                   NOP
0033   EA                   NOP
0034   EA                   NOP
0035   EA                   NOP
0036   EA                   NOP
0037   EA                   NOP
0038   EA                   NOP
0039   A2 00                LDX #$00
003B   A9 01                LDA #$01
003D   60                   RTS
003E   A0 00                LDY #$00
0040   F0 07                BEQ L0049
0042   A9 9A                LDA #$9A
0044   A2 08                LDX #$08
0046   4C 9A 08             JMP $089A
0049   60         L0049     RTS
004A   A9 9A                LDA #$9A
004C   85 08                STA $08
004E   A9 08                LDA #$08
0050   85 09                STA $09
0052   A9 9A                LDA #$9A
0054   85 0A                STA $0A
0056   A9 08                LDA #$08
0058   85 0B                STA $0B
005A   A2 DA                LDX #$DA
005C   A9 FF                LDA #$FF
005E   85 10                STA $10
0060   A0 00                LDY #$00
0062   E8         L0062     INX
0063   F0 0D                BEQ L0072
0065   B1 08      L0065     LDA ($08),Y
0067   91 0A                STA ($0A),Y
0069   C8                   INY
006A   D0 F6                BNE L0062
006C   E6 09                INC $09
006E   E6 0B                INC $0B
0070   D0 F0                BNE L0062
0072   E6 10      L0072     INC $10
0074   D0 EF                BNE L0065
0076   60                   RTS
0077   A9 BF                LDA #$BF
0079   85 08                STA $08
007B   A9 08                LDA #$08
007D   85 09                STA $09
007F   A9 00                LDA #$00
0081   A8                   TAY
0082   A2 00                LDX #$00
0084   F0 0A                BEQ L0090
0086   91 08      L0086     STA ($08),Y
0088   C8                   INY
0089   D0 FB                BNE L0086
008B   E6 09                INC $09
008D   CA                   DEX
008E   D0 F6                BNE L0086
0090   C0 00      L0090     CPY #$00
0092   F0 05                BEQ L0099
0094   91 08                STA ($08),Y
0096   C8                   INY
0097   D0 F7                BNE L0090
0099   60         L0099     RTS
009A   8D A8 08             STA $08A8
009D   8E A9 08             STX $08A9
00A0   8D AF 08             STA $08AF
00A3   8E B0 08             STX $08B0
00A6   88         L00A6     DEY
00A7   B9 FF FF             LDA $FFFF,Y
00AA   8D B9 08             STA $08B9
00AD   88                   DEY
00AE   B9 FF FF             LDA $FFFF,Y
00B1   8D B8 08             STA $08B8
00B4   8C BB 08             STY $08BB
00B7   20 FF FF             JSR $FFFF
00BA   A0 FF                LDY #$FF
00BC   D0 E8                BNE L00A6
00BE   60                   RTS
                            .END

;auto-generated symbols and labels
 L0000        $00
 L0030        $30
 L0049        $49
 L0072        $72
 L0062        $62
 L0065        $65
 L0090        $90
 L0086        $86
 L0099        $99
 L00A6        $A6
```