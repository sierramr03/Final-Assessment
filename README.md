# Final-Assessment
Python Project for Data Science

#Question 1
tesla = yf.Ticker("TSLA")

tesla_data = tesla.history(period = 'max')

tesla_data.reset_index(inplace = True)
tesla_data.head()

#Question 2
url = ('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm')
html_data = requests.get(url)

soup = BeautifulSoup(html_data.text, 'html5lib')

tesla_revenue = pd.DataFrame(columns = ['Date', 'Revenue'])

for table in soup.find_all('table'): 
    if ('Tesla Quarterly Revenue' in table.find('th').text):
        rows = table.find_all('tr')
        
        for row in rows:
            col = row.find_all('td')

            if col != []:
                date = col[0].text
                revenue = col[1].text

                 # I recieved an error using .append
                 # I switched to pd.concat
                new_row = pd.DataFrame([{"Date": date, "Revenue": revenue}])
                tesla_revenue = pd.concat([tesla_revenue, new_row], ignore_index=True)
  
tesla_revenue["Revenue"] = tesla_revenue['Revenue'].str.replace(',|\$',"",regex=True)

tesla_revenue.dropna(inplace=True)

tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != ""]

tesla_revenue.tail()

#Question 3
gameStop = yf.Ticker("GME")

gme_data = gameStop.history(period = "max")

gme_data.reset_index(inplace= True)
gme_data.head()

#Question 4
URL2 = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html"
html_data_2 = requests.get(URL2)

soup = BeautifulSoup(html_data_2.text, 'html5lib')

gme_revenue = pd.DataFrame(columns=['Date', 'Revenue'])

for table in soup.find_all('table'):

    if ('GameStop Quarterly Revenue' in table.find('th').text):
        rows = table.find_all('tr')
        
        for row in rows:
            col = row.find_all('td')
            
            if col != []:
                date = col[0].text
                revenue = col[1].text.replace(',','').replace('$','')

                new_row2 = pd.DataFrame([{"Date": date, "Revenue": revenue}])
                gme_revenue = pd.concat([gme_revenue, new_row2], ignore_index=True)

gme_revenue.tail()

#Question 5
make_graph(tesla_data, tesla_revenue, "Tesla")

#Question 6
make_graph(gme_data, gme_revenue, 'GameStop')
