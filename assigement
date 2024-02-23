
;;; Directives
	PRESERVE8
	THUMB 
; Vector Table Mapped to Address 0 at Reset
; Linker requires __Vectors to be exported
	AREA RESET, DATA, READONLY
	EXPORT __Vectors
__Vectors 
	DCD 0x20001000 ; stack pointer value when stack is empty
	DCD Reset_Handler ; reset vector
 
	ALIGN
 
ARRAY DCD 43,-56,27,-5,38,5,-46,-2,17,-1,'#'               ;this where i declare the array 
ARRAY_POINTER DCD ARRAY                ;create a pointer to the array to make easier to recall

SUM RN R0            ;the sum of elements          ; declare the variables -give each regeister a name
SUMP RN R3           ;the sum of postive elements 
SUMN RN R4           ;the sum of negative elements 
ENERGY RN R5         ; a variable to save the energy
COUNTER RN R6        ; a counter to use it the division
SUME RN R8           ; the summation of the squared elemnts 
CROSSING RN R12      ; a variable to save the number of crosses


	AREA MYCODE, CODE, READONLY
ENTRY
	EXPORT Reset_Handler
Reset_Handler


	LDR R1,ARRAY_POINTER       ;load the first element of the array to R1 but it is the address
	MOV SUM,#0                 ;give initial values for each 
	MOV SUMP,#0
	MOV SUMN,#0
	MOV ENERGY,#0
	MOV COUNTER,#0
	MOV SUME,#0
	MOV CROSSING,#0
SUM_LOOP   						;starting the summation loop 
;;in this loop all opreations will be done 
	
	LDR R2,[R1]                   ;Load number at each time 
	CMP R2,'#'                    ;if the element was # which is a character the this array is ended and 
	BEQ END_SUM_LOOP              ;it will exsit the loop
    ADD COUNTER,COUNTER,#1        ; increament the loop
	
	CMP COUNTER,#2                ; compare the counter with 2 means as lond as the counter is more than 2 then
	                              ;go and find the crossing 
	BHS FIND_CROSSING             ; do Barnch to find_crossing if the counter is more than or same to 2
		
CONTINUE                          ;this label here to come back from the Find_crossing Label -come back-

	CMP R2,#0                     ;check if R2 has 0 , if it is 0 then it is not postive and not negative 
	
	ADDPL SUMP,SUMP,R2            ;Addpl it is like a function that add only the postive elements 
	ADDMI SUMN,SUMN,R2            ;Addmi add only the negative elements 
	
	ADD SUM,SUM,R2                ; add the element to the summation 
	ADD R1,R1,#4                  ;increament R1 so it do the next number in the next loop
	MUL R7,R2,R2                  ; muliply by itself -as a power 2-
	ADD SUME,SUME,R7              ;sum the squared element to energy summation 
	
	MOV R9,R2                     ;we do copy to use it in the crossing -easier to compare--this will be i+1 in the next loop-
	
    B SUM_LOOP                    ;go back and do the loop aganin
	
END_SUM_LOOP                      ;end the loop 

	UDIV ENERGY,SUME,COUNTER      ;divide the energy summation by the counter
	B STOP                        ; branch to go to the end of program                      
	
FIND_CROSSING                     ; this label to some how split the crossing prossec from the code and
                                  ;connect them by branches 
	BIC R10,R9,0x7FFFFFFF         ;do the AND logical opration to compare the MSB  between  the two elements 
	BIC R11,R2,0x7FFFFFFF         
	
	CMP R10,R11                   ; if the two elements with differenet signs
	ADDNE CROSSING,CROSSING,#1    ; the increament the crossing counter
	B CONTINUE                    ; back to the main loop 
	
STOP B STOP                       ; do to stop which ends the program 
	END 