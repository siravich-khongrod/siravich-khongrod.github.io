# Automating Downloads on American Fact Finder
#### 2019/07/18

American Fact Finder is a handy tool that allows the user to navigate to data tables of census data. Despite having the API for downloading from the American Community Survey, I occasionally need to download data from the Fact Finder simply because I do not know which dataset I was looking for. As far as I know, the API does not provide a tool that allow searching using keywords such as 'tenure'.

Let's get started. The Fact Finder is an asynchronous web application, meaning that parts of the page loads up after the request was sent by the client.
This implies that I cannot use the requests library to download the tables.
For this reason, I used Selenium webdriver to create an instance of an automated browser to navigate to the downloads. 
The snnippet below is to configure the browser and browse the search page.

``` python
import time
from selenium import webdriver
browser = webdriver.Chrome(executable_path=r'C:\Users\skhongro.DPU\Documents\chromedriver.exe')
link='https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml?refresh=t'
browser.get(link)
```

Next is the major part of automation. The way this works is by looking for the search box, input the keyword, click search, and click the checkboxes on results for download.
In this case, I want to download the five year estimate ACS data of the tables I listed on first line from 2009 to 2010.
Note that there is a limitation of 40 maximum tables on a single download. It is possible to simply download one batch at a time or create multiple instances of automated browser.
Sit back and enjoy the auto clicking.

``` python
tbl_nbrs = ["B01001","B15002","B03002","B17001","B23001","B25002","B25004","B25024","B25032","B25007","B11016","B25106","B25064","B25063","B19013","B19001","B25072","B19019","B19001"]

for tbl_nbr in tbl_nbrs[1:5]:
    print(tbl_nbr)
    browser.find_element_by_class_name('remove-it').click()
    time.sleep(3)
    browser.find_element_by_id('searchTopicInput').click()
    browser.find_element_by_id('searchTopicInput').send_keys(tbl_nbr)
    browser.find_element_by_id('df-go-div').find_element_by_class_name('button-g').click()
    time.sleep(1)
    cols=browser.find_element_by_id('resulttable').find_elements_by_class_name('yui-dt-col-d_dataset')
    check = []
    for col in cols[1:]:
        if(col.text[5:]=='ACS 5-year estimates'): check.append(1)
        else: check.append(0)
    checkboxes = browser.find_elements_by_name('prod')
    for x in range(0,len(check)):
        if(check[x]==1):
            checkboxes[x].click()
```

After the tables have been chosen, you can now click download. Since I am downloading 40 tables at a time, I do not want to wait until all files a processed before I can hit download.
This is why I added some extra codes to watch the button for me.
Although this is not a good design, I just only want a quick piece of code that works.
If this were to be executed regularly I would recommend using <a href="https://selenium-python.readthedocs.io/waits.html">waits</a> to wait for a DOM element to be ready.
Same goes for the automation part which relies on the sleep function.

``` python
while(1):
    if(browser.find_element_by_id('yui-gen5-button').is_enabled()):
        browser.find_element_by_id('yui-gen5-button').click()
        break
    time.sleep(5)
# browser.find_element_by_id('yui-gen14-button').is_enabled()
```
