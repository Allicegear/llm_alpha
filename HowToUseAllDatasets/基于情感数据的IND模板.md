S1=ts_mean(ts_backfill({sentiment},250),22);# 月平均情感分数

R1=ts_product(1+returns,22); # 月度收益率，本来应该是ts_product(1+returns,22)-1，没有-1不影响结果。

alpha=ts_quantile(S1-R1,5,driver=’cauchy’);# 归一化并加重极端值的权重，使用gaussian也可以。

 

// alpha idea:做多情感分数相对于收益高的股票，反之亦是。

// 初始setting:Region=IND,Universe=TOP500,Decay=10,Delay=1,Truncation=0.01,Neu=Industry,

// Pasteurization=On,NaN Handing=Off

// 迭代:{sentiment}使用与sentiment相关的datafiled。

 

和sentiment相关的datafiled可以通过以下代码获得：

def get_datafields(s,searchScope,dataset_id:str='',search:str=''):

    instrument_type=searchScope['instrumentType']

    region=searchScope['region']

    delay=searchScope['delay']

    universe=searchScope['universe']

    if len(search)==0:

        url_template="https://api.worldquantbrain.com/data-fields?"+\

            f"&instrumentType={instrument_type}"+\

            f"&region={region}&delay={str(delay)}&universe={universe}&dataset.id={dataset_id}&limit=50"+\

            "&offset={x}"

        count=s.get(url_template.format(x=0)).json()['count'] #.json()将response 转为字典

    else:

        url_template="https://api.worldquantbrain.com/data-fields?"+\

            f"&instrumentType={instrument_type}"+\

            f"&region={region}&delay={str(delay)}&universe={universe}&search={search}&limit=50"+\

            "&offset={x}"

        count=s.get(url_template.format(x=0)).json()['count']

    

    datafields_list=[]

    for x in range(0,count,50):

        while True:

            try:

                datafields=s.get(url_template.format(x=x))

                datafields_list.append(datafields.json()['results'])

                break

            except:

                time.sleep(1)

 

    datafields_list_flat=[item for sublist in datafields_list for item in sublist]

    datafields_df=pd.DataFrame(datafields_list_flat)

return datafields_df

 

dataFields=get_datafields(s=sess,searchScope=searchScope,search=’sentiment’)
