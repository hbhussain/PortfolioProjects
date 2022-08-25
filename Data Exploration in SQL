--SELECT *
--FROM PortfolioProject..CovidDeaths$
--ORDER BY 3,4


--SELECT *
--FROM PortfolioProject..CovidVaccination$
--ORDER BY 3,4

--Select Data that we are going to be using

Select location, date, total_cases, new_cases, total_deaths, population
from PortfolioProject..CovidDeaths$
order by 1,2

-- Looking at Total cases vs Total Death
-- Shows likelihood of dying if you contract covid

Select location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as DeathPercentage 
from PortfolioProject..CovidDeaths$
where location like '%states%' and continent is not null
order by 1,2

--Looking at Total cases vs population 
--shows percentage of population got covid
Select location, date, population, total_cases,  (total_cases/population)*100 as PercentOfPopulationInfected 
from PortfolioProject..CovidDeaths$
where continent is not null
--where location like '%states%'
order by 1,2

--Looking at Country with highest infection rate compared to population
Select location, population, MAX(total_cases) as HighestInfectionCount,  MAX((total_cases/population))*100 as PercentOfPopulationInfected
from PortfolioProject..CovidDeaths$
where continent is not null
Group by location, population
order by PercentOfPopulationInfected desc

--Showing Countries with the highest death count per population

select continent, Max(cast(total_deaths as int )) as TotalDeathCount
from PortfolioProject..CovidDeaths$
where continent is not null
Group by continent 
order by TotalDeathCount desc


-- Global Numbers

select date, sum(new_cases) as total_cases, sum(cast(new_deaths as int)) as total_death, sum(cast(New_deaths as int))/sum(New_Cases)*100 as DeathPercentage
from PortfolioProject..CovidDeaths$
where continent is not null
Group by date
order by 1,2



select sum(new_cases) as total_cases, sum(cast(new_deaths as int)) as total_death, sum(cast(New_deaths as int))/sum(New_Cases)*100 as DeathPercentage
from PortfolioProject..CovidDeaths$
where continent is not null
---Group by date
order by 1,2


--looking at total population vs vaccination

select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, sum(convert(int, vac.new_vaccinations)) OVER (PARTITION by dea.location order by dea.location, dea.date)
from PortfolioProject..CovidDeaths$ dea
join PortfolioProject..CovidVaccination$ vac
on dea.location = vac.location and dea.date = vac.date
where dea.continent is not null
order by 2,3

--TEMP TABLE

create table #PercentPopulationVaccinated(
Continent nvarchar(255), 
location nvarchar(255),
date datetime,
population numeric,
New_vaccination numeric,
RollingPeopleVaccinated numeric
)

insert into #PercentPopulationVaccinated
select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, sum(convert(int, vac.new_vaccinations)) OVER (PARTITION by dea.location order by dea.location, dea.date) as RollingPeopleVaccinated
from PortfolioProject..CovidDeaths$ dea
join PortfolioProject..CovidVaccination$ vac
on dea.location = vac.location and dea.date = vac.date
--where dea.continent is not null

--creating view to store data for later visualization

create view PercentPopulationVaccinated as 
select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, 
sum(convert(int,vac.new_vaccinations)) --over(partition by dea.location order by dea.location,dea.date)as RollingPeopleVaccinated
from PortfolioProject..CovidDeaths$ dea
join PortfolioProject..CovidVaccination$ vac
on dea.location=vac.location
and dea.date=vac.date
where dea.continent is not null

select * from PercentPopulationVaccinated
