**This is a'How to...', not a programming problem.**

### Randomization schedule using SAS Proc Plan for CASE STUDY: A TWO-TREATMENT, TWOPERIOD, TWO-SEQUENCE CROSS-OVER DESIGN

**Why blocks?** Block randomization helps to increase the comparability of the treatment groups, particularly when patient characteristics may change over time, as a result, for example, of changes in recruitment policy. It also provides a better guarantee that the treatment groups will be of nearly equal size. In crossover trials it provides the means of obtaining balanced designs with their greater efficiency and easier interpretation. Care should be taken to choose block lengths that are sufficiently short to limit possible imbalance, but that are long enough to avoid predictability towards the end of the sequence in a block.

**Seed:** By using the same seed, we can get the same random number. This provides us the possibility of reproducing a randomization schedule. (the seed is by default generated from reading the time of day from the computer’s clock).

**Block size:** (length) should be a factor of sample size.

**NVALS:** lists *n* (sequence) numeric values for the factor. (By default, the procedure uses NVALS=(1 2 3 ... *n*)).



### Code

```sas
/* n=24, i choose block length=6, so number of blocks=24/6=4 */
title1 "RANDOMIZATION SCHEDULE";
title2 "A Single-Dose, Randomized, Twoperiod Crossover Study";

proc plan seed=122700;
	factors block=4 random sequence=6 random/noprint;
	output out=first sequence nvals=(1 1 1 2 2 2) random;
	run;

data first(keep=pid sequence period1 period2);
	set first;
	length period1 period2 $35;
	pid=put(_n_, z3.);

	if sequence=1 then
		do;
			period1='T';
			period2='R';
		end;

	if sequence=2 then
		do;
			period1='R';
			period2='T';
		end;
run;

proc print data=first noobs double uniform split='*';
	var pid sequence period1 period2;
	label pid="PATIENT*ID" period1="PERIOD 1" period2="PERIOD 2";
run;
```

Result:

| PATIENT ID | sequence | PERIOD 1 | PERIOD 2 |
| ---------- | --------:| -------- | -------- |
| 001        | 1        | T        | R        |
| 002        | 2        | R        | T        |
| 003        | 2        | R        | T        |
| 004        | 1        | T        | R        |
| 005        | 2        | R        | T        |
| 006        | 1        | T        | R        |
| 007        | 2        | R        | T        |
| 008        | 1        | T        | R        |
| 009        | 1        | T        | R        |
| 010        | 2        | R        | T        |
| 011        | 1        | T        | R        |
| 012        | 2        | R        | T        |
| 013        | 1        | T        | R        |
| 014        | 1        | T        | R        |
| 015        | 2        | R        | T        |
| 016        | 1        | T        | R        |
| 017        | 2        | R        | T        |
| 018        | 2        | R        | T        |
| 019        | 2        | R        | T        |
| 020        | 1        | T        | R        |
| 021        | 2        | R        | T        |
| 022        | 2        | R        | T        |
| 023        | 1        | T        | R        |
| 024        | 1        | T        | R        |

### 

### References:

[SUGI 27: Generating Randomization Schedules Using SAS(r) Programming](https://support.sas.com/resources/papers/proceedings/proceedings/sugi27/p267-27.pdf)

### Done

bye
