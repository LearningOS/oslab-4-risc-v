##ucore-4-risc-v
###lab1
使用make命令编译这个lab之后,使用spike bin/kernel 命令即可运行这个.需要把riscv-tools的目录设置为RISCV变量,并且加到path路径中,否则无法识别相关命令.riscv-tools下载地址:https://github.com/riscv/riscv-tools/tree/priv-1.7
具体修改的方式参考了https://github.com/LearningOS/os-4-risc-v.git的思路,从启动/系统调用/时钟中断/输入输出几个方面入手.因为lab1主要是平台相关的东西,所以基本思路是参考FREERTOS来,放弃掉ucore原来的思路.主要的工作包括:
1.启动&系统调用:参考FREERTOS的做法,主要是bootasm.S/syscalls.c两个部分的代码来完成,放弃原来ucore比较复杂的方式,bootasm.S主要完成寄存器和各种段的初始化工作,syscalls.c主要处理System call相关的内容.
2.时钟中断:ucore原本的方式是使用中断向量表,产生256个中断向量,用trap.c相关的代码处理,riscv与之完全不同,参考FREERTOS中相关代码(Source/portabla/GCC/RISCV目录)进行移植,完全舍弃掉原本的内容,根据port.c修改trap.c,移植portasc.S完成时钟中断相关内容,暂时不用原本处理时钟中断的clock.c了.
3.输入输出:使用系统调用完成输入输出工作
4.相关代码细节的修改:修改init函数,因为经过上述修改,一些函数名字和调用关系发生了变化,需要修改init函数;其他一些妨碍编译的部分,或者一些函数调用的修改,比较小.
5.Makefile&link文件:makefile文件改了几个参数,link文件改动比较大,主要参考FREERTOS
