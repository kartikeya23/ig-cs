```
Names[20], Ages[20], pb[20], Id[20], Runs[20], AgeBand[20], FastestAge[3], Timings[20][52], Count, Name, Age

FastestAge[] <-- {-1, -1, -1}

//generating id of a new runner
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
			ENDIF
    NEXT Count
  UNTIL swapped == TRUE
  Id[index] <-- num
  IF Age <= 6
  	THEN AgeBand[index] <-- 0
  	ELSE 
	  	IF Age <= 10
	  		THEN AgeBand[index] <-- 1
	  		ELSE AgeBand[index] <-- 2
	  	ENDIF
	ENDIF 
EndSub

//gets the index of a given id
SubProcedure getIndex(MyId)
  FOR Count <-- 0 TO 19
  	IF Id[Count] == MyId
  		THEN RETURN Count
  	ENDIF
  NEXT Count
  RETURN -1
EndSub

//add a time, and update pb, and fastest from each age range
SubProcedure addTime(Index, Time)
  Timings[Index][Runs[Index]] <-- Time
  Runs[Index] <-- Runs[Index] + 1
  IF pb[Index] == 0
  	THEN pb[Index] <-- Time
  	ELSE 
  		IF pb[Index] > Time
  			THEN pb[Index] <-- Time
  		ENDIF
  ENDIF
  IF FastestAge[AgeBand[Index]] == -1
  	THEN FastestAge[AgeBand[Index]] <-- Index
  	ELSE 
  		IF pb[FastestAge[AgeBand[Index]]] > Time
  			THEN FastestAge[AgeBand[Index]] <-- Index
  		ENDIF
  ENDIF
EndSub

//finds out the time duration
SubProcedure getSpan(Start, End)
  Time <-- End - Start
  RETURN Time
EndSub

//init all runners
FOR Count <-- 0 TO 19
  REPEAT
    INPUT Name, Age
  UNTIL Age < 4 AND Age > 14
  Names[Count] <-- Name
  Ages[Count] <-- Count
  makeID(Name, Age, Count)
  pb[Count] <-- 0
  Runs[Count] <-- 0 
NEXT Count

//input and add all times of the runners
FOR Count <-- 0 TO 19
	// verify id
	REPEAT
		INPUT MyId
		Index <-- getIndex(MyId)
		IF Index == -1
			THEN PRINT 'ENTER AGAIN'
	UNTIL Index == -1
	
	// add times till -1 is not input as starttime or max number is reached 
	FOR Run <-- 0 TO 51
		INPUT StartTime and EndTime
		Time <-- getSpan(StartTime, EndTime)
		IF StartTime == -1
			THEN BREAK
		addTime(Index, Time) 
	NEXT Run
NEXT Count
	
//print the fastest from each age range
PRINT "Fastest from range 4 to 6 is " + Names[FastestAge[0]] + "with a time of" + pb[FastestAge[0]]
PRINT "Fastest from range 7 to 10 is " + Names[FastestAge[1]] + "with a time of" + pb[FastestAge[1]]
PRINT "Fastest from range 11 to 14 is " + Names[FastestAge[2]] + "with a time of" + pb[FastestAge[2]]

//print the bands each runner won
FOR Count <-- 0 TO 19
	IF Runs[Count] / 11 >= 2
		THEN PRINT Names[Count] + 'has a full marathon wristband'
		ELSE
			IF Runs[Count] / 11 >= 1
				THEN PRINT Names[Count] + 'has a half marathon wristband'
				ELSE PRINT Names[Count] + 'has no wristband'
			ENDIF
	ENDIF	
NEXT Count
```
