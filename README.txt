1、基于freemodbus-V1.6 编写的程序。

2、硬件基于EasyARM287，使用串口0。

移植步骤：

1.修改 配置地址  串口参数 等，
  位置：demo.c 
  L112
  原：
  	else if( eMBInit( MB_RTU, 0x0A, 0, 38400, MB_PAR_EVEN ) != MB_ENOERR )
  修改为  else if( eMBInit( MB_RTU, 0x01, 0, 9600, MB_PAR_NONE ) != MB_ENOERR )

  之所以这么改的原因是，modbus通信，不适合波特率过高，为了传输质量。
	
2、删掉 设备信息配置，不需要，
	位置：demo
	L117...
	else if( eMBSetSlaveID( 0x34, TRUE, ucSlaveID, 3 ) != MB_ENOERR )
    {
        fprintf( stderr, "%s: can't set slave id!\n", PROG );
        iExitCode = EXIT_FAILURE;
    }
	
	
3、修改 串口 初始化中的  打开串口的 “路径”字符串
	位置：port/portserial.c
	L101
	
	原：
    snprintf( szDevice, 16, "/dev/ttyS%d", ucPort );
	改：
    snprintf( szDevice, 16, "/dev/ttySP%d", ucPort );
	
3、修改 定时器 超时统计函数,这里应该是个bug，
	位置：port/porttimer.c
	L73-74
	
	原：
    ulDeltaMS = ( xTimeCur.tv_sec - xTimeLast.tv_sec ) * 1000L +
                ( xTimeCur.tv_usec - xTimeLast.tv_usec ) * 1000L;
	改：
    ulDeltaMS = ( xTimeCur.tv_sec - xTimeLast.tv_sec ) * 1000L +
                ( xTimeCur.tv_usec - xTimeLast.tv_usec ) / 1000L;
				
4、修改寄存器起始地址及 数量
	位置：demo.c
	L38-41
	原：
	#define REG_INPUT_START 1000
	#define REG_INPUT_NREGS 4
	#define REG_HOLDING_START 2000
	#define REG_HOLDING_NREGS 130
	
	该：
	#define REG_INPUT_START 	1
	#define REG_INPUT_NREGS 	200
	#define REG_HOLDING_START 	1
	#define REG_HOLDING_NREGS 	200
	
	即输入和保持寄存器起始地址都为1，数量为200个。
