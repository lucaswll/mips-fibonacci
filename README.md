# mips-fibonacci
Fibonacci function in Assembly of MIPS

# Code
Made using MARS simulator.

	.data
STR_N:  
  .asciiz "Entre com N: "
N_ESIMO:
  .asciiz "O N-Esimo valor é: "
STR_N_0:
  .asciiz "Para N=0, Fibonacci é 0"
	.text
	addi $t0, $zero, 2	    #t0 <- 2 para usar no ble
	
	la   $a0, STR_N
	addi $v0, $zero, 4
	syscall			    #chamará PRINT STR
	
	addi $v0, $zero, 5	    #v0 <- N
	syscall			    #chamará READ INT
	
	beq  $v0, $zero, NEHZERO     #se n=0, FBNCC(0)=0. Como a série é uma soma, esse valor ñ interfere nos calculos (já dou um branch)
	
	add  $a0, $v0, $zero	    #argumento (a0) da função FBNCC <- N (v0)
	jal  FBNCC
	add  $t1, $v0, $zero 	    #t1 <- v0 :: t1 recebe o resultado FBNCC(N)
	
	la   $a0, N_ESIMO
	addi $v0, $zero, 4
	syscall	
	
	add  $v0, $zero, 1
	add  $a0, $t1, $zero
	syscall	
	
	addi $v0, $zero, 10
	syscall
	
FBNCC:	
  addi $sp, $sp, -12          #"reservando" 3 palavras na pilha
	sw   $ra, 0($sp)
	sw   $s0, 4($sp)
	sw   $s1, 8($sp)
	
	add  $s0, $a0, $zero	    #s0 <- argumento (N)
	addi $v0, $zero, 1	    #v0 <- 1 (usarei para caso N <= 2)	
	ble  $s0, $t0, EXIT_FBNCC   #quando N for <= 2, saia da funcao FBNCC (significa que o retorno é o 1 (v0<-1)
	addi $a0, $s0, -1	    #fzd com que argumento (a0) receba FBNCC(n-1)
	jal  FBNCC		    #N=1,2, FBNCC(N)=1;  p/ caso N=0, fiz o beq no inicio
	
	add  $s1, $v0, $zero	    #s1 <- FBNCC(N-1) :: s1 recebe o resultado da funcao FBNCC(N-1)
	addi $a0, $s0, -2 	    #fzd com que argumento (a0) receba FBNCC(n-2)
	jal  FBNCC
	
	add  $v0, $s1, $v0	    #v0 <- s1 + v0 :: v0 <- FBNCC(n-1) + FBNCC(n-2)
	
EXIT_FBNCC:
	lw   $ra, 0($sp)
	lw   $s0, 4($sp)
	lw   $s1, 8($sp)
	addi $sp, $sp, 12
	jr   $ra		    #finalizo FBNCC e retorno para a chamadora (jal)
	
NEHZERO:la   $a0, STR_N_0
	addi $v0, $zero, 4
	syscall
