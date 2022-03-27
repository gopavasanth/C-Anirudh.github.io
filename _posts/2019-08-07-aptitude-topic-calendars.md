---
title: Aptitude Topic - Calendars
date: 2019-08-07 21:41:00 +0530
categories: [Academic Notes, Aptitude & Reasoning]
tags: [aptitude, calendar]
---

Years can be classified into two types, a leap year and a non-leap year. A leap year has 366 days while a non-leap year has 365 days.

```
For an year to be a leap year, it must be divisible by 4 but for centuries, it must also be
divisible by 400. 
So the years 1700 and 2200 are not leap years as they are not divisible by 400.
```

## Odd days
It is the number of days left after a week, i.e., the remainder we get when we divide a given number of days by 7. For example, in 72 days we have 2 odd days.

| Odd day | Associated day of the week |
| --- | --- |
| 0 | Sunday |
| 1 | Monday |
| 2 | Tuesday |
| 3 | Wednesday |
| 4 | Thursday |
| 5 | Friday |
| 6 | Saturday | 

```
A leap year has 2 odd days ( 366 % 7 ) while a non-leap year has 1 odd day. 
```

| Number of years | Total number of odd days |
| --- | --- |
| 100 | 5 |
| 200 | 3 |
| 300 | 1 |
| 400 | 0 |

*What was the day of the week on 21st June, 2019?*

> The steps we must follow are:
>  - Write the closest leap year which is a multiple of 400 as it will have 0 odd days
>  - Find the odd days for the rest of the years (taking into consideration the leap years) and the months.
>  - Check the day of the week corresponding to the final number of odd days we got.

**21June2019 = 2000years + 18years + Jan + Feb + Mar + Apr + May + 21daysofJune**

**oddDays = 0 + 22 + 3 + 0 + 3 + 2 + 3 + 0 = 33**


> If we divide by 7 again, we get the final odd days left as

**oddDays = 5**

```
Explanation
-----------

2000 years have 0 odd days. Then, till 2019 , 18 years have gone by which contribute 22 odd
days (4 leap years + 14 non-leap years). January , March and May give 3 odd days ( 31 % 7 )
and April gives 2 odd days ( 30 % 7 ). As 2019 is non-leap, February gives 0 odd days
( 28 % 7). As for the 21 days in June, they too give 0 odd days. As the number of odd days
we got is 5, the 21st day of June will be a Friday.
```

## Dooms Day method
This is a method to find the day of the week of a particular date.

```
Last day of February = 4th day of April = 6th day of June = 8th day of August = 10th day of October = 12th day of December
```

You can remember this as:
```
Last day of February = 4/4 = 6/6 = 8/8 = 10/10 =12/12
```

**Where the numerator refers to the day and the denominator to the month.**


The last day of February is called Dooms Day and can be calculated according to the equation given below:

**Doomsday = Anchorday + (YY/12) + R(YY/12) + [R(YY/12)]/4**

| Range of years | Number of Anchor days |
| --- | --- |
| 1800 - 1899 | 5 |
| 1900 - 1999 | 3 |
| 2000 - 2099 | 2 |
| 2100 - 2199 | 0 |

YY are the last two digits of the year for which we are finding and R is the reminder we get when we divide YY by 12. 


*What was the day of the week on 28th June, 2019?*

**Doomsday = 2 + 19/12 + [R.(19/12)] + [R.(19/12)]/4**

**Doomsday=2+1+7+1=11**

```
Here the year is 2019, so the Anchor day will be 2 and YY will be 19. The value of R will
be 19%12 which is equal to 7.

We take only the integer values from the division.
```

Now we got 11 days which has 4 odd days (11 % 7), the 4th odd day is a Thursday. Hence, the last day of February in 2019 was a Thursday. This implies that the 6th day of June is a Thursday, so is the 13th, 20th and 27th day of June. Thus June 28 will be a Friday.

*Different type of question: In what future year can we reuse the calendar of 2007 ?*

```
Jan 1st 2007 --> Monday
        2008 --> Tuesday
        2009 --> Thursday (As 2008 is leap year and gives 2 odd days)
        2010 --> Friday
        2011 --> Saturday
        2012 --> Sunday
        2013 --> Tuesday (As 2012 is a leap year)
        2014 --> Wednesday
        2015 --> Thursday
        2016 --> Friday
        2017 --> Sunday (As 2016 is a leap year)
        2018 --> Monday
```

**So 2007 's calendar can be reused in 2018.**
*In questions such as above we can only match a leap year with another leap year and a non-leap year with another non-leap year, i.e, if we started with a leap year, we must end at a leap year and NOT at a non-leap which has same week day.*