import requests
import collections
import copy
from bs4 import BeautifulSoup

###############################################################################
isDisplayFloor   = True
isDisplayName    = True
isDisplayId      = True
isDisplayContent = True

isIncludeEdit    = True
isIncludeRepeat  = True
isIncludeDelete  = True

orderDict = {}
bsn = 60076
snA = 6229618
###############################################################################
def param_global():
    global bsn, snA, orderDict
    if orderDict =={} :
        orderDict_()
    return [bsn, snA, orderDict, displayList_(), controlList_()]

def orderDict_():
    global orderDict
    orderDict = {}
    string_list = ['Floor', 'Id', 'Name', 'Content']
    order_list_temp = list(filter(str.isdigit,input("Order (Floor, Id, Name, Content) (example:'0123'):")))[:4]
    for i in range(4):
        orderDict[str(i)] = string_list[order_list_temp.index(min(order_list_temp))]
        string_list.remove(orderDict[str(i)])
        order_list_temp.pop(order_list_temp.index(min(order_list_temp)))
    return orderDict

def displayList_():
    global isDisplayFloor, isDisplayName, isDisplayId, isDisplayContent
    return [isDisplayFloor, isDisplayName, isDisplayId, isDisplayContent]

def controlList_():
    global isIncludeEdit, isIncludeRepeat, isIncludeDelete
    return [isIncludeEdit, isIncludeRepeat, isIncludeDelete]

def bsn_(num = None):
    global bsn
    if(num==None):
        return bsn
    else:
        return (bsn:=num)

def snA_(num = None):
    global snA
    if(num==None):
        return snA
    else:
        return (snA:=num)

def get_requests(page = 1,bsn = None, snA = None):
    params = param_global()
    bsn         = type(None)==type(bsn)         and params[0] or bsn
    snA         = type(None)==type(snA)         and params[1] or snA

    head = {'user-agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36'}
    url = f'https://forum.gamer.com.tw/C.php?page={page}&bsn={bsn}&snA={snA}'
    # https://forum.gamer.com.tw/C.php?page=1&bsn=60076&snA=6327426
    return requests.get(url, headers = head)

def get_last_floor(bsn,snA):
    head = {'user-agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36'}
    url = f'https://forum.gamer.com.tw/C.php?bsn={bsn}&snA={snA}&last=1#down'
    soup = BeautifulSoup(requests.get(url, headers = head).text,"html.parser")
    floors = soup.find_all('a', {'class': 'floor'})
    last_floor = "".join(list(filter(str.isdigit,str(floors[len(floors)-1].text))))
    return int(last_floor)

def isCN(ch):
    if '\u4e00' <= ch <= '\u9fff':
            return True
    return False

def get_list_by_floor_and_soup(floor, soup, orderDict, displayList, controlList):
    output_list = []
    output_text = {}
    try:
        for i in range(20):
            newSoup=soup.find_all('div', {'class': 'c-section__main c-post'})[i]
            if(newSoup.find_all('a', {'data-floor': str(floor)}) != []):
                if displayList[0]: output_text['Floor'] = newSoup.find_all('a', {'data-floor': str(floor)})[0].text
                if displayList[1]: output_text['Id'] = newSoup.find_all('a', {'class': 'userid'})[0].text
                if displayList[2]: output_text['Name'] = newSoup.find_all('a', {'class': 'username'})[0].text
                if displayList[3]: output_text['Content'] = newSoup.find_all('div', {'class': 'c-article__content'})[0].text.replace("\n","")
                break
    except:
        for i in range(20):
            newSoup=soup.find_all('div', {'class': 'c-section__main c-disable nocontent'})[i]
            if(newSoup.find_all('div', {'data-floor': str(floor)}) != []):
                if displayList[0]: output_text['Floor'] = newSoup.find_all('div', {'data-floor': str(floor)})[0].text
                if displayList[3]: output_text['Content'] = newSoup.find_all('div', {'class': 'hint'})[0].text
                if displayList[1]: output_text['Id'] = output_text['Content'][9:-3]
                if displayList[2]: output_text['Name'] = 'None'
                break
    output_list += [[output_text[orderDict[elem]] for elem in orderDict]]
    return output_list

def get_string_by_floor_and_soup(floor, soup, orderDict, displayList, controlList):
    output_string = ""
    output_text = {}
    try:
        for i in range(20):
            newSoup=soup.find_all('div', {'class': 'c-section__main c-post'})[i]
            if(newSoup.find_all('a', {'data-floor': str(floor)}) != []):
                if displayList[0]: output_text['Floor'] = newSoup.find_all('a', {'data-floor': str(floor)})[0].text
                if displayList[1]: output_text['Id'] = newSoup.find_all('a', {'class': 'userid'})[0].text
                if displayList[2]: output_text['Name'] = newSoup.find_all('a', {'class': 'username'})[0].text
                if displayList[3]: output_text['Content'] = newSoup.find_all('div', {'class': 'c-article__content'})[0].text.replace("\n","")
                break
    except:
        for i in range(20):
            newSoup=soup.find_all('div', {'class': 'c-section__main c-disable nocontent'})[i]
            if(newSoup.find_all('div', {'data-floor': str(floor)}) != []):
                if displayList[0]: output_text['Floor'] = newSoup.find_all('div', {'data-floor': str(floor)})[0].text
                if displayList[2]: output_text['Name'] = 'None'
                if displayList[3]: output_text['Content'] = newSoup.find_all('div', {'class': 'hint'})[0].text
                if displayList[1]: output_text['Id'] = output_text['Content'][9:-3]
                break
    for elem in orderDict:
        if displayList[int(elem)] and orderDict[elem] != "Name" and orderDict[elem] != "Floor":
            output_string += "{0:<{1}}".format(output_text[orderDict[elem]],20)
        elif orderDict[elem] == "Floor":
            output_string += "{0:<{1}}".format((string:=output_text[orderDict[elem]]),15-int((len(string.encode())-len(string))/2))
        else:
            output_string += "{0:<{1}}".format((string:=output_text[orderDict[elem]]),25-int((len(string.encode())-len(string))/2))
    return output_string +"\n"

def get_result_by_floor(floor, result = None, bsn = None, snA = None, orderDict = None, displayList = None, controlList = None):
    result_dict = {("STRING", "String", "string", "str", None, "None") : [get_string_by_floor_and_soup,""], \
                    ("LIST", "List", "list") : [get_list_by_floor_and_soup,[]]}
    func = (tmp:=[result_dict[x] for x in result_dict if result in x][0])[0]
    result_temp =  copy.copy(tmp[1])
    params = param_global()
    bsn         = type(None)==type(bsn)         and params[0] or bsn
    snA         = type(None)==type(snA)         and params[1] or snA
    orderDict   = type(None)==type(orderDict)   and params[2] or orderDict
    displayList = type(None)==type(displayList) and params[3] or displayList
    controlList = type(None)==type(controlList) and params[4] or controlList

    if (floor > get_last_floor(bsn,snA)) :
        print("Error Floor")
        return
    else :
        res = get_requests((floor-1)/20+1,bsn = bsn,snA = snA)
        soup = BeautifulSoup(res.text,"html.parser")
        return func(floor, soup, orderDict, displayList, controlList)

def get_result_by_page(page, result = None, bsn = None, snA = None, orderDict = None, displayList = None, controlList = None):
    result_dict = {("STRING", "String", "string", "str", None, "None") : [get_string_by_floor_and_soup,""], \
                    ("LIST", "List", "list") : [get_list_by_floor_and_soup,[]]}
    func = (tmp:=[result_dict[x] for x in result_dict if result in x][0])[0]
    result_temp =  copy.copy(tmp[1])
    params = param_global()
    bsn         = type(None)==type(bsn)         and params[0] or bsn
    snA         = type(None)==type(snA)         and params[1] or snA
    orderDict   = type(None)==type(orderDict)   and params[2] or orderDict
    displayList = type(None)==type(displayList) and params[3] or displayList
    controlList = type(None)==type(controlList) and params[4] or controlList
    if (page > (last_floor:=get_last_floor(bsn,snA))/20+1) :
        print("Error Page")
        return
    else :
        res = get_requests(page,bsn = bsn,snA = snA)
        soup = BeautifulSoup(res.text,"html.parser")
        for temp in range(20):
            if((floor:= (page-1)*20+temp+1)<=last_floor):
                result_temp += func(floor, soup, orderDict, displayList, controlList)
    return result_temp

def get_result_all_floor(result = None, bsn = None,snA = None):
    result_dict = {("STRING", "String", "string", "str", None, "None") : [get_string_by_floor_and_soup,""], \
                    ("LIST", "List", "list") : [get_list_by_floor_and_soup,[]]}
    result_temp =  copy.copy([result_dict[x] for x in result_dict if result in x][0][1])
    params = param_global()
    bsn         = type(None)==type(bsn)         and params[0] or bsn
    snA         = type(None)==type(snA)         and params[1] or snA
    page_range = int(get_last_floor(bsn,snA)/20)
    print("Start get_result_all_floor")
    for i in range(page_range):
        result_temp += (y:=get_result_by_page(i+1,result=result))
        print(y)
    return result_temp

if __name__ == "__main__":
    bsn_(60076)
    snA_(6229618)
    print(get_result_all_floor(result="None"))
