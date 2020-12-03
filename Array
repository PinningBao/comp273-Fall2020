.data

beginarray: .word 2, 77,5, 5,9,10,6 -999			#’beginarray' with some contents	 DO NOT CHANGE THE NAME "beginarray"
array: .space 4000					#allocated space for ‘array'
newArray: .space 4000
str_command:	.asciiz "\nEnter a command (i, d, s or q): " # command to execute
insert: .asciiz "\nEnter an index:"
wrong: .asciiz "Invalid index"
value: .asciiz "Enter an value:"
currentArray: .asciiz "\nThe current array is "
	.text
	.globl main

main:	
	la  $a2,newArray	
	la $a0,array				#t0 contains the address of the array
	la $a1,beginarray			#t1 contains the address of the beginarray
	
	jal copyarray
	la $a1,array				#let a1 be the address of array inorder to call length 		
	jal printarray

	j io					#io subroutine are working for looping and checking
	
	
Exit:	li $v0,10
	syscall
#######################################################################subroutine:io
#works for looping and check the index you enter is valid
io:	add $t9,$a1,$0
ioloop:	li   $v0,4			
	la   $a0,str_command			#print"enter a command"
	syscall
	
	li   $v0,12				#read the command
	syscall
	
	
	
	li   $s2,'i'				
	beq  $v0,$s2,ForInsert			#if the command you enter is i then we could do the second step
	li   $s2,'d'
	beq  $v0,$s2,ForDelete
	li   $s2,'s'
	beq  $v0,$s2,ForSort
	li   $s2,'q'				
	beq  $v0,$s2,Exit
	
	j ioloop

ForInsert:
	add  $a1,$t9,$0
	jal Insert
	jal printarray
	j ioloop
ForDelete:
	add $a1,$t9,$0
	jal Delete
	jal printarray	
	j ioloop				#keep looping
ForSort:
	add $a1,$t9,$0
	jal Sort
	jal printarray
	j ioloop
####################################################################
##################################################################subroutine:Insert
#input:a1:array
#output:array after insertion
Insert: 
	addi $sp,$sp,-4        			# create 4 bytes on the stack to save $ra
	sw   $ra,0($sp)
	
						
       	jal length
       	add $s2,$zero,$v0			#s2 stores the length of the array
	
       	
       	li  $v0,4
       	la  $a0,insert				#if the command you enter is i then print:Enter an index
       	syscall
       	
       	li $v0,5				#read the index you entered, and stored it in v0
       	syscall
       	
       	add $t2,$zero,$v0			#t2 stores the index you enter
        beq $t2,0,start   
       	bge $t2,$s2,Error			#if the index you enter is larger than the length then print"INVALID INDEX"
 	bltz $t2,Error
 	j start
Error:
         li   $v0,4				
	 la   $a0,wrong 			#print:invalid index  
	 syscall
	 
	 lw   $ra,0($sp)			# restore $ra and 
	 addi $sp,$sp,4				# restore the stack
	 j ForInsert				#ask for the index
	
start:
	add $t9,$a1,$0				#t9 contains array 
						
	move $t3,$0				#t3=counter
	add $s1,$0,$t2				#let s1 stores the index you entered, so we could firstly copy the numbers before the index
						#then add the value you entered
					
	
	subu $s3,$s2,$t2 			#let s3 stores the address after the index,so we could copy them after adding the 
						#value you chose
	li $v0,4				
	la $a0,value				#print:enter a value
	syscall
	
	li $v0,5				#read the value you choose
	syscall
	
	add $s4,$zero,$v0			#s4 stores the value you want to insert	
			

ToInsert:
	
	
	bge $t3,$s1,InsertYours			#s1=the index you entered
						#t3=counter 
						#done the copying before index
				
	lw $t4,0($t9)				#load first element of the array into t4
	sw $t4,0($a2)				#save the first element of array into newarray
	
	addi $t9,$t9,4				#move the address of array to the second element
	addi $a2,$a2,4				#move the address of newarray to the second element	
	
	addi $t3,$t3,1				#increment the count
	j ToInsert

InsertYours:				
	sw $s4,0($a2)				#save the value you entered into array
	
	addi $a2,$a2,4				#move the address of newarray to the next address
	addi $s2,$s2,1				#increment the length
	
InsertAfter:	
	bge $t3,$s2,Back			#s2=new length
						#t3=counter
											
	lw $t4,0($t9)				#load the first element after the index in array
	sw $t4,0($a2)				#save the first element after the index into newarray
	
	addi $t9,$t9,4				#move the address to the second element
	addi $a2,$a2,4				#move the address to the second element
	
	addi $t3,$t3,1				#increment the count	
	j InsertAfter
Back:	
	
	subu $t9,$t9,$t3			#Move back the address of array 
	subu $t9,$t9,$t3
	subu $t9,$t9,$t3
	subu $t9,$t9,$t3
	
	addi $t3,$t3,1
	
	subu $a2,$a2,$t3     
  subu $a2,$a2,$t3 
  subu $a2,$a2,$t3 
 subu $a2,$a2,$t3 
	add $a1,$a2,$0
	add $a0,$t9,$0
	jal copyarray					
	lw   $ra,0($sp)				#restore $ra and 
	addi $sp,$sp,4				#restore the stack
	jr $ra
##################################################################subroutine:Delete
#input:a1:array
#output:array after delete
Delete: 
	addi $sp,$sp,-4        			# create 4 bytes on the stack to save $ra
	sw   $ra,0($sp)
	add $t9,$a1,$0				
						
  jal length
 add $s2,$zero,$v0			#s2 stores the length of the array
	
 li  $v0,4
 la  $a0,insert				#if the command you enter is i then print:Enter an index
  syscall
       	
 li $v0,5				#read the index you entered, and stored it in v0
 syscall
       	
  add $t2,$zero,$v0			#t2 stores the index you enter
            
 bge $t2,$s2,ErrorDelete			#if the index you enter is larger than the length then print"INVALID INDEX"
 	bltz $t2,ErrorDelete
 	
 	j startDelete
ErrorDelete:
         li   $v0,4				
	 la   $a0,wrong 			#print:invalid index  
	 syscall
	 
	 lw   $ra,0($sp)			# restore $ra and 
	 addi $sp,$sp,4				# restore the stack
	 j ForDelete				#ask for index again
	
startDelete:
	add $t9,$a1,$0
	move $t3,$0				#t3=counter
	add $s1,$0,$t2				#let s1 stores the index you entered
					
	
	#subu $s3,$s2,$t2 			#let s3 stores the address after the index,so we could copy them after adding the 
	#value you chose
DeleteBeforeIndex:
	
	
	bge $t3,$s1,DeleteYours			#s1=the index you entered
						#t3=counter 
						#done the copying before index
				
	lw $t4,0($t9)				#load first element of the array into t4
	sw $t4,0($a2)				#save the first element of array into newarray
	
	addi $t9,$t9,4				#move the address of array to the second element
	addi $a2,$a2,4				#move the address of newarray to the second element	
	
	addi $t3,$t3,1				#increment the count
	j DeleteBeforeIndex

DeleteYours:				
	
	addi $t9,$t9,4				#skip one index
	addi $s2,$s2,1				#increment the length
	
DeleteAfter:	
	bge $t3,$s2,BackForDelete		#s2=new length
						#t3=counter
											
	lw $t4,0($t9)				#load the first element after the index in array
	sw $t4,0($a2)				#save the first element after the index into newarray
	
	addi $t9,$t9,4				#move the address to the second element
	addi $a2,$a2,4				#move the address to the second element
	
	addi $t3,$t3,1				#increment the count	
	j DeleteAfter
BackForDelete:	
	
	subu $t9,$t9,$t3			#Move back the address of array 
	subu $t9,$t9,$t3
	subu $t9,$t9,$t3
	subu $t9,$t9,$t3
	
	
	
	subu $a2,$a2,$t3     
  subu $a2,$a2,$t3 
  subu $a2,$a2,$t3 
  subu $a2,$a2,$t3 
	add $a1,$a2,$0
	add $a0,$t9,$0
	jal copyarray					
	lw   $ra,0($sp)				#restore $ra and 
	addi $sp,$sp,4				#restore the stack
	jr $ra
##################################################################

						

############################################################### subroutine length
#input:$a1
#ouput:$v0

length:	add $v0,$zero,$zero			#use $v0 as the counter 
loop:	
	lw  $t5,0($a1)				#load the first element of beginarray
	beq $t5,-999,Exit3			#reach the null Character(passing through all the elements,we are done)
	addi $v0,$v0,1				#increment the counter
	addi $a1,$a1,4				#advance pointer by 4
	j loop

Exit3:	
	subu $a1,$a1,$v0
	subu $a1,$a1,$v0
	subu $a1,$a1,$v0
	subu $a1,$a1,$v0			#move the pointer to the initial position
						#we don't count -999			
	jr $ra
################################################################
################################################################ subroutine copyarray
#input:$a0,$a1
# a0 stores the address of array, a1 stores the address of beginarray
#a0 is the copy of a1
#No output
copyarray:
	addi $sp,$sp,-4				# create stack to save $ra
	addi $t2,$zero,0			#t2=0		
	sw   $ra,0($sp)   
	jal length				
	addi $s1,$v0,1				#$s1=length+1 ，so we could copy -999
	
copyloop:
	bge $t2,$s1,Exit1	
	lw $t3,0($a1)				#load first element of the begin array into t0
	sw $t3,0($a0)				#save the first element of begin array into array
	addi $a0,$a0,4				#move the address of array to the second element
	addi $a1,$a1,4				#move the address of beginarray to the second element	
	addi $t2,$t2,1				#increment the length
	j copyloop
Exit1:	
	subu $a0,$a0,$s1
	subu $a0,$a0,$s1
	subu $a0,$a0,$s1
	subu $a0,$a0,$s1			#move the pointer back to initial position
	subu $a1,$a1,$s1
	subu $a1,$a1,$s1
	subu $a1,$a1,$s1
	subu $a1,$a1,$s1
	lw   $ra,0($sp)				# restore $ra and 
	addi $sp,$sp,4				# restore the stack
	jr $ra

################################################################
################################################################subroutine printarray
#input:a1
printarray:
	li   $v0,4				
	la   $a0,currentArray 
	syscall
	addi $sp,$sp,-4				# create stack to save $ra
	sw   $ra,0($sp)
	jal  length				#get the length we need to print
	add $s2,$zero,$v0			#s2 stores the length we need to print			
	addi $t4,$zero,0			#initialize the counter
	move $t1,$a1				#t1 stores the address of array												
printloop:
	bge  $t4, $s2, ExitPrint			#load word from addrs and goes to the next addrs   		
	
	lw    $t5, 0($t1)
	addi  $t1, $t1,4 			#syscall to print value
   	li    $v0, 1      
   	move  $a0, $t5
    	syscall
   	 					#syscall number for printing character (space)
 	li    $a0, 32
   	li    $v0, 11  
   	syscall

	addi   $t4, $t4, 1
    	j      printloop

ExitPrint:
	li  $a0, '\n'
   	li  $v0, 11  
   	syscall
        subu $t1,$t1,$s2
        subu $t1,$t1,$s2
        subu $t1,$t1,$s2
        subu $t1,$t1,$s2
        move $a1,$t1
	lw   $ra,0($sp)				# restore $ra and 
	addi $sp,$sp,4				# restore the stack
	jr $ra		




		
###############################################################
 
###############################################################sort
Sort:   
       addi $sp,$sp,-4       
       sw   $ra,0($sp)
       jal length 
       add $s2,$v0,$0	     			#s2 stores the length of array
       add  $t1,$0,$0				#t1 as i
      
iLoop:
       beq  $t1,$s2,SortEnd			#if i=length,loop-end
						#if not:i++ ,j=0
						#go to j loop

       addi $t1,$t1,1				#i++
       
       addi $t2,$0,0				#j as 0

       
       j jLoop
jLoop:
       subu $t3,$s2,$t1				#t3=length-i, 
						#if j larger than it we go to next i-loop

        beq $t2,$t3,JBack	
	lw $t5,0($a1)
	lw $t6,4($a1)
	addi $t2,$t2,1
	bgt  $t5,$t6,swap			#if j+1 smaller than j,swap them
						#otherwise go to next address
					
	addi $a1,$a1,4
	j jLoop			
	
JBack:
	subu $a1,$a1,$t3
	subu $a1,$a1,$t3
	subu $a1,$a1,$t3
	subu $a1,$a1,$t3
	j iLoop	
swap:
	sw $t6,0($a1)				#swap them
	sw $t5,4($a1)
	addi $a1,$a1,4				#move a0 to next address
	j jLoop				

SortEnd:
      lw   $ra,0($sp)		
      addi $sp,$sp,4	
      # restore $ra and the stack	
      jr $ra
############################################################
