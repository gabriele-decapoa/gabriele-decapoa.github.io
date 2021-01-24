---
layout: post
title: Excel advanced built-in functions - ordering and dynamic refencences.
categories:
  - data_analysis
  - Excel
---

The simplest tool you could use to perform some data analysis is _Microsoft Excel_.
If you have _few_ structured data and you need to order them, filter them or graph them, Excel is perfect.  
Obviously, Excel is not a Big Data tool, as you could crash the process using it with lots and lots of data.

Sometimes you cannot leverage the UI tools to order data in ascending or descending order, so you need to use Excel's advanced built-in functions.  
To order your data in a new sheet in descending way, you could use `LARGE` function (in Italian version, `GRANDE`).
This function, which syntax is `LARGE(matrix; k)`, returns the `k`-th largest value in the dataset referenced in `matrix`.  
Vice versa, to order your data in a new sheet in ascending way, you could use `SMALL` function (in Italian version, `PICCOLO`): the syntax of this function is the same of `LARGE` function, but the behavior is different, as returns the `k`-th smallest value in dataset.  
Once you have ordered a colum of your dataset, you could get the related values of the row using a couple of functions together: `INDEX` (`INDICE`) and `COMPARE`(`CONFRONTA`), with also the `LARGE`/`SMALL` function.
The formula will be:
```
INDEX(dataset_column;COMPARE(LARGE(dataset_column_to_order;k);dataset_column_to_order;0))
```
With that formula, you could retrieve the value of the column `dataset_column` exactly related to the value retrived by `LARGE` function.
Obvioulsy, `dataset_column` and `dataset_column_to_order` must be Excel's refencences, so `B1:B4000` or `F3:F50000`.

Microsoft Excel is very useful also when you retrieve data from external source, like databases.
The cons of using Excel to order data retrieve from database is that you cannot know start and/or end range of your result set.  
When you face this problem, you could build dinamically the reference range, using `ADDRESS` (`INDIRZZO`) and `INDIRECT` (`INDIRETTO`).
Using `ADDRESS` function you could get a string representing a reference, while using `INDIRECT` function you could transform that string in real Excel reference.