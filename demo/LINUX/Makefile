
# 交叉环境变量
CROSS  = arm-fsl-linux-gnueabi-

# gcc 变量
CC     = $(CROSS)gcc

# lib
LIBS    = -pthread

# strip函数-优化代码
STRIP  = $(CROSS)strip

# CFLAGS-系统变量，编译器参数 
CFLAGS = -g -O2 -Wall


# 头文件路径
INC = -I. -Iport -I../../modbus/rtu \
      -I../../modbus/ascii -I../../modbus/include

# 目标
TARGET = modbusrtu

# 源文件
SRC = demo.c port/portevent.c port/portother.c \
      port/portserial.c port/porttimer.c \
      ../../modbus/mb.c \
      ../../modbus/rtu/mbrtu.c ../../modbus/rtu/mbcrc.c \
      ../../modbus/ascii/mbascii.c \
      ../../modbus/functions/mbfunccoils.c \
      ../../modbus/functions/mbfuncdiag.c \
      ../../modbus/functions/mbfuncdisc.c \
      ../../modbus/functions/mbfuncholding.c \
      ../../modbus/functions/mbfuncinput.c \
      ../../modbus/functions/mbfuncother.c \
      ../../modbus/functions/mbutils.c 

# 目标文件
OBJS = $(SRC:.c=.o)

# 链接为可执行文件
$(TARGET): $(OBJS)
	$(CC) $^ -o $@ $(LIBS)

.PHONY: clean

clean:
	rm -f $(OBJS)

install : $(TARGET) clean
	@echo compile....
	cp $(TARGET) /var/tftpboot
	@echo end..

%.o : %.c
	$(CC) $(CFLAGS) $(INC) -o $@ -c $<






