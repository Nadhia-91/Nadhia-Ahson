
import os
import json
import pandas as pd
import psycopg2
import plotly.graph_objects as go
import plotly.express as px
from sqlalchemy import create_engine
import streamlit as st


# In[64]:


path1="C:/Users/jpcom/Desktop/youtube dhaw/phonepe plus/pulse/data/aggregated/transaction/country/india/state/"
state_loct=os.listdir(path1)
List={"States":[],"Year":[],"File":[],"Name":[],"Count":[],"Amount":[]}
for state in state_loct:
    year_path=path1+state+"/"
    year_loct=os.listdir(year_path)
    for year in year_loct:
        file_path=year_path+year+"/"
        file_loct=os.listdir(file_path)
        for file in file_loct:
            last_path=file_path+file
            last_loct=open(last_path,"r")
            open_file=json.load(last_loct)
            for i in open_file["data"]["transactionData"]:
                name_file=i["name"]
                count_file=i["paymentInstruments"][0]["count"]
                amount_file=i["paymentInstruments"][0]["amount"]
                List["Name"].append(name_file)
                List["Count"].append(count_file)
                List["Amount"].append(amount_file)
                List["States"].append(state)
                List["Year"].append(year)
                List["File"].append(int(file.strip(".json")))
            aggregated1=pd.DataFrame(List)
aggregated1["States"] = aggregated1["States"].str.replace("andaman-&-nicobar-islands","Andaman & Nicobar")
aggregated1["States"] = aggregated1["States"].str.replace("-"," ")
aggregated1["States"] = aggregated1["States"].str.title()
aggregated1['States'] = aggregated1['States'].str.replace("Dadra & Nagar Haveli & Daman & Diu", "Dadra and Nagar Haveli and Daman and Diu")


# In[65]:


#path2 agg user
agg_path2="C:/Users/jpcom/Desktop/youtube dhaw/phonepe plus/pulse/data/aggregated/user/country/india/state/"
state_loct=os.listdir(agg_path2)
List2={"State":[],"Year":[],"File":[],"Brand":[],"Count_Users":[],"Percentage":[]}
for state in state_loct:
    year_path=agg_path2+state+"/"
    year_loct=os.listdir(year_path)
    for year in year_loct:
        file_path=year_path+year+"/"
        file_loct=os.listdir(file_path)
        for file in file_loct:
            last_path=file_path+file
            last_loct=open(last_path,"r")
            open_file=json.load(last_loct)
            try:
                for i in open_file["data"]["usersByDevice"]:
                    user_brand=i["brand"]
                    user_count=i["count"]
                    user_persentage=i["percentage"]
                    List2["Brand"].append(user_brand)
                    List2["Count_Users"].append(user_count)
                    List2["Percentage"].append(user_persentage)
                    List2["State"].append(state)
                    List2["Year"].append(year)
                    List2["File"].append(int(file.strip(".json")))
            except:
                pass 
        aggregated2=pd.DataFrame(List2)
aggregated2["State"] = aggregated2["State"].str.replace("andaman-&-nicobar-islands","Andaman & Nicobar")
aggregated2["State"] = aggregated2["State"].str.replace("-"," ")
aggregated2["State"] = aggregated2["State"].str.title()
aggregated2['State'] = aggregated2['State'].str.replace("Dadra & Nagar Haveli & Daman & Diu", "Dadra and Nagar Haveli and Daman and Diu")


# In[66]:


#path3 map trans"C:\Users\jpcom\Desktop\youtube dhaw\phonepe plus\pulse\data\map\transaction\hover\country\india\state"
map_path1="C:/Users/jpcom/Desktop/youtube dhaw/phonepe plus/pulse/data/map/transaction/hover/country/india/state/"
state_loct=os.listdir(map_path1)
List3={"State":[],"Year":[],"File":[],"Name":[],"Count":[],"Amount":[]}
for state in state_loct:
    year_path=map_path1+state+"/"
    year_loct=os.listdir(year_path)
    for year in year_loct:
        file_path=year_path+year+"/"
        file_loct=os.listdir(file_path)
        for file in file_loct:
            last_path=file_path+file
            last_loct=open(last_path,"r")
            open_file=json.load(last_loct)
            for i in open_file["data"]["hoverDataList"]:
                name=i["name"]
                count=i["metric"][0]["count"]
                amount=i["metric"][0]["amount"]
                List3["Name"].append(name)
                List3["Count"].append(count)
                List3["Amount"].append(amount)
                List3["State"].append(state)
                List3["Year"].append(year)
                List3["File"].append(int(file.strip(".json")))
            map1=pd.DataFrame(List3)
map1["State"] = map1["State"].str.replace("andaman-&-nicobar-islands","Andaman & Nicobar")
map1["State"] = map1["State"].str.replace("-"," ")
map1["State"] = map1["State"].str.title()
map1['State'] = map1['State'].str.replace("Dadra & Nagar Haveli & Daman & Diu", "Dadra and Nagar Haveli and Daman and Diu")


# In[67]:


#map_path2
map_path2="C:/Users/jpcom/Desktop/youtube dhaw/phonepe plus/pulse/data/map/user/hover/country/india/state/"
state_loct=os.listdir(map_path2)
List4={"State":[],"Year":[],"File":[],"District":[],"Registereduser":[],"Appopens":[]}
for state in state_loct:
    year_path=map_path2+state+"/"
    year_loct=os.listdir(year_path)
    for year in year_loct:
        file_path=year_path+year+"/"
        file_loct=os.listdir(file_path)
        for file in file_loct:
            last_path=file_path+file
            last_loct=open(last_path,"r")
            open_file1=json.load(last_loct)
            for i in open_file1["data"]["hoverData"].items():
                district=i[0]
                registeredusers=i[1]["registeredUsers"]
                appopens=i[1]["appOpens"]
                List4["District"].append(district)
                List4["Registereduser"].append(registeredusers)
                List4["Appopens"].append(appopens)
                List4["State"].append(state)
                List4["Year"].append(year)
                List4["File"].append(int(file.strip(".json")))
            map2=pd.DataFrame(List4)
map2["State"] = map2["State"].str.replace("andaman-&-nicobar-islands","Andaman & Nicobar")
map2["State"] = map2["State"].str.replace("-"," ")
map2["State"] = map2["State"].str.title()
map2['State'] = map2['State'].str.replace("Dadra & Nagar Haveli & Daman & Diu", "Dadra and Nagar Haveli and Daman and Diu")


# In[68]:


#top_path1
top_path1="C:/Users/jpcom/Desktop/youtube dhaw/phonepe plus/pulse/data/top/transaction/country/india/state/"
top_loct=os.listdir(top_path1)
List5={"State":[],"Year":[],"File":[],"Pincode":[],"Amount":[],"Count":[]}
for district in top_loct:
    year_path=top_path1+district+"/"
    year_loct=os.listdir(year_path)
    for year in year_loct:
        file_path=year_path+year+"/"
        file_loct=os.listdir(file_path)
        for file in file_loct:
            last_path=file_path+file
            last_loct=open(last_path,"r")
            open_top=json.load(last_loct)
            for i in open_top["data"]["pincodes"]:
                name=i["entityName"]
                count=i["metric"]["count"]
                amount=i["metric"]["amount"]
                List5["Pincode"].append(name)
                List5["Count"].append(count)
                List5["Amount"].append(amount)
                List5["State"].append(state)
                List5["Year"].append(year)
                List5["File"].append(int(file.strip(".json")))
            top1=pd.DataFrame(List5)
top1["State"] = top1["State"].str.replace("andaman-&-nicobar-islands","Andaman & Nicobar")
top1["State"] = top1["State"].str.replace("-"," ")
top1["State"] = top1["State"].str.title()
top1['State'] = top1['State'].str.replace("Dadra & Nagar Haveli & Daman & Diu", "Dadra and Nagar Haveli and Daman and Diu")


# In[69]:


#top_path2
top_path2="C:/Users/jpcom/Desktop/youtube dhaw/phonepe plus/pulse/data/top/user/country/india/state/"
top_loct=os.listdir(top_path2)
List6={"State":[],"Year":[],"File":[],"Pincode":[],"Registeredusers":[]}
for district in top_loct:
    year_path=top_path2+district+"/"
    year_loct=os.listdir(year_path)
    for year in year_loct:
        file_path=year_path+year+"/"
        file_loct=os.listdir(file_path)
        for file in file_loct:
            last_path=file_path+file
            last_loct=open(last_path,"r")
            open_top1=json.load(last_loct)
            for i in open_top1["data"]["pincodes"]:
                name=i["name"]
                registeredusers=i["registeredUsers"]
                List6["Pincode"].append(name)
                List6["Registeredusers"].append(registeredusers)
                List6["State"].append(state)
                List6["Year"].append(year)
                List6["File"].append(int(file.strip(".json")))
            top2=pd.DataFrame(List6)
top2["State"] = top2["State"].str.replace("andaman-&-nicobar-islands","Andaman & Nicobar")
top2["State"] = top2["State"].str.replace("-"," ")
top2["State"] = top2["State"].str.title()
top2['State'] = top2['State'].str.replace("Dadra & Nagar Haveli & Daman & Diu", "Dadra and Nagar Haveli and Daman and Diu")


# In[70]:


#connect to aggregated1
mycon=psycopg2.connect(host='localhost',user='postgres',password='dhia',database='phonepe_pulse',port='5432')
cur=mycon.cursor()
table='''create table if not exists aggregated1(States varchar(50), Year int, File int, Name varchar(50), Count bigint, Amount bigint)'''
cur.execute(table)
mycon.commit()

for index,row in aggregated1.iterrows():
        insert ='''insert into aggregated1 (States, Year, File, Name, Count, Amount)values(%s,%s,%s,%s,%s,%s)'''
        values=(row['States'],row['Year'],row['File'],row['Name'],row['Count'],row['Amount'])
        cur.execute(insert,values)
        mycon.commit()
#connect to aggregated2
mycon=psycopg2.connect(host='localhost',user='postgres',password='dhia',database='phonepe_pulse',port='5432')
cur=mycon.cursor()
table2='''create table if not exists aggregated2(State varchar(50), Year int, File int, Brand varchar(50), Count_Users bigint, Percentage float)'''
cur.execute(table2)
mycon.commit()

for index,row in aggregated2.iterrows():
        insert2 ='''insert into aggregated2 (State, Year, File, Brand, Count_Users, Percentage)values(%s,%s,%s,%s,%s,%s)'''
        values=(row['State'],row['Year'],row['File'],row['Brand'],row['Count_Users'],row['Percentage'])
        cur.execute(insert2,values)
        mycon.commit()

        
#connection to map1
mycon=psycopg2.connect(host='localhost',user='postgres',password='dhia',database='phonepe_pulse',port='5432')
cur=mycon.cursor()
table3='''create table if not exists map1(State varchar(50), Year int, File int, Name varchar(50), Count bigint, Amount bigint)'''
cur.execute(table3)
mycon.commit()

for index,row in map1.iterrows():
        insertmap ='''insert into map1 (State, Year, File, Name, Count, Amount)values(%s,%s,%s,%s,%s,%s)'''
        values=(row['State'],row['Year'],row['File'],row['Name'],row['Count'],row['Amount'])
        cur.execute(insertmap,values)
        mycon.commit()
#connection to map2"District":[],"Registereduser":[],"Appopens":
mycon=psycopg2.connect(host='localhost',user='postgres',password='dhia',database='phonepe_pulse',port='5432')
cur=mycon.cursor()
table4='''create table if not exists map2(State varchar(50), Year int, File int, District varchar(50), Registereduser bigint, Appopens bigint)'''
cur.execute(table4)
mycon.commit()

for index,row in map2.iterrows():
        insertmap1 ='''insert into map2 (State, Year, File, District, Registereduser, Appopens)values(%s,%s,%s,%s,%s,%s)'''
        values=(row['State'],row['Year'],row['File'],row['District'],row['Registereduser'],row['Appopens'])
        cur.execute(insertmap1,values)
        mycon.commit()

#connection to top1
mycon=psycopg2.connect(host='localhost',user='postgres',password='dhia',database='phonepe_pulse',port='5432')
cur=mycon.cursor()
table5='''create table if not exists top1(State varchar(50), Year int, File int, Pincode int, Count bigint, Amount bigint)'''
cur.execute(table5)
mycon.commit()

for index,row in top1.iterrows():
        inserttop ='''insert into top1 (State, Year, File, Pincode, Count, Amount)values(%s,%s,%s,%s,%s,%s)'''
        values=(row['State'],row['Year'],row['File'],row['Pincode'],row['Count'],row['Amount'])
        cur.execute(inserttop,values)
        mycon.commit()


#connection to top2
mycon=psycopg2.connect(host='localhost',user='postgres',password='dhia',database='phonepe_pulse',port='5432')
cur=mycon.cursor()
table6='''create table if not exists top2(State varchar(50), Year int, File int, Pincode int, Registeredusers bigint)'''
cur.execute(table6)
mycon.commit()



for index,row in top2.iterrows():
        inserttop1 ='''insert into top2 (State, Year, File, Pincode, Registeredusers)values(%s,%s,%s,%s,%s)'''
        values=(row['State'],row['Year'],row['File'],row['Pincode'],row['Registeredusers'])
        cur.execute(inserttop1,values)
        mycon.commit()


# In[71]:


e=create_engine('postgresql+psycopg2://postgres:dhia@localhost/phonepe_pulse')


# In[72]:


trans_agg=pd.read_sql('select*from aggregated1',e)


# In[ ]:


user_agg=pd.read_sql('select*from aggregated2',e)


# In[ ]:


trans_map=pd.read_sql('select*from map1',e)


# In[ ]:


user_map=pd.read_sql('select*from map2',e)


# In[ ]:


trans_top=pd.read_sql('select*from top1',e)


# In[ ]:


user_top=pd.read_sql('select*from top2',e)


# In[103]:


#on map trans 
def states_amount():
    
    list1=[]
    for yr in trans_agg["year"].unique():
        for qtr in trans_agg["file"].unique():
            all_year=trans_agg[(trans_agg["year"]==yr)&(trans_agg["file"]==qtr)]
            new=all_year[["states","count","amount"]]
            new=new.sort_values(by="states")
            new["year"]=yr
            new["file"]=qtr
            list1.append(new)
    new1=pd.concat(list1)
    fig=px.choropleth(new1,             geojson="https://gist.githubusercontent.com/jbrobst/56c13bbbf9d97d187fea01ca62ea5112/raw/e388c4cae20aa53cb5090210a42ebb9b765c0a36/india_states.geojson",
             featureidkey='properties.ST_NM',   
             locations='states',        #column in dataframe
              color='amount',  #dataframe
              color_continuous_scale='Sunsetdark', 
              height=600)
    fig.update_geos(fitbounds="locations", visible=False)
    return st.plotly_chart(fig)


# In[108]:





# In[110]:




# In[112]:




# In[113]:


def state_count():
    list2=[]
    for yr in trans_agg["year"].unique():
        for qtr in trans_agg["file"].unique():
            all_year=trans_agg[(trans_agg["year"]==yr)&(trans_agg["file"]==qtr)]
            new=all_year[["states","count","amount"]]
            new=new.sort_values(by="states")
            new["year"]=yr
            new["file"]=qtr
            list2.append(new)
    new1=pd.concat(list2)
    fig=px.choropleth(new1,         geojson="https://gist.githubusercontent.com/jbrobst/56c13bbbf9d97d187fea01ca62ea5112/raw/e388c4cae20aa53cb5090210a42ebb9b765c0a36/india_states.geojson",
             featureidkey='properties.ST_NM',   
             locations='states',        #column in dataframe
              color='count',  #dataframe
              color_continuous_scale='Sunsetdark',  
              height=600)
    fig.update_geos(fitbounds="locations", visible=False)
    return st.plotly_chart(fig)


# In[78]:


#(trans) top state
def top_state_amount():
   List3=[]
   for yr in trans_agg["year"].unique():
       for qtr in trans_agg["file"].unique():
           all_year=trans_agg[(trans_agg["year"]==yr)&(trans_agg["file"]==qtr)]
           new = all_year[["states","amount"]]
           new = new.groupby("states")["amount"].sum().sort_values(ascending=False)
           new["year"]=yr
           new["file"]=qtr
           List3.append(new)
   s2=pd.concat(List3).head(10).reset_index()
   fig7= px.pie(s2, values= "amount", names= "states",color_discrete_sequence=px.colors.sequential.Emrld_r)
   return st.plotly_chart(fig7)


# In[79]:


#year(name, amount) trans
def categories_count():
    List6=[]
    for yr in trans_agg["year"].unique():
        for qtr in trans_agg["file"].unique():
            all_year=trans_agg[(trans_agg["year"]==yr)&(trans_agg["file"]==qtr)]
            new=all_year[["year","name","count"]]
            new=new.groupby("name")["count"].sum()
            List6.append(new)
    new1=pd.concat(List6).reset_index()
    fig5= px.bar(new1,x= "name",y= "count",
                color_discrete_sequence=px.colors.sequential.Redor_r)
    fig5.update_layout(width=200, height= 500)
    return st.plotly_chart(fig5)


# In[82]:


#(user) top state_user
def state_user():
   List7=[]
   for yr in user_agg["year"].unique():
       for qtr in user_agg["file"].unique():
           all_year=user_agg[(user_agg["year"]==yr)&(user_agg["file"]==qtr)]
           new= all_year[["state","count_users"]]
           new= new.groupby("state")["count_users"].sum().sort_values(ascending=False)
           new["year"]=yr
           new["file"]=qtr
           List7.append(new)
   new1= pd.concat(List7).head(10).reset_index()

   figb= px.pie(new1, values= "count_users", names= "state",
                   color_discrete_sequence=px.colors.sequential.Emrld_r)
   figb.update_layout(width=200, height= 500)

   return st.plotly_chart(figb)


# In[85]:


def district_amount():
    Dist=[]
    for yr in trans_map["year"].unique():
        for qtr in trans_map["file"].unique():
            all_year=trans_map[(trans_map["year"]==yr)&(trans_map["file"]==qtr)]
            new=all_year[["name","amount"]]
            new=new.groupby("name")["amount"].sum()
            Dist.append(new)
        new1=pd.concat(Dist).reset_index()
    figs= px.bar(new1,x= "amount",y= "name", color_discrete_sequence=px.colors.sequential.Emrld_r)
    figs.update_layout(width=500, height= 600)
    return st.plotly_chart(figs)


# In[ ]:


def district_count():
    Dist_count=[]
    for yr in trans_map["year"].unique():
        for qtr in trans_map["file"].unique():
            all_year=trans_map[(trans_map["year"]==yr)&(trans_map["file"]==qtr)]
            new=all_year[["name","count"]]
            new=new.groupby("name")["count"].sum()
            Dist_count.append(new)
        new1=pd.concat(Dist_count).reset_index()
    fig_dist= px.bar(new1,x= "count",y= "name", color_discrete_sequence=px.colors.sequential.Emrld_r)
    return st.plotly_chart(fig_dist)


# In[86]:


def top_district_amount():
    List4=[]
    for yr in trans_map["year"].unique():
        for qtr in trans_map["file"].unique():
            all_year=trans_map[(trans_map["year"]==yr)&(trans_map["file"]==qtr)]
            new= all_year[["name","amount"]]
            new1= new.groupby("name")["amount"].sum().sort_values(ascending=False)
            new1["year"]=yr
            new1["file"]=qtr
            List4.append(new1)
    new2=pd.concat(List4).head(10).reset_index()
    fig= px.pie(new2, values= "amount", names= "name",color_discrete_sequence=px.colors.sequential.Emrld_r)
    return st.plotly_chart(fig)


# In[89]:


#(user) top district
def top_district_users():
   List5=[]
   for yr in user_map["year"].unique():
       for qtr in user_map["file"].unique():
           all_year=user_map[(user_map["year"]==yr)&(user_map["file"]==qtr)]
           new= all_year[["district","registereduser"]]
           new= new.groupby("district")["registereduser"].sum().sort_values(ascending=False)
           List5.append(new)
   new1= pd.concat(List5).head(10).reset_index()
   figu= px.pie(new1, values= "registereduser", names= "district",color_discrete_sequence=px.colors.sequential.Emrld_r)
   return st.plotly_chart(figu)


# In[90]:
def district_registereduser():
    list9=[]
    for yr in user_map["year"].unique():
        for qtr in user_map["file"].unique():
            all_year=user_map[(user_map["year"]==yr)&(user_map["file"]==qtr)]
            new=all_year[["district","registereduser"]]
            new=new.sort_values(by="district")
            new["year"]=yr
            new["file"]=qtr
            list9.append(new)
    new1=pd.concat(list9)
    fig= px.bar(new1,x= "registereduser",y= "district", color_discrete_sequence=px.colors.sequential.Emrld_r)
    fig.update_layout(width=400, height= 800)
    return st.plotly_chart(fig)




# In[91]:
def dist_Appopens():
    lst9=[]
    for yr in user_map["year"].unique():
        for qtr in user_map["file"].unique():
            all_year=user_map[(user_map["year"]==yr)&(user_map["file"]==qtr)]
            new=all_year[["district","appopens"]]
            new["year"]=yr
            new["file"]=qtr
            lst9.append(new)
    new1=pd.concat(lst9)
    fig_dist= px.bar(new1,x= "appopens",y= "district", color_discrete_sequence=px.colors.sequential.Emrld_r)
    fig_dist.update_layout(width=400, height= 800)
    return st.plotly_chart(fig_dist)



# In[92]:


def state_registereduser():
    list9=[]
    for yr in user_map["year"].unique():
        for qtr in user_map["file"].unique():
            all_year=user_map[(user_map["year"]==yr)&(user_map["file"]==qtr)]
            new=all_year[["state","registereduser"]]
            new["year"]=yr
            new["file"]=qtr
            list9.append(new)
    new1=pd.concat(list9)
    fig9=px.choropleth(new1,             geojson="https://gist.githubusercontent.com/jbrobst/56c13bbbf9d97d187fea01ca62ea5112/raw/e388c4cae20aa53cb5090210a42ebb9b765c0a36/india_states.geojson",
             featureidkey='properties.ST_NM',   
             locations='state',        #column in dataframe
              color='registereduser',  #dataframe
              color_continuous_scale='Sunsetdark',
              title='Total Users by States',
               height=400)
    fig9.update_geos(fitbounds="locations", visible=False)
    return st.plotly_chart(fig9)


# In[93]:


def Appopens():
    lst9=[]
    for yr in user_map["year"].unique():
        for qtr in user_map["file"].unique():
            all_year=user_map[(user_map["year"]==yr)&(user_map["file"]==qtr)]
            new=all_year[["state","appopens"]]
            new=new.sort_values(by="state")
            new["year"]=yr
            new["file"]=qtr
            lst9.append(new)
    new1=pd.concat(lst9)
    fig9=px.choropleth(new1,             geojson="https://gist.githubusercontent.com/jbrobst/56c13bbbf9d97d187fea01ca62ea5112/raw/e388c4cae20aa53cb5090210a42ebb9b765c0a36/india_states.geojson",
             featureidkey='properties.ST_NM',   
             locations='state',        #column in dataframe
              color='appopens',  #dataframe
              color_continuous_scale='Sunsetdark',
              title='Total Appopens by States',
               height=400)
    fig9.update_geos(fitbounds="locations", visible=False)
    return st.plotly_chart(fig9)


# In[97]:


#(trans) top pincode
def top_postcode_amount():
   List8=[]
   for yr in trans_top["year"].unique():
       for qtr in trans_top["file"].unique():
           all_year=trans_top[(trans_top["year"]==yr)&(trans_top["file"]==qtr)]
           new= all_year[["pincode","amount"]]
           new= new.groupby("pincode")["amount"].sum().sort_values(ascending=False)
           List8.append(new)
   new1=pd.concat(List8).head(10).reset_index()
   fig8= px.pie(new1, values= "amount", names= "pincode",color_discrete_sequence=px.colors.sequential.Emrld_r)
   return st.plotly_chart(fig8)


# In[100]:


#(trans) top pincode
def top_postcode_users():
   top=[]
   for yr in user_top["year"].unique():
       for qtr in user_top["file"].unique():
           all_year=user_top[(user_top["year"]==yr)&(user_top["file"]==qtr)]
           new= user_top[["pincode","registeredusers"]]
           new= new.groupby("pincode")["registeredusers"].sum().sort_values(ascending=False)
           top.append(new)
   new1= pd.concat(top).head(10).reset_index()
   figr= px.pie(new1, values= "registeredusers", names= "pincode", color_discrete_sequence=px.colors.sequential.Emrld_r)
   return st.plotly_chart(figr)


# In[ ]:


with st.sidebar:
        select= st.selectbox("Menu",["Home","Explore Data","About"])
if select =="Home":
    st.title('Phonepe Pulse | :violet[THE BEAT OF PROGRESS]')
    st.header(":violet[Innovation leading to accelerated growth]")
    st.subheader("The merchant journey")
    st.markdown("PhonePe SmartSpeakers are validating over 100 crore transactions per month") 
    st.markdown("By enabling merchants in more than 17,000 postal codes")
    st.markdown("successfully deploying over 4.4 million devices across India")            
    st.header(":violet[Unlocking the digitisation potential of North East India]")
    st.subheader(" North East region (NER) is linguistically and culturally distinct")
    st.markdown("UPI has now become the most popular mode of payment and 58%* of the total 15 lakh merchants in the region have been digitized")
    st.header(":violet[The changing landscape of Insurance in India]")  
    st.subheader("Busting myths about the Indian Insurance market")
    st.markdown("As per the PhonePe Pulse data, a whopping 77% of all Insurance purchased on PhonePe is from non-metro cities")
    st.download_button(':violet[Download The App]', "https://www.phonepe.com/app-download/")            
                

if select == "Explore Data":
    option = st.sidebar.selectbox('Select',('Transaction', 'User'))
    if option =='Transaction':
                Year = st.sidebar.select_slider('Select Year',options=[2018,2019,2020,2021,2022,2023])
                Quarter = st.sidebar.select_slider('Select Quarter',options=[1,2,3,4])
                st.header(":violet[Transaction Details]")
                tab1, tab2,tab3,tab4 = st.tabs(["Transaction Amount","Transaction Count","Transaction Categories","Top10"])
                with tab1:
                        st.markdown(":violet[States Transaction Amount]")
                        states_amount()
                        st.markdown(":violet[District Transaction Amount]")
                        district_amount()
                        
                with tab2:
                        st.markdown(":violet[States Transaction Count]")
                        state_count()
                        st.markdown(":violet[District Transaction Count]")
                        district_count()
                with tab3:
                        st.markdown(":violet[Transaction Categories]")
                        categories_count()
                with tab4:
                         st.markdown(":violet[Top 10 States Transaction]")
                         top_state_amount()
                         st.markdown(":violet[Top 10 Districts Transaction]")
                         top_district_amount()
                         st.markdown(":violet[Top 10 Postalcodes Transaction]")
                         top_postcode_amount()

                    
                          

    if option =='User':
                Year = st.sidebar.select_slider('Select Year',options=[2018,2019,2020,2021,2022,2023])
                Quarter = st.sidebar.select_slider('Select Quarter',options=[1,2,3,4])
                st.header(":violet[User Details]")
                tab1, tab2, tab3 = st.tabs(["Registereduser","Appopens", "Top10"])
                with tab1:
                        st.markdown(":violet[State Registereduser]")
                        state_registereduser()
                        st.markdown(":violet[District Registereduser]")
                        district_registereduser()
                        
                with tab2:
                        st.markdown(":violet[State Appopens]")
                        Appopens()
                        st.markdown(":violet[District Appopens]")
                        dist_Appopens()
                        
                with tab3:
                         st.markdown(":violet[Top 10 States Users]")
                         state_user()
                         st.markdown(":violet[Top 10 District Users]")
                         top_district_users()
                         st.markdown(":violet[Top 10 Postalcodes Users]")
                         top_postcode_users()
if select== "About":
    st.title(':violet[Phonepe Pulse Data Visualization and Exploration]')
    st.markdown("The Phonepe pulse Github repository contains a large amount of data related to various metrics and statistics") 
    st.markdown("The goal is to extract this data and process it to obtain insights and information")
    st.markdown("Visualize that process in a user-friendly manner")
    st.markdown(" ")
    st.title(":violet[Using Tools]")
    st.header('A User-Friendly Tool Streamlit and Plotly')
    st.title(":violet[Using Technologies]")
    st.markdown("Github Cloning, Python, Pandas, MySQL, mysql-connector-python, Streamlit, and Plotly")
    st.markdown(" ")
    st.markdown(" ")
    st.header(":violet[Libraries used for:,]")
    st.markdown("Github repository :  Extract data")
    st.markdown("Python & Pandas : Manipulate & Pre-process the data")
    st.markdown("MySQL : Store processed data into table format")
    st.markdown("Mysql-connector-python : To connect to the MySQL database")
    st.markdown("Streamlit : To create a user-friendly interface")
    st.markdown("Plotly : To Visualization")
    st.markdown("Plotly's built-in geo map functions : To display the data on a map")
