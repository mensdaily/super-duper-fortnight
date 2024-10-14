# super-duper-fortnight
```ncl
&CONTROL SHRVARS=($DS)
* Define the list of jobs to be monitored
&JOB_LIST = 'JOB1,JOB2,JOB3,JOB4,JOB5,JOB6,JOB7,JOB8,JOB9,JOB10'

* Define the threshold for runtime in minutes
&RUNTIME_THRESHOLD = 60

* Split the job list and monitor each job
&CALL PROC=PARSELIST &PARM=&JOB_LIST

PARSELIST:
&VAR = &SUBSTR(0,:,&PARM,'')
&IF &VAR = '' &THEN &RETURN
&JOB_NAME = &SUBSTR(0,1,&VAR,',')
&PARM = &SUBSTR(2,:,&VAR,',')

* Check job status and runtime
&CALL PROC=CHECK_JOB &PARM=&JOB_NAME

* Check next job
&CALL PROC=PARSELIST &PARM=&PARM
&RETURN

CHECK_JOB:
&JOB_NAME = &1
* Save the actual start time if the job is not already being tracked
&IF &VARVAL(&'START_TIME_'||&JOB_NAME) = '' &THEN +
   &DO
      &ASSIGN &'START_TIME_'||&JOB_NAME = &TIME
   &END

* Check job status
&JOB_STATUS = &GET_JOB_STATUS(&JOB_NAME)

* Calculate the job runtime
&JOB_START_TIME = &VARVAL(&'START_TIME_'||&JOB_NAME)
&JOB_CURRENT_TIME = &TIME
&JOB_RUNTIME = &TIME_DIFF(&JOB_CURRENT_TIME, &JOB_START_TIME)

* If job is complete
&IF &JOB_STATUS = 'COMPLETE' &THEN +
   &DO
      &WRITE JOB &JOB_NAME COMPLETED SUCCESSFULLY
      &CALL CLIST='SYSSHR.ZOOPS.CLIST(FUNDTRAIN)'
      * Clear the start time variable
      &ASSIGN &'START_TIME_'||&JOB_NAME = ''
   &END

* If job runtime exceeds threshold
&IF &JOB_RUNTIME > &RUNTIME_THRESHOLD &THEN +
   &DO
      &ALERT JOB &JOB_NAME EXCEEDED RUNTIME THRESHOLD
      &CALL CLIST='SYSSHR.ZOOPS.CLIST(FUNDTRAIN)'
   &END
&RETURN

* Real-time job status function (replace with actual logic)
GET_JOB_STATUS:
&JOB_NAME = &1
* Placeholder lines to be replaced with actual job status checking logic
&IF &JOB_NAME = 'JOB1' &THEN &REPLY RUNNING
&IF &JOB_NAME = 'JOB2' &THEN &REPLY COMPLETE
&REPLY RUNNING

* Time difference calculation function
TIME_DIFF:
&START = &1
&END = &2
* Calculate the difference in minutes
&START_HHMMSS = &SUBSTR(8,6,&START)
&END_HHMMSS = &SUBSTR(8,6,&END)
&DIFF_HHMMSS = &TIMEDIF(&START_HHMMSS,&END_HHMMSS)
&REPLY &DIFF_HHMMSS
```


```ncl
&CONTROL SHRVARS=($DS)
* Define the list of jobs to be monitored
&JOB_LIST = 'JOB1,JOB2,JOB3,JOB4,JOB5,JOB6,JOB7,JOB8,JOB9,JOB10'

* Split the job list into individual job names
&CALL PROC=PARSELIST &PARM=&JOB_LIST

* Define the parse list procedure
PARSELIST:
&VAR = &SUBSTR(0,:,&PARM,'')
&IF &VAR = '' &THEN &RETURN
&JOB_NAME = &SUBSTR(0,1,&VAR,',')
&PARM = &SUBSTR(2,:,&VAR,',')

* Check job status for each job
&IF &JOB_STATUS(&JOB_NAME) = 'COMPLETE' &THEN +
   &DO
      &WRITE JOB &JOB_NAME COMPLETED SUCCESSFULLY
      &CALL CLIST='SYSSHR.ZOOPS.CLIST(FUNDTRAIN)'
   &END

* Check next job
&CALL PROC=PARSELIST &PARM=&PARM
&RETURN

* Define the job status function to check real-time status
JOB_STATUS:
&JOB_NAME = &1
* Placeholder for real-time job status check
* Replace the following lines with actual job status checking logic
&IF &JOB_NAME = 'JOB1' &THEN &REPLY COMPLETE
&IF &JOB_NAME = 'JOB2' &THEN &REPLY COMPLETE
&REPLY RUNNING
```

