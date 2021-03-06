# Figuring out study plan with Web Scrapping

![Sankey Diagram Example](sankey-example.png)

The first time I went into the University website to browse through the course, I had difficulties in finding the course I should study first. Although the courses are in categories, the pre-requisites are not explicitly displayed. I have to click through each subject, browse the content and take note of the prerequisite on paper. 

I think having a simple list that displays the course name with the prerequisite would help students to better plan their course so I decided to Scrape this data from the school website and create a visualization that is more user friendly.


To do this, I will use the most common parsing library, <a href='https://www.crummy.com/software/BeautifulSoup/bs4/doc/'>BeautifulSoup</a> 

``` Python
import requests
from bs4 import BeautifulSoup
```

First, we want a list of all majors. I will start all master programs in the School of Computing and Digital Media.
``` Python
links = []
link='https://www.cdm.depaul.edu/academics/Pages/MastersDegrees.aspx'

r=requests.get(link)
soup = BeautifulSoup(r.text, 'html.parser')
# content = soup.findAll("div", {"class": "Index-Item"})
content = soup.findAll("ul", {"class": "dropdown-menu"})

uls = BeautifulSoup(str(content), 'html.parser')

for a in uls.findAll('a'):
    links.append('https://www.cdm.depaul.edu'+a['href'])

# no dropdown > "btn-requirements"
for item in soup.findAll("a", {"class": "btn-requirements"}):
    link = (item['href'])
    if('/academics/Pages/Current/Requirements' in (str.split(link,'-'))):
        links.append('https://www.cdm.depaul.edu'+link)

links

```

Here is the list of url that has the requirements:
```
https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MA-In-Animation-Motion-Graphics.aspx
https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MA-In-Animation-Technical-Artist.aspx
https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MA-In-Animation-Traditional-Animation.aspx
https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MA-In-Animation-3D-Animation.aspx
https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MS-in-Cybersecurity-Computer-Security.aspx
https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MS-in-Cybersecurity-Compliance.aspx
https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MS-in-Cybersecurity-Networking-and-Infrastructure.aspx
https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MS-In-Data-Science-Computational-Methods.aspx
...
```

Next we get the list of subjects
``` Python
links=[]
subjects = []
link = 'https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MS-In-Data-Science-Computational-Methods.aspx'
r=requests.get(link)
soup = BeautifulSoup(r.text, 'html.parser')
tds = soup.findAll("td", {"class": "CDMExtendedCourseInfo"})
for td in tds:
#     print(td.text)
    subject,courseno=str.split(td.text)
#     print('https://www.cdm.depaul.edu/academics/pages/courseinfo.aspx?Subject='+subject+'&CatalogNbr='+courseno)
    links.append('https://www.cdm.depaul.edu/academics/pages/courseinfo.aspx?Subject='+subject+'&CatalogNbr='+courseno)
    subjects.append(td.text)
subjects
```
```['IT 403', 'CSC 412', 'CSC 401', 'DSC 423', 'CSC 555', 'CSC 521', 'CSC 575', 'CSC 578', 'DSC 425', 'DSC 433', 'CSC 452', 'DSC 465', 'DSC 478', 'CSC 481', 'CSC 482', 'DSC 480', 'CSC 521', 'CSC 528', 'DSC 540', 'CSC 543', 'CSC 555', 'CSC 575', 'CSC 576', 'CSC 577', 'CSC 578', 'CSC 594', 'CSC 598', 'DSC 484', 'GEO 441', 'GEO 442', 'GPH 565', 'HCI 512', 'IPD 451', 'IS 549', 'IS 550', 'IS 574', 'IS 578', 'MGT 559', 'MKT 555', 'MKT 530', 'MKT 534', 'MKT 595']```


Now we can query the course catalogue for the prerequisites.
``` Python
def get_course_req(link):
    subjects = []
    r=requests.get(link)
    soup = BeautifulSoup(r.text, 'html.parser')
    print(soup.title.text.strip())
    tds = soup.findAll("td", {"class": "CDMExtendedCourseInfo"})
    for td in tds:
        subject,courseno=str.split(td.text)
        links.append('https://www.cdm.depaul.edu/academics/pages/courseinfo.aspx?Subject='+subject+'&CatalogNbr='+courseno)
        subjects.append(td.text)
    return list(set(subjects))

link='https://www.cdm.depaul.edu/academics/Pages/Current/Requirements-MS-In-Computational-Finance.aspx'
get_course_req(link)

def get_prereq(subjects):
    PRE = []
    for subject in subjects:
        Subject,CatalogNbr=str.split(subject)
        link='https://www.cdm.depaul.edu/academics/pages/courseinfo.aspx?Subject='+Subject+'&CatalogNbr='+CatalogNbr
        r=requests.get(link)
        soup = BeautifulSoup(r.text, 'html.parser')
        pageContent = soup.find("div", {"class": "pageContent"})
        pageContent = pageContent.find('p')
        if(pageContent):
            if(pageContent.text.lower().rfind("prerequisite")>0):
                prereq = pageContent.text.lower()[pageContent.text.lower().rfind("prerequisite"):]
                if('none' not in str.split(prereq)):
                    prereq = prereq_parser(prereq.upper(),subjects)
                    PRE.append([subject,prereq])
                else:PRE.append([subject,[]])
            else:PRE.append([subject,[]])
        else:
            PRE.append([subject,[]])
    return PRE
                
print(get_prereq(['CSC 540','CSC 471','CSC 242']))
```
Now we get the prerequisite in a list of list
```[['CSC 540', ['CSC 471']], ['CSC 471', []], ['CSC 242', []]]```

Final step is to visualize this in a sankey diagram

<div>
<svg viewBox="0,0,975,720" width="975" height="720" style="background: rgb(255, 255, 255); width: 100%; height: auto;"><g><rect x="1" y="475" height="135.00000000000023" width="13" fill="rgb(185, 185, 185)"><title>CSC 412
3</title></rect><rect x="641" y="363.05303406198345" height="90" width="13" fill="rgb(185, 185, 185)"><title>CSC 578
2</title></rect><rect x="321" y="259.9999999999999" height="89.99999999999994" width="13" fill="rgb(185, 185, 185)"><title>DSC 478
2</title></rect><rect x="1" y="5" height="224.99999999999997" width="13" fill="rgb(185, 185, 185)"><title>CSC 401
5</title></rect><rect x="321" y="59.99999999999996" height="89.99999999999996" width="13" fill="rgb(185, 185, 185)"><title>DSC 465
2</title></rect><rect x="1" y="239.99999999999997" height="225.00000000000003" width="13" fill="rgb(185, 185, 185)"><title>IT 403
5</title></rect><rect x="321" y="5" height="44.99999999999996" width="13" fill="rgb(185, 185, 185)"><title>CSC 452
1</title></rect><rect x="321" y="469.99999999999983" height="89.99999999999994" width="13" fill="rgb(185, 185, 185)"><title>DSC 423
2</title></rect><rect x="321" y="359.99999999999983" height="45" width="13" fill="rgb(185, 185, 185)"><title>HCI 512
1</title></rect><rect x="641" y="463.05303406198345" height="45" width="13" fill="rgb(185, 185, 185)"><title>DSC 425
1</title></rect><rect x="961" y="365.91211222228054" height="90" width="13" fill="rgb(185, 185, 185)"><title>DSC 540
2</title></rect><rect x="321" y="414.99999999999983" height="45" width="13" fill="rgb(185, 185, 185)"><title>DSC 484
1</title></rect><rect x="321" y="569.9999999999998" height="90.00000000000023" width="13" fill="rgb(185, 185, 185)"><title>CSC 481
2</title></rect><rect x="641" y="573.0530340619832" height="45" width="13" fill="rgb(185, 185, 185)"><title>CSC 482
1</title></rect><rect x="641" y="132.71804509048044" height="89.99999999999994" width="13" fill="rgb(185, 185, 185)"><title>CSC 555
2</title></rect><rect x="321" y="670" height="45" width="13" fill="rgb(185, 185, 185)"><title>CSC 577
1</title></rect><rect x="321" y="159.99999999999991" height="89.99999999999997" width="13" fill="rgb(185, 185, 185)"><title>DSC 433
2</title></rect><rect x="641" y="628.0530340619832" height="45" width="13" fill="rgb(185, 185, 185)"><title>CSC 528
1</title></rect><rect x="641" y="518.0530340619833" height="44.999999999999886" width="13" fill="rgb(185, 185, 185)"><title>DSC 480
1</title></rect></g><g fill="none"><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,497.5C327.5,497.5,327.5,430.55303406198345,640,430.55303406198345" stroke-width="45"></path><title>CSC 412 → CSC 578
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M335,327.4999999999999C487.5,327.4999999999999,487.5,385.55303406198345,640,385.55303406198345" stroke-width="45"></path><title>DSC 478 → CSC 578
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,72.5C167.5,72.5,167.5,82.49999999999996,320,82.49999999999996" stroke-width="45"></path><title>CSC 401 → DSC 465
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,262.5C167.5,262.5,167.5,127.49999999999996,320,127.49999999999996" stroke-width="45"></path><title>IT 403 → DSC 465
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,207.5C167.5,207.5,167.5,282.4999999999999,320,282.4999999999999" stroke-width="45"></path><title>CSC 401 → DSC 478
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,27.5C167.5,27.5,167.5,27.5,320,27.5" stroke-width="45"></path><title>CSC 401 → CSC 452
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,442.5C167.5,442.5,167.5,492.49999999999983,320,492.49999999999983" stroke-width="45"></path><title>IT 403 → DSC 423
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,352.5C167.5,352.5,167.5,382.49999999999983,320,382.49999999999983" stroke-width="45"></path><title>IT 403 → HCI 512
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M335,492.49999999999983C487.5,492.49999999999983,487.5,485.55303406198345,640,485.55303406198345" stroke-width="45"></path><title>DSC 423 → DSC 425
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M655,385.55303406198345C807.5,385.55303406198345,807.5,388.41211222228054,960,388.41211222228054" stroke-width="45"></path><title>CSC 578 → DSC 540
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M335,437.49999999999983C647.5,437.49999999999983,647.5,433.41211222228054,960,433.41211222228054" stroke-width="45"></path><title>DSC 484 → DSC 540
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M335,592.4999999999998C487.5,592.4999999999998,487.5,595.5530340619832,640,595.5530340619832" stroke-width="45"></path><title>CSC 481 → CSC 482
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M335,282.4999999999999C487.5,282.4999999999999,487.5,200.21804509048044,640,200.21804509048044" stroke-width="45"></path><title>DSC 478 → CSC 555
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,117.5C327.5,117.5,327.5,155.21804509048044,640,155.21804509048044" stroke-width="45"></path><title>CSC 401 → CSC 555
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,587.5C167.5,587.5,167.5,692.5,320,692.5" stroke-width="45"></path><title>CSC 412 → CSC 577
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,162.5C167.5,162.5,167.5,182.49999999999991,320,182.49999999999991" stroke-width="45"></path><title>CSC 401 → DSC 433
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,307.5C167.5,307.5,167.5,227.49999999999991,320,227.49999999999991" stroke-width="45"></path><title>IT 403 → DSC 433
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,397.5C167.5,397.5,167.5,437.49999999999983,320,437.49999999999983" stroke-width="45"></path><title>IT 403 → DSC 484
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M335,637.4999999999998C487.5,637.4999999999998,487.5,650.5530340619832,640,650.5530340619832" stroke-width="45"></path><title>CSC 481 → CSC 528
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M15,542.5C167.5,542.5,167.5,592.4999999999998,320,592.4999999999998" stroke-width="45"></path><title>CSC 412 → CSC 481
1</title></g><g stroke="rgb(221, 221, 221)" style="mix-blend-mode: multiply;"><path d="M335,537.4999999999998C487.5,537.4999999999998,487.5,540.5530340619833,640,540.5530340619833" stroke-width="45"></path><title>DSC 423 → DSC 480
1</title></g></g><g style="font: 10px sans-serif;"><text x="21" y="542.5000000000001" dy="0.35em" text-anchor="start">CSC 412<tspan fill-opacity="0.7"> 3</tspan></text><text x="634" y="408.05303406198345" dy="0.35em" text-anchor="end">CSC 578<tspan fill-opacity="0.7"> 2</tspan></text><text x="341" y="304.9999999999999" dy="0.35em" text-anchor="start">DSC 478<tspan fill-opacity="0.7"> 2</tspan></text><text x="21" y="117.49999999999999" dy="0.35em" text-anchor="start">CSC 401<tspan fill-opacity="0.7"> 5</tspan></text><text x="341" y="104.99999999999994" dy="0.35em" text-anchor="start">DSC 465<tspan fill-opacity="0.7"> 2</tspan></text><text x="21" y="352.5" dy="0.35em" text-anchor="start">IT 403<tspan fill-opacity="0.7"> 5</tspan></text><text x="341" y="27.49999999999998" dy="0.35em" text-anchor="start">CSC 452<tspan fill-opacity="0.7"> 1</tspan></text><text x="341" y="514.9999999999998" dy="0.35em" text-anchor="start">DSC 423<tspan fill-opacity="0.7"> 2</tspan></text><text x="341" y="382.49999999999983" dy="0.35em" text-anchor="start">HCI 512<tspan fill-opacity="0.7"> 1</tspan></text><text x="634" y="485.55303406198345" dy="0.35em" text-anchor="end">DSC 425<tspan fill-opacity="0.7"> 1</tspan></text><text x="954" y="410.91211222228054" dy="0.35em" text-anchor="end">DSC 540<tspan fill-opacity="0.7"> 2</tspan></text><text x="341" y="437.49999999999983" dy="0.35em" text-anchor="start">DSC 484<tspan fill-opacity="0.7"> 1</tspan></text><text x="341" y="614.9999999999999" dy="0.35em" text-anchor="start">CSC 481<tspan fill-opacity="0.7"> 2</tspan></text><text x="634" y="595.5530340619832" dy="0.35em" text-anchor="end">CSC 482<tspan fill-opacity="0.7"> 1</tspan></text><text x="634" y="177.71804509048042" dy="0.35em" text-anchor="end">CSC 555<tspan fill-opacity="0.7"> 2</tspan></text><text x="341" y="692.5" dy="0.35em" text-anchor="start">CSC 577<tspan fill-opacity="0.7"> 1</tspan></text><text x="341" y="204.9999999999999" dy="0.35em" text-anchor="start">DSC 433<tspan fill-opacity="0.7"> 2</tspan></text><text x="634" y="650.5530340619832" dy="0.35em" text-anchor="end">CSC 528<tspan fill-opacity="0.7"> 1</tspan></text><text x="634" y="540.5530340619832" dy="0.35em" text-anchor="end">DSC 480<tspan fill-opacity="0.7"> 1</tspan></text></g></svg>
</div>
