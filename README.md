[![alt text](https://github.com/Justmaister/Calculation-Groups-in-DAX/blob/master/Images/tabular_editor_icon.png)](https://www.sqlbi.com/tools/tabular-editor/)

# Calculation Groups with Tabular Editor                

Calculation Groups is a set of calculation items created on Tabular Editor that are applied on top of existing DAX measures. 

To write this DAX expresions you should download the last version of [Tabular Editor].

With calculation groups we can easily create different functions for all different measures created on the Tabular Model that help all the Power BI developers and save a lot of time, also it take the BI Reports to another level of interactivity, as is the creation of the different Time Intelligence functions, and now we have the ability to format each of the calculation items.  

![alt text](https://github.com/Justmaister/Calculation-Groups-in-DAX/blob/master/Images/Format_String.PNG)

In the file “Calculation Groups Demo.pbix” you could find the implementation of Calculation Groups in 3 different scopes. 

-	Time Intelligence
-	Metrics
-	Currency Format 

### Time Intelligence Calculations

The most common use of Calculation Group is on the implementation of different time Intelligence calculations over the measures on the model and the other common use is the creation of slicer where you could select the measure to implement. 

```sh
CY = SELECTEDMEASURE ()
PY = VAR PrevYear =
        CALCULATE ( SELECTEDMEASURE (), SAMEPERIODLASTYEAR ( 'Date'[Date] ) )
    VAR CurrYear =
        SELECTEDMEASURE ()
    VAR Result =
        IF ( ISBLANK ( CurrYear ), BLANK (), PrevYear )
    RETURN
        Result
QTD = CALCULATE (
        SELECTEDMEASURE (),
        DATESQTD ( 'Date'[Date] )
    )
YTD = CALCULATE (
        SELECTEDMEASURE (),
        DATESYTD ( 'Date'[Date] )
    )
YOY = VAR CurrYear =
        SELECTEDMEASURE ()
    VAR PrevYear =
        CALCULATE ( SELECTEDMEASURE (), SAMEPERIODLASTYEAR ( 'Date'[Date] ) )
    VAR Result =
        IF (
            ISBLANK ( CurrYear )  || ISBLANK ( PrevYear ),
            BLANK (),
            CurrYear - PrevYear
        )
    RETURN
        Result
```

With the new update of Power BI where we can format each measure inside the calculation group you could format them to show the correct format for each value.

![alt text](https://github.com/Justmaister/Calculation-Groups-with-Tabular-Editor/blob/master/Images/Time%20intelligence%20CG%202.png)

As show in the image above insering the "Time Calc" (which is the name of the calculation group) on the Columns values it shows all the values for the "Sales Amount" metric for all the metrics developed on the Tabular Editor and formated in the way it has been written in each metric. 

### Calculated Groups in Metrics 

To give a more interactive approach to our Report we can create a calculation group for each metrics we already have calculated in the report. 

![alt text](https://github.com/Justmaister/Calculation-Groups-with-Tabular-Editor/blob/master/Images/Calculated%20groups%20Metricas.png)

And format them to show the correct value as it could be the Quantity by YoY %.

```
IF (
    SELECTEDVALUE ( 'Time Intelligence'[Time Calculation] ) = "YOY%",
    "#,##%;-#,##%;0%",
    "#,###.##;-#,###.##; 0"
)
```
With this aproach, we can creat crossfilter between the both calculation groups created depending on the metrics filtered in the Report. So now we have one graph that has 42 KPIs in there whith the creation of only 13 metrics and 2 Calculation Groups, now the graph shows different results depending on the filters selected and Formated in the way each meaure need to be seen for the end users. 

![alt text](https://github.com/Justmaister/Calculation-Groups-with-Tabular-Editor/blob/master/Images/Calculated%20groups%20Grafica.png)
    
One of the bigest advantages of having develop calculation groups on Tabular Editor is that before the introduction of Calculation Groups we should create a table with all the name of the Measures and the order of them to be shown in the filter and later create a measure to select them.

![alt text](https://github.com/Justmaister/Calculation-Groups-with-Tabular-Editor/blob/master/Images/Create%20Table.png)

![alt text](https://github.com/Justmaister/Calculation-Groups-with-Tabular-Editor/blob/master/Images/Measure%20Create%20Table.png)

This approach could not give us the oportunity to do a cross filter between the Time Intelligence Calulation Group developed before unless you write a DAX measure with all the combinations that we would develop wichi is impossible to maintain in the time if we need some changes and it is confusing and time consuming. 


![alt text](https://github.com/Justmaister/Calculation-Groups-in-DAX/blob/master/Images/Calculation_Groups_in_Action.PNG)

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


[Tabular Editor]: <https://www.sqlbi.com/tools/tabular-editor/>
