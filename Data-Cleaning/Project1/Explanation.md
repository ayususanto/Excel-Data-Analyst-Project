## Data Cleaning - Project 1 : Artist Tour Revenue
This data contains 21 rows representing Artist Tour Revenue. This dataset is dirty of unwanted columns, null vales, symbols etc.  
Source: `https://www.kaggle.com/datasets/amruthayenikonda/dirty-dataset-to-practice-data-cleaning/data)`  
I implement formula on power query (add costum column) as explained below:  


**Data Cleaning Step**
1. **Looking Missing Values**  
We can see on the table that there are  many empty cells on peak & all the time peak columns. It gives me a sight that what columna are necessary to keep and for this case I want to remove peak, all time peak, and ref. column.  
   
| Rank | Peak  | All Time Peak | Actual gross    | Adjusted gross (in 2022 dollars) | Artist       | Tour title                               | Year(s)   | Shows | Average gross | Ref.     |
|------|-------|---------------|-----------------|----------------------------------|--------------|------------------------------------------|-----------|-------|---------------|----------|
| 9    |       |               | $256,084,556    | $312,258,401                     | Beyoncé      | The Formation World Tour                 | 2016      | 49    | $5,226,215    | [13]     |
| 10   |       |               | $250,400,000    | $309,141,878                     | Taylor Swift | The 1989 World Tour                      | 2015      | 85    | $2,945,882    | [14]     |
| 11   |       |               | $229,100,000[b] | $283,202,896                     | Beyoncé      | The Mrs. Carter Show World Tour          | 2013–2014 | 132   | $1,735,606    | [15][16] |
| 12   |       | 14[17]        | $227,400,000    | $295,301,479                     | Lady Gaga    | The Monster Ball Tour *                  | 2009–2011 | 203   | $1,118,227    | [18]     |
| 13   |       |               | $204,000,000    | $251,856,802                     | Katy Perry   | Prismatic World Tour                     | 2014–2015 | 151   | $1,350,993    | [19]     |
| 14   | 1[20] |               | $200,000,000    | $299,676,265                     | Cher         | Living Proof: The Farewell Tour ‡[21][a] | 2002–2005 | 325   | $615,385      | [20]     |
| 15   | 2[c]  |               | $194,000,000    | $281,617,035                     | Madonna      | Confessions Tour                         | 2006      | 60    | $3,233,333    | [5]      |
| 16   |       |               | $184,000,000    | $227,452,347                     | Pink         | The Truth About Love Tour                | 2013–2014 | 142   | $1,295,775    | [22]     |
| 17   |       |               | $170,000,000    | $213,568,571                     | Lady Gaga    | Born This Way Ball                       | 2012–2013 | 98    | $1,734,694    | [d]      |
| 18   |       |               | $169,800,000    | $207,046,755                     | Madonna      | Rebel Heart Tour                         | 2015–2016 | 82    | $2,070,732    | [4]      |
| 19   |       |               | $167,700,000[e] | $204,486,106                     | Adele        | Adele Live 2016                          | 2016–2017 | 121   | $1,385,950    | [25]     |
| 20   |       |               | $150,000,000    | $185,423,109                     | Taylor Swift | The Red Tour                             | 2013–2014 | 86    | $1,744,186    | [26]     |


2. **Removing Symbols**  
   There are a lot of symbols on the table. 
   - **Actual Gross, Adjusted Gross, and Average Gross Column**  
     *The Formula*
     ```=Text.Select(Text.BeforeDelimiter([#"Column Name"],"["),{"0".."9"})```  

     *The Cleaned Column*
        | Actual Gross   | Adjusted Gross | Average Gross |
        |----------------|----------------|---------------|
        | 780.000.000    | 780.000.000    | 13.928.571    |
        | 579.800.000    | 579.800.000    | 10.353.571    |
        | 411.000.000    | 560.622.615    | 4.835.294     |
        | 397.300.000    | 454.751.555    | 2.546.795     |
        | 345.675.146    | 402.844.849    | 6.522.173     |

   - **Tour Tittle Column**  
     *The Formula*
      ```
      let  
          Source = [Tour title],  
          Result1 = Text.Trim(Text.Remove(Source, {"†", "‡", "*"})),  
          TourTitle1 = Text.BeforeDelimiter(Result1, "[")  
      
      in  
          TourTitle1
      ```
      *The Cleaned Column*
      | Tour Title                  |
      |-----------------------------|
      | The Eras Tour               |
      | Renaissance World Tour      |
      | Sticky & Sweet Tour         |
      | Beautiful Trauma World Tour |

    - **Year(s) Column**  
      I split the value into two column including Start Year and End Year using:  
      *The Formula* ```if Text.Contains([#"Year(s)"], "–") then Text.BeforeDelimiter([#"Year(s)"], "–") else [#"Year(s)"]```  
      *The Cleaned Column*  
      | Start Year | End Year |
      |------------|----------|
      | 2023       | 2024     |
      | 2023       | 2023     |
      | 2008       | 2009     |
      | 2018       | 2019     |  

3. **Evaluating the Accuracy of the Average Gross Column**  
   The result of average gross column is form devision between actual gross & number of shows. I combine power query and conditional formatting for this case. Conditional formatting is needed to highlight any values that differ from the original data.  
   *The Power Query Formula* ```=Number.Round ([Actual Gross] / [Shows])```  
   *The Conditional Formula* Select **Use formula to determine which cells to format** > Edit the rule with ```=[Actual Gross]<>[Comparison Volumn]```  
   
5. **Removing Duplicate**  
   The duplicate is happened on rank column which is based on actual gross column. So I sorted the column in descading order then add index column.

❓ Why Power Query?  
Even though the dataset is small (21 rows), I used Power Query to:
- Practice cleaning logic using a visual and structured tool
- Build a repeatable and scalable transformation workflow
- Show capability to automate tedious cleaning steps, especially useful in real-world scenarios with larger datasets

✅ Result:  
A clean, structured dataset ready for analysis or visualization.

     
   
