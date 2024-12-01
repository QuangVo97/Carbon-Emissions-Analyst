# Carbon-Emissions-Analyst

## Table product emissions
```
select * from product_emissions limit 5
```
### Resul
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|

## Analyst & conclusion part

### The products contribute the most to carbon emissions.
```
select   year as Year
        ,product_name as Product_Name
        ,ROUND(AVG(weight_kg),2) as Agv_Weight_kg
        ,round(avg(carbon_footprint_pcf),2) as Avg_Carbon_Footprint_pcf
from product_emissions 
group by year, product_name
order by carbon_footprint_pcf desc
limit 10
```
|Year|Product_Name|Agv_Weight_kg|Avg_Carbon_Footprint_pcf|
|----|------------|-------------|------------------------|
|2015|Wind Turbine G128 5 Megawats|600000.0|3718044.00|
|2015|Wind Turbine G132 5 Megawats|600000.0|3276187.00|
|2015|Wind Turbine G114 2 Megawats|400000.0|1532608.00|
|2015|Wind Turbine G90 2 Megawats|361000.0|1251625.00|
|2016|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|2272.33|191687.00|
|2013|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|140000.0|167000.00|
|2017|TCDE|12000.0|99075.00|
|2016|Mercedes-Benz GLE (GLE 500 4MATIC)|3500.0|91000.00|
|2014|Electric Motor|90.0|87589.00|
|2016|Mercedes-Benz S-Class (S 500)|2050.0|85000.00|

#### Conclusion: Wind turbines are the highest contributors to carbon emissions, especially the Wind Turbine G128 5 Megawats in 2015, with an astonishing 3,718,044 pcf. Automobiles and vehicles also have a noticeable carbon footprint. The Land Cruiser Prado, FJ Cruiser, Dyna trucks, and Mercedes-Benz models (GLE and S-Class) all show relatively high carbon footprints, with the Land Cruiser Prado in 2016 having 191,687 pcf. Smaller industrial products like the Retaining wall structure (136 tonnes of steel sheet piles) and the Electric Motor have much lower emissions in comparison but still contribute to the overall impact, particularly in heavy construction and manufacturing industries.

### The industry groups of top 10 products which have the most to carbon emissions
```
Select product_name, product.industry_group_id, Agv_Weight_kg,Avg_Carbon_Footprint_pcf
From (
select 	industry_group_id,
		product_name, ROUND(AVG(weight_kg),2) as Agv_Weight_kg
		,round(avg(carbon_footprint_pcf),2) as Avg_Carbon_Footprint_pcf
from product_emissions 
group by year, product_name
order by carbon_footprint_pcf desc
limit 10
 ) as product
 left join carbon_emissions.industry_groups as indgroup
 	on product.industry_group_id = indgroup.id
```
|product_name|industry_group|Agv_Weight_kg|Avg_Carbon_Footprint_pcf|
|------------|--------------|-------------|------------------------|
|Wind Turbine G128 5 Megawats|Electrical Equipment and Machinery|600000.0|3718044.00|
|Wind Turbine G132 5 Megawats|Electrical Equipment and Machinery|600000.0|3276187.00|
|Wind Turbine G114 2 Megawats|Electrical Equipment and Machinery|400000.0|1532608.00|
|Wind Turbine G90 2 Megawats|Electrical Equipment and Machinery|361000.0|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|Automobiles & Components|2272.33|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|Materials|140000.0|167000.00|
|TCDE|Materials|12000.0|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|Automobiles & Components|3500.0|91000.00|
|Electric Motor|Capital Goods|90.0|87589.00|
|Mercedes-Benz S-Class (S 500)|Automobiles & Components|2050.0|85000.00|

#### Conclusion: The Electrical Equipment and Machinery and Automobiles & Components sectors are the top contributors to carbon emissions, with large machinery and vehicles being key drivers


### The industries with the highest contribution to carbon emissions
```
select 	  industry_group
		, round(avg(carbon_footprint_pcf),2) as Avg_Carbon_Footprint_pcf
from product_emissions as product
left join carbon_emissions.industry_groups as indgroup
	on product.industry_group_id = indgroup.id
group by industry_group
order by avg(carbon_footprint_pcf) desc
Limit 10
```
|industry_group|Avg_Carbon_Footprint_pcf|
|--------------|------------------------|
|Electrical Equipment and Machinery|891050.73|
|Automobiles & Components|35373.48|
|"Pharmaceuticals, Biotechnology & Life Sciences"|24162.00|
|Capital Goods|7391.77|
|Materials|3208.86|
|"Mining - Iron, Aluminum, Other Metals"|2727.00|
|Energy|2154.80|
|Chemicals|1949.03|
|Media|1534.47|
|Software & Services|1368.94|

#### Conclusion: The industry with the highest contribution to carbon emissions is Electrical Equipment and Machinery, with an average carbon footprint of 891,050.73 pcf. This is significantly higher than other sectors. Following this, Automobiles & Components contributes 35,373.48 pcf on average, making it the second-largest emitter. Other industries like Pharmaceuticals, Biotechnology & Life Sciences and Capital Goods have comparatively lower emissions, averaging 24,162.00 pcf and 7,391.77 pcf, respectively

### The companies with the highest contribution to carbon emissions
```
select 	  company_name
		    , round(avg(carbon_footprint_pcf),2) as Avg_Carbon_Footprint_pcf
from product_emissions as product
left join carbon_emissions.companies as compgroup
	on product.company_id = compgroup.id
group by company_name
order by avg(carbon_footprint_pcf) desc
Limit 10
```
|company_name|Avg_Carbon_Footprint_pcf|
|------------|------------------------|
|"Gamesa Corporación Tecnológica, S.A."|2444616.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|83503.50|
|Weg S/A|53551.67|
|Daimler AG|43089.19|
|General Motors Company|34251.75|
|Volkswagen AG|26238.40|
|Waters Corporation|24162.00|
|"Daikin Industries, Ltd."|17600.00|
|CJ Cheiljedang|15802.83|

#### Conclusion: Gamesa Corporación Tecnológica stands out as the highest emitter, followed by major automotive manufacturers and heavy industry companies like Hino Motors and Arcelor Mittal. Other significant contributors include Weg S/A (53,551.67 pcf), Daimler AG (43,089.19 pcf), and General Motors Company (34,251.75 pcf), all of which are major players in the automotive and manufacturing sectors.

### The countries with the highest contribution to carbon emissions
```
select 	  country_name
	, round(avg(carbon_footprint_pcf),2) as Avg_Carbon_Footprint_pcf
from product_emissions as product
left join carbon_emissions.countries as contgroup
	on product.company_id = contgroup.id
where country_name is not null
group by country_name
order by avg(carbon_footprint_pcf) desc
Limit 10
```

|country_name|Avg_Carbon_Footprint_pcf|
|------------|------------------------|
|Germany|2444616.00|
|Greece|191687.00|
|Colombia|17600.00|
|Lithuania|13251.00|
|Italy|10000.00|
|India|9328.00|
|South Africa|3550.50|
|China|3499.50|
|Canada|3025.00|
|Belgium|2344.00|

#### Conclusion: Germany stands as the top emitter, followed by Greece and Colombia, with Lithuania, Italy, and India contributing lower but still notable carbon footprints.Other notable contributors include Lithuania (13,251 pcf), Italy (10,000 pcf), and India (9,328 pcf), with their emissions stemming from various industrial, manufacturing, and energy production processes.

### The trend of carbon footprints (PCFs) over the years
```
select   year
		,round(avg(carbon_footprint_pcf),2) as Avg_Carbon_Footprint_pcf
from product_emissions
group by year 
order by year desc
```
|year|Avg_Carbon_Footprint_pcf|
|----|------------------------|
|2017|4050.85|
|2016|6891.52|
|2015|43188.90|
|2014|2457.58|
|2013|2399.32|

#### Conclusion: The data suggests a sharp rise in carbon footprints in 2015, followed by a steady decline over the next two years (2016-2017). The increase in 2015 could have been due to the production or transportation of high-emission products, while the subsequent decrease reflects a reduction in emissions or changes in industry practices.

### The industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time
```
Select indgroup.industry_group as Industry_Group,
	round(avg(case when pro.year = 2013 then pro.carbon_footprint_pcf else 0 end),2) as 2013_Emission,
	round(avg(case when pro.year = 2014 then pro.carbon_footprint_pcf else 0 end),2) as 2014_Emission,
	round(avg(case when pro.year = 2015 then pro.carbon_footprint_pcf else 0 end),2) as 2015_Emission,
	round(avg(case when pro.year = 2016 then pro.carbon_footprint_pcf else 0 end),2) as 2016_Emission,
	round(avg(case when pro.year = 2017 then pro.carbon_footprint_pcf else 0 end),2) as 2017_Emission
From product_emissions as pro
 left join carbon_emissions.industry_groups as indgroup
 	on pro.industry_group_id = indgroup.id
 group by indgroup.industry_group
```
|Industry_Group|2013_Emission|2014_Emission|2015_Emission|2016_Emission|2017_Emission|
|--------------|-------------|-------------|-------------|-------------|-------------|
|"Consumer Durables, Household and Personal Products"|0.00|0.00|116.38|0.00|0.00|
|"Food, Beverage & Tobacco"|39.02|20.98|0.00|783.51|24.70|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|0.00|0.00|685.31|0.00|0.00|
|"Mining - Iron, Aluminum, Other Metals"|0.00|0.00|2727.00|0.00|0.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|10757.00|13405.00|0.00|0.00|0.00|
|"Textiles, Apparel, Footwear and Luxury Goods"|0.00|0.00|14.33|0.00|0.00|
|Automobiles & Components|1783.41|3150.89|11194.89|19244.29|0.00|
|Capital Goods|1719.71|2677.11|100.14|181.97|2712.83|
|Chemicals|0.00|0.00|1949.03|0.00|0.00|
|Commercial & Professional Services|26.30|10.84|0.00|65.68|16.84|
|Consumer Durables & Apparel|42.16|48.24|0.00|17.09|0.00|
|Containers & Packaging|0.00|0.00|373.50|0.00|0.00|
|Electrical Equipment and Machinery|0.00|0.00|891050.73|0.00|0.00|
|Energy|150.00|0.00|0.00|2004.80|0.00|
|Food & Beverage Processing|0.00|0.00|7.05|0.00|0.00|
|Food & Staples Retailing|0.00|32.21|29.42|0.08|0.00|
|Gas Utilities|0.00|0.00|61.00|0.00|0.00|
|Household & Personal Products|0.00|0.00|0.00|0.00|0.00|
|Materials|1113.96|420.43|0.00|490.37|1184.09|
|Media|643.00|643.00|127.93|120.53|0.00|
|Retailing|0.00|3.80|2.20|0.00|0.00|
|Semiconductors & Semiconductor Equipment|0.00|10.00|0.00|0.80|0.00|
|Semiconductors & Semiconductors Equipment|0.00|0.00|1.00|0.00|0.00|
|Software & Services|0.18|4.29|672.24|671.94|20.29|
|Technology Hardware & Equipment|228.84|626.82|397.59|5.87|103.34|
|Telecommunication Services|5.78|20.33|20.33|0.00|0.00|
|Tires|0.00|0.00|1011.00|0.00|0.00|
|Tobacco|0.00|0.00|1.00|0.00|0.00|
|Trading Companies & Distributors and Commercial Services & Supplies|0.00|0.00|39.83|0.00|0.00|
|Utilities|30.50|0.00|0.00|30.50|0.00|


#### Conclusion: Several industries have made remarkable progress in reducing their carbon footprints over the years. Automobiles & Components, Electrical Equipment and Machinery, and Pharmaceuticals, Biotechnology & Life Sciences show the most dramatic decreases, with some even reaching zero emissions by 2017. Other industries, such as Media, Software & Services, and Technology Hardware & Equipment, have also made significant strides in reducing their emissions, reflecting a broader trend toward sustainability across various sectors. These reductions could be attributed to various factors, including the adoption of cleaner technologies, improved energy efficiency, better environmental policies, and changes in production practices. However, some sectors still have a long way to go in completely eliminating their emissions, and continued efforts will be needed to drive further reductions.






