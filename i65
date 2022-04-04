#!/usr/bin/env python3

from sys import path, argv
from os.path import expanduser
path.append(expanduser("~/python/"))

from color import *
from sys import exit
import curses


OP_INFO={

0x0:("BRK","IMP",1,7,"","","","","BDI","none"),
0x1:("ORA","IZX",2,6,"","","","","NZ","none"),
0x4:("TSB","ZP",2,5,"Yes","","","","Z","none"),
0x5:("ORA","ZP",2,3,"","","","","NZ","none"),
0x6:("ASL","ZP",2,5,"","","","","NZC","none"),
0x7:("RMB0","ZP",2,5,"Yes","","","","none","none"),
0x8:("PHP","IMP",1,3,"","","","","none","none"),
0x9:("ORA","IMMED",2,2,"","","","","NZ","none"),
0xA:("ASL","IMP",1,2,"","","","","NZC","none"),
0xC:("TSB","ABS",3,6,"Yes","","","","Z","none"),
0xD:("ORA","ABS",3,4,"","","","","NZ","none"),
0xE:("ASL","ABS",3,6,"","","","","NZC","none"),
0xF:("BBR0","ZPR",3,5,"Yes","","","","none","none"),
0x10:("BPL","REL",2,2,"","Yes","Yes","","none","N"),
0x11:("ORA","IZY",2,5,"","Yes","","","NZ","none"),
0x12:("ORA","IZP",2,5,"Yes","","","","NZ","none"),
0x14:("TRB","ZP",2,5,"Yes","","","","Z","none"),
0x15:("ORA","ZPX",2,4,"","","","","NZ","none"),
0x16:("ASL","ZPX",2,6,"","","","","NZC","none"),
0x17:("RMB1","ZP",2,5,"Yes","","","","none","none"),
0x18:("CLC","IMP",1,2,"","","","","C","none"),
0x19:("ORA","ABSY",3,4,"","Yes","","","NZ","none"),
0x1A:("INC","IMP",1,2,"Yes","","","","NZ","none"),
0x1C:("TRB","ABS",3,6,"Yes","","","","Z","none"),
0x1D:("ORA","ABSX",3,4,"","Yes","","","NZ","none"),
0x1E:("ASL","ABSX",3,6,"","Yes","","","NZC","none"),
0x1F:("BBR1","ZPR",3,5,"Yes","","","","none","none"),
0x20:("JSR","ABS",3,6,"","","","","none","none"),
0x21:("AND","IZX",2,6,"","","","","NZ","none"),
0x24:("BIT","ZP",2,3,"","","","","NVZ","none"),
0x25:("AND","ZP",2,3,"","","","","NZ","none"),
0x26:("ROL","ZP",2,5,"","","","","NZC","C"),
0x27:("RMB2","ZP",2,5,"Yes","","","","none","none"),
0x28:("PLP","IMP",1,4,"","","","","NVBDIZC","none"),
0x29:("AND","IMMED",2,2,"","","","","NZ","none"),
0x2A:("ROL","IMP",1,2,"","","","","NZC","C"),
0x2C:("BIT","ABS",3,4,"","","","","NVZ","none"),
0x2D:("AND","ABS",3,4,"","","","","NZ","none"),
0x2E:("ROL","ABS",3,6,"","","","","NZC","C"),
0x2F:("BBR2","ZPR",3,5,"Yes","","","","none","none"),
0x30:("BMI","REL",2,2,"","Yes","Yes","","none","N"),
0x31:("AND","IZY",2,5,"","Yes","","","NZ","none"),
0x32:("AND","IZP",2,5,"Yes","","","","NZ","none"),
0x34:("BIT","ZPX",2,4,"Yes","","","","NVZ","none"),
0x35:("AND","ZPX",2,4,"","","","","NZ","none"),
0x36:("ROL","ZPX",2,6,"","","","","NZC","C"),
0x37:("RMB3","ZP",2,5,"Yes","","","","none","none"),
0x38:("SEC","IMP",1,2,"","","","","C","C"),
0x39:("AND","ABSY",3,4,"","Yes","","","NZ","none"),
0x3A:("DEC","IMP",1,2,"Yes","","","","NZ","none"),
0x3C:("BIT","ABSX",3,4,"Yes","Yes","","","NVZ","none"),
0x3D:("AND","ABSX",3,4,"","Yes","","","NZ","none"),
0x3E:("ROL","ABSX",3,6,"","Yes","","","NZC","C"),
0x3F:("BBR3","ZPR",3,5,"Yes","","","","none","none"),
0x40:("RTI","IMP",1,6,"","","","","NVDIZC","none"),
0x41:("EOR","IZX",2,6,"","","","","NZ","none"),
0x45:("EOR","ZP",2,3,"","","","","NZ","none"),
0x46:("LSR","ZP",2,5,"","","","","NZC","none"),
0x47:("RMB4","ZP",2,5,"Yes","","","","none","none"),
0x48:("PHA","IMP",1,3,"","","","","none","none"),
0x49:("EOR","IMMED",2,2,"","","","","NZ","none"),
0x4A:("LSR","IMP",1,2,"","","","","NZC","none"),
0x4C:("JMP","ABS",3,3,"","","","","none","none"),
0x4D:("EOR","ABS",3,4,"","","","","NZ","none"),
0x4E:("LSR","ABS",3,6,"","","","","NZC","none"),
0x4F:("BBR4","ZPR",3,5,"Yes","","","","none","none"),
0x50:("BVC","REL",2,2,"","Yes","Yes","","none","V"),
0x51:("EOR","IZY",2,5,"","Yes","","","NZ","none"),
0x52:("EOR","IZP",2,5,"Yes","","","","NZ","none"),
0x55:("EOR","ZPX",2,4,"","","","","NZ","none"),
0x56:("LSR","ZPX",2,6,"","","","","NZC","none"),
0x57:("RMB5","ZP",2,5,"Yes","","","","none","none"),
0x58:("CLI","IMP",1,2,"","","","","I","none"),
0x59:("EOR","ABSY",3,4,"","Yes","","","NZ","none"),
0x5A:("PHY","IMP",1,3,"Yes","","","","none","none"),
0x5D:("EOR","ABSX",3,4,"","Yes","","","NZ","none"),
0x5E:("LSR","ABSX",3,6,"","Yes","","","NZC","none"),
0x5F:("BBR5","ZPR",3,5,"Yes","","","","none","none"),
0x60:("RTS","IMP",1,6,"","","","","none","none"),
0x61:("ADC","IZX",2,6,"","","","Yes","NVZC","C"),
0x64:("STZ","ZP",2,3,"Yes","","","","none","none"),
0x65:("ADC","ZP",2,3,"","","","Yes","NVZC","C"),
0x66:("ROR","ZP",2,5,"","","","","NZC","C"),
0x67:("RMB6","ZP",2,5,"Yes","","","","none","none"),
0x68:("PLA","IMP",1,4,"","","","","NZ","none"),
0x69:("ADC","IMMED",2,2,"","","","Yes","NVZC","C"),
0x6A:("ROR","IMP",1,2,"","","","","NZC","C"),
0x6C:("JMP","IND",3,6,"","","","","none","none"),
0x6D:("ADC","ABS",3,4,"","","","Yes","NVZC","C"),
0x6E:("ROR","ABS",3,6,"","","","","NZC","C"),
0x6F:("BBR6","ZPR",3,5,"Yes","","","","none","none"),
0x70:("BVS","REL",2,2,"","Yes","Yes","","none","V"),
0x71:("ADC","IZY",2,5,"","Yes","","Yes","NVZC","C"),
0x72:("ADC","IZP",2,5,"Yes","","","Yes","NVZC","C"),
0x74:("STZ","ZPX",2,4,"Yes","","","","none","none"),
0x75:("ADC","ZPX",2,4,"","","","Yes","NVZC","C"),
0x76:("ROR","ZPX",2,6,"","","","","NZC","C"),
0x77:("RMB7","ZP",2,5,"Yes","","","","none","none"),
0x78:("SEI","IMP",1,2,"","","","","I","none"),
0x79:("ADC","ABSY",3,4,"","Yes","","Yes","NVZC","C"),
0x7A:("PLY","IMP",1,4,"Yes","","","","NZ","none"),
0x7C:("JMP","IAX",3,6,"Yes","","","","none","none"),
0x7D:("ADC","ABSX",3,4,"","Yes","","Yes","NVZC","C"),
0x7E:("ROR","ABSX",3,6,"","Yes","","","NZC","C"),
0x7F:("BBR7","ZPR",3,5,"Yes","","","","none","none"),
0x80:("BRA","REL",2,3,"Yes","Yes","","","none","none"),
0x81:("STA","IZX",2,6,"","","","","none","none"),
0x84:("STY","ZP",2,3,"","","","","none","none"),
0x85:("STA","ZP",2,3,"","","","","none","none"),
0x86:("STX","ZP",2,3,"","","","","none","none"),
0x87:("SMB0","ZP",2,5,"Yes","","","","none","none"),
0x88:("DEY","IMP",1,2,"","","","","NZ","none"),
0x89:("BIT","IMMED",2,2,"Yes","","","","Z","none"),
0x8A:("TXA","IMP",1,2,"","","","","NZ","none"),
0x8C:("STY","ABS",3,4,"","","","","none","none"),
0x8D:("STA","ABS",3,4,"","","","","none","none"),
0x8E:("STX","ABS",3,4,"","","","","none","none"),
0x8F:("BBS0","ZPR",3,5,"Yes","","","","none","none"),
0x90:("BCC","REL",2,2,"","Yes","Yes","","none","C"),
0x91:("STA","IZY",2,6,"","","","","none","none"),
0x92:("STA","IZP",2,5,"Yes","","","","none","none"),
0x94:("STY","ZPX",2,4,"","","","","none","none"),
0x95:("STA","ZPX",2,4,"","","","","none","none"),
0x96:("STX","ZPY",2,4,"","","","","none","none"),
0x97:("SMB1","ZP",2,5,"Yes","","","","none","none"),
0x98:("TYA","IMP",1,2,"","","","","NZ","none"),
0x99:("STA","ABSY",3,5,"","","","","none","none"),
0x9A:("TXS","IMP",1,2,"","","","","none","none"),
0x9C:("STZ","ABS",3,4,"Yes","","","","none","none"),
0x9D:("STA","ABSX",3,5,"","","","","none","none"),
0x9E:("STZ","ABSX",3,5,"Yes","","","","none","none"),
0x9F:("BBS1","ZPR",3,5,"Yes","","","","none","none"),
0xA0:("LDY","IMMED",2,2,"","","","","NZ","none"),
0xA1:("LDA","IZX",2,6,"","","","","NZ","none"),
0xA2:("LDX","IMMED",2,2,"","","","","NZ","none"),
0xA4:("LDY","ZP",2,3,"","","","","NZ","none"),
0xA5:("LDA","ZP",2,3,"","","","","NZ","none"),
0xA6:("LDX","ZP",2,3,"","","","","NZ","none"),
0xA7:("SMB2","ZP",2,5,"Yes","","","","none","none"),
0xA8:("TAY","IMP",1,2,"","","","","NZ","none"),
0xA9:("LDA","IMMED",2,2,"","","","","NZ","none"),
0xAA:("TAX","IMP",1,2,"","","","","NZ","none"),
0xAC:("LDY","ABS",3,4,"","","","","NZ","none"),
0xAD:("LDA","ABS",3,4,"","","","","NZ","none"),
0xAE:("LDX","ABS",3,4,"","","","","NZ","none"),
0xAF:("BBS2","ZPR",3,5,"Yes","","","","none","none"),
0xB0:("BCS","REL",2,2,"","Yes","Yes","","none","C"),
0xB1:("LDA","IZY",2,5,"","Yes","","","NZ","none"),
0xB2:("LDA","IZP",2,5,"Yes","","","","NZ","none"),
0xB4:("LDY","ZPX",2,4,"","","","","NZ","none"),
0xB5:("LDA","ZPX",2,4,"","","","","NZ","none"),
0xB6:("LDX","ZPY",2,4,"","","","","NZ","none"),
0xB7:("SMB3","ZP",2,5,"Yes","","","","none","none"),
0xB8:("CLV","IMP",1,2,"","","","","V","V"),
0xB9:("LDA","ABSY",3,4,"","Yes","","","NZ","none"),
0xBA:("TSX","IMP",1,2,"","","","","NZ","none"),
0xBC:("LDY","ABSX",3,4,"","Yes","","","NZ","none"),
0xBD:("LDA","ABSX",3,4,"","Yes","","","NZ","none"),
0xBE:("LDX","ABSY",3,4,"","Yes","","","NZ","none"),
0xBF:("BBS3","ZPR",3,5,"Yes","","","","none","none"),
0xC0:("CPY","IMMED",2,2,"","","","","NZC","none"),
0xC1:("CMP","IZX",2,6,"","","","","NZC","none"),
0xC4:("CPY","ZP",2,3,"","","","","NZC","none"),
0xC5:("CMP","ZP",2,3,"","","","","NZC","none"),
0xC6:("DEC","ZP",2,5,"","","","","NZ","none"),
0xC7:("SMB4","ZP",2,5,"Yes","","","","none","none"),
0xC8:("INY","IMP",1,2,"","","","","NZ","none"),
0xC9:("CMP","IMMED",2,2,"","","","","NZC","none"),
0xCA:("DEX","IMP",1,2,"","","","","NZ","none"),
0xCB:("WAI","IMP",1,3,"","","","","none","none"),
0xCC:("CPY","ABS",3,4,"","","","","NZC","none"),
0xCD:("CMP","ABS",3,4,"","","","","NZC","none"),
0xCE:("DEC","ABS",3,6,"","","","","NZ","none"),
0xCF:("BBS4","ZPR",3,5,"Yes","","","","none","none"),
0xD0:("BNE","REL",2,2,"","Yes","Yes","","none","Z"),
0xD1:("CMP","IZY",2,5,"","Yes","","","NZC","none"),
0xD2:("CMP","IZP",2,5,"Yes","","","","NZC","none"),
0xD5:("CMP","ZPX",2,4,"","","","","NZC","none"),
0xD6:("DEC","ZPX",2,6,"","","","","NZ","none"),
0xD7:("SMB5","ZP",2,5,"Yes","","","","none","none"),
0xD8:("CLD","IMP",1,2,"","","","","D","none"),
0xD9:("CMP","ABSY",3,4,"","Yes","","","NZC","none"),
0xDA:("PHX","IMP",1,3,"Yes","","","","none","none"),
0xDB:("STP","IMP",1,3,"","","","","none","none"),
0xDD:("CMP","ABSX",3,4,"","Yes","","","NZC","none"),
0xDE:("DEC","ABSX",3,7,"","","","","f","none"),
0xDF:("BBS5","ZPR",3,5,"Yes","","","","none","none"),
0xE0:("CPX","IMMED",2,2,"","","","","NZC","none"),
0xE1:("SBC","IZX",2,6,"","","","Yes","NVZC","C"),
0xE4:("CPX","ZP",2,3,"","","","","NZC","none"),
0xE5:("SBC","ZP",2,3,"","","","Yes","NVZC","C"),
0xE6:("INC","ZP",2,5,"","","","","NZ","none"),
0xE7:("SMB6","ZP",2,5,"Yes","","","","none","none"),
0xE8:("INX","IMP",1,2,"","","","","NZ","none"),
0xE9:("SBC","IMMED",2,2,"","","","Yes","NVZC","C"),
0xEA:("NOP","IMP",1,2,"","","","","none","none"),
0xEC:("CPX","ABS",3,4,"","","","","NZC","none"),
0xED:("SBC","ABS",3,4,"","","","Yes","NVZC","C"),
0xEE:("INC","ABS",3,6,"","","","","NZ","none"),
0xEF:("BBS6","ZPR",3,5,"Yes","","","","none","none"),
0xF0:("BEQ","REL",2,2,"","Yes","Yes","","none","Z"),
0xF1:("SBC","IZY",2,5,"","Yes","","Yes","NVZC","C"),
0xF2:("SBC","IZP",2,5,"Yes","","","Yes","NVZC","C"),
0xF5:("SBC","ZPX",2,4,"","","","Yes","NVZC","C"),
0xF6:("INC","ZPX",2,6,"","","","","NZ","none"),
0xF7:("SMB7","ZP",2,5,"Yes","","","","none","none"),
0xF8:("SED","IMP",1,2,"","","","","D","none"),
0xF9:("SBC","ABSY",3,4,"","Yes","","Yes","NVZC","C"),
0xFA:("PLX","IMP",1,4,"Yes","","","","NZ","none"),
0xFD:("SBC","ABSX",3,4,"","Yes","","Yes","NVZC","C"),
0xFE:("INC","ABSX",3,7,"","","","","NZ","none"),
0xFF:("BBS7","ZPR",3,5,"Yes","","","","none","none")

}


#Instruction information list indexes
INDEX_NAME=0
INDEX_MODE=1
INDEX_SIZE=2
INDEX_CYCLES=3
INDEX_65C02_ONLY=4
INDEX_BOUNDARY_CYCLE=5
INDEX_BRANCH_CYCLE=6
INDEX_BCD_CYCLE=7
INDEX_FLAGS=8
INDEX_AFFECTS=9

#Generate dictionaries based on op dictionary
OP_INFO_NAME={}
OP_INFO_MODE={}
for k,v in OP_INFO.items():

    #Dictionary by instruction name
    if v[INDEX_NAME] not in OP_INFO_NAME:
        OP_INFO_NAME[v[INDEX_NAME]]=[(k,)+v]
    else:
        OP_INFO_NAME[v[INDEX_NAME]]+=[(k,)+v]

    #Dictionary by addressing mode
    if v[INDEX_MODE] not in OP_INFO_MODE:
        OP_INFO_MODE[v[INDEX_MODE]]=[(k,)+v]
    else:
        OP_INFO_MODE[v[INDEX_MODE]]+=[(k,)+v]

#Alternative names for addressing modes
OP_MODE_ALT={
    #Shell filters out # so can't use
    #"#":"IMMED"
    }

#Strings for addressing modes in examples
OP_MODE_EXAMPLES={
    "ABS":   "$1234",
    "ABSX":  "$1234,X",
    "ABSY":  "$1234,Y",
    "IAX":   "($1234,X)",
    "IMMED": "#$34",
    "IMP":   "",
    "IND":   "($34)",
    "IZX":    "($34,X)",
    "IZY":    "($34),Y",
    "IZP":   "($34)",
    "REL":   "label",
    "ZP":    "$34",
    "ZPR":   "$34,label",
    "ZPX":   "$34,X",
    "ZPY":   "$34,Y"
    }

#Colors for instruction information
COLOR_NAME="blue"
COLOR_SIZE="green"
COLOR_FLAGS="magenta"
COLOR_CYCLES="yellow"

#Define same colors as above for curses
CURSES_COLOR_NAME=1
CURSES_COLOR_SIZE=2
CURSES_COLOR_FLAGS=3
CURSES_COLOR_CYCLES=4
CURSES_COLOR_PARTIAL=5
CURSES_COLORS=[
    curses.COLOR_BLUE,      #Name
    curses.COLOR_GREEN,     #Size
    curses.COLOR_MAGENTA,   #Flags
    curses.COLOR_YELLOW,    #Cycles
    curses.COLOR_GREEN      #Partial search result
    ]
CURSES_COLOR_DICT={
    COLOR_NAME:CURSES_COLOR_NAME,
    COLOR_SIZE:CURSES_COLOR_SIZE,
    COLOR_FLAGS:CURSES_COLOR_FLAGS,
    COLOR_CYCLES:CURSES_COLOR_CYCLES,
    "none":0
    }

#Interactive mode using curses
def InteractiveMode():
    curses.wrapper(InteractiveModeBody)

IMODE_DRAW_X=1
IMODE_DRAW_Y=2
IMODE_DRAW_WIDTH=6
def InteractiveModeBody(screen):
    #Initialize color pairs
    for i,color in enumerate(CURSES_COLORS):
        curses.init_pair(i+1,color,curses.COLOR_BLACK)
    key=0
    input_str=""
    op_names=[name for name in OP_INFO_NAME]
    op_names.sort()
    while(True):
        screen.clear() 
        #Don't draw partial matches if no input
        if input_str!="":
            matches=[name for name in op_names if input_str in name]
            if len(matches)==1:
                #Draw instruction info
                match=matches[0]
                max_y,max_x=screen.getmaxyx()
                draw_y=IMODE_DRAW_Y
                for i,instruction in enumerate(OP_INFO_NAME[match]):
                    DisplayOpCurses(instruction[0],screen,draw_y)
                    draw_y+=1
                    if draw_y==max_y-1:
                        #If no room left and not last item, notify that instructions left 
                        if i!=len(OP_INFO_NAME[match])-1:
                            screen.addstr(draw_y,draw_x,"("+str(len(OP_INFO_NAME[match])-i-1)+" more instructions)")
                            break
            elif len(matches)>1:
                #Draw partial matches
                max_y,max_x=screen.getmaxyx()
                draw_y=IMODE_DRAW_Y
                draw_x=IMODE_DRAW_X
                for name in matches:
                    index=0
                    while index<len(name):
                        if name[index:index+len(input_str)]==input_str:
                            screen.addstr(draw_y,draw_x+index,input_str,curses.color_pair(CURSES_COLOR_PARTIAL))
                            index+=len(input_str)
                        else:
                            screen.addstr(draw_y,draw_x+index,name[index])
                            index+=1

                    draw_y+=1
                    if draw_y==max_y-1:
                        draw_y=IMODE_DRAW_Y
                        draw_x+=IMODE_DRAW_WIDTH

        #Draw input line last so cursor is visible    
        screen.addstr(0,0,"> "+input_str)
        screen.refresh()
        try:
            key=screen.getkey()
        except KeyboardInterrupt:
            #User pressed Ctrl+C
            exit(0)

        if key=="KEY_RESIZE":
            #Redraw whole screen every time so shouldn't be  needed
            pass
        elif key=="KEY_BACKSPACE":
            input_str=input_str[:-1]
        elif len(key)==1 and key.isalnum() and len(input_str)<4:
            input_str+=key.upper()

#Display functions
MAX_NAME_LEN=4
MAX_INSTRUCTION_LEN=len("$34,label ")
MAX_MODE_LEN=len("IMMED  ")
MAX_CYCLES_LEN=len("7-9 cycles  ")

def FormatFlags(flags):
    if flags=="none":
        return "........"
    retval=""
    for char in "NV-BDIZC":
        if char in flags:
            retval+=char
        else:
            retval+="."
    return retval

def DisplayOp(op_code):
    #Op code
    printc("$"+("0"+hex(op_code)[2:].upper())[-2:]+": ")
    #Instruction name
    if op_code not in OP_INFO:
        printc("NOP ",COLOR_NAME)
        printc("on 65C02. Unassigned on 6502.")
        print()
        return
    else:
        info=OP_INFO[op_code]
        printc(info[INDEX_NAME]+" ",COLOR_NAME)
        op_spacing=4-len(info[INDEX_NAME])
        printc(OP_MODE_EXAMPLES[info[INDEX_MODE]],width=MAX_INSTRUCTION_LEN+op_spacing)
    #Addressing mode
    printc(info[INDEX_MODE],width=MAX_MODE_LEN)
    #Size
    printc(str(info[INDEX_SIZE])+" bytes  ",COLOR_SIZE)
    #Cycles
    cycles=0
    if info[INDEX_BOUNDARY_CYCLE]=="Yes":
        cycles+=1
    if info[INDEX_BRANCH_CYCLE]=="Yes":
        cycles+=1
    if info[INDEX_BCD_CYCLE]=="Yes":
        cycles+=1
    cycles_base=info[INDEX_CYCLES]
    if cycles==0:
        printc(str(cycles_base)+" cycles",COLOR_CYCLES,width=MAX_CYCLES_LEN)
    else:
        printc(str(cycles_base)+"-"+str(cycles_base+cycles)+" cycles",COLOR_CYCLES,width=MAX_CYCLES_LEN)
    #Flags
    printc(FormatFlags(info[INDEX_FLAGS])+"  ",COLOR_FLAGS)
    #65C02 only
    if info[INDEX_65C02_ONLY]=="Yes":
        printc("*65C02 only")

    print()

def CursesHelper(text,screen,draw_x,draw_y,width=0,color="none"):
    if width!=0 and len(text)<width:
        text+=" "*(width-len(text))
    screen.addstr(draw_y,draw_x,text,curses.color_pair(CURSES_COLOR_DICT[color]))
    return draw_x+len(text)

def DisplayOpCurses(op_code,screen,draw_y):
    draw_x=0
    #Op code
    draw_x=CursesHelper("$"+("0"+hex(op_code)[2:].upper())[-2:]+": ",screen,draw_x,draw_y)
    #Instruction name
    info=OP_INFO[op_code]
    draw_x=CursesHelper(info[INDEX_NAME]+" ",screen,draw_x,draw_y,color=COLOR_NAME)
    op_spacing=4-len(info[INDEX_NAME])
    draw_x=CursesHelper(OP_MODE_EXAMPLES[info[INDEX_MODE]],screen,draw_x,draw_y,width=MAX_INSTRUCTION_LEN+op_spacing)
    #Addressing mode
    draw_x=CursesHelper(info[INDEX_MODE],screen,draw_x,draw_y,width=MAX_MODE_LEN)
    #Size
    draw_x=CursesHelper(str(info[INDEX_SIZE])+" bytes  ",screen,draw_x,draw_y,color=COLOR_SIZE)
    #Cycles
    cycles=0
    if info[INDEX_BOUNDARY_CYCLE]=="Yes":
        cycles+=1
    if info[INDEX_BRANCH_CYCLE]=="Yes":
        cycles+=1
    if info[INDEX_BCD_CYCLE]=="Yes":
        cycles+=1
    cycles_base=info[INDEX_CYCLES]
    if cycles==0:
        draw_x=CursesHelper(str(cycles_base)+" cycles",screen,draw_x,draw_y,color=COLOR_CYCLES,width=MAX_CYCLES_LEN)
    else:
        draw_x=CursesHelper(str(cycles_base)+"-"+str(cycles_base+cycles)+" cycles",screen,draw_x,draw_y,color=COLOR_CYCLES,width=MAX_CYCLES_LEN)
    #Flags
    draw_x=CursesHelper(FormatFlags(info[INDEX_FLAGS])+"  ",screen,draw_x,draw_y,color=COLOR_FLAGS)
    #65C02 only
    if info[INDEX_65C02_ONLY]=="Yes":
        draw_x=CursesHelper("*65C02 only",screen,draw_x,draw_y)

def DisplayInstruction(name):
    for instruction in OP_INFO_NAME[name.upper()]:
        DisplayOp(instruction[0])

def DisplayMode(mode):
    for instruction in OP_INFO_MODE[mode.upper()]:
        DisplayOp(instruction[0])

def HelpMessage():
    mode_list=[mode for mode in OP_INFO_MODE]
    mode_list+=[mode for mode in OP_MODE_ALT]
    mode_list.sort()
    exceptions=["BBR","BBS","RMB","SMB"]
    instruction_list=[name for name in OP_INFO_NAME if name[:3] not in exceptions]
    instruction_list+=[name+"x" for name in exceptions]
    instruction_list.sort()
    print("65C02 instruction information")
    print("Usage: i65 [options] [op code|instruction|mode]")
    print("Options:")
    #print("  -a    List instructions alphabetically instead of by op code.")
    print("  -h    Display this help message.")
    print("  -i    Enter interactive mode. If no arguments specified, enter this mode.")
    print("Instructions: ",end="")
    print(*instruction_list,sep=", ")
    print("Modes: ",end="")
    print(*mode_list,sep=", ")

#Check arguments
if len(argv)==1:
    #Interactive mode - no arguments
    InteractiveMode()
else:
    #Parse arguments
    arg=""
    a_option=False
    h_option=False
    i_option=False
    parse_failed=False
    for argv_i in argv[1:]:
        if argv_i=="-a":
            if a_option:
                parse_failed=True
            else:
                a_option=True
        elif argv_i=="-h":
            if h_option:
                parse_failed=True
            else:
                h_option=True
        elif argv_i=="-i":
            if i_option:
                parse_failed=True
            else:
                i_option=True
        else:
            if arg:
                parse_failed=True
            else:
                arg=argv_i
        if parse_failed:
            #More than one arg given or option specified twice
            HelpMessage()
            exit(1)

    #Print help message and exit if -h specified
    if h_option:
        HelpMessage()
        exit(0)

    #Enter interactive mode if -i specified
    if i_option:
        InteractiveMode()        
        exit(0)

    #Empty arg meaning -a with no arg specified
    if arg=="":
        HelpMessage()
        exit(0)

    #Handle alternate addressing modes name - ie # for IMMED
    if arg.upper() in OP_MODE_ALT:
        arg=OP_MODE_ALT[arg]

    #Figure out type of arg - op code, instruction, or mode
    if arg[:2] in ["0x","0X"]:
        #Search by op code
        op_hex=arg[2:]
        #Make sure hex is valid
        try:
            op_hex=int(op_hex,16)
        except ValueError:
            print("Invalid op code:",arg)
            exit(1)
        #Make sure op code in valid range
        if op_hex>255:
            print("Invalid op code:",arg)
            exit(1)
        #Display information
        DisplayOp(op_hex)
    elif arg.upper() in OP_INFO_NAME:
        #Search by instruction name
        DisplayInstruction(arg)
    elif arg.upper() in OP_INFO_MODE:
        #Search by addressing mode
        DisplayMode(arg)
    elif "*" in arg:
        #Wildcard in argument - search instruction names
        arg=arg.upper()
        instructions_copy=[name for name in OP_INFO_NAME if len(arg)==len(name)]
        for i,char in enumerate(arg):
            if char=="*":
                continue
            else:
                new_copy=[]
                for name in instructions_copy:
                    if name[i]==char:
                        new_copy+=[name]
                instructions_copy=new_copy[:]
        #Check if any matches found
        if instructions_copy==[]:
            print("Not found")
            exit(0)
        #Print out matches
        for k,v in OP_INFO.items():
            if v[INDEX_NAME] in instructions_copy:
                DisplayOp(k)
    else:
        #No instruction or mode match found - check if valid hex without 0x prefix
        hex_good=False
        try:
            op_hex=int(arg,16)
            if op_hex <256:
                #if op_hex in OP_INFO:
                    #Only treat arg as hex without prefix if range valid and not unassigned
                    #hex_good=True
                #Don't check if hex value is assigned
                hex_good=True
        except ValueError: 
            #Not valid hex and wasn't found above in instructions or modes
            pass
        
        if hex_good:
            #Display info if valid hex assigned to instruction
            DisplayOp(op_hex)
        else:
            print("Not found")
