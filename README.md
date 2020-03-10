# python_tests
##Python
First approach to web scraping world

##How to use 

code(
    from bs4 import BeautifulSoup as bsoup
    from urllib.request import Request, urlopen
    import string
    from collections import OrderedDict
        hdr = {'User-Agent': 'Mozilla/5.0'}
        mapa={}
        countries=[]
        cities = []
        for i in string.ascii_uppercase:
            req = Request(r'https://en.wikipedia.org/wiki/List_of_towns_and_cities_with_100,000_or_more_inhabitants/cityname:_'+i, headers=hdr)
            page = urlopen(req)
            soup = bsoup(page, 'html.parser')
            table_body = soup.find("table",{"class":"wikitable sortable"}).tbody
            children = table_body.findChildren("tr" , recursive=False)
            print(len(children))
            for child in children:
                brothers = child.findChildren("td" , recursive=False)
                city = ""
                country = ""
                for id,bro in enumerate(brothers):
                    if id==0:
                        #city
                        if hasattr(bro.a,'text'):
                            city=bro.a.text
                        else:
                            city="unknow1"
                    else:
                        #country
                        if hasattr(bro.a,'text'):
                            country=bro.a.text
                        else:
                            country="unknow2"

                        if country in mapa:
                            cities=mapa[country]
                        else:
                            cities = []
                        cities.append(city)
                        mapa[country]=cities
        print(mapa)
)
