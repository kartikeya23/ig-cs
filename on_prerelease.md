```
Names[20], Ages[20], pb[20], Id[20], Runs[20], Timings[20][52], Count, Name, Age

SubProcedure makeID(Name, Age, Index)
  d123 <-- (ASCII(Name[0]) - 25)* 10 + Age
  num <-- d123
  sum <-- 0
  WHILE num > 0 DO
    sum <-- sum + MOD(num, 10)
    num <-- DIV(num, 10)
  ENDWHILE
  chkdgt <-- MOD(sum, 10)
  num <-- d123 * 10 + chkdgt
  REPEAT
    swapped <-- FALSE
    FOR Count <-- 0 TO 19
      IF [Count] == num
        THEN 
          num <-- num + 1
          swapped <-- True
    NEXT Count
  UNTIL swapped == TRUE
  Id[index] <-- num
EndSub

FOR Count <-- 0 TO 19
  REPEAT
    INPUT Name, Age
  UNTIL Age <= 0
  Names[Count] <-- Name
  Ages[Count] <-- Count
  makeID(Name, Age, Count)
NEXT Count
```

