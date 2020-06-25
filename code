from bs4 import BeautifulSoup
import requests

import pandas as pd
import numpy as np

outfilename = "WaterSystem_01001-09999.txt"
outfile = open(outfilename,"w")
print ("WATER SYSTEM NAME","|","PWSID","|","WATER SYSTEM TYPE","|", "EPA REGION","|","PRIMARY SOURCE","|","POP SERVED","|","CONTAM","|","CONTAM YEAR","|","CONTAM TEST DATE","|","CONTAM MRL","|","CONTAM HRL","|","CONTAM RESULT","|","LC SAMPLE","|","LC CONTAM","|","LC SAMPLE START","|","LC SAMPLE END","|","LC RESULT","|","VIOLATIONS COMPLIANCE DATES","|","VIOLATION TYPE","|","VIOLATION CONTAM","|","VIOLATION STATUS", file = outfile)

#Zip Code Opener#
headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.77 Safari/537.36'}

zipurlprefix = 'https://mytapwater.org/zip/0'
urlcount=0
urllist=[]
for page in range(1001,9999):
    pagestring = str(page)

    zipurl= zipurlprefix + pagestring + '/'
    #print (zipurl)

    try:
        zippage = requests.get(zipurl,headers=headers)
        ziphtml = zippage.text
        zipsoup = BeautifulSoup(ziphtml, 'lxml')
        source_name_list = zipsoup.find('table', class_='search-results-table')
        source_name_list_items = source_name_list.find_all('a')
        for source_name in source_name_list_items:
            link = source_name.get('href')
            if link not in urllist:
                urllist.append(link)
                urlcount+=1
        print('working')
    except:
        pass
print('URL Count:',urlcount)

pagecount=0
for url in urllist:
    sourcepage = requests.get(url,headers=headers)
    html = sourcepage.text
    soup = BeautifulSoup(html, 'lxml')

    #SOURCE WATER TITLE#
    
    try:
        title_section = soup.find('article', id='pws-info')
        title_detail = title_section.find('h1')
        title_cells = str(title_detail)
        title_clean = BeautifulSoup(title_cells,"lxml").get_text()
        title_clean = title_clean.replace('Public Water System: ','')
        title_clean = title_clean.strip()
        title_clean = title_clean.replace('\t','')
        title_clean = title_clean.replace('  ','')
        title_clean = title_clean.replace('\n','')
        title_clean = title_clean.replace(',','')
        title_clean = title_clean.upper()
    except:
        title_clean = ''
    #INFO#
    info_section = soup.find('table')
    info_detail = info_section.find('td')
    info_cells = str(info_detail)
    info_clean = BeautifulSoup(info_cells,"lxml").get_text()
    info_fin = info_clean.replace('\t','')
    info_fin = info_fin.replace(',','')
    info_list = info_fin.split('\n')
    info_string = ','.join(info_list)
    info_string = info_string.replace(',PWS Service Information,PWS ID: ','')
    info_string = info_string.replace(',','|')
    info_string = info_string.upper()

    #CONTAMINATIONS#
    contam_list=[]
    try:
        contam_section = soup.find('section',id='contaminant-detail')
        contam_detail = contam_section.find_all('div',class_='contaminant')
        for elem in contam_detail:
            contam = elem.find('h4').get_text()
            contamstrip = contam.replace('\n','')
            contamfin = contamstrip.replace('\t','')
            contam_info = elem.find_all('tr')
            for info in contam_info[1:]:
                contam_spec = info.find_all('td')
                contam_cells = str(contam_spec)
                contam_clean = BeautifulSoup(contam_cells,"lxml").get_text()
                contam_complete = title_clean +'|'+ info_string+'|'+ contamfin+'|'+ contam_clean+'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+'|'+''
                contam_list.append(contam_complete)
                #print(contam_complete)
    except:
        pass

    #print(contam_list)

    #LEAD AND COPPER#
    lc_list = []
    try:
        lead_section = soup.find('section',id='lead-copper-data')
        lead_detail = lead_section.find_all('tr')
        for elem in lead_detail[1:]:
            lc_info = elem.find_all('td')
            lc_cells = str(lc_info)
            lc_clean = BeautifulSoup(lc_cells,"lxml").get_text()
            lc_complete = title_clean +'|'+ info_string +'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+ lc_clean +'|'+''+'|'+''+'|'+''+'|'+''
            lc_list.append(lc_complete)
            #print(lc_clean)
    except:
        pass

    #print(lc_list)
                    
    #VIOLATIONS#
    vio_list = []
    try:
        vio_section = soup.find('section', id="violations")
        vio_detail = vio_section.find_all('tr')
        for elem in vio_detail[1:]:
            vio_info = elem.find_all('td')
            vio_cells = str(vio_info)
            vio_clean = BeautifulSoup(vio_cells,"lxml").get_text()
            vio_complete = title_clean + '|' + info_string +'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+'|'+''+ '|'+ ''+ '|'+ ''+ '|'+ ''+ '|'+ ''+ '|'+''+'|'+''+ vio_clean 
            vio_list.append(vio_complete)
            #print(vio_clean)
    except:
        pass

    #print(vio_list)

    #MASTER LIST#
    master_list = contam_list + lc_list + vio_list
    for row in master_list:
        row = row.replace('[','')
        row = row.replace(']','')
        row = row.replace('Monitoring,','Monitoring')
        row = row.replace('Notification,','Notification')
        row = row.replace('Xylenes,','Xylenes')
        row = row.replace('Technique,','Technique')
        row = row.replace(', Monitoring',' Monitoring')
        row = row.replace('CARBON,','CARBON')
        row = row.replace('Rule, Treatment','Rule Treatment')
        row = row.replace('Violation, Monthly','Violation Monthly')
        row = row.replace('Antimony, Total','Antimony Total')
        row = row.replace('Beryllium, Total','Beryllium Total')
        row = row.replace('Thallium, Total','Thallium Total')
        row = row.replace('Selenium, Total','Selenium Total')
        row = row.replace('Rule, Maximum','Rule Maximum')
        row = row.replace('Rule, Consumer','Rule Consumer')
        row = row.replace('Rule, Follow','Rule Follow')
        row = row.replace('Chemicals,','Chemicals')
        row = row.replace('Rule, Water','Rule Water')
        row = row.replace('Nitrates, Maximum','Nitrates Maximum')
        row = row.replace('Nitrates, Monitoring','Nitrates Monitoring')
        row = row.replace('Rule, Initial','Rule Initial')
        row = row.replace('Rule, Failure','Rule Failure')
        row = row.replace('Rule, Monthly','Rule Monthly')
        row = row.replace('Rule, OCCT','Rule OCCT')
        row = row.replace('Rule, Lead','Rule Lead')
        row = row.replace('Rule, Single','Rule Single')
        row = row.replace('Rule, Notification','Rule Notification')
        row = row.replace('Arsenic, Maximum','Arsenic Maximum')
        row = row.replace('Rule, Public','Rule Public')
        row = row.replace('), Surface,',') Surface')
        row = row.replace('Miscellaneous,','Miscellaneous')
        row = row.replace('Rule, Sanitary','Rule Sanitary')
        row = row.replace('Trihalomethanes, ','Trihalomethanes ')
        row = row.replace(', Maximum ',' Maximum')
        row = row.replace('Rule, WQP','Rule WQP')
        row = row.replace('Alpha, ','Alpha ')
        row = row.replace('Violation, Average','Violation Average')
        row = row.replace('Violation, Acute','Violation Acute')
        row = row.replace('Rule, MPL','Rule MPL')
        row = row.replace('Copper, Free','Copper Free')
        row = row.replace(', Follow-up,','Follow-up')
        row = row.replace('Violation, Single','Violation Single')
        row = row.replace('Radionuclides, Notification','Radionuclides Notification')
        row = row.replace(', HOA','')
        row = row.replace(', EARLY SETTLERS CG','')
        row = row.replace(',',' ')
        row = row.replace('  ','|')
        row = row.upper()
        print(row,file = outfile)
    #I ran a counter so I was aware of any lapses#
    pagecount += 1
    print ('page:',pagecount)


outfile.close()      
print('COMPLETE')


