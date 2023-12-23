# Chemicals-in-cosmetics
**Questions Answered**

--Find out which chemicals were used the most in cosmetics and personal care products
select chemicalname, count(chemicalname) as ChemicalsUsedTheMost
from cosmetics
where ChemicalName is not null and PrimaryCategory = 'Personal Care Products'
group by ChemicalName
order  by count(chemicalname) desc

--Find out which companies used the most reported chemicals in their cosmetics and personal care products and makeup products 
select BrandName,count(InitialDateReported) as ChemicalsReported
from cosmetics
where PrimaryCategory = 'Personal Care Products' or PrimaryCategory = 'Makeup Products (non-permanent)'
group by BrandName
order by count(InitialdateReported) desc


--Identify the brands that had chemicals that were mostly reported in 2018
select Brandname,count(InitialDateReported) as Initialreporteddate 
from cosmetics
where InitialDateReported between '2010-01-01' and '2010-12-31'
group  by brandname
order by count(InitialDateReported) desc

--Which brands had chemicals discontinued and removed
select Brandname
from cosmetics
where DiscontinuedDate is not null and ChemicalDateRemoved is not null
group by BrandName
order by count(DiscontinuedDate) desc, count(ChemicalDateRemoved) desc


--identify the period of creation of the removed chemicals and when they were actually removed
select Chemicalname,ChemicalCreatedAt,ChemicalDateRemoved,
abs(datediff(month,chemicaldateremoved,ChemicalCreatedAt)) as removalperiod
from cosmetics
where ChemicalDateRemoved is not null and ChemicalCreatedAt is not null

--can you tell if discontinued chemicals in bath products were removed 
select Productname,Brandname,ChemicalName,
case 
    when discontinueddate is not null and chemicaldateremoved is not null 
	then 'yes'
	else 'no'
	end as DiscontinueStatus
	from cosmetics
	where primarycategory='Bath Products'

--identify the relationship between cemicals that were recently reported and discontinued (does most recently reported chemicals equal discontinuation of such chemicals 

select chemicalname,MostRecentDateReported,discontinueddate,
case 
    when MostRecentDateReported is not null and discontinueddate is not null
	then 'yes'
	else 'no'
	end as status
	from cosmetics


