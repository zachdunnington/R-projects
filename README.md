# R-projects
# web scrape espn

library(rvest)
#free Agents from 2016
agents2016url <- 'http://www.espn.com/mlb/freeagents/_/type/dollars'
free_agents <- read_html(agents2016url)
free_agents
free_agents_tab <- html_nodes(free_agents, 'table')
free_agents <- html_table(free_agents_tab)[[1]]
head(free_agents)

newNamesFA <- c(free_agents[2,])
newNamesFA
names(free_agents) <- newNamesFA

free_agents <- free_agents[-(1:2),]

minLg <- c("Minor Lg")
free_agents <- free_agents[!(free_agents$DOLLARS == minLg | free_agents$RK == rk),]

#PLayer salaries
salaries <- NULL
playerSalariesUrl <- 'http://www.usatoday.com/sports/mlb/salaries/'
salaries <- read_html(playerSalariesUrl)
salaries
salaries_tab <- html_nodes(salaries, 'table')
salaries <- html_table(salaries_tab)[[1]]
head(salaries)

salaries <- salaries[,-1]
colnames(salaries)[colnames(salaries)=="Name"] <- "PLAYER"


#fielding stats from 2016
fielding2016url <- 'http://www.espn.com/mlb/stats/fielding/_/year/2016/seasontype/2'
fielding_stats2016 <- read_html(fielding2016url)


allFielding2016url <- c('http://www.espn.com/mlb/stats/fielding/_/year/2016/seasontype/2', 
                        'http://www.espn.com/mlb/stats/fielding/_/position/2b/order/true',
                        'http://www.espn.com/mlb/stats/fielding/_/position/3b/order/true',
                        'http://www.espn.com/mlb/stats/fielding/_/position/ss/order/true',
                        'http://www.espn.com/mlb/stats/fielding/_/position/lf/order/true',
                        'http://www.espn.com/mlb/stats/fielding/_/position/cf/order/true',
                        'http://www.espn.com/mlb/stats/fielding/_/position/rf/order/true')

  

for (i in allFielding2016url){
  scrapedData<- read_html(i)
  scrapedDataTab <- html_nodes(scrapedData, 'table')
  scrapedData <- html_table(scrapedDataTab)[[1]]
  scrapedDataFull <- rbind(scrapedDataFull, scrapedData)
}

print(head(scrapedDataFull))
print(tail(scrapedDataFull))
fielding2016Full <- scrapedDataFull


newNames <- c(fielding2016Full[2,])
newNames
names(fielding2016Full) <- newNames
rk <- c("RK")
sortField <- c("Sortable Fielding")

finFielding2016 <- fielding2016Full[!(fielding2016Full$RK== rk | fielding2016Full$RK== sortField),]


#####BATTING

#batting Stats from 2016

allBatting2016url <- c('http://www.espn.com/mlb/stats/batting/_/qualified/true', 
                       'http://www.espn.com/mlb/stats/batting/_/count/41/qualified/true',
                       'http://www.espn.com/mlb/stats/batting/_/count/81/qualified/true',
                       'http://www.espn.com/mlb/stats/batting/_/count/121/qualified/true')


scrapedDataFull1 <- NULL
for (i in allBatting2016url){
  scrapedData1 <- read_html(i)
  scrapedDataTab1 <- html_nodes(scrapedData1, 'table')
  scrapedData1 <- html_table(scrapedDataTab1)[[1]]
  scrapedDataFull1 <- rbind(scrapedDataFull1, scrapedData1)
}
batting2016Full <- scrapedDataFull1 

newNamesBat <- c(batting2016Full[2,])
newNamesBat
names(batting2016Full) <- newNamesBat
rk <- c("RK")
sortBatting <- c("Sortable Batting")

finBatting2016 <- batting2016Full[!(batting2016Full$RK== rk | batting2016Full$RK== sortBatting),]
