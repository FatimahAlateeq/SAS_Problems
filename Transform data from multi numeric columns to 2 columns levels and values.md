### Describe the problem

I have 4 numeric columns i want to apply the ANOVA test on them, but i want the names of columns as levels. so, it will be 2 columns: levels (which is the variable names) and values (which is the variables values).

Before transforming:

![](C:\Users\abdul\AppData\Roaming\marktext\images\2024-08-14-12-54-26-image.png) (screenshot 1)

After transforming:

![](C:\Users\abdul\AppData\Roaming\marktext\images\2024-08-14-12-55-42-image.png) (screenshot 1)

Here are the names of variables changed to be coded as numbers (for example: 30_deg=1, 60_deg=2,....).

### Solution

The code:

```sas
/* transform data to be 2 columns only, column for levels, column for numbers. Note: you need to redifine the data before rerun the function. */
%let level_='30_deg'n '60_deg'n '90_deg'n '120_deg'n; /*  a macro for columns names (variables names) */)
%macro convert_two_columns(name_,input_data);
data temp; /* create empty dataset */
    attrib 
    level length=8
    inputs length=8;
    stop;
run;
    /* make loop inside function to work on each name */
  %do i=1 %to %sysfunc(countw (&name_));
        %let string = %scan(&name_, &i, " ");

    data leveled; /* get sigle level (variable) with its inputs */
    set &input_data(keep=&string);
    where not (missing(&string));
    level=&i; /* change the variable's name to number, it got a number according to its order in the macro (lavel_) */
    inputs=&string;
    drop &string;
    run;

    proc append base=temp data=leveled; /* append leveled data to the empty dataset */
    run;

    %end;
data &input_data; /* rename filled dataset (temp) with original data name */
set temp;
where not (missing(level));
run;
%mend convert_two_columns;
%convert_two_columns(&level_,Exe_8_2_1); /* call the functions with my variables and original dataset */
```

**Finally, if you have easer solution or if there is formal way in sas share it with me, please.**

### Done

bye
