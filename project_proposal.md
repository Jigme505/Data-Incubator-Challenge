# Exploratory Data Analysis for Project Proposal

### Author: Jigme Norbu

## The Data

I used the World Bank website to download all my data in .xls format. Then I converted them into .csv format and loaded it into R:


```r
setwd("C:/Users/Jigme505/Desktop/Data Incubator Project/")

economy_size_df <- read.csv("WB dataset/1_size_of_the_economy.csv", na.strings = "..")
private_sector_df <- read.csv("WB dataset/2_private_sector_in_the_economy.csv", na.strings = "..")
enterprise_survey_df <- read.csv("WB dataset/3_business_environment_enterprise_survey.csv", na.strings = "..")
doing_business_ind_df <- read.csv("WB dataset/4_business_environment_Doing_business_indicators.csv", na.strings = "..")
stock_markets_df <- read.csv("WB dataset/5_stock_markets.csv", na.strings = "..")
finan_acss_stab_and_effi_df <- read.csv("WB dataset/6_Financial_access_stability_and_efficiency.csv", na.strings = "..")
tax_policies_df <- read.csv("WB dataset/7_tax_policies.csv", na.strings = "..")
military_exp_df <- read.csv("WB dataset/8_military_expenditure_and_arms_transfers.csv", na.strings = "..")
power_communication_df <- read.csv("WB dataset/9_power_and_communications.csv", na.strings = "..")
information_society_df <- read.csv("WB dataset/10_the_information_society.csv", na.strings = "..")
sci_and_tech_df <- read.csv("WB dataset/11_science_and_technology.csv", na.strings = "..")
```

## Collecting the relevant variables



```r
economy_size_df_n <- economy_size_df[-c(1:3, 232:309),]
countries <- as.character(economy_size_df_n$X)
population <- as.numeric(as.character(economy_size_df_n$Population))
GNI <- as.numeric(as.character(economy_size_df_n$Gross.national.income..Atlas.method))
GNI_per_capita <- as.numeric(as.character(economy_size_df_n$Gross.national.income.per.capita..Atlas.method))
pop_density <- as.numeric(as.character(economy_size_df_n$Population.density))
PPP_GNI <- as.numeric(as.character(economy_size_df_n$Purchasing.power.parity.gross.national.income))
PPP_per_capita <- as.numeric(as.character(economy_size_df_n$X.1))
GDP <- as.numeric(as.character(economy_size_df_n$Gross.domestic.product))
GDP_per_capita <- as.numeric(as.character(economy_size_df_n$X.2))

##
private_sector_df_n <- private_sector_df[-c(1:3, 232:298),]
dom_crdt_to_priv_sec <- as.numeric(as.character(private_sector_df_n$Domestic.credit.to.private.sector)) #as % of GDP
new_business_registered <- as.numeric(as.character(private_sector_df_n$Businesses.registered))

##
enterprise_survey_df_n <- enterprise_survey_df[-c(1:3,232:316),]
reg_and_tax_time <- as.numeric(as.character(enterprise_survey_df_n$Regulations.and.tax)) # % of the senior managment time
num_visit_tax_office <- as.numeric(as.character(enterprise_survey_df_n$X.1)) 
licenses_obt_time <- as.numeric(as.character(enterprise_survey_df_n$Permits.and.licenses))  # time taken to get operating license
corruption <- as.numeric(as.character(enterprise_survey_df_n$Corruption)) # % of firms w/atleast 1 Bribery incidence
crime <- as.numeric(as.character(enterprise_survey_df_n$Crime))  # % of sales lost due to crime
informal_compete <- as.numeric(as.character(enterprise_survey_df_n$Informality)) # % of firms competing with unregistered firms
finanacing <- as.numeric(as.character(enterprise_survey_df_n$Finance)) # % of firms using banks to finance working capital
trade <- as.numeric(as.character(enterprise_survey_df_n$Trade)) # avg time(days) to clear exports through custom
training_workforce <- as.numeric(as.character(enterprise_survey_df_n$Workforce)) # % of firms offering formal training

##
doing_business_ind_df_n <- doing_business_ind_df[-c(1:3, 232:363),]
starting_bness_process <- as.numeric(as.character(doing_business_ind_df_n$Starting.a.business)) # number of procedures required to start a business
starting_bness_time <- as.numeric(as.character(doing_business_ind_df_n$X.1))  # time (days) required to start a business
starting_bness_cost <- as.numeric(as.character(doing_business_ind_df_n$X.2))  # cost of starting business as % of per capita income

## 
finan_acss_stab_and_effi_df_n <- finan_acss_stab_and_effi_df[-c(1:3, 232:338),]
risk_premium_lending <- as.numeric(as.character(finan_acss_stab_and_effi_df_n$Risk.premium.on.lending))    # Prime lending rate minus treasury bill rate

##
sci_and_tech_df_n <- sci_and_tech_df[-c(1:3, 232:326),]
RD_expenditure <- as.numeric(as.character(sci_and_tech_df_n$Expenditures.for.R.D))    # % of GDP

## 
stock_markets_df_n <- stock_markets_df[-c(1:3, 232:300),]
market_capitalization <- as.numeric(as.character(stock_markets_df_n$X.1))         # market capitalization (2012) [no.of shares x outstanding share prices]

##
tax_policies_df_n <- tax_policies_df[-c(1:3, 232:310),]
business_tax_total <- as.numeric(as.character(tax_policies_df_n$X.6))   # total tax(including, profit, labor, others) tax payable by bussinesses as % of commercial profit
```

## Creating a new data frame and subdata

```r
newdata<- data.frame(countries,population, pop_density, GNI, 
                           GNI_per_capita, GDP, GDP_per_capita, PPP_GNI, PPP_per_capita,
                           dom_crdt_to_priv_sec, new_business_registered,reg_and_tax_time, 
                           num_visit_tax_office,licenses_obt_time, corruption,crime, 
                           informal_compete,finanacing,trade, training_workforce,
                           starting_bness_process, starting_bness_time, starting_bness_cost,
                           risk_premium_lending, RD_expenditure, market_capitalization,
                           business_tax_total)


region_df <- newdata[221:228,]
country_df <- newdata[1:220,]
```

## Graphs and Plots

Here I used rcharts to make a highplot 

```r
require(devtools)
require(rCharts)

h1 = hPlot(x = "business_tax_total", y = "starting_bness_cost",
           size ="pop_density", data= region_df, group="countries",
           type = "bubble")
h1$title(text="Cost of Starting a New Business")
h1$xAxis(startOnTick = FALSE, endOnTick = FALSE, title=list(enabled = TRUE, text='Business Taxation (% of commercial profit)'))
h1$yAxis(min = 0, max = 60, startOnTick = FALSE, endOnTick = FALSE, 
           title=list(enabled = TRUE, text='Cost (% of GNI per capita)'))
h1$plotOptions(bubble = list(dataLabels = list(enabled = TRUE, x = 0, 
                                            formatter="#! function() {
                                            return this.point.name;
                                            } !#", style=list(color= 'black'))))
h1$save('h1.html', cdn=TRUE)
browseURL('h1.html')
```


