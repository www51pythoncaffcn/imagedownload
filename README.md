#!/usr/bin/env python3
# -*- coding: utf-8-*-
# 解决这个问题分为以下步骤：
# 1.通过分析网络请求：发现图集请求服务端时，返回的数据，在json中，需要解析json获取相应数据.
# 2.找到接续数据的的规律后，下一步需要调用系统自带的文件打开函数以及操作系统os，进而将图片保存到文件内。

import requests

import traceback

import json

import os

from urllib.parse import urlencode

import time

path ="/Users/limo/Desktop/wj/"

#请求URL，返回相应的json数据

def json_req(page):


    data={

        'q':'',
        'type':2,
        'page':page
    }


    url="https://haiwai.ibaotu.com/photos/ajaxSearch"

    s = requests.session ()
    s.keep_alive = False

    respose=requests.get(url,params=data)

    try:

        if respose.status_code == 200:

            return respose.text


        else:

            return None
    except:

        traceback.print_exc ()

    else:

        print ("没有发现异常")





# # 针对返回的数据，进行解析


def json_parse(jsondata):

    list=[]


    da=json.loads(jsondata)

    # print(type(da))


    data1=da["data"]

    for i in data1:

        list.append("http:"+i["p_path"])

    return list;

# 保存图片

def sava_picuture(list1):

    n=0

    for i in list1:

        s = requests.session ()
        s.keep_alive = False

        respo=requests.get(i).content


        with open(path+str(n)+".jpeg",'wb') as f:

            f.write(respo)

        n+=1


# 调用各种业务函数

def main():

    for i in range(2,11):

        global path

        path=path+"第"+str(i)+"页/"

        os.mkdir(path)

        jsondata=json_req(i)

        # print(jsondata)

        list2=json_parse(jsondata)

        sava_picuture(list2)

        path = "/Users/limo/Desktop/wj/"

        time.sleep(5)


    # savepicture=json_parse(jsondata)

#启动main函数的调用

if __name__ == '__main__':

    main()

