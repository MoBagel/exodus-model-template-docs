# Time component encoding

```python
    training_df, components = time_component_encoding(training_df)
```

This encodes the datetime columns in the dataframe to different numeric columns.

For example, suppose we have the following dataframe:

|ds|
|---|
|2022/01/01|
|2022/03/12|
|2022/05/23|
|2022/07/04|

After we perform time component encoding, the result will be:


|  ds|YEAR\_ds|QUARTER\_ds|MONTH\_ds|WEEK\_ds|WEEKDAY\_ds|DAY\_ds|HOUR\_ds|
|  --------|-----|-----|-----|-----|-----|-----|-----|
|2022/01/01|2022|1|1|52|6|1|0
|2022/03/12|2022|1|3|10|6|12|0
|2022/05/23|2022|2|5|21|1|23|0
|2022/07/04|2022|2|7|27|1|4|0
