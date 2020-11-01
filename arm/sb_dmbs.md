
Example below is from Professor Peter Sewell (University of Cambridge [Computer Laboratory](http://www.cl.cam.ac.uk/))

[[SB+dmbs](https://www.cl.cam.ac.uk/~pes20/arm-supplemental/arm040.html#tst@SB+dmbs)]
```shell
ARM SB+dmbs
"DMBdWR Fre DMBdWR Fre"
Cycle=Fre DMBdWR Fre DMBdWR
{
%x0=x; %y0=y;
%y1=y; %x1=x;
}
 P0            | P1            ;
 MOV R0, #1    | MOV R0, #1    ;
 STR R0, [%x0] | STR R0, [%y1] ;
 DMB           | DMB           ;
 LDR R1, [%y0] | LDR R1, [%x1] ;
exists
(0:R1=0 /\ 1:R1=0)
```

run `herd7` on x86
```shell
$ herd7 -model arm.cat sb_dmbs.litmus 
Test SB+dmbs Allowed
States 3
0:R1=0; 1:R1=1;
0:R1=1; 1:R1=0;
0:R1=1; 1:R1=1;
No
Witnesses
Positive: 0 Negative: 3
Condition exists (0:R1=0 /\ 1:R1=0)
Observation SB+dmbs Never 0 3
Time SB+dmbs 0.02
Hash=631b5d7f1cd13c8b2af0b5ebf9c34dda
```

run `litmus7` on VM
```shell
#START _litmus_P1
	mov x8,#1
	str x8,[x2]
	dmb sy
	ldr x4,[x0]
#START _litmus_P0
	mov x8,#1
	str x8,[x0]
	dmb sy
	ldr x4,[x2]

Test SB+dmbs Allowed
Histogram (4 states)
2     *>0:R1=0; 1:R1=0;
490580:>0:R1=1; 1:R1=0;
481247:>0:R1=0; 1:R1=1;
28171 :>0:R1=1; 1:R1=1;
Ok

Witnesses
Positive: 2, Negative: 999998
Condition exists (0:R1=0 /\ 1:R1=0) is validated
```

with [armmem](https://www.cl.cam.ac.uk/~pes20/ppcmem/index.html#ARM-SB+dmbs)
```shell
Test SB+dmbs Allowed
States 3
0:R1=0; 1:R1=1; via [0;0;0;1;0;0;2;0;3;0;0;0;1;0;4;0;0;0]
0:R1=1; 1:R1=0; via [0;0;0;1;0;0;1;0;3;4;0;0;0;2;0;0;0;0]
0:R1=1; 1:R1=1; via [0;0;0;1;0;0;1;0;2;0;3;0;0;0;4;0;0;0]
No (allowed not found)
Condition
exists (0:R1=0 /\ 1:R1=0)
Hash=631b5d7f1cd13c8b2af0b5ebf9c34dda
Cycle=Fre DMBdWR Fre DMBdWR
Observation SB+dmbs Never 0 3
```

We found that the result is different when run at VM, 
`0:R1=0; 1:R1=0` is exists when run at VM

## Raspberry Pi 3 Model B+

:::danger
without `-ccopts -march=armv8-a` such error will occurs **selected processor does not support `dsb sy` in ARM mode**
:::

```shell
$ litmus7 sb_dmbs.litmus -ccopts -march=armv8-a
...
Test SB+dmbs Allowed
Histogram (3 states)
438024:>0:R1=1; 1:R1=0;
438206:>0:R1=0; 1:R1=1;
123770:>0:R1=1; 1:R1=1;
No
```
![](https://i.imgur.com/Q2ncTZx.png)


