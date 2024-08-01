### Problem describtion:

The variable "Q2" is `character` type, and have 2 different forms of dates: 

![screenshot SAS problem](Images/Screenshot%202024-05-29%20105947.png) (screenshot SAS problem) 

When i try to convert it in **SAS** to type `date` it calculates number `44413` as `06AUG2081`. 

But **Excel** calculate it as `05AUG2021`. 

![screenshot Excel](Images/Screenshot%202024-05-29%20112144.png)(screenshot Excel) 

They became different dates. 

I tried many date types and formats in **SAS**. And have not fixed, i think the problem is the SAS read it in a different way than how excel read it. 

(Note: SAS reads `24/8/2021` form correctly when i convert it, the problem is the date in the number form) 

### Solution:

These lines fixed the type `char` and the values will be formatted to date format.

```sas
data want;
    set have;
    /* create new variable */
    if index(var_old,'/') then var_new = put(var_old, ddmmyy10.); /* if cell in m/d/y format (means it has '/' sign), just convert the type to date. */
        else var_new = put(var_old - 21916, ddmmyy10.); /* if cell in numeric form of 5 digits, add (-21916), then convert it to date type */
    drop var_old; /* drop old variable */
    rename var_new = var_old; /* rename new variable with old name */
run;
```

 

![screenshot SAS code](Images/Screenshot%202024-05-30%20113514.png)(screenshot SAS code) 

Result: 

![screanshot SAS result](Images/Screenshot%202024-05-30%20112947.png) (screanshot SAS result)



### Reference:

Back to this post for explanation:  [Re: Importing a date - SAS Support Communities](https://communities.sas.com/t5/SAS-Programming/Importing-a-date/m-p/161112#M31336) 

![screenshot reference](Images/Screenshot%202024-05-30%20113225.png)

### Done

bye
