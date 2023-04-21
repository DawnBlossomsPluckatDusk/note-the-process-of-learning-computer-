

#  UCOSIII 学习

## 任务的定义与任务切换的实现



### 任务的定义：

在多任务系统中，根据功能的不同，将一个系统分割为若干个独立且无法返回的函数，该函数就是一个任务

```c
void task_entry(void){
    /*任务主体，无限循环且不能返回*/
    for( ; ;){
        /*任务执行主体*/
    }
}
```



### 任务的创建

#### 定义任务栈

任务栈的作用：保存任务中的信息(全局变量，局部变量，子函数...)

在多任务系统中，需要对每个任务分配独立的栈空间。所以，有多少个任务就需要定义多少个任务栈。

该栈空间一般是预定义的全局数组

```c
/*
定义任务栈
*/
#define TASK1_STK_SIZE 128 
#define TASK2_STK_SIZE 128

static CPU_STK Task1Stk[TASK1_STK_SIZE];
static CPU_STK Task2Stk[TASK2_STK_SIZE];
```

在`ucosIII`中，数据类型都通过`typedef`进行别名。

凡是与` CPU `类型相关的数据类型则统一在` cpu.h `中定义，与 `OS `相关的数据类型则在 `os_type.h` 定义

```c
/*
* cpu.h中的数据类型
*/
typedef unsigned short CPU_INT16U;
typedef unsigned int CPU_INT32U;
typedef unsigned char CPU_INT08U;

typedef CPU_INT32U CPU_ADDR;

/* 栈数据类型重定义 */
typedef CPU_INT32U CPU_STK;
typedef CPU_ADDR CPU_STK_SIZE;

typedef volatile CPU_INT32U CPU_REG32;
```



#### 定义任务函数

任务是一个独立的函数，函数主体无限循环且不能返回

```c
//任务1
void Task1(void *p_arg){
    for(;;){
        /*任务函数主体*/
    }
}
```





#### 定义任务控制块TCB

作用：帮助系统完成任务调度

每个任务都定义了一个任务控制块TCB，这个任务控制块里面包含了任务的所有信息(任务名，任务的形参)

`TCB`是一个新的数据类型，在`os.h`中声明

```c
typedef struct os_tcb    OS_TCB;

struct os_tcb{
    //栈指针和栈大小
    CPU_STK         *StkPtr;
    CPU_STK_SIZE     StkSize;
    /*
    ...
    */
};
```





#### 实现任务创建函数

作用：将任务的栈、任务的函数实体和任务控制块联系在一起，从而能够被系统进行调度

步骤：

1. 创建任务
2. 将任务添加到就序列表

##### 创建任务

任务创建函数`OSTaskCreate`在`os_ask.c`文件中

```c
 /*
 μC/OS-III 中的函数命名规则，以大小的 OS 开头,表示这是一个外部函数，可以由用户调用
以OS_开头的函数表示内部函数,只能由μC/OS-III内部使用。
*/
void OSTaskCreate(OS_TCB      *p_tcb,
                 OS_TASK_PTR   p_task,
                 void         *p_arg,
                 CPU_STK      *p_stk_base,
                 CPU_STK_SIZE  stk_size,
                 OS_ERR        *p_err){
    CPU_STK   *p_sp;
    p_sp= OSTaskStkInit(p_task,
                       p_arg,
                       p_stk_base,
                       stk_size);
    p_tcb->StkPtr = p_sp;
    p_tcb->StkSize = stk_size;
}
```

`OS_TASK_PTR` 在`os.h`中声明：

```c
typedef void (*OS_TASK_PTR) (void *p_arg);
```



`p_err`用于存储错误码，`usocIII`预定义许多的错误码，都定义在`os.h`中



`OsTaskStkInit()`是任务栈初始化函数，该函数在`os_cpu_c.c`中

``` c
CPU_STK *OSTaskStkInit(OS_TASK_PTR  p_task,
                      void         *p_ayg,
                      CPU_STK      *p_stk_base,
                      CPU_STK_SIZE  stk_size){
    CPU_STK *p_stk;
    p_stk = &p_stk_base[stk_size];
   /* 异常发生时自动保存的寄存器 */
	*--p_stk = (CPU_STK)0x01000000u; /* xPSR 的 bit24 必须置 1 */
    *--p_stk = (CPU_STK)p_task; /* R15(PC) 任务的入口地址 */
	*--p_stk = (CPU_STK)0x14141414u; /* R14 (LR) */
	*--p_stk = (CPU_STK)0x12121212u; /* R12 */
	*--p_stk = (CPU_STK)0x03030303u; /* R3 */
	*--p_stk = (CPU_STK)0x02020202u; /* R2 */
	*--p_stk = (CPU_STK)0x01010101u; /* R1 */
	*--p_stk = (CPU_STK)p_arg; /* R0 : 任务形参 */
	/* 异常发生时需手动保存的寄存器 */
	*--p_stk = (CPU_STK)0x11111111u; /* R11 */
	*--p_stk = (CPU_STK)0x10101010u; /* R10 */
	*--p_stk = (CPU_STK)0x09090909u;
    *--p_stk = (CPU_STK)0x08080808u; /* R8 */
	*--p_stk = (CPU_STK)0x07070707u; /* R7 */
	*--p_stk = (CPU_STK)0x06060606u; /* R6 */
	*--p_stk = (CPU_STK)0x05050505u; /* R5 */
	*--p_stk = (CPU_STK)0x04040404u; /* R4 */
	return (p_stk);
}
```

##### 将任务添加到就序列表

```c
OSRdyList[0].HeadPtr = &Task1TCB;
OSRdyList[1].HeadPtr = &Task2TCB;
```

`OSRdyList[]`为全局变量在`os.h`中定义：

```c
OS_EXT OS_RDY_LIST OSRdyList[OS_CFG_PRIO_MAX];
```

`OS_EXT` 是一个在`os.h`中定义的宏

```c
//作用:避免重复声明全局变量
#ifdef      OS_GLOBALS
#define     OS_EXT
#else
#define     OS_EXT   extern
#endif
```

对于宏`OS_GLOBALS`，只有在`os_var.c`声明，其他地方都没有声明，保证了全局变量不会被重复声明

数据类型`OS_EXT OS_RDY_LIST`为就序列表的数据类型，在`os.h`中声明

```c
typedef struct os_rdy_list   OS_RDY_LIST;
struct os_rdy_list{
    OS_TCB   *HeadPtr;
    OS_TCB   *TailPtr;
};
```

当同一个优先级支持多个任务的时候，需要使用头尾指针来将 `TCB `串成一个双向链表



`OS_CFG_PRIO_MAX`代表支持最大的优先级，在`os_cfg.h`中定义

```c
#define OS_CFG_PRIO_MAX 32u 
```



### OS系统初始化

主要工作：在硬件初始化完成后，初始化`ucosIII`中定义的全局变量。

`OSInit()`函数用于初始化`ucosIII`，在文件`os_core.c`中定义

```c
void OSInit(OS_ERR *p_err){
    //指示系统的运行状态，默认为停止状态，即OS_STATE_OS_STOPED
    OSRunning = OS_STATE_OS_STOPED;
    
    //用于指向当前正在运行的任务的TCB指针
    OSTCBCurPtr = (OS_TCB *) 0;
    //用于指向就绪任务中优先级最高的任务的TCB
    OSTCBHighRdyPtr = (OS_TCB *)0;
    
    //用于初始化全局变量 OSRdyList[]
    OS_RdyListInit();
    
    //代码运行到这里表示没有错误，即 OS_ERR_NONE
    *p_err = OS_ERR_NONE;
}
```



全局变量 `OSTCBCurPtr` 和 `OSTCBHighRdyPtr `均在 `os.h` 中定义

```c
#define OS_STATE_OS_STOPPED (OS_STATE)(0u)
#define OS_STATE_OS_RUNNING (OS_STATE)(1u)
OS_EXT OS_TCB       *OSTCBCurPtr;
OS_EXT OS_TCB       *OSTCBHighRdyPtr;
OS_EXT OS_RDY_LIST  OSRdyList[OS_CFG_PRIO_MAX];
OS_EXT OS_STATE     OSRunning;
```





`OS_RdyListInit()`函数在文件`os_core.c`中定义

```c
void OS_RdyListInit(void){
    OS_PRIO i;
    OS_RDY_LIST  *p_rdy_list;
    for(i=0u;i<OS_CFG_PRIO_MAX;I++){
        p_rdy_list = &OSRdyList[i];
        p_rdy_list->HeadPtr = (OS_TCB *)0;
        p_rdy_list->TailPtr = (Os_TCB *)0;
    }
}
```



### 启动系统

使用`OSStart()`启动系统函数，该函数在`os_core.c`中定义

```c
void OSStart(OS_ERR  *p_err){
    if(OSRunning == OS_STATE_OS_STOPPED){
        //手动配置任务一运行
        OSTCBHighRdyPtr = OSRdyList[0].HeadPtr;
        //启动任务切换，不会返回
        OSStartHighRdy();
        //运行到这里表示发生错误
        *p_err=OS_ERR_RETURN;
    }
    else{
        *p_err=OS_STATE_OS_RUNNING;
    }
}
```



`OSStartHighRdy()`函数用于启动任务切换，即配置`PendSV`的优先级为最低，然后触发`PendSV`异常，在`PendSV`异常服务函数中进行任务切换。该函数在`os_cpu_a.s`文件中定义

```assembly
OSStarHightRdy
LDR   R0, =NVIC_SYSPRI14  ;设置PendSV异常优先级为最低
LDR   R1, =NVIC_PENDSV_PRI
STRB  R1, [R0]

MOV   R0, #0    ;设置psp的值为0,开始第一次上下文切换
MSR   PSP, R0

LDR   R0, =NVIC_INT_CTRL ;触发PendSV异常
LDR   R1, =NVIC_PENDSVSET
ATR   R1, [R0]

CPSIE   I  ;启用总中断，NMI和HardFault除外
OSStartHang
B    OSStartHang ;程序应该不会运行到此处
```

`NVIC_INT_CTRL`、`NVIC_SYSPRI14`、`NVIC_PENDSV_PRI`和 `NVIC_PENDSVSET` 这四个常量在 `os_cpu_a.s` 的开头定义

```assembly
NVIC_INT_CTRL EQU 0xE000ED04 ;中断控制及状态寄存器 SCB_ICSR。
NVIC_SYSPRI14 EQU 0xE000ED22 ;系统优先级寄存器SCB_SHPR3:bit16~23
NVIC_PENDSV_PRI EQU 0xFF ;PendSV 优先级的值(最低)。
NVIC_PENDSVSET EQU 0x10000000 ;触发 PendSV 异常的值Bit28：PENDSVSET
```

在CM3中专门设置了一条CPS指令，用于快速开关中断

```assembly
CPSID I ;PRIMASK=1 ; 关中断
CPSIE I ;PRIMASK=0 ; 开中断
CPSID F ;FAULTMASK=1 ; 关异常
CPSIE F ;FAULTMASK=0 ; 开异常
```

| 名字      | 功能                                                         |
| --------- | ------------------------------------------------------------ |
| PRIMASK   | 只有单一比特的寄存器，在它被置 1 后， 就关掉所有可屏蔽的异常，只剩下 NMI 和硬 FAULT 可以响应 |
| FAULTMASK | 只有 1 个位的寄存器，当它置 1 时，只 有 NMI 才能响应，所有其他的异常，甚至是硬 FAULT，也通通闭嘴 |
| BASEPRI   | 这个寄存器最多有 9 位（由表达优先级的位数 决定）。它定义了被屏蔽优先级的阈值，当它被设成某个值后，所有优先级号大于等于此值 的中断都被关（优先级号越大，优先级越低） |

### 任务切换

编写`PendSV`异常服务函数，在该函数内进行任务切换

**PendSV异常服务函数名称必须与启动文件里面向量表中PendSV的向量名相同**

```assembly
PendSV_Handler
CPSID  I(1);关中断，防止上下文切换
MRS    R0,PSP;将psp的值加载到R0

;如果R0为0则跳转到OS_CPU_PendSVHAndler_nosave
CBZ    R0,OS_CPU_PendSVHAndler_nosave

;保存上下文
;手动存储CPU寄存器R4-R11的值到当前任务的栈
STMDB  R0!,{R4-R11}(15)

LDR    R1,= OSTCBCurPtr(16)

LDR    R1,[R1](17)

STR    R0,[R1](18)

OS_CPU_PendSVHandler_nosave

LDR    R0,=OSTCBCurPtr(5)

LDR    R1,=OSTCBHighRdyPtr(6)

LDR    R2,[R1](7)

STR    R2,[R0](8)

LDR    R0,[R2]

LDMIA  R0!,{R4-R11}
```

