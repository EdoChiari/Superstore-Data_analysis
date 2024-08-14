Superstore_project
================
2024-08-14

## Data info and cleaning

``` r
# Display the structure of the dataset
str(data)
```

    ## 'data.frame':    9994 obs. of  21 variables:
    ##  $ Row.ID       : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Order.ID     : chr  "CA-2016-152156" "CA-2016-152156" "CA-2016-138688" "US-2015-108966" ...
    ##  $ Order.Date   : chr  "11/8/2016" "11/8/2016" "6/12/2016" "10/11/2015" ...
    ##  $ Ship.Date    : chr  "11/11/2016" "11/11/2016" "6/16/2016" "10/18/2015" ...
    ##  $ Ship.Mode    : chr  "Second Class" "Second Class" "Second Class" "Standard Class" ...
    ##  $ Customer.ID  : chr  "CG-12520" "CG-12520" "DV-13045" "SO-20335" ...
    ##  $ Customer.Name: chr  "Claire Gute" "Claire Gute" "Darrin Van Huff" "Sean O'Donnell" ...
    ##  $ Segment      : chr  "Consumer" "Consumer" "Corporate" "Consumer" ...
    ##  $ Country      : chr  "United States" "United States" "United States" "United States" ...
    ##  $ City         : chr  "Henderson" "Henderson" "Los Angeles" "Fort Lauderdale" ...
    ##  $ State        : chr  "Kentucky" "Kentucky" "California" "Florida" ...
    ##  $ Postal.Code  : int  42420 42420 90036 33311 33311 90032 90032 90032 90032 90032 ...
    ##  $ Region       : chr  "South" "South" "West" "South" ...
    ##  $ Product.ID   : chr  "FUR-BO-10001798" "FUR-CH-10000454" "OFF-LA-10000240" "FUR-TA-10000577" ...
    ##  $ Category     : chr  "Furniture" "Furniture" "Office Supplies" "Furniture" ...
    ##  $ Sub.Category : chr  "Bookcases" "Chairs" "Labels" "Tables" ...
    ##  $ Product.Name : chr  "Bush Somerset Collection Bookcase" "Hon Deluxe Fabric Upholstered Stacking Chairs, Rounded Back" "Self-Adhesive Address Labels for Typewriters by Universal" "Bretford CR4500 Series Slim Rectangular Table" ...
    ##  $ Sales        : num  262 731.9 14.6 957.6 22.4 ...
    ##  $ Quantity     : int  2 3 2 5 2 7 4 6 3 5 ...
    ##  $ Discount     : num  0 0 0 0.45 0.2 0 0 0.2 0.2 0 ...
    ##  $ Profit       : num  41.91 219.58 6.87 -383.03 2.52 ...

``` r
# Get summary statistics for the dataset
summary(data)
```

    ##      Row.ID       Order.ID          Order.Date         Ship.Date        
    ##  Min.   :   1   Length:9994        Length:9994        Length:9994       
    ##  1st Qu.:2499   Class :character   Class :character   Class :character  
    ##  Median :4998   Mode  :character   Mode  :character   Mode  :character  
    ##  Mean   :4998                                                           
    ##  3rd Qu.:7496                                                           
    ##  Max.   :9994                                                           
    ##   Ship.Mode         Customer.ID        Customer.Name        Segment         
    ##  Length:9994        Length:9994        Length:9994        Length:9994       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##    Country              City              State            Postal.Code   
    ##  Length:9994        Length:9994        Length:9994        Min.   : 1040  
    ##  Class :character   Class :character   Class :character   1st Qu.:23223  
    ##  Mode  :character   Mode  :character   Mode  :character   Median :56430  
    ##                                                           Mean   :55190  
    ##                                                           3rd Qu.:90008  
    ##                                                           Max.   :99301  
    ##     Region           Product.ID          Category         Sub.Category      
    ##  Length:9994        Length:9994        Length:9994        Length:9994       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##  Product.Name           Sales              Quantity        Discount     
    ##  Length:9994        Min.   :    0.444   Min.   : 1.00   Min.   :0.0000  
    ##  Class :character   1st Qu.:   17.280   1st Qu.: 2.00   1st Qu.:0.0000  
    ##  Mode  :character   Median :   54.490   Median : 3.00   Median :0.2000  
    ##                     Mean   :  229.858   Mean   : 3.79   Mean   :0.1562  
    ##                     3rd Qu.:  209.940   3rd Qu.: 5.00   3rd Qu.:0.2000  
    ##                     Max.   :22638.480   Max.   :14.00   Max.   :0.8000  
    ##      Profit         
    ##  Min.   :-6599.978  
    ##  1st Qu.:    1.729  
    ##  Median :    8.666  
    ##  Mean   :   28.657  
    ##  3rd Qu.:   29.364  
    ##  Max.   : 8399.976

### Extra columns removal

``` r
# Drop columns Row ID, Order ID, Ship Date, Customer ID, Customer Name,Product ID,Country,Postal Code
data <- data[, !names(data) %in% c("Row.ID", "Order.ID","Ship.Date","Customer.ID", "Customer.Name", "Product.ID", "Country", "Postal Code")]
```

### Removal for duplicates and null variable

``` r
# Count missing values in each column
missing_counts <- colSums(is.na(data))
print(missing_counts)
```

    ##   Order.Date    Ship.Mode      Segment         City        State  Postal.Code 
    ##            0            0            0            0            0            0 
    ##       Region     Category Sub.Category Product.Name        Sales     Quantity 
    ##            0            0            0            0            0            0 
    ##     Discount       Profit 
    ##            0            0

``` r
# Identify duplicated rows and display them
duplicates <- data[duplicated(data), ]
print(duplicates)
```

    ##      Order.Date      Ship.Mode     Segment     City State Postal.Code Region
    ## 3407  4/23/2014 Standard Class Home Office Columbus  Ohio       43229   East
    ##       Category Sub.Category
    ## 3407 Furniture       Chairs
    ##                                                                         Product.Name
    ## 3407 Global Leather Highback Executive Chair with Pneumatic Height Adjustment, Black
    ##        Sales Quantity Discount   Profit
    ## 3407 281.372        2      0.3 -12.0588

``` r
# Remove subsequent duplicates while keeping the first occurrence
data <- data[!duplicated(data), ]

# Verify the dataset after removal
summary(data)
```

    ##   Order.Date         Ship.Mode           Segment              City          
    ##  Length:9993        Length:9993        Length:9993        Length:9993       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##     State            Postal.Code       Region            Category        
    ##  Length:9993        Min.   : 1040   Length:9993        Length:9993       
    ##  Class :character   1st Qu.:23223   Class :character   Class :character  
    ##  Mode  :character   Median :56560   Mode  :character   Mode  :character  
    ##                     Mean   :55192                                        
    ##                     3rd Qu.:90008                                        
    ##                     Max.   :99301                                        
    ##  Sub.Category       Product.Name           Sales              Quantity    
    ##  Length:9993        Length:9993        Min.   :    0.444   Min.   : 1.00  
    ##  Class :character   Class :character   1st Qu.:   17.280   1st Qu.: 2.00  
    ##  Mode  :character   Mode  :character   Median :   54.480   Median : 3.00  
    ##                                        Mean   :  229.853   Mean   : 3.79  
    ##                                        3rd Qu.:  209.940   3rd Qu.: 5.00  
    ##                                        Max.   :22638.480   Max.   :14.00  
    ##     Discount          Profit         
    ##  Min.   :0.0000   Min.   :-6599.978  
    ##  1st Qu.:0.0000   1st Qu.:    1.731  
    ##  Median :0.2000   Median :    8.671  
    ##  Mean   :0.1562   Mean   :   28.661  
    ##  3rd Qu.:0.2000   3rd Qu.:   29.364  
    ##  Max.   :0.8000   Max.   : 8399.976

We can see that now the lenght of each variable has lowered by one,
meaning that the first istance has been kept while the subsequent
duplicate has been removed.

### Trend of Sales, Profits and Discounts

``` r
# Convert 'Order Date' to Date format
data$Order.Date <- mdy(data$Order.Date)
str(data$Order.Date)
```

    ##  Date[1:9993], format: "2016-11-08" "2016-11-08" "2016-06-12" "2015-10-11" "2015-10-11" ...

``` r
# Aggregate sales data by month
monthly_sales <- data %>%
  group_by(Month = floor_date(`Order.Date`, "month")) %>%
  summarise(Sales = sum(Sales))

monthly_discount <- data %>%
  group_by(Month = floor_date(`Order.Date`, "month")) %>%
  summarise(Discount = sum(Discount))

monthly_profit <- data %>%
  group_by(Month = floor_date(`Order.Date`, "month")) %>%
  summarise(Profit = sum(Profit))

# Calculate average values
avg_sales <- mean(monthly_sales$Sales)
avg_discount <- mean(monthly_discount$Discount)
avg_profit <- mean(monthly_profit$Profit)

# Aggregate sales data by year
yearly_sales <- data %>%
  group_by(Year = floor_date(`Order.Date`, "year")) %>%
  summarise(Sales = sum(Sales))

yearly_discount <- data %>%
  group_by(Year = floor_date(`Order.Date`, "year")) %>%
  summarise(Discount = sum(Discount))

yearly_profit <- data %>%
  group_by(Year = floor_date(`Order.Date`, "year")) %>%
  summarise(Profit = sum(Profit))

# Create the line plot for monthly sales
plot_monthly_sales <- ggplot(monthly_sales, aes(x = Month, y = Sales)) +
  geom_line(color = "#00F5D4") +  # Line color
  geom_point(color = "#00F5D4") +  # Marker color
  geom_hline(yintercept = avg_sales, color = "black", linetype = "dashed") +  # Add average sales line
  labs(title = "Monthly Sales Trend", x = "Order Date", y = "Sales") +  # Add titles and labels
  theme_minimal() +  # Use a minimal theme
  theme(panel.grid.major = element_line(color = "gray", linetype = "dotted"),
        plot.title = element_text(hjust = 0.5))  # Add grid lines

# Create the line plot for monthly discount 
plot_monthly_discount <- ggplot(monthly_discount, aes(x = Month, y = Discount)) +
  geom_line(color = "#FEE440") +  
  geom_point(color = "#FEE440") +  
  geom_hline(yintercept = avg_discount, color = "black", linetype = "dashed") +  
  labs(title = "Monthly Discount Trend", x = "Order Date", y = "Discount") +  
  theme_minimal() + 
  theme(panel.grid.major = element_line(color = "gray", linetype = "dotted"),
        plot.title = element_text(hjust = 0.5)) 

# Create the line plot for monthly profit
plot_monthly_profit <- ggplot(monthly_profit, aes(x = Month, y = Profit)) +
  geom_line(color = "#00BBF9") +  
  geom_point(color = "#00BBF9") +  
  geom_hline(yintercept = avg_profit, color = "black", linetype = "dashed") +  
  labs(title = "Monthly Profit Trend", x = "Order Date", y = "Profit") +
  theme_minimal() +  
  theme(panel.grid.major = element_line(color = "gray", linetype = "dotted"),
        plot.title = element_text(hjust = 0.5)) 

# Create the line plot for the yearly sales
plot_yearly_sales <- ggplot(yearly_sales, aes(x = Year, y = Sales)) +
  geom_line(color = "#00F5D4") +  # Line color
  geom_point(color = "#00F5D4") +  # Marker color
  labs(title = "Yearly Sales Trend", x = "Order Date", y = "Sales") +  # Add titles and labels
  theme_minimal() +  # Use a minimal theme
  theme(panel.grid.major = element_line(color = "gray", linetype = "dotted"),
        plot.title = element_text(hjust = 0.5))  # Add grid lines

# Create the line plot for the yearly disocount
plot_yearly_discount <- ggplot(yearly_discount, aes(x = Year, y = Discount)) +
  geom_line(color = "#FEE440") +  
  geom_point(color = "#FEE440") +  
  labs(title = "Yearly Discount Trend", x = "Order Date", y = "Discount") +  
  theme_minimal() + 
  theme(panel.grid.major = element_line(color = "gray", linetype = "dotted"),
        plot.title = element_text(hjust = 0.5)) 

# Create the line plot for the yearly profit
plot_yearly_profit <- ggplot(yearly_profit, aes(x = Year, y = Profit)) +
  geom_line(color = "#00BBF9") +  
  geom_point(color = "#00BBF9") +  
  labs(title = "Yearly Profit Trend", x = "Order Date", y = "Profit") +
  theme_minimal() +  
  theme(panel.grid.major = element_line(color = "gray", linetype = "dotted"),
        plot.title = element_text(hjust = 0.5)) 
```

![](Superstore_project_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-5-4.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-5-5.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-5-6.png)<!-- -->

From these graphs we could answer various business related questions.

1.  Which month had the highest monthly Sales, Discount and Profit?

The highest values for the three categories seems to be achieved in
November 2017

``` r
# Filter the dataset for November 2017 and summarize Sales, Discount, and Profit
november_2017_summary <- data %>%
  filter(format(Order.Date, "%Y-%m") == "2017-11") %>%
  summarise(
    November_2017_Sales = sum(Sales, na.rm = TRUE),
    November_2017_Discount = sum(Discount, na.rm = TRUE),
    November_2017_Profit = sum(Profit, na.rm = TRUE)
  )

# Display the result
print(november_2017_summary)
```

    ##   November_2017_Sales November_2017_Discount November_2017_Profit
    ## 1            118447.8                  73.89             9690.104

2.  Which month had the lowest monthly Sales, Discount and Profit?

- Lowest monthly Sales: February 2014
- Lowest monthly Discount: February 2016
- Lowest monthly Profit: January 2015 (Loss)

``` r
# Calculate Sales for February 2014
february_2014_sales <- data %>%
  filter(format(Order.Date, "%Y-%m") == "2014-02") %>%
  summarise(February_2014_Sales = sum(Sales, na.rm = TRUE))

# Calculate Discount for February 2016
february_2016_discount <- data %>%
  filter(format(Order.Date, "%Y-%m") == "2016-02") %>%
  summarise(February_2016_Discount = sum(Discount, na.rm = TRUE))

# Calculate Profit for January 2015
january_2015_profit <- data %>%
  filter(format(Order.Date, "%Y-%m") == "2015-01") %>%
  summarise(January_2015_Profit = sum(Profit, na.rm = TRUE))

# Combine all results into a single data frame
summary_results <- cbind(february_2014_sales, february_2016_discount, january_2015_profit)

# Display the combined result
print(summary_results)
```

    ##   February_2014_Sales February_2016_Discount January_2015_Profit
    ## 1            4519.892                      8           -3281.007

3.  Which year showed the most growth in Sales, Discount and Profit?

The year that showed the most growth it’s the year 2017

4.  What is monthly average of sales, Discount And Profit in 2015 and
    2017?

``` r
# Filter data for the years 2015 and 2017
data_2015_2017 <- data %>%
  filter(format(Order.Date, "%Y") %in% c("2015", "2017"))

# Group by year and month and then calculate monthly sums
monthly_summary <- data_2015_2017 %>%
  group_by(Year = format(Order.Date, "%Y"), Month = format(Order.Date, "%m")) %>%
  summarise(
    Total_Sales = sum(Sales, na.rm = TRUE),
    Total_Discount = sum(Discount, na.rm = TRUE),
    Total_Profit = sum(Profit, na.rm = TRUE)
  ) %>%
  ungroup()

# Calculate the monthly averages for each year
monthly_averages <- monthly_summary %>%
  group_by(Year) %>%
  summarise(
    Avg_Sales = mean(Total_Sales),
    Avg_Discount = mean(Total_Discount),
    Avg_Profit = mean(Total_Profit)
  )

# Display the monthly averages for 2015 and 2017
print(monthly_averages)
```

    ## # A tibble: 2 × 4
    ##   Year  Avg_Sales Avg_Discount Avg_Profit
    ##   <chr>     <dbl>        <dbl>      <dbl>
    ## 1 2015     39211.         27.3      5135.
    ## 2 2017     61101.         43.2      7787.

### Sales, Profits and Discounts by state

``` r
# Summarize the data by State for each metric
state_summary <- data %>%
  group_by(State) %>%
  summarise(
    Total_Sales = sum(Sales, na.rm = TRUE),
    Total_Profit = sum(Profit, na.rm = TRUE),
    Total_Discount = sum(Discount, na.rm = TRUE)
  )

# Bar plot for Sales by State (Flipped Axes, Reversed Order, and Formatted Y-Axis)
barplot_sales <- ggplot(state_summary, aes(x = fct_rev(State), y = Total_Sales)) +
  geom_bar(stat = "identity", fill = "#00F5D4", color = "black") +
  labs(title = "Total Sales by State", x = "State", y = "Sales") +
  theme_minimal() +
  coord_flip() +  # Flip the axes
  scale_y_continuous(labels = comma) +  # Format y-axis labels
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5))   # Adjust text size

# Bar plot for Profit by State 
barplot_profit <- ggplot(state_summary, aes(x = fct_rev(State), y = Total_Profit)) +
  geom_bar(stat = "identity", fill = "#00BBF9", color = "black") +
  labs(title = "Total Profits by State", x = "State", y = "Profits") +
  theme_minimal() +
  coord_flip() +  
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
 

# Bar plot for Discount by State 
barplot_discount <- ggplot(state_summary, aes(x = fct_rev(State), y = Total_Discount)) +
  geom_bar(stat = "identity", fill = "#FEE440", color = "black") +
  labs(title = "Total Discounts by State", x = "State", y = "Discounts") +
  theme_minimal() +
  coord_flip() +  
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
```

![](Superstore_project_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-10-2.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-10-3.png)<!-- -->

#### Top 10 States by Sales, Profits and Discounts

``` r
# Filter the top 10 states by Sales, Profit, and Discount
top_10_sales <- state_summary %>% arrange(desc(Total_Sales)) %>% slice(1:10)
top_10_profit <- state_summary %>% arrange(desc(Total_Profit)) %>% slice(1:10)
top_10_discount <- state_summary %>% arrange(desc(Total_Discount)) %>% slice(1:10)

# Bar plot for Top 10 States by Sales
barplot_top_sales <- ggplot(top_10_sales, aes(x = fct_reorder(State, Total_Sales), y = Total_Sales)) +
  geom_bar(stat = "identity", fill = "#00F5D4", color = "black") +
  labs(title = "Top 10 States by Sales", x = "State", y = "Sales") +
  theme_minimal() +
  coord_flip() +  # Flip the axes
  scale_y_continuous(labels = comma) +  # Format y-axis labels
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
  # Adjust text size

# Bar plot for Top 10 States by Profit
barplot_top_profit <- ggplot(top_10_profit, aes(x = fct_reorder(State, Total_Profit), y = Total_Profit)) +
  geom_bar(stat = "identity", fill = "#00BBF9", color = "black") +
  labs(title = "Top 10 States by Profits", x = "State", y = "Profits") +
  theme_minimal() +
  coord_flip() +  
  scale_y_continuous(labels = comma) +  
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
  

# Bar plot for Top 10 States by Discount
barplot_top_discount <- ggplot(top_10_discount, aes(x = fct_reorder(State, Total_Discount), y = Total_Discount)) +
  geom_bar(stat = "identity", fill = "#FEE440", color = "black") +
  labs(title = "Top 10 States by Discounts", x = "State", y = "Discounts") +
  theme_minimal() +
  coord_flip() +  
  scale_y_continuous(labels = comma) + 
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
```

![](Superstore_project_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-12-2.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-12-3.png)<!-- -->

General Conclusions from the Graphs:

- High Sales vs. Profitability: States with the highest total sales
  (like Texas and California) don’t necessarily have the highest
  profits. In fact, Texas, despite having the highest sales, is
  experiencing significant losses, likely due to the substantial
  discounts offered. This suggests that high sales alone don’t guarantee
  profitability; effective cost management and pricing strategies are
  crucial.

- Impact of Discounts: There appears to be a correlation between high
  discounts and lower profitability. States that offer significant
  discounts (e.g., Texas and Tennessee) tend to have lower profits or
  even losses, indicating that excessive discounting might erode profit
  margins. Conversely, states with moderate discounts (e.g., California
  and New York) tend to show better profitability, suggesting a more
  balanced and effective use of discounts.

- Profit Outliers: States like California and New York, which are among
  the highest in terms of profit, have found a sweet spot between
  offering discounts and maintaining profitability. They manage to
  generate significant sales without overly relying on discounts,
  resulting in substantial profits.

- Low Sales and Profitability: States with low total sales generally
  show low profits as well, which is expected. However, if these states
  also have low discounts, it might suggest that their sales volumes are
  too low to cover fixed costs, or that they might need to reconsider
  their pricing or marketing strategies to improve sales.

- Sales and Profit Distribution: The data suggests a wide disparity in
  sales and profitability across states. A few states dominate in terms
  of both sales and profit, while many others lag significantly behind.
  This could indicate market saturation in top-performing states or
  untapped potential in underperforming ones.

Strategic Insights:

- Balancing Discounts and Profit: Excessive discounting does not always
  lead to better financial outcomes. States that carefully balance
  discounts while focusing on driving sales through other means (e.g.,
  better marketing, customer loyalty programs) tend to perform better in
  terms of profitability.

- Focus on Underperforming States: States with low sales and profits
  might benefit from targeted strategies to boost market presence and
  customer engagement. This could involve tailored promotions, localized
  marketing campaigns, or reassessing product offerings to better meet
  local demand.

- Leverage High-Performing States: States that are already performing
  well in terms of both sales and profit could serve as models for
  strategies that could be applied to other regions. Understanding what
  drives success in these states could help in replicating that success
  elsewhere.

#### Top 10 Cities by Sales, Profits and Discounts

``` r
# Summarize the data by Cities for each metric
cities_summary <- data %>%
  group_by(City) %>%
  summarise(
    Total_Sales = sum(Sales, na.rm = TRUE),
    Total_Profit = sum(Profit, na.rm = TRUE),
    Total_Discount = sum(Discount, na.rm = TRUE)
  )

# Filter the top 10 Cities by Sales, Profit, and Discount
top_10_cities_sales <- cities_summary %>% arrange(desc(Total_Sales)) %>% slice(1:10)
top_10_cities_profit <- cities_summary %>% arrange(desc(Total_Profit)) %>% slice(1:10)
top_10_cities_discount <- cities_summary %>% arrange(desc(Total_Discount)) %>% slice(1:10)

# Bar plot for Top 10 States by Sales
barplot_top_city_sales <- ggplot(top_10_cities_sales, aes(x = fct_reorder(City, Total_Sales), y = Total_Sales)) +
  geom_bar(stat = "identity", fill = "#00F5D4", color = "black") +
  labs(title = "Top 10 Cities by Sales", x = "City", y = "Sales") +
  theme_minimal() +
  coord_flip() +  # Flip the axes
  scale_y_continuous(labels = comma) +  # Format y-axis labels
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5)) # Adjust text size

# Bar plot for Top 10 States by Profit
barplot_top_city_profit <- ggplot(top_10_cities_profit, aes(x = fct_reorder(City, Total_Profit), y = Total_Profit)) +
  geom_bar(stat = "identity", fill = "#00BBF9", color = "black") +
  labs(title = "Top 10 Cities by Profits", x = "City", y = "Profits") +
  theme_minimal() +
  coord_flip() +  
  scale_y_continuous(labels = comma) +  
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 

# Bar plot for Top 10 States by Discount
barplot_top_city_discount <- ggplot(top_10_cities_discount, aes(x = fct_reorder(City, Total_Discount), y = Total_Discount)) +
  geom_bar(stat = "identity", fill = "#FEE440", color = "black") +
  labs(title = "Top 10 Cities by Discounts", x = "City", y = "Discounts") +
  theme_minimal() +
  coord_flip() +  
  scale_y_continuous(labels = comma) + 
  theme(axis.text.y = element_text(size = 8, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
```

![](Superstore_project_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-14-2.png)<!-- -->![](Superstore_project_files/figure-gfm/unnamed-chunk-14-3.png)<!-- -->

### Other visualizations

#### Count of Category and Sub-category

``` r
#Count the occurrences of each category
category_counts <- data %>%
  count(Category) %>%
  arrange(desc(n))

#Count the occurrences of each sub-category
subcategory_counts <- data %>%
  count(Sub.Category) %>%
  arrange(desc(n))

#Create the category barplot
category_barplot <- ggplot(category_counts, aes(x = reorder(Category, -n), y = n)) +
  geom_bar(stat = "identity", fill = "steelblue") +  # Create bar plot
  labs(title = "Category Counts", x = "Category", y = "Count") +  # Add titles and labels
  theme_minimal() +  # Use a minimal theme
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5)) # Rotate x-axis labels for readability

#Create the sub-category barplot
subcategory_barplot <- ggplot(subcategory_counts, aes(x = reorder(Sub.Category, -n), y = n)) +
  geom_bar(stat = "identity", fill = "steelblue") + 
  labs(title = "Sub-category Counts", x = "Sub-category", y = "Count") +
  theme_minimal() + 
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
```

![](Superstore_project_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

#### Distribution of Shipments

``` r
#Calculate the counts and percentages of Ship Mode
ship_mode_counts <- data %>%
  count(Ship.Mode) %>%
  mutate(Percentage = n / sum(n) * 100)

# Step 2: Create the pie chart
pie_chart <- ggplot(ship_mode_counts, aes(x = "", y = Percentage, fill = Ship.Mode)) +
  geom_bar(stat = "identity", width = 1) +
  coord_polar(theta = "y") +
  labs(title = "Distribution of Ship Mode", fill = "Ship Mode") +
  theme_void() +  # Remove background, grids, and axis lines
  theme( plot.title = element_text(hjust = 0.5)) +
  geom_text(aes(label = paste0(round(Percentage, 1), "%")),
            position = position_stack(vjust = 0.5)) +  # Add percentage labels inside the pie slices
  scale_fill_manual(values = c("#007d7c", "#a6cee3", "#1f78b4", "#b2df8a", "#33a02c", "#fb9a99", "#e31a1c"))  #Define custom colors
```

![](Superstore_project_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

#### Count and Distribution of Customer Segment

``` r
# Calculate the counts of the Segment variable
segment_counts <- data %>%
  count(Segment)

# Create a color palette with the right number of colors
num_segments <- nrow(segment_counts)
colors <- scales::hue_pal()(num_segments)

# Create the pie chart for Segment
pie_chart_segment <- ggplot(segment_counts, aes(x = "", y = n, fill = Segment)) +
  geom_bar(width = 1, stat = "identity") +
  coord_polar(theta = "y") +
  theme_void() +  # Clean background and remove axis labels
  geom_text(aes(label = paste0(round(n / sum(n) * 100, 1), "%")),
            position = position_stack(vjust = 0.5)) +  # Add percentage labels
  scale_fill_manual(values = colors)  # Dynamically generated colors

# Create the bar chart for Segment
bar_chart_segment <- ggplot(segment_counts, aes(x = Segment, y = n, fill = Segment)) +
  geom_bar(stat = "identity", color = "black") +
  geom_text(aes(label = n), vjust = -0.3, size = 4) +  # Add values on top of the bars
  labs(y = "Count", x = "Segment") +
  theme_minimal() +
  scale_fill_manual(values = colors) +  # Use the same dynamic color palette
  theme(legend.position = "none")  # Remove the legend
```

![](Superstore_project_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->
