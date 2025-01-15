# **Telecommunication Customers Churn Prediction**

**By Muhammad Rizdky Maulady**

-   **Programming Language**

[![forthebadge
made-with-python](http://ForTheBadge.com/images/badges/made-with-python.svg)](https://www.python.org/)

-   **Git and Github**

Repository : [Telecommunication Customers Churn
Prediction](https://github.com/nurimammasri/Marketing-Campaign-Model-Prediction-by-Datalicious.git)

-   **Dataset**

[Telecommunication
Customer](https://www.kaggle.com/datasets/rodsaldanha/arketing-campaign "Telecommunication Customer")
:::

 
`1. Fitur mana yang sebaiknya digunakan dari hasil EDA?`

`2. Apakah ada feature tambahan lain yang mendukung?`

`3. Melakukan training model & prediksi churn sebagai variabel target.`

`4. Mengevaluasi model dengan metrics Recall dan ROC-AUC.`
:::

::: {.cell .markdown id="aogI7aEAweIT"}
Definisi masing-masing kolom

● Customerid: id dari customer

● Gender: gender dari customer

● Seniorcitizen: apakah merupakan senior citizen atau tidak

● Partner: apakah memiliki partner atau tidak

● Dependents: apakah memiliki tanggungan atau tidak seperti anak dll

● Tenure: tenure dari langganan customer

● PhoneService: apakah menggunakan layanan phone atau tidak

● MultipleLines: apakah menggunakan multiple lines atau tidak

● InternetService: Tipe dari internet service yang digunakan

● OnlineSecurity: apakah menggunakan fitur online security

● OnlineBackup: apakah menggunakan fitur online backup

● DeviceProtection: apakah menggunakan fitur device protection

● TechSupport: apakah menggunakan fitur tech support atau tidak

● StreamingTV: apakah menggunakan fitur streaming TV atau tidak

● StreamingMovies: apakah menggunakan fiture streaming film atau tidak

● Contract: tipe contract dari customer

● PaperlessBilling: apakah menggunakan fitur paperless billing atau
tidak

● PaymentMethod: payment tipe yang digunakan oleh customer

● MonthlyCharges: total charges/biaya bulanan yang dibayarkan

● TotalCharges: total charges secara keseluruhan yang dibayarkan

● Churn: target variabel yang menunjukan bahwa customer churn atau tidak
:::

::: {.cell .markdown id="WeSv2WwXtpGU"}
# **📌 Ekstraksi Data** {#-ekstraksi-data}
:::

::: {.cell .code execution_count="98" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":313}" id="dh_SKWROmRAW" outputId="d85dbac9-99e2-492c-80aa-4626faff0edf"}
``` python
# Import library yang dibutuhkan
import pandas as pd


# Tampilkan semua kolom (agar tidak di-truncate)
pd.set_option('display.max_columns', None)
pd.set_option('display.float_format', '{:.6f}'.format)

# Ekstraksi data
raw_data = pd.read_csv('Telecom_Customers_Churn.csv')

# Tampilkan sampel data
raw_data.head()
```

::: {.output .execute_result execution_count="98"}
``` json
{"type":"dataframe","variable_name":"raw_data"}
```
:::
:::

::: {.cell .markdown id="cGaCzyMUuAZ8"}
# **📌 Informasi Umum Data** {#-informasi-umum-data}
:::

::: {.cell .code execution_count="99" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="3L7M7eYVuEha" outputId="3590eb31-822b-40c7-95e5-e45a31e64043"}
``` python
# Periksa informasi umum pada data
raw_data.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 7043 entries, 0 to 7042
    Data columns (total 21 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   customerID        7043 non-null   object 
     1   gender            7043 non-null   object 
     2   SeniorCitizen     7043 non-null   int64  
     3   Partner           7043 non-null   object 
     4   Dependents        7043 non-null   object 
     5   tenure            7043 non-null   int64  
     6   PhoneService      7043 non-null   object 
     7   MultipleLines     7043 non-null   object 
     8   InternetService   7043 non-null   object 
     9   OnlineSecurity    7043 non-null   object 
     10  OnlineBackup      7043 non-null   object 
     11  DeviceProtection  7043 non-null   object 
     12  TechSupport       7043 non-null   object 
     13  StreamingTV       7043 non-null   object 
     14  StreamingMovies   7043 non-null   object 
     15  Contract          7043 non-null   object 
     16  PaperlessBilling  7043 non-null   object 
     17  PaymentMethod     7043 non-null   object 
     18  MonthlyCharges    7043 non-null   float64
     19  TotalCharges      7043 non-null   object 
     20  Churn             7043 non-null   object 
    dtypes: float64(1), int64(2), object(18)
    memory usage: 1.1+ MB
:::
:::

::: {.cell .markdown id="nzkFNZGNvHKk"}
# **📌Transformasi & Cleaning Data** {#transformasi--cleaning-data}
:::

::: {.cell .markdown id="H-TFsHkD1Uu8"}
### **Melakukan Standarisasi Nama Kolom**
:::

::: {.cell .code execution_count="100" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":313}" id="fFxtj6HtvD_5" outputId="195ddfaf-7dbf-45f6-9bf8-d29bcf903734"}
``` python
# Mengubah nama kolom menjadi lower case
raw_data.columns = raw_data.columns.str.lower()

# Tampilkan hasilnya
raw_data.head()
```

::: {.output .execute_result execution_count="100"}
``` json
{"type":"dataframe","variable_name":"raw_data"}
```
:::
:::

::: {.cell .markdown id="6DeNlSOw1Y6J"}
### **Konversi Tipe Data**
:::

::: {.cell .code execution_count="101" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":606}" id="Xkn5FtSyvo4h" outputId="5fc672c4-a213-4d23-b88d-8c5c3c0d9f1f"}
``` python
raw_data[raw_data['totalcharges'].str.contains('[^0-9\.]', regex=True)]
```

::: {.output .execute_result execution_count="101"}
``` json
{"type":"dataframe"}
```
:::
:::

::: {.cell .code execution_count="102" id="7yG56tOqw5HA"}
``` python
import numpy as np

# Proses mengganti spasi menjadi null
raw_data['totalcharges'] = raw_data['totalcharges'].replace('\s+', np.nan, regex=True)

# Proses mengubah tipe data menjadi float
raw_data['totalcharges'] = raw_data['totalcharges'].astype('float')

# Ubah seniorcitizen menjadi tipe object
raw_data['seniorcitizen'] = raw_data['seniorcitizen'].astype('object')
```
:::

::: {.cell .markdown id="WaVE7HBs9SYF"}
-   Penanganan Nilai Tidak Valid: Dengan mengganti spasi dalam kolom
    totalcharges dengan nilai NaN, data yang tidak valid diidentifikasi
    dan ditandai. Ini penting untuk menjaga integritas data dan mencegah
    kesalahan dalam analisis.

-   Konversi Tipe Data: Mengubah tipe data kolom totalcharges menjadi
    float memastikan bahwa nilai-nilai dalam kolom tersebut dapat
    diproses sebagai angka desimal. Ini memungkinkan analisis numerik
    yang akurat dan perhitungan statistik yang diperlukan.

-   Pengelolaan Data Kategorikal: Mengubah tipe data kolom seniorcitizen
    menjadi object memberikan fleksibilitas dalam menyimpan dan
    menganalisis nilai kategorikal. Ini penting untuk analisis yang
    melibatkan klasifikasi atau pengelompokan berdasarkan status
    kewarganegaraan senior.
:::

::: {.cell .code execution_count="103" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":606}" id="Ftejm5S09DB1" outputId="15b6fec1-c278-43e2-ba3c-3fad2de7d80f"}
``` python
# Menampilkan baris yang memiliki null pada totalcharges
raw_data[raw_data['totalcharges'].isna()]
```

::: {.output .execute_result execution_count="103"}
``` json
{"type":"dataframe"}
```
:::
:::

::: {.cell .code execution_count="104" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="InLaeC-DyxwL" outputId="82642ebb-02f7-4511-d8ec-181a44971ed1"}
``` python
# Periksa kembali
raw_data.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 7043 entries, 0 to 7042
    Data columns (total 21 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   customerid        7043 non-null   object 
     1   gender            7043 non-null   object 
     2   seniorcitizen     7043 non-null   object 
     3   partner           7043 non-null   object 
     4   dependents        7043 non-null   object 
     5   tenure            7043 non-null   int64  
     6   phoneservice      7043 non-null   object 
     7   multiplelines     7043 non-null   object 
     8   internetservice   7043 non-null   object 
     9   onlinesecurity    7043 non-null   object 
     10  onlinebackup      7043 non-null   object 
     11  deviceprotection  7043 non-null   object 
     12  techsupport       7043 non-null   object 
     13  streamingtv       7043 non-null   object 
     14  streamingmovies   7043 non-null   object 
     15  contract          7043 non-null   object 
     16  paperlessbilling  7043 non-null   object 
     17  paymentmethod     7043 non-null   object 
     18  monthlycharges    7043 non-null   float64
     19  totalcharges      7032 non-null   float64
     20  churn             7043 non-null   object 
    dtypes: float64(2), int64(1), object(18)
    memory usage: 1.1+ MB
:::
:::

::: {.cell .markdown id="K_yOegeW1coa"}
### **Handle Missing Values**
:::

::: {.cell .code execution_count="105" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="DrzvbwuU078Q" outputId="c48b7739-3456-477e-bb93-5983cb2e92d5"}
``` python
# Hitung mediannya
med_totalcharges = raw_data['totalcharges'].median()

# Melakukan imputasi (pengisian missing value)
raw_data['totalcharges'] = raw_data['totalcharges'].fillna(med_totalcharges)

# Re-Check info umum
raw_data.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 7043 entries, 0 to 7042
    Data columns (total 21 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   customerid        7043 non-null   object 
     1   gender            7043 non-null   object 
     2   seniorcitizen     7043 non-null   object 
     3   partner           7043 non-null   object 
     4   dependents        7043 non-null   object 
     5   tenure            7043 non-null   int64  
     6   phoneservice      7043 non-null   object 
     7   multiplelines     7043 non-null   object 
     8   internetservice   7043 non-null   object 
     9   onlinesecurity    7043 non-null   object 
     10  onlinebackup      7043 non-null   object 
     11  deviceprotection  7043 non-null   object 
     12  techsupport       7043 non-null   object 
     13  streamingtv       7043 non-null   object 
     14  streamingmovies   7043 non-null   object 
     15  contract          7043 non-null   object 
     16  paperlessbilling  7043 non-null   object 
     17  paymentmethod     7043 non-null   object 
     18  monthlycharges    7043 non-null   float64
     19  totalcharges      7043 non-null   float64
     20  churn             7043 non-null   object 
    dtypes: float64(2), int64(1), object(18)
    memory usage: 1.1+ MB
:::
:::

::: {.cell .markdown id="NxiZfjTj1mVw"}
### **Handle Duplicated Data**
:::

::: {.cell .code execution_count="106" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="aAJE2w6B1fkg" outputId="7ec8d832-3bff-423a-ab1d-5c13c955fc12"}
``` python
raw_data.duplicated(subset = 'customerid').sum()
```

::: {.output .execute_result execution_count="106"}
    0
:::
:::

::: {.cell .markdown id="hV6uoZ_s6QuR"}
Tidak ada data yang duplikat.
:::

::: {.cell .markdown id="U2q2oqUM1wnV"}
# **📌Exploratory Data Analysis**
:::

::: {.cell .code execution_count="107" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":542}" id="wn2azhyjciQ7" outputId="569c659e-2863-4292-cdae-b1810fb6cd7d"}
``` python
import plotly.express as px

# Membuat dataframe untuk distribusi churn
churn_counts = raw_data['churn'].value_counts().reset_index()
churn_counts.columns = ['Churn', 'Count']

# Membuat plot menggunakan Plotly
fig = px.bar(churn_counts, x='Churn', y='Count',
             title='Distribusi Churn',
             labels={'Churn': 'Churn Status', 'Count': 'Jumlah Pelanggan'},
             color='Churn')

fig.show()
```

::: {.output .display_data}
```{=html}
<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script charset="utf-8" src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>                <div id="c3adbc57-1e04-4a30-bfb4-d509a769e4e1" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c3adbc57-1e04-4a30-bfb4-d509a769e4e1")) {                    Plotly.newPlot(                        "c3adbc57-1e04-4a30-bfb4-d509a769e4e1",                        [{"alignmentgroup":"True","hovertemplate":"Churn Status=%{x}\u003cbr\u003eJumlah Pelanggan=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"No","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"No","offsetgroup":"No","orientation":"v","showlegend":true,"textposition":"auto","x":["No"],"xaxis":"x","y":[5174],"yaxis":"y","type":"bar"},{"alignmentgroup":"True","hovertemplate":"Churn Status=%{x}\u003cbr\u003eJumlah Pelanggan=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Yes","marker":{"color":"#EF553B","pattern":{"shape":""}},"name":"Yes","offsetgroup":"Yes","orientation":"v","showlegend":true,"textposition":"auto","x":["Yes"],"xaxis":"x","y":[1869],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Churn Status"},"categoryorder":"array","categoryarray":["No","Yes"]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Jumlah Pelanggan"}},"legend":{"title":{"text":"Churn Status"},"tracegroupgap":0},"title":{"text":"Distribusi Churn"},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){
                            
var gd = document.getElementById('c3adbc57-1e04-4a30-bfb4-d509a769e4e1');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>
```
:::
:::

::: {.cell .markdown id="ehV1F4eOH59q"}
Dari grafik ini, dapat disimpulkan bahwa mayoritas pelanggan tidak
mengalami churn, yang menunjukkan bahwa perusahaan atau layanan tersebut
memiliki tingkat retensi pelanggan yang baik. Sebaliknya, jumlah
pelanggan yang mengalami churn relatif kecil, yang mungkin menunjukkan
bahwa ada peluang untuk meningkatkan strategi retensi pelanggan lebih
lanjut.
:::

::: {.cell .code execution_count="108" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":300}" id="cExuW9MRcsTF" outputId="2e088ce7-da48-4788-e89f-e5c202375895"}
``` python
# Memilih kolom numerik
numeric_columns = raw_data.select_dtypes(include=['int64', 'float64'])

# Menampilkan statistik deskriptif untuk kolom numerik
numeric_columns.describe()
```

::: {.output .execute_result execution_count="108"}
``` json
{"summary":"{\n  \"name\": \"numeric_columns\",\n  \"rows\": 8,\n  \"fields\": [\n    {\n      \"column\": \"tenure\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 2478.9752758409018,\n        \"min\": 0.0,\n        \"max\": 7043.0,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          32.37114865824223,\n          29.0,\n          7043.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"monthlycharges\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 2468.7047672837775,\n        \"min\": 18.25,\n        \"max\": 7043.0,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          64.76169246059918,\n          70.35,\n          7043.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"totalcharges\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 3119.0484860242914,\n        \"min\": 18.8,\n        \"max\": 8684.8,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          2281.9169281556156,\n          1397.475,\n          7043.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe"}
```
:::
:::

::: {.cell .markdown id="32Abf-nJcuST"}
Di asumsikan bahwa nilai pada kolom tenure adalah jumlah bulan
:::

::: {.cell .markdown id="fK8LAPLwcwvH"}
## **Handling Outliers**
:::

::: {.cell .code execution_count="109" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":1000}" id="PCoOzXG1c0eJ" outputId="e5001cf5-593f-4142-a104-157159a74b30"}
``` python
import plotly.express as px

df_numeric = raw_data.select_dtypes(include=['int64', 'float64'])  # Memilih kolom numerik

# Definisikan warna yang digunakan
color = ['#ff6d00', '#ff8500', '#ff9e00', '#240046', '#5a189a', '#9d4edd', '#18af9d']

# Loop untuk membuat box plot untuk setiap kolom numerik
for i in range(len(df_numeric.columns)):
    # Membuat box plot horizontal
    fig = px.box(
        df_numeric,
        x=df_numeric.columns[i],
        orientation='h',
        color_discrete_sequence=[color[i % len(color)]]
    )

    # Memperbarui layout dan menampilkan plot
    fig.update_layout(
        title=f'<b>Box Plot untuk {df_numeric.columns[i]}</b>',
        yaxis=dict(
            title='',
            showgrid=False,
            showline=False,
            showticklabels=False,
            zeroline=False,
        ),
        xaxis=dict(
            title='Total',
            showgrid=False,
            showline=True,
            showticklabels=True,
            zeroline=False,
        )
    )

    fig.show()
```

::: {.output .display_data}
```{=html}
<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script charset="utf-8" src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>                <div id="abd9ee47-54a9-4654-86b5-38e62e48f635" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("abd9ee47-54a9-4654-86b5-38e62e48f635")) {                    Plotly.newPlot(                        "abd9ee47-54a9-4654-86b5-38e62e48f635",                        [{"alignmentgroup":"True","hovertemplate":"tenure=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#ff6d00"},"name":"","notched":false,"offsetgroup":"","orientation":"h","showlegend":false,"x":[1,34,2,45,2,8,22,10,28,62,13,16,58,49,25,69,52,71,10,21,1,12,1,58,49,30,47,1,72,17,71,2,27,1,1,72,5,46,34,11,10,70,17,63,13,49,2,2,52,69,43,15,25,8,60,18,63,66,34,72,47,60,72,18,9,3,47,31,50,10,1,52,64,62,3,56,46,8,30,45,1,11,7,42,49,9,35,48,46,29,30,1,66,65,72,12,71,5,52,25,1,1,38,66,68,5,72,32,43,72,55,52,43,37,64,3,36,10,41,27,56,6,3,7,4,33,27,72,1,71,13,25,67,1,2,43,23,64,57,1,72,8,61,64,71,65,3,1,30,15,8,7,70,62,6,14,22,22,16,10,13,20,2,53,11,69,4,72,58,16,43,2,14,53,32,34,15,7,15,61,1,1,8,33,13,1,20,3,13,40,43,6,69,72,59,20,24,59,72,1,27,14,71,13,44,33,72,1,19,64,2,1,61,29,23,57,72,66,65,8,4,71,1,4,12,24,31,1,30,47,54,50,1,72,29,2,10,18,11,16,72,72,41,65,13,4,41,15,1,42,51,2,1,32,10,67,61,50,2,29,3,13,57,31,45,61,50,19,59,71,16,57,1,20,1,5,52,21,14,5,6,10,1,68,18,22,20,1,8,10,24,35,23,6,12,1,71,35,40,1,23,4,4,68,38,52,32,29,38,48,1,22,43,5,5,51,71,38,24,35,54,72,1,9,69,52,11,2,28,17,35,8,46,7,2,68,43,68,36,63,32,71,66,63,41,1,2,70,23,64,37,17,7,4,21,10,16,64,27,42,5,41,58,47,18,5,23,1,71,72,33,2,24,56,37,43,1,25,61,17,41,1,72,1,48,11,55,42,44,1,27,27,2,19,42,66,33,34,33,23,32,11,69,68,20,72,60,32,1,1,3,46,29,51,48,16,70,40,22,1,5,7,29,44,10,55,52,10,18,68,61,72,2,12,41,26,36,72,35,1,16,49,54,18,36,60,1,52,8,72,64,22,60,28,61,24,28,30,2,1,6,24,4,7,72,70,64,72,44,13,17,1,9,24,1,24,35,7,5,15,11,48,20,72,8,72,15,72,0,1,63,2,2,61,1,22,28,70,5,12,34,71,70,52,69,20,11,2,6,1,20,61,5,56,30,40,28,5,27,12,67,29,55,23,34,52,72,58,35,56,24,70,2,68,1,12,63,33,69,60,72,11,1,10,13,34,39,65,50,15,72,72,55,23,32,56,1,38,11,1,56,3,7,59,7,71,15,71,35,11,60,47,11,56,28,61,31,9,35,2,12,1,4,1,3,1,52,5,72,71,72,46,63,30,1,12,16,4,51,65,16,2,66,46,32,72,38,51,72,65,9,9,66,44,50,15,8,66,57,7,10,62,40,20,7,25,23,66,72,49,43,46,72,10,40,65,31,68,56,10,68,43,1,49,15,20,1,50,2,24,3,1,35,17,8,10,68,45,2,37,4,10,1,65,57,3,2,49,4,70,53,53,1,22,52,65,48,2,3,45,1,61,3,40,1,1,51,2,52,51,1,31,47,3,22,1,72,3,47,72,66,35,29,2,4,25,65,27,29,29,1,20,58,14,72,46,71,32,26,68,2,61,4,3,33,9,22,5,30,65,45,5,25,72,27,32,30,70,42,72,47,2,10,61,5,72,72,3,48,63,27,70,7,0,2,20,66,3,15,72,1,22,3,72,65,11,22,14,41,17,11,15,1,5,33,72,3,2,59,2,71,5,27,1,63,46,72,34,24,72,60,68,8,34,6,2,31,20,1,62,70,10,39,46,6,72,18,71,40,1,58,70,42,34,5,25,2,55,21,70,61,43,47,5,62,16,7,14,60,34,50,38,70,37,4,60,62,1,36,44,55,72,12,13,1,15,65,12,72,72,72,52,2,5,68,62,72,1,66,72,26,64,20,3,22,4,62,5,59,3,72,57,66,60,45,3,15,51,60,33,10,26,6,67,49,1,7,27,37,63,31,50,32,1,63,30,71,53,12,50,2,9,17,56,67,9,4,19,8,71,10,15,72,12,72,1,23,72,26,21,60,12,16,63,22,32,3,13,68,30,16,33,72,4,12,4,0,6,65,15,24,13,24,72,54,3,4,32,35,35,2,8,22,15,22,1,71,4,25,32,7,17,8,56,1,8,7,3,71,2,1,49,58,44,59,71,1,11,62,35,20,40,39,1,72,33,12,1,27,34,56,58,22,10,13,35,34,4,72,2,7,27,4,37,21,53,18,2,32,23,3,71,9,1,18,12,71,64,4,23,39,28,5,45,37,60,8,47,26,3,50,27,8,62,71,66,68,13,56,38,14,16,14,32,8,43,52,3,29,1,12,16,40,5,40,36,5,10,2,23,26,72,34,10,14,23,47,24,49,20,2,2,22,7,1,59,58,41,59,3,32,46,0,2,52,13,11,32,17,16,51,29,70,71,41,1,7,25,67,5,15,20,3,54,42,9,63,69,69,40,60,4,71,37,32,39,38,52,48,70,20,50,19,25,12,39,7,23,27,47,26,14,11,2,26,72,63,71,11,14,13,6,11,18,1,32,29,3,2,13,41,1,7,52,45,70,53,62,60,3,23,1,67,12,71,25,5,26,1,70,72,60,32,1,14,13,6,46,15,43,39,21,57,53,18,1,58,71,35,3,38,35,7,47,14,20,66,15,42,17,37,12,53,60,18,1,3,9,1,56,17,11,7,69,19,3,54,62,24,62,17,9,64,2,1,16,72,30,49,61,47,20,34,70,54,61,3,13,16,3,25,30,21,1,15,23,45,24,11,1,56,1,1,7,55,2,72,45,47,46,2,2,12,68,69,56,4,64,59,62,63,53,5,49,62,55,71,72,36,25,72,36,1,72,59,7,1,30,64,63,72,8,62,67,6,70,20,5,24,11,72,66,45,69,15,28,70,36,16,18,34,42,48,47,39,11,7,3,8,1,32,60,10,71,4,1,43,59,23,72,22,1,69,50,1,2,15,31,1,66,0,3,8,64,28,57,14,19,10,51,67,11,72,66,18,9,9,48,10,9,13,4,4,72,51,59,10,61,54,33,27,1,23,1,45,39,5,72,58,70,61,2,46,1,22,48,64,72,12,34,72,29,33,1,62,41,64,4,24,14,3,4,18,8,35,1,66,8,71,43,2,29,15,65,35,64,58,18,67,63,60,9,70,15,48,12,71,44,1,45,23,43,35,9,12,65,2,27,40,5,8,58,52,3,41,20,1,4,23,6,8,18,52,31,29,36,16,42,1,60,5,22,36,4,9,1,12,23,62,37,8,31,13,24,45,69,2,61,41,44,39,72,13,51,71,22,2,56,1,23,66,1,19,11,8,52,3,51,15,64,37,13,49,45,18,1,68,54,23,17,71,67,14,1,63,41,17,56,5,2,3,37,29,8,63,7,3,72,19,59,2,35,14,14,69,7,69,72,8,4,63,72,46,5,30,63,60,63,25,1,6,22,31,39,26,53,1,12,16,2,39,1,7,4,10,55,72,10,11,15,23,1,3,47,15,66,68,17,7,12,21,21,56,6,65,42,68,48,50,7,63,17,42,4,62,2,2,48,27,70,1,46,30,15,69,65,72,13,17,51,51,72,67,34,67,49,53,27,23,69,2,35,46,54,56,9,20,11,30,68,38,17,48,1,63,3,48,66,68,17,7,72,29,37,34,42,59,11,60,27,1,1,17,58,1,3,53,35,50,68,47,65,5,51,46,9,8,14,45,8,1,66,72,41,23,29,4,6,67,7,56,72,72,23,35,27,26,12,40,7,70,60,39,72,1,54,3,63,71,42,47,66,21,11,1,55,69,3,4,30,5,71,29,52,68,46,8,72,17,3,2,9,51,6,3,17,30,31,45,64,1,1,61,1,9,72,1,7,66,1,40,16,2,67,41,56,72,3,54,52,50,14,27,72,62,12,44,54,68,20,50,58,35,2,63,58,27,71,63,71,41,13,2,68,1,65,72,28,72,2,18,60,26,1,4,68,38,42,57,54,12,44,42,72,71,19,23,30,35,10,1,22,7,36,34,72,36,1,23,32,71,23,17,1,12,72,1,72,60,61,6,32,31,19,72,32,65,45,42,8,32,22,57,1,1,1,24,1,54,4,65,56,45,71,59,69,19,55,38,10,47,2,1,1,1,46,38,65,19,52,71,1,52,6,26,48,64,3,1,72,1,51,41,72,43,72,47,72,3,1,2,26,29,35,27,24,67,16,23,14,1,1,4,16,46,68,38,30,5,17,4,12,72,3,56,41,40,7,69,7,5,72,44,65,3,24,44,72,24,1,22,70,25,37,22,59,49,47,31,1,3,53,1,20,3,51,51,13,1,1,63,3,46,1,8,71,55,70,2,67,65,14,20,1,1,49,72,46,24,5,33,42,23,8,66,24,24,69,53,60,7,20,23,72,11,21,1,31,57,45,10,58,14,27,14,12,69,25,58,35,16,45,17,1,22,1,67,67,2,23,9,5,54,57,24,49,5,2,4,70,5,53,47,31,13,28,10,38,1,67,52,62,16,5,12,72,71,24,15,67,2,5,15,1,41,43,1,1,26,22,71,7,28,16,7,69,1,3,21,69,71,69,48,47,2,45,51,22,72,37,71,7,66,51,30,34,64,65,47,1,49,67,39,14,43,56,14,1,16,70,72,23,21,1,1,32,17,4,36,50,48,50,72,10,18,1,1,9,2,40,69,37,18,11,8,3,55,33,46,34,3,30,33,45,40,71,1,72,22,46,55,1,12,31,5,67,1,40,41,1,51,42,23,1,1,56,15,12,54,7,33,16,21,30,3,11,62,18,6,46,21,68,1,25,24,30,2,51,57,15,72,2,28,29,70,13,59,13,7,62,21,2,1,4,19,30,67,72,53,5,71,50,56,2,2,24,46,71,29,69,71,1,56,56,1,28,19,66,17,52,19,36,7,72,67,34,57,7,1,8,69,50,10,12,14,70,64,66,71,20,72,71,38,28,17,33,23,58,70,4,45,10,36,54,23,41,5,27,1,67,72,56,44,66,34,69,1,40,30,11,15,11,64,72,72,1,15,60,56,8,3,49,2,6,70,12,52,72,40,1,3,40,1,30,23,1,44,65,7,72,8,16,66,1,3,53,8,69,5,72,13,4,54,72,12,1,1,54,69,48,48,8,71,2,67,34,3,9,71,57,72,48,18,43,72,35,4,49,71,11,63,65,49,29,15,4,72,26,35,57,28,25,47,57,16,5,17,56,72,21,48,68,30,3,14,4,71,8,61,72,5,49,8,3,9,67,46,67,55,33,62,1,49,1,14,18,1,1,72,64,69,1,71,66,2,71,11,47,35,32,60,11,29,21,48,3,43,5,1,71,8,8,20,33,71,31,38,1,2,12,9,11,6,71,42,8,5,2,45,28,43,60,42,7,25,40,27,10,27,11,4,68,1,18,57,26,17,1,38,59,30,2,50,9,3,14,31,7,8,17,32,2,7,72,31,27,18,7,14,11,72,28,15,4,71,5,47,57,50,8,48,70,1,8,1,1,60,49,4,29,67,53,67,6,47,53,69,3,4,56,59,61,2,46,12,14,28,24,31,68,39,42,13,6,35,38,18,4,27,41,50,72,70,44,2,34,72,71,64,72,1,29,23,52,25,64,16,1,24,2,34,36,53,47,72,72,1,9,8,45,7,71,41,67,69,70,25,72,34,65,70,72,35,13,12,62,25,52,8,2,56,12,47,2,18,8,45,3,38,72,46,71,66,25,18,13,65,60,15,72,30,42,71,1,39,35,53,1,31,48,30,10,12,57,58,37,44,27,8,3,25,57,12,62,65,71,21,71,7,72,1,72,64,72,29,13,31,1,7,61,39,10,14,1,67,72,6,1,25,33,18,71,28,2,17,56,60,33,1,2,63,7,55,65,1,63,70,36,52,22,22,5,47,33,18,1,56,2,35,64,15,24,1,70,1,4,39,29,14,61,13,66,2,59,62,33,66,72,1,19,51,63,27,22,4,42,29,4,30,4,71,46,4,7,69,72,19,28,5,72,8,7,22,72,8,52,68,71,2,34,35,61,1,1,53,72,2,3,13,41,24,28,8,1,54,41,19,72,62,56,15,10,32,21,62,2,27,5,25,2,49,63,4,1,11,52,60,64,43,61,1,5,66,67,42,1,31,7,4,34,3,19,31,1,3,46,1,69,5,1,26,10,25,64,30,13,64,46,12,15,17,13,67,24,6,53,16,10,13,9,25,7,38,43,4,25,27,72,71,24,50,57,15,4,28,9,55,3,10,55,20,62,32,43,9,60,58,7,2,37,65,39,66,68,62,3,72,41,29,4,53,1,41,39,63,15,13,1,1,8,60,12,40,66,42,66,49,1,41,41,23,3,4,52,4,11,2,26,24,12,60,64,66,60,17,42,1,47,10,70,67,1,7,1,4,66,12,24,26,6,57,14,42,25,64,22,19,61,22,70,12,31,11,68,72,67,60,1,1,58,47,1,1,22,48,37,13,43,6,71,1,72,6,12,25,21,6,20,18,43,35,1,32,52,32,72,51,68,8,49,72,9,28,54,11,50,69,1,68,40,31,33,55,68,12,71,40,64,53,12,53,72,46,40,12,9,51,49,41,56,4,20,26,20,7,7,51,4,1,27,22,12,3,34,24,51,14,59,3,65,5,59,72,62,28,3,19,1,24,57,72,67,52,71,26,35,55,33,72,1,10,37,12,1,62,1,18,69,2,19,12,9,27,27,1,24,14,32,11,1,38,9,54,29,44,59,3,18,67,22,33,5,2,72,9,67,16,8,5,23,1,50,17,68,1,25,67,32,67,72,71,1,46,2,1,48,61,32,2,3,5,71,37,65,67,49,50,25,17,64,25,23,24,37,21,1,10,6,51,10,6,47,61,52,35,71,6,45,2,4,2,4,51,60,9,3,17,8,46,68,1,4,1,28,39,11,71,2,30,17,55,58,5,1,9,26,50,72,43,56,1,72,72,36,5,13,44,70,44,32,69,16,68,16,68,4,26,29,5,70,24,72,1,70,36,38,17,41,1,2,14,2,1,13,6,4,5,15,47,8,17,15,26,23,4,29,25,9,18,3,69,14,19,39,31,24,14,64,50,52,28,21,25,17,58,17,51,72,52,27,3,64,45,3,71,1,58,34,8,15,66,12,58,3,43,9,3,22,40,68,54,50,1,72,40,72,6,5,48,1,64,17,40,41,51,41,1,2,68,24,70,3,2,3,7,13,7,12,53,12,63,15,36,4,24,61,16,65,26,16,54,1,5,19,10,23,3,72,10,10,11,37,17,36,17,66,61,22,1,6,31,68,34,52,10,29,72,47,24,65,4,12,1,33,34,14,4,13,65,23,55,49,60,69,40,67,35,19,13,41,4,24,5,5,1,72,24,42,4,68,33,1,31,4,69,38,3,48,15,25,1,48,1,1,37,66,26,63,10,2,18,64,9,28,1,4,38,66,1,18,51,0,1,12,41,12,55,7,12,68,5,49,40,16,10,72,2,23,71,11,1,16,1,12,54,68,4,1,27,21,13,64,1,57,21,19,31,52,46,11,53,11,57,2,2,71,1,68,72,2,1,41,72,6,4,12,58,7,65,1,56,4,58,62,26,62,58,68,61,42,18,56,4,4,35,64,31,67,4,70,3,53,2,29,47,68,12,8,54,69,26,72,70,1,10,28,1,21,51,53,53,24,70,61,11,2,25,41,18,72,71,34,29,40,36,46,58,39,4,52,70,65,1,70,29,1,67,1,26,30,48,55,7,37,31,4,72,5,1,15,8,35,56,42,65,2,65,18,23,4,70,4,19,18,38,2,47,52,9,26,8,44,3,2,9,1,25,2,43,1,58,59,44,66,68,9,19,4,70,1,8,53,51,11,60,17,3,70,1,43,16,57,37,72,11,50,5,1,16,2,17,16,15,10,46,64,1,25,71,8,72,49,29,72,31,50,71,70,71,61,32,1,68,62,7,20,6,33,28,27,7,26,5,30,63,1,53,14,21,17,16,35,32,28,1,59,72,36,40,40,9,63,3,40,8,34,5,9,9,31,50,2,1,8,9,2,3,25,1,45,51,55,38,2,38,34,70,13,39,61,12,41,21,55,69,26,69,18,47,72,33,2,72,37,62,71,23,16,9,17,4,1,24,1,72,72,11,9,2,60,29,49,30,53,39,9,39,8,51,71,71,70,1,38,28,32,49,37,10,67,7,51,9,9,4,71,50,24,22,44,33,1,54,42,1,1,30,1,16,1,9,46,1,71,43,50,13,19,41,1,24,40,3,37,67,32,6,32,59,30,20,27,20,9,68,69,26,69,11,1,10,55,44,46,69,11,11,29,57,28,42,2,23,18,62,1,16,3,67,62,57,2,23,25,72,2,8,5,35,24,2,72,41,4,26,7,1,4,48,2,12,60,55,1,1,4,1,42,1,7,3,72,15,4,11,5,1,72,55,40,57,1,1,1,52,41,43,47,3,66,55,29,12,66,35,10,27,58,54,9,2,6,26,9,8,12,15,43,42,31,66,18,1,61,10,1,18,24,3,50,1,2,17,69,72,3,50,53,58,46,72,1,6,72,4,52,0,2,65,43,4,25,51,12,57,24,64,4,26,15,64,36,27,1,35,4,8,10,2,58,51,46,1,46,50,53,61,5,47,54,19,26,70,17,30,1,19,26,21,50,68,3,9,51,9,41,22,21,71,1,26,71,4,12,18,3,72,11,1,13,72,42,17,7,68,56,38,72,48,52,35,67,1,53,34,3,1,19,60,11,47,18,60,72,39,59,2,1,20,6,71,24,67,1,48,37,11,3,18,50,67,25,2,9,10,70,9,4,2,1,19,7,1,1,9,3,9,5,56,18,49,70,72,6,17,29,6,63,16,59,3,8,7,68,68,52,72,32,72,1,42,25,45,43,37,20,4,63,3,66,28,8,71,1,72,16,66,11,51,8,14,4,70,70,54,28,24,69,42,2,39,45,72,38,72,1,72,55,51,63,1,23,1,2,52,36,1,28,7,14,72,1,10,42,7,4,72,20,63,56,5,72,68,67,8,52,18,59,60,7,59,46,5,59,70,14,44,64,58,46,58,72,30,11,34,54,3,72,40,2,54,14,1,10,1,1,56,68,14,68,55,16,9,14,58,53,70,14,22,10,29,1,49,68,1,30,72,10,7,9,1,20,1,29,1,3,20,64,1,6,50,6,7,72,8,67,24,72,33,2,70,22,59,36,51,53,20,63,40,35,26,27,53,34,19,43,6,56,57,34,10,1,13,56,55,36,47,12,1,24,63,35,67,25,21,13,35,71,29,71,7,57,65,27,6,72,1,11,39,59,26,2,72,65,72,6,32,50,61,15,72,9,1,12,37,61,18,21,68,12,2,62,29,1,5,1,62,36,28,69,11,63,23,10,71,45,70,22,52,55,65,72,10,7,5,24,72,21,69,44,61,24,1,6,4,72,72,14,7,48,55,1,45,3,71,8,3,69,1,72,11,71,1,33,16,56,1,5,57,56,8,22,1,40,46,63,68,69,56,10,63,24,19,22,29,13,70,49,43,3,42,57,2,72,46,66,62,72,35,17,72,28,56,31,45,1,2,6,48,25,64,50,52,4,32,45,9,66,3,54,1,64,31,14,12,67,35,45,10,29,24,66,51,45,49,29,40,37,25,22,72,7,33,23,24,1,69,3,56,65,71,14,2,32,40,1,1,7,15,17,19,71,54,31,11,18,72,71,5,38,5,2,52,8,68,69,42,50,1,1,33,7,64,1,59,6,3,15,13,23,31,29,49,56,63,63,24,36,9,3,21,13,1,25,71,66,45,22,67,68,0,49,4,63,2,21,55,1,17,30,22,9,1,21,19,69,1,72,70,66,7,46,39,32,24,6,37,8,72,71,16,57,66,17,21,66,17,1,58,8,27,34,30,33,1,14,16,49,19,70,32,18,37,4,16,17,19,60,51,28,43,42,3,1,3,63,3,68,30,60,15,45,70,10,4,1,68,22,38,1,18,29,16,1,12,31,4,48,15,50,7,41,68,26,57,3,1,19,3,59,1,42,7,67,1,66,61,4,42,64,54,1,54,18,3,1,72,60,11,12,61,39,55,17,37,72,72,8,22,1,38,17,70,72,28,15,72,11,8,57,1,46,30,10,23,32,13,39,44,9,67,9,15,71,1,30,1,17,3,67,1,1,32,41,1,1,12,62,22,17,72,56,9,72,20,19,2,53,27,6,9,8,71,10,1,71,68,34,26,22,7,20,60,72,72,4,16,62,10,31,71,58,70,71,69,1,72,26,33,10,57,10,39,11,21,68,18,6,18,52,56,45,67,3,65,63,11,1,55,25,72,72,65,54,7,72,21,2,4,3,72,6,52,69,8,8,63,60,12,13,22,5,1,72,2,40,44,71,2,26,1,1,65,3,13,33,1,4,2,72,37,15,23,30,42,32,22,42,8,65,2,70,22,4,2,67,25,20,2,51,46,25,13,25,26,43,19,10,2,72,18,9,27,24,69,46,72,22,70,2,31,56,16,52,13,35,59,72,66,49,2,21,54,24,1,6,1,49,56,56,6,32,50,58,65,64,66,38,20,36,64,1,60,1,50,1,72,60,46,69,31,19,71,12,39,44,56,72,5,11,24,15,72,56,64,34,2,35,22,5,9,11,23,4,68,33,31,1,56,1,66,72,34,58,2,37,71,1,71,35,6,3,69,44,53,24,5,2,62,19,9,53,5,71,1,18,72,4,59,1,31,3,65,49,2,53,55,72,36,10,1,72,28,38,61,52,67,34,54,1,15,4,9,46,22,38,55,1,64,53,58,56,72,1,72,22,8,16,39,12,54,18,32,41,67,65,25,1,67,7,43,24,9,69,37,20,7,37,5,41,54,3,69,53,18,64,31,20,57,63,13,48,2,57,71,7,16,34,37,16,48,58,72,7,38,48,10,30,31,46,50,28,66,8,41,72,7,38,44,47,53,4,20,2,57,44,24,15,3,4,37,1,24,5,33,58,72,71,28,51,30,72,36,14,72,22,2,15,51,70,71,39,61,52,1,64,62,30,4,63,1,15,27,4,72,45,45,36,17,1,16,3,4,71,10,20,4,26,4,5,4,29,2,29,1,1,8,13,59,1,50,18,17,47,26,6,19,3,68,2,7,18,71,13,3,72,66,24,1,56,22,14,61,40,42,72,12,71,26,7,6,58,51,72,18,7,47,2,62,16,6,19,69,11,64,39,15,25,6,66,61,43,12,23,71,34,5,41,72,14,41,23,71,1,72,6,23,10,72,7,6,9,12,1,48,20,16,2,10,2,20,20,19,19,22,35,1,39,54,1,66,56,18,16,68,53,72,9,30,36,18,55,39,21,2,33,44,30,71,4,35,1,23,22,49,42,33,7,67,15,67,53,21,40,22,39,45,2,57,8,7,6,7,49,65,55,71,35,3,11,1,17,72,28,18,40,52,47,23,66,8,47,7,71,50,46,1,66,42,5,7,29,27,15,25,11,57,67,47,13,8,44,71,24,15,1,2,55,71,50,1,5,66,49,3,66,11,28,65,62,2,2,55,41,17,30,17,16,72,9,1,23,8,19,7,1,61,57,9,15,1,12,54,4,7,20,26,36,53,3,68,72,12,34,68,50,1,41,30,1,29,23,60,72,22,72,66,72,47,51,70,9,59,3,38,37,37,24,14,72,53,8,72,17,2,8,48,10,0,1,29,65,8,61,45,72,12,7,9,43,58,16,2,8,40,9,41,26,33,68,65,55,20,19,45,70,2,27,12,72,12,5,71,35,70,31,52,37,69,30,33,54,59,55,69,66,37,9,69,10,40,13,6,69,66,11,46,6,56,70,33,72,3,19,5,71,8,1,1,61,71,68,46,33,53,50,57,54,60,28,1,29,10,43,13,43,19,1,69,61,43,6,1,56,70,1,49,6,32,72,37,69,26,58,24,5,15,30,55,25,10,44,47,13,49,64,1,20,37,30,38,1,37,52,71,26,66,72,25,69,53,12,26,21,1,48,26,60,18,10,5,4,65,70,18,62,66,65,3,34,16,54,50,71,10,1,18,4,58,56,2,32,56,36,4,53,10,4,1,51,12,6,63,1,48,5,35,6,2,50,33,31,9,54,46,34,71,63,51,26,64,1,61,15,64,18,57,14,18,72,70,38,68,13,65,30,51,31,9,72,10,37,2,55,33,46,1,20,9,32,19,70,61,26,45,62,1,3,41,67,1,71,37,60,1,6,13,11,7,10,34,62,64,1,25,26,10,53,7,33,71,29,24,20,1,54,5,72,52,9,1,1,33,55,69,1,54,33,45,11,6,21,65,6,8,11,43,49,1,15,60,17,16,35,44,12,1,28,70,5,18,70,9,67,1,18,4,71,30,1,55,59,1,7,45,54,51,72,44,2,66,68,31,21,21,55,9,71,1,22,1,61,67,14,59,21,4,3,70,3,21,20,22,1,63,70,13,5,72,13,61,1,56,4,35,18,72,49,44,3,37,61,70,1,41,70,1,51,42,70,48,68,48,26,11,1,27,46,1,46,25,4,13,31,23,2,65,22,55,9,7,35,6,1,17,10,15,40,13,29,3,58,45,72,68,1,38,2,11,20,72,3,23,40,62,22,11,7,13,1,39,3,58,6,1,22,14,64,1,6,1,39,20,1,1,64,1,46,28,33,39,42,1,7,70,65,1,18,24,63,44,4,1,37,10,34,35,4,39,43,17,61,49,4,64,3,1,40,1,8,1,34,1,39,58,45,6,43,41,5,72,4,9,72,33,72,22,70,21,15,29,15,71,72,19,1,1,2,11,12,70,20,23,49,4,32,2,69,6,24,32,27,27,58,1,18,47,70,13,36,67,10,19,71,72,48,1,18,1,67,69,19,72,38,40,61,10,32,21,59,13,47,69,2,22,15,53,28,22,1,16,48,30,3,57,68,23,65,44,71,37,12,69,35,5,58,72,72,1,39,53,27,1,1,18,46,72,36,4,25,40,63,15,14,72,39,47,19,5,13,17,34,42,5,71,19,2,57,72,6,17,61,1,48,16,9,3,1,65,70,60,69,35,22,66,1,1,34,72,31,30,9,20,19,65,30,6,2,53,7,61,70,13,35,2,3,3,62,72,63,20,35,21,62,15,55,11,17,61,71,2,35,17,21,47,3,3,44,1,44,5,24,1,18,10,65,1,53,3,33,3,34,14,13,46,23,47,17,49,59,69,11,10,12,45,39,71,71,33,67,37,49,9,52,70,1,14,1,1,52,6,7,47,26,25,69,72,4,59,67,26,27,72,6,62,20,6,51,61,62,72,13,5,3,26,13,38,8,34,18,56,36,9,1,12,57,42,33,70,68,1,37,4,1,20,72,31,18,11,33,62,1,16,22,49,36,42,4,12,31,5,66,15,64,10,7,29,57,46,53,17,38,15,22,14,57,11,1,12,3,36,16,65,2,42,72,62,6,48,35,52,1,6,71,67,60,23,39,15,53,24,37,5,50,54,3,68,5,33,41,34,13,20,51,3,41,13,35,12,4,43,12,68,25,7,66,53,63,70,27,1,5,37,3,12,38,9,13,29,47,61,16,41,43,36,6,58,19,11,39,8,26,53,70,1,59,2,7,12,59,61,72,13,64,1,10,65,62,55,25,1,1,59,64,36,3,61,26,1,1,68,2,72,71,57,4,1,72,21,71,29,69,64,16,4,52,2,1,18,2,19,40,66,21,8,72,48,69,72,14,6,8,17,65,57,13,19,56,14,52,58,47,67,2,6,71,46,5,67,3,3,52,42,50,23,67,25,39,69,1,32,9,16,60,72,5,26,3,2,2,36,7,60,19,45,4,31,47,1,1,1,59,10,35,4,32,43,4,54,11,66,61,72,44,41,50,47,8,18,72,1,42,18,13,68,4,69,17,25,43,59,5,21,69,13,42,52,46,61,29,25,5,15,19,44,6,58,62,70,1,10,26,66,7,51,72,65,2,70,72,1,1,5,3,58,22,33,1,54,72,1,3,72,72,54,59,54,60,60,3,69,1,50,56,60,69,1,1,3,60,13,62,45,25,44,2,33,1,22,35,29,27,54,2,57,62,15,2,70,21,23,6,4,3,23,26,8,26,2,67,71,59,39,21,1,48,31,64,46,52,67,67,5,71,9,26,71,32,2,71,60,55,54,2,6,48,63,1,12,54,30,30,4,40,9,17,62,28,70,46,23,47,68,60,67,14,57,55,1,1,23,13,47,38,38,2,1,15,26,35,3,50,42,10,61,68,10,65,72,55,1,7,2,9,27,7,64,70,2,67,45,24,4,44,72,1,66,1,13,10,65,1,38,23,10,4,72,35,1,58,70,38,60,26,8,41,36,54,71,55,72,3,54,72,52,60,39,15,69,43,63,2,72,32,40,58,67,51,31,69,32,21,52,72,72,52,41,41,6,67,16,17,35,58,1,52,70,19,1,35,32,17,67,9,31,4,58,60,58,1,27,66,15,47,41,59,50,17,6,51,44,49,2,59,50,59,18,10,14,35,8,18,60,1,6,19,53,72,60,1,13,5,1,13,37,64,5,61,1,1,26,1,24,17,26,1,40,52,1,1,21,67,44,70,3,56,13,58,42,1,46,63,11,15,72,29,1,6,1,63,2,18,43,15,10,55,49,6,70,2,63,25,18,28,53,35,1,70,2,26,34,19,15,62,42,9,24,68,31,1,21,63,2,61,1,18,6,33,16,56,23,9,14,15,5,61,70,15,8,8,4,34,68,45,9,22,2,70,10,72,49,54,71,22,50,43,45,64,23,68,1,2,26,55,14,71,64,7,57,13,3,72,40,14,2,66,38,1,22,1,5,29,1,3,71,9,43,48,26,9,1,46,2,1,64,12,6,59,7,72,16,25,34,1,10,24,10,69,57,50,28,16,25,3,61,2,51,71,20,6,6,29,36,28,7,63,48,49,27,72,1,72,47,1,36,43,27,9,38,35,0,59,27,2,7,36,41,13,19,60,48,3,69,43,11,45,72,2,12,67,37,39,41,25,8,71,5,30,40,54,72,28,18,2,59,22,1,72,14,50,48,49,28,68,13,11,3,57,3,72,70,49,67,46,64,37,2,13,72,68,15,24,24,27,12,71,67,63,1,4,40,12,52,10,68,54,4,52,1,70,43,52,12,56,0,42,22,51,27,51,4,1,35,71,1,69,14,57,72,48,4,31,38,37,1,57,62,3,72,29,13,3,11,21,19,61,11,35,25,1,67,19,56,72,43,55,2,27,13,70,14,19,20,43,5,70,40,6,39,4,15,1,45,64,57,72,1,72,3,55,59,18,32,4,66,27,4,60,8,8,35,7,53,18,15,67,6,6,13,11,1,5,13,9,29,1,1,18,2,30,66,38,44,54,2,42,58,58,25,71,37,14,4,48,3,8,1,67,13,45,49,52,63,68,31,64,62,1,6,21,72,32,71,34,3,12,8,35,3,3,53,4,48,6,3,54,1,62,22,1,51,30,56,35,64,30,25,41,9,1,70,57,9,69,43,72,44,72,33,54,27,54,3,53,1,15,56,5,48,25,3,58,10,1,71,65,5,28,67,35,72,61,68,1,3,70,48,68,47,32,5,49,48,13,15,12,67,9,13,38,42,24,27,9,49,61,50,25,22,1,4,18,56,53,51,24,62,24,70,1,16,8,72,23,31,37,30,35,23,20,36,8,71,50,43,57,41,27,13,3,67,3,64,26,38,23,40,72,3,23,1,4,62,40,41,34,1,51,1,39,12,12,72,63,44,18,9,13,68,6,2,55,1,38,67,19,12,72,24,72,11,4,66],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Total"},"showgrid":false,"showline":true,"showticklabels":true,"zeroline":false},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":""},"showgrid":false,"showline":false,"showticklabels":false,"zeroline":false},"legend":{"tracegroupgap":0},"margin":{"t":60},"boxmode":"group","title":{"text":"\u003cb\u003eBox Plot untuk tenure\u003c\u002fb\u003e"}},                        {"responsive": true}                    ).then(function(){
                            
var gd = document.getElementById('abd9ee47-54a9-4654-86b5-38e62e48f635');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>
```
:::

::: {.output .display_data}
```{=html}
<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script charset="utf-8" src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>                <div id="76cdf4f7-3de5-4fee-ab0c-07dbeb5d95bf" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("76cdf4f7-3de5-4fee-ab0c-07dbeb5d95bf")) {                    Plotly.newPlot(                        "76cdf4f7-3de5-4fee-ab0c-07dbeb5d95bf",                        [{"alignmentgroup":"True","hovertemplate":"monthlycharges=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#ff8500"},"name":"","notched":false,"offsetgroup":"","orientation":"h","showlegend":false,"x":[29.85,56.95,53.85,42.3,70.7,99.65,89.1,29.75,104.8,56.15,49.95,18.95,100.35,103.7,105.5,113.25,20.65,106.7,55.2,90.05,39.65,19.8,20.15,59.9,59.6,55.3,99.35,30.2,90.25,64.7,96.35,95.5,66.15,20.2,45.25,99.9,69.7,74.8,106.35,97.85,49.55,69.2,20.75,79.85,76.2,84.5,49.25,80.65,79.75,64.15,90.25,99.1,69.5,80.65,74.85,95.45,99.65,108.45,24.95,107.5,100.5,89.9,42.1,54.4,94.4,75.3,78.9,79.2,20.15,79.85,49.05,20.4,111.6,24.25,64.5,110.5,55.65,54.65,74.75,25.9,79.35,50.55,75.15,103.8,20.15,99.3,62.15,20.65,19.95,33.75,82.05,74.7,84.0,111.05,100.9,78.95,66.85,21.05,21.0,98.5,20.2,19.45,95.0,45.55,110.0,24.3,104.15,30.15,94.35,19.4,96.75,57.95,91.65,76.5,54.6,89.85,31.05,100.25,20.65,85.2,99.8,20.7,74.4,50.7,20.85,88.95,78.05,23.55,19.75,56.45,85.95,58.6,50.55,35.45,44.35,25.7,75.0,20.2,19.6,70.45,88.05,71.15,101.05,84.3,23.95,99.05,19.6,45.65,64.5,69.5,68.55,95.0,108.15,86.1,19.7,80.9,84.15,20.15,64.25,25.7,56.0,82.4,69.7,73.9,20.6,19.9,70.9,89.05,45.3,20.4,84.25,104.4,81.95,94.85,20.55,24.7,74.45,76.45,105.35,20.55,29.95,45.3,84.5,74.75,79.25,24.8,51.8,30.4,19.65,56.6,71.9,91.0,19.75,109.7,19.3,96.55,24.1,111.35,112.25,20.75,101.9,80.05,105.55,78.3,68.85,79.95,55.45,79.9,106.6,102.45,46.0,25.25,19.75,20.0,86.8,58.75,45.25,56.6,84.2,80.0,70.15,24.75,20.2,50.05,19.35,50.6,81.15,55.2,89.9,85.3,108.0,93.5,84.6,20.25,25.15,54.4,29.6,73.15,95.0,19.75,86.6,109.2,74.7,94.4,54.8,75.35,65.0,74.4,48.55,99.0,93.5,70.4,40.2,83.7,19.85,59.55,115.1,114.35,44.6,45.0,41.15,106.9,89.85,49.85,113.3,88.1,24.9,105.0,19.35,24.25,94.45,59.75,24.8,107.05,70.6,85.4,105.05,64.95,55.0,50.55,55.15,51.2,25.4,54.45,95.15,76.0,44.35,70.0,74.5,44.85,76.1,61.2,86.8,89.35,19.7,20.25,76.05,100.8,74.55,73.6,64.9,95.45,90.4,60.3,81.85,24.8,74.9,75.55,101.15,78.75,19.25,89.05,115.05,69.35,80.6,110.05,19.9,80.3,93.15,91.5,82.45,60.0,44.8,48.6,60.05,102.7,82.9,70.35,35.9,82.65,19.85,19.2,94.9,73.85,80.6,75.8,104.6,88.15,94.8,103.4,54.65,85.75,67.45,20.5,20.25,72.1,90.4,19.45,44.95,97.0,62.8,44.6,89.15,84.8,41.9,80.25,54.1,105.25,30.75,97.1,20.2,98.8,50.3,20.55,75.9,96.5,59.95,19.15,98.65,112.6,20.6,85.65,35.75,99.75,96.1,85.1,25.35,104.95,89.65,86.75,86.2,50.65,64.8,90.85,108.1,19.95,85.45,54.75,90.4,44.0,95.6,84.8,44.3,19.9,95.05,90.05,109.9,73.95,54.6,20.05,19.75,20.05,99.45,55.9,19.7,19.8,95.4,93.95,19.9,19.6,81.35,24.45,74.95,87.35,70.65,73.25,98.7,24.8,83.3,75.3,24.3,69.85,100.55,25.7,40.7,51.65,105.1,85.95,75.6,58.25,19.4,65.2,53.45,45.4,19.75,44.45,20.85,114.05,89.85,55.05,112.95,101.55,114.65,64.8,80.4,105.9,69.55,25.05,94.75,105.5,24.7,69.75,60.2,81.05,24.4,104.15,92.9,80.8,20.0,75.1,19.65,69.45,101.15,99.8,116.05,40.05,102.1,89.7,19.9,55.95,20.65,55.0,70.05,53.6,74.7,80.25,76.05,75.7,96.1,69.0,19.65,45.3,81.45,108.5,83.55,84.5,100.15,88.6,52.55,74.35,104.8,59.0,74.4,64.05,20.4,43.75,60.9,19.8,28.45,99.7,116.25,80.7,65.2,84.05,79.45,94.1,78.0,94.2,80.5,19.85,94.3,106.45,74.35,105.45,95.0,104.8,54.3,70.05,75.2,20.05,105.4,51.6,85.5,75.6,100.05,91.25,115.75,94.7,19.6,99.9,21.1,20.05,79.95,107.15,85.0,89.55,81.55,58.45,95.65,80.6,113.1,58.95,19.55,86.05,45.55,78.95,86.3,105.05,101.9,19.75,110.3,115.6,19.35,25.6,80.35,68.75,19.9,70.6,70.2,49.3,107.25,23.6,69.7,99.5,64.3,70.85,101.9,73.5,100.25,40.4,19.25,59.6,64.9,100.3,110.85,81.05,98.05,70.5,94.55,19.65,19.0,75.3,89.2,19.0,20.0,85.7,63.25,20.1,99.15,90.4,111.9,24.9,83.5,84.3,45.6,61.65,54.85,65.55,90.35,20.4,74.55,19.95,74.25,108.65,109.55,86.65,81.0,47.85,114.55,105.25,29.95,65.0,20.55,109.8,69.5,48.85,25.25,102.85,87.55,78.55,34.55,92.05,85.05,19.7,20.0,95.15,84.25,104.6,111.65,90.05,110.75,55.0,89.85,20.35,54.55,105.5,99.45,70.9,104.55,85.25,25.4,56.15,89.55,89.85,25.25,94.55,45.7,69.65,89.5,70.0,69.55,74.6,20.1,24.8,19.65,95.1,88.85,78.8,19.85,20.35,24.25,45.25,20.05,69.55,19.5,74.75,69.65,30.2,45.65,57.8,19.85,25.55,75.05,24.85,49.15,110.35,24.55,34.7,107.95,81.4,80.0,73.8,64.4,103.75,71.1,49.9,24.6,49.25,30.1,83.4,20.45,75.25,20.55,75.1,20.05,20.65,85.15,50.15,84.95,66.5,63.3,83.15,84.9,20.55,49.25,79.85,59.6,104.65,75.3,80.1,19.55,81.0,24.7,86.0,25.4,89.15,58.25,85.65,50.35,80.35,20.2,20.55,85.95,45.35,94.5,21.25,26.25,80.85,91.7,74.2,87.25,20.35,75.5,79.05,90.15,50.6,110.45,101.0,79.35,89.85,65.0,80.45,98.55,24.1,44.05,110.8,114.95,75.05,19.25,90.05,56.7,80.15,71.35,20.25,90.35,98.55,19.7,19.85,85.9,90.35,20.8,89.25,70.3,66.85,19.9,35.8,78.85,20.4,74.25,64.8,20.45,93.35,19.9,88.9,95.8,110.65,40.3,82.0,107.0,45.35,73.35,44.8,54.75,52.2,40.6,110.0,55.3,60.85,78.4,69.65,59.85,76.9,19.85,67.65,45.0,64.2,81.7,25.55,20.0,96.75,75.65,98.5,23.8,64.2,85.35,76.8,55.2,108.55,101.3,69.55,103.25,104.0,25.25,30.4,20.05,84.6,86.2,103.7,111.2,88.0,106.35,79.15,103.1,63.95,25.8,89.45,95.6,25.55,90.95,44.85,108.55,25.05,74.1,88.8,78.85,93.25,71.4,44.4,79.2,20.4,100.0,105.0,19.8,30.85,89.9,20.55,84.85,33.15,92.0,89.8,115.8,85.15,24.85,64.35,20.5,100.15,86.05,50.8,89.0,64.8,19.8,93.4,73.65,95.1,94.65,80.6,39.0,20.5,85.55,26.4,98.2,97.55,19.95,50.8,99.7,34.8,105.1,60.15,64.75,54.65,110.1,19.3,83.9,111.25,35.8,20.05,84.35,110.5,91.2,100.55,89.3,103.85,81.1,24.6,81.2,94.3,116.1,105.55,98.9,94.4,19.5,98.3,93.85,105.6,81.35,100.5,56.4,65.35,19.95,111.25,72.85,89.0,106.1,20.05,25.2,73.55,75.4,65.55,80.7,104.55,24.15,20.45,75.4,79.7,81.7,76.3,79.4,81.15,103.75,86.45,75.1,80.6,19.3,84.6,33.6,83.25,80.85,79.05,108.05,19.9,21.05,30.15,79.85,65.5,104.1,74.4,20.5,91.35,99.05,20.5,44.95,75.6,55.1,58.95,95.1,44.7,25.45,56.75,81.75,86.1,29.8,20.5,60.9,73.25,45.7,100.3,19.25,20.85,77.35,96.0,90.55,93.85,70.1,30.35,75.95,108.05,69.9,75.25,103.75,54.95,19.5,19.6,47.85,86.6,23.75,80.6,43.8,19.75,19.15,19.6,80.3,24.35,25.25,26.1,20.0,85.3,70.0,94.3,20.7,70.3,95.35,75.5,69.55,19.85,20.0,95.85,90.1,68.95,99.55,20.75,50.15,58.65,95.9,49.5,57.45,53.65,80.1,24.4,40.05,19.5,51.05,54.35,84.7,86.1,70.35,110.0,100.6,94.9,83.75,88.3,69.75,71.6,92.1,23.65,81.85,25.1,114.7,49.15,80.9,79.45,90.45,19.3,70.2,69.75,54.25,99.3,74.0,50.25,19.8,19.65,43.65,35.5,80.75,39.5,97.1,19.55,80.0,84.7,89.55,90.6,20.05,112.4,50.2,62.25,55.7,90.05,19.65,89.25,99.05,54.0,69.75,49.05,56.75,98.05,21.1,96.65,24.5,114.5,79.2,69.55,20.05,98.85,25.75,80.95,19.6,74.3,89.7,87.65,100.45,74.75,107.45,75.35,64.95,100.45,68.5,80.55,81.25,90.4,89.55,55.7,24.8,20.0,56.15,105.2,19.55,79.75,97.45,24.25,24.6,50.15,39.6,94.4,89.85,78.95,98.85,53.85,24.25,89.45,105.25,59.5,70.55,82.5,44.85,61.6,49.05,105.65,74.65,66.25,19.4,86.05,19.15,64.7,104.05,19.25,81.95,114.65,20.0,19.8,65.15,19.65,88.95,20.2,75.2,56.8,35.55,75.5,35.6,60.25,95.15,96.65,40.35,18.85,54.85,64.3,24.65,76.1,18.7,97.95,94.1,80.4,95.1,31.35,72.35,89.75,82.7,19.9,53.8,51.55,19.65,44.05,114.0,94.4,100.4,19.85,54.25,80.0,109.9,79.2,101.35,94.3,49.8,60.05,53.75,93.45,87.9,60.15,61.05,104.05,99.25,85.7,104.85,69.15,90.45,74.45,50.45,60.0,85.25,19.45,20.75,78.9,104.5,49.4,94.25,25.0,25.55,74.9,70.15,69.4,80.25,93.15,69.0,66.35,69.55,20.2,86.0,80.3,20.4,23.75,90.55,70.45,65.75,24.6,69.25,75.9,45.85,49.95,24.65,90.4,100.85,75.35,87.2,64.4,78.3,24.7,105.85,98.3,76.95,19.45,96.15,58.7,20.15,64.5,28.5,45.3,19.4,90.45,105.15,83.15,90.15,45.05,103.2,75.8,19.45,79.3,88.8,30.9,85.9,34.2,20.15,95.25,50.3,80.15,51.25,89.6,95.2,94.8,80.25,76.1,110.15,115.55,24.65,53.6,19.45,88.2,101.15,56.8,99.4,20.1,60.7,20.95,114.85,19.25,62.8,105.5,19.85,89.5,74.1,107.5,19.55,68.8,84.45,75.0,84.5,111.2,44.75,80.6,80.7,75.6,57.6,44.05,110.6,58.2,81.0,19.7,85.6,59.55,115.55,75.55,86.6,85.2,97.65,45.1,70.95,109.55,89.55,20.9,19.95,24.6,66.7,19.45,94.8,65.85,19.95,24.65,20.35,69.25,51.25,99.5,54.25,19.4,56.25,25.15,23.95,35.4,25.2,45.0,75.35,20.4,20.15,105.0,56.05,54.7,20.0,73.05,20.5,100.75,87.25,19.95,79.95,49.65,65.65,20.45,60.95,20.35,88.35,19.5,75.2,111.45,70.15,94.75,95.05,78.45,70.2,92.0,85.5,41.05,85.6,82.15,84.4,60.9,20.25,79.2,95.3,19.85,84.35,19.85,70.0,82.3,66.8,44.6,98.45,70.7,24.95,49.95,69.25,102.5,86.55,24.3,58.35,94.25,68.75,85.8,20.1,20.35,110.8,73.0,100.05,82.85,84.35,19.55,19.95,99.8,35.0,66.25,23.3,76.0,25.3,44.55,104.1,92.55,93.85,101.45,84.3,94.55,95.5,100.3,55.5,49.85,89.55,19.15,99.8,84.4,113.05,101.1,19.95,74.15,92.0,73.85,50.45,24.45,24.8,64.85,20.75,68.95,99.95,109.4,91.4,49.0,50.25,75.55,19.9,97.8,100.3,55.8,111.15,98.55,50.05,80.8,20.85,19.5,19.35,69.5,48.8,94.5,20.65,106.05,100.0,108.3,20.55,99.65,85.3,95.9,20.0,70.4,64.95,74.6,49.2,73.75,92.3,98.8,19.2,88.65,74.4,98.75,95.95,105.4,20.25,106.0,104.7,49.05,35.55,65.1,96.85,69.75,99.2,96.7,55.05,106.8,51.25,57.75,70.85,19.55,88.2,79.5,19.75,98.15,20.25,79.15,75.65,94.25,40.2,19.95,55.35,102.15,71.1,74.7,54.1,19.65,88.45,76.65,80.4,19.25,84.8,25.8,19.5,68.6,92.6,100.55,20.55,42.6,19.6,67.45,68.85,43.55,109.85,20.65,95.4,21.0,56.2,18.4,90.0,25.75,19.6,75.35,19.8,64.2,75.75,78.95,100.85,50.3,80.3,19.85,21.1,69.95,50.0,104.75,19.85,107.5,85.9,45.85,80.8,25.25,80.55,81.5,20.9,106.1,91.7,67.25,95.6,20.35,45.05,74.95,34.65,69.35,95.35,81.55,75.4,67.8,111.4,46.3,20.4,20.05,45.0,96.1,19.65,99.5,60.65,98.6,59.5,80.45,71.7,36.0,65.2,48.95,53.5,80.45,109.05,26.3,106.8,64.95,19.35,21.1,77.95,18.85,26.0,74.7,70.35,96.9,19.55,80.4,88.8,94.65,90.25,64.65,95.75,19.55,104.1,89.05,20.1,111.55,60.5,90.95,87.4,19.7,50.95,20.05,19.4,59.45,94.75,81.5,29.05,86.45,70.6,97.2,98.25,75.75,59.2,75.9,90.05,70.95,102.6,85.35,106.1,43.8,59.0,69.95,24.35,29.45,84.4,45.05,20.65,87.1,19.85,90.35,109.8,84.65,65.5,79.5,80.95,56.15,85.8,79.1,34.4,20.75,18.8,44.3,90.8,25.6,105.95,70.8,25.4,108.8,69.75,94.65,96.05,76.85,20.25,24.8,115.65,74.6,50.15,103.15,72.1,113.6,25.1,78.9,80.15,25.4,105.4,45.75,24.45,25.0,85.25,19.6,50.15,70.55,60.05,26.4,20.15,58.85,97.55,19.65,25.25,114.45,34.7,70.7,85.3,75.55,84.8,20.65,20.45,102.45,104.4,35.65,99.75,90.45,97.65,73.85,74.4,69.1,82.75,24.4,55.25,61.35,76.75,19.4,54.75,19.7,19.9,107.95,83.8,74.25,56.4,20.1,94.9,94.2,49.9,71.05,81.65,89.45,59.85,69.6,99.0,19.05,45.4,114.45,19.5,44.25,90.55,69.9,20.4,71.4,87.15,24.85,104.45,19.8,116.45,84.75,20.05,110.75,89.7,89.95,48.7,96.6,74.3,54.3,74.85,79.95,20.05,19.4,54.9,24.45,89.65,45.4,75.7,110.65,20.55,115.15,58.55,93.25,113.2,90.5,79.0,19.35,48.75,109.05,25.0,54.9,24.75,91.15,20.15,104.35,66.05,71.65,20.35,92.2,84.25,105.2,19.6,30.4,78.1,61.5,69.4,24.75,91.05,89.65,73.65,19.4,26.2,98.7,43.85,69.7,38.55,53.1,20.65,64.45,25.1,76.35,79.15,85.0,95.15,79.35,96.65,75.5,19.7,20.5,19.2,98.35,74.35,51.35,45.65,85.3,86.55,73.85,20.3,54.2,90.65,50.9,25.05,74.85,20.5,63.55,44.85,47.95,45.1,45.0,96.0,20.05,90.05,25.3,108.65,24.3,75.95,19.7,66.4,35.75,18.8,19.4,19.3,45.55,67.45,35.1,46.2,45.15,43.3,20.1,57.15,58.9,73.2,85.35,19.45,45.95,50.5,25.1,60.7,99.0,104.4,83.75,44.05,24.1,45.55,93.8,19.7,70.65,86.45,114.1,95.2,88.55,20.75,70.05,86.0,44.65,60.2,100.5,55.45,70.3,60.4,72.65,55.8,31.1,21.0,45.1,50.95,69.1,43.95,86.5,69.95,50.4,78.95,90.95,19.9,20.15,90.6,92.0,94.45,24.85,36.0,78.5,19.95,20.65,30.5,106.1,20.5,95.5,64.6,51.1,84.8,89.1,54.95,50.9,20.45,85.95,60.35,19.8,85.35,72.1,99.8,107.35,19.55,81.05,20.5,111.8,20.2,19.7,79.1,19.85,60.5,19.55,20.9,21.05,71.5,54.65,19.2,49.8,25.5,20.5,90.4,90.25,80.75,104.6,91.85,50.2,95.5,75.35,75.45,95.4,101.3,53.1,84.85,34.25,88.6,60.15,99.95,70.7,54.8,49.55,54.8,78.6,100.3,53.6,81.1,19.35,85.6,80.8,74.95,19.6,93.55,90.7,69.75,20.0,95.25,102.1,19.95,80.85,90.9,29.2,93.3,89.15,108.85,46.35,84.75,78.75,83.55,45.7,19.6,69.95,67.85,105.65,44.6,74.95,75.5,20.15,45.2,95.25,89.85,100.45,47.15,80.2,87.1,79.25,75.9,85.7,98.75,20.1,61.8,49.9,86.45,20.4,45.3,104.1,75.4,108.15,86.25,81.0,95.7,116.85,105.75,20.15,19.6,90.6,60.95,25.05,88.15,20.2,60.3,63.95,74.3,70.6,90.8,79.35,90.55,19.45,64.45,69.65,19.5,110.5,24.7,77.4,96.8,85.4,47.6,19.4,103.85,83.35,49.4,108.45,81.0,79.2,86.65,92.95,90.35,48.7,25.15,76.4,19.55,85.35,24.8,103.15,100.75,95.6,59.75,94.1,19.35,19.9,108.15,101.05,59.1,71.35,55.85,106.05,84.1,75.3,24.7,20.15,69.75,93.2,80.85,33.65,55.8,39.7,29.5,20.15,79.55,24.8,19.65,79.95,19.3,94.05,90.75,78.85,99.5,99.2,80.55,70.2,85.2,75.25,59.45,93.35,44.95,26.1,20.2,21.25,59.4,95.0,61.9,118.65,54.35,64.45,80.15,20.2,21.0,20.45,75.85,80.45,24.95,75.5,44.45,42.35,74.55,75.3,94.8,48.15,19.65,70.55,20.15,106.6,91.0,25.4,69.95,66.85,86.15,20.15,64.85,74.85,50.5,72.9,115.05,19.0,19.55,101.1,84.1,24.15,50.1,74.6,19.75,85.0,80.55,106.8,84.5,25.05,83.7,75.8,96.6,98.5,101.1,20.2,94.05,95.25,74.4,81.0,60.25,60.85,43.95,86.05,20.25,85.15,19.4,102.65,19.9,19.55,95.5,84.15,103.2,50.2,88.55,54.75,19.95,116.25,31.2,24.45,84.2,91.3,85.65,21.2,79.5,25.55,20.2,63.85,61.95,25.75,58.2,85.85,70.1,104.9,111.3,99.85,95.25,86.25,100.8,19.55,104.0,104.4,19.5,25.25,86.3,49.85,108.95,89.9,82.0,89.95,79.35,64.05,101.15,89.95,76.45,39.1,34.6,19.55,104.45,70.5,20.35,70.0,19.45,69.9,59.7,78.35,71.45,45.85,95.85,35.7,89.55,24.95,24.85,100.8,64.4,105.35,102.45,19.65,54.45,70.5,20.1,69.35,19.8,74.4,93.05,51.2,65.6,80.55,52.7,20.85,80.1,52.15,80.2,98.15,114.95,112.95,104.45,113.65,20.6,70.9,86.85,91.55,49.85,19.8,99.85,74.5,104.15,109.15,48.2,25.1,100.15,65.2,99.5,71.55,55.9,93.9,64.4,108.4,85.3,107.45,48.75,85.65,91.3,85.95,106.7,25.15,45.2,110.35,79.2,55.5,103.25,90.25,91.25,47.8,100.9,97.7,69.85,65.6,104.65,90.45,63.7,104.5,20.1,104.3,93.25,73.45,20.7,25.25,100.5,90.6,89.4,95.45,20.45,98.6,83.05,19.95,109.15,85.7,102.05,94.7,64.4,26.8,66.05,65.2,85.05,55.8,70.4,104.75,19.95,94.25,45.0,114.9,106.4,46.1,39.7,20.05,95.75,24.4,33.6,90.45,84.0,67.4,19.7,80.35,19.6,54.2,45.2,75.1,19.7,72.75,20.05,45.95,39.2,44.75,82.65,93.9,70.15,85.55,117.15,99.25,112.55,25.7,90.3,49.4,19.4,109.7,61.25,55.3,70.3,106.35,103.75,19.5,39.5,26.05,91.05,29.65,50.2,105.3,55.45,85.45,19.8,59.25,90.7,103.7,79.05,90.7,95.0,88.35,30.25,49.85,93.0,54.55,19.7,84.8,94.45,94.2,96.25,70.7,20.85,60.0,80.45,84.95,33.55,49.65,20.2,94.55,100.5,35.75,86.45,53.8,38.55,39.9,70.05,20.1,112.95,20.3,35.65,35.9,99.25,82.95,55.65,24.45,25.2,50.8,19.65,59.8,73.55,61.4,103.35,19.9,19.45,81.5,84.8,109.55,99.95,74.4,90.0,74.9,104.85,59.65,110.45,106.1,74.2,74.45,24.55,89.35,24.55,90.65,105.05,20.45,19.55,19.7,70.45,85.65,77.15,35.25,20.55,97.95,48.55,20.0,25.25,98.4,70.9,19.85,106.35,99.5,84.7,86.05,44.55,75.85,93.85,25.0,45.0,100.7,20.5,80.45,90.45,60.45,55.25,78.45,100.55,20.35,54.45,90.75,75.35,20.25,20.05,19.6,53.8,70.2,75.5,20.35,26.05,20.6,75.7,20.1,24.3,24.5,110.5,25.25,74.25,90.1,68.75,19.2,89.7,115.1,96.4,69.5,99.65,91.45,84.75,85.25,78.75,20.25,19.9,97.75,19.4,83.3,80.1,62.7,100.4,24.45,101.1,50.9,107.2,92.2,25.3,113.4,40.55,26.0,111.95,53.8,72.1,98.15,78.85,70.75,76.15,39.1,69.95,20.05,20.05,19.45,26.9,19.2,50.0,60.0,84.55,45.45,20.05,115.55,93.7,99.0,50.55,105.95,82.0,25.0,91.55,95.75,19.35,24.85,94.05,100.4,25.0,54.75,95.65,19.25,108.25,94.6,98.9,20.15,101.3,20.0,105.3,69.85,65.25,19.8,19.6,20.05,49.4,76.05,88.4,100.6,19.45,20.3,107.65,80.45,58.85,109.6,75.15,73.0,70.1,98.65,111.45,114.9,100.55,20.4,104.35,69.75,34.5,105.55,30.1,70.3,80.45,80.2,94.35,91.35,44.6,19.6,19.9,110.45,68.35,79.1,51.0,80.55,66.7,86.4,50.05,25.7,83.4,70.7,84.65,99.25,64.75,100.15,84.8,25.25,113.0,40.65,105.0,54.45,94.95,59.9,85.3,83.35,33.5,19.8,81.8,20.0,59.6,25.0,84.35,90.35,55.55,75.35,90.75,89.6,59.3,66.1,18.8,86.45,52.1,47.4,49.25,109.15,94.95,93.55,79.5,115.05,19.75,95.15,95.15,105.4,20.1,101.35,20.05,20.7,20.35,70.05,19.7,74.65,85.45,40.4,50.4,79.65,105.2,100.65,79.85,91.0,78.75,116.75,80.45,59.1,49.8,19.3,19.65,81.4,38.9,87.95,19.85,96.35,24.15,19.1,44.0,50.1,60.6,25.65,76.4,98.7,100.8,53.95,20.4,90.1,29.35,20.45,95.1,25.25,44.9,92.65,43.7,72.6,51.55,79.25,18.95,20.5,19.95,24.5,20.6,94.85,61.05,85.7,106.65,108.25,20.4,55.3,20.25,72.95,89.45,104.65,75.2,101.15,44.4,89.5,68.75,111.05,99.0,86.05,21.0,19.4,44.55,77.2,19.45,24.85,35.4,95.65,41.35,19.6,20.95,84.45,20.25,19.65,20.65,34.7,99.3,81.05,67.6,70.15,115.0,84.8,19.7,19.75,92.55,63.15,74.0,29.1,50.05,60.05,74.3,20.0,74.65,85.35,74.3,44.4,85.4,94.1,98.1,108.9,56.2,26.1,85.45,88.95,109.65,74.35,48.85,80.1,56.05,74.55,89.8,100.95,94.9,19.1,20.35,106.05,104.9,19.65,24.1,59.85,86.1,19.45,97.1,36.65,103.9,19.75,104.05,24.55,48.7,88.35,109.55,20.65,94.65,55.2,24.05,74.4,79.9,20.45,19.25,26.35,43.8,50.15,20.45,69.7,61.4,98.1,70.75,61.15,20.25,63.85,98.7,20.5,20.0,19.3,84.4,25.1,48.25,19.85,99.6,94.2,62.15,79.3,56.25,20.3,99.0,90.6,85.9,79.2,70.35,19.35,50.15,63.8,20.55,88.55,101.4,81.95,69.35,44.6,63.75,109.25,84.6,20.45,85.75,91.1,107.95,86.1,22.95,94.7,19.45,85.1,19.7,99.15,87.0,102.95,79.95,64.0,64.9,25.75,90.15,116.1,104.95,45.05,71.0,50.0,70.55,79.7,20.45,59.0,60.35,19.85,19.95,26.45,63.4,53.95,69.25,95.1,74.1,35.5,70.95,79.2,48.8,89.0,99.4,55.45,25.4,73.5,93.5,63.9,64.85,63.8,44.45,19.95,43.35,49.65,85.1,95.5,92.35,89.8,74.55,103.05,116.0,69.9,95.1,40.25,25.75,105.35,113.6,24.0,19.4,86.1,102.65,92.85,97.75,83.8,54.45,97.95,19.95,24.6,50.95,75.6,80.75,90.4,99.8,60.25,20.2,64.15,20.25,105.85,75.45,93.85,99.0,80.3,19.55,100.75,100.75,53.75,31.0,25.6,58.35,80.0,46.35,113.75,90.4,109.3,70.25,90.3,65.25,100.15,94.5,60.65,24.1,19.5,85.95,53.5,25.45,20.5,20.85,89.9,26.0,113.2,69.05,20.1,109.65,19.2,33.9,90.0,34.0,20.4,38.6,25.25,60.6,89.95,74.75,20.6,84.45,20.4,81.7,79.5,89.15,20.3,74.95,74.4,20.0,25.0,80.45,19.75,65.65,71.0,89.2,86.75,55.3,61.5,25.1,55.15,34.05,19.95,19.95,89.7,20.4,26.3,84.95,20.7,43.25,48.35,79.55,71.05,19.45,110.8,84.5,69.3,49.35,20.35,105.6,64.45,108.6,49.9,30.3,30.4,45.4,65.65,103.3,84.15,44.45,19.75,85.4,89.9,55.05,104.1,106.6,75.2,70.5,19.6,55.85,24.05,38.1,106.4,34.25,100.05,68.65,45.8,75.75,84.4,96.4,20.55,50.95,90.5,79.4,58.75,59.45,105.7,56.25,53.3,85.55,68.65,24.3,77.85,59.9,23.95,20.15,105.35,95.65,87.05,81.0,82.45,53.5,20.5,25.1,54.4,58.6,84.8,61.4,20.4,79.65,20.15,94.45,79.8,54.2,19.45,74.05,49.15,19.4,113.65,106.0,25.95,19.1,103.4,100.55,95.4,75.15,84.45,89.15,107.9,19.5,85.95,24.95,59.4,19.5,69.95,82.85,19.0,38.85,30.6,20.35,95.0,74.4,78.45,74.3,51.05,19.2,99.55,70.0,109.1,45.3,29.85,76.45,95.1,19.8,72.8,18.95,76.65,99.15,101.75,75.45,64.1,25.65,75.1,95.85,54.4,72.75,19.85,19.05,44.95,49.55,94.85,46.25,19.35,69.6,90.7,101.4,20.25,48.8,74.35,19.35,68.75,100.2,20.85,95.9,19.35,45.0,81.5,25.5,48.9,84.1,19.6,20.0,81.3,95.2,36.45,83.3,25.05,20.3,89.85,49.85,19.8,54.65,29.35,19.15,19.1,55.55,80.55,20.25,69.5,106.0,25.5,104.3,79.6,55.25,88.05,20.4,117.6,20.0,19.65,70.55,93.85,65.8,20.05,80.0,35.4,79.6,80.25,50.45,20.45,79.6,24.7,77.3,29.75,44.9,29.8,74.65,71.95,20.75,56.3,105.25,94.2,19.55,84.45,53.65,29.9,19.7,43.7,55.3,19.85,19.65,49.45,106.55,20.1,20.45,39.7,54.5,83.8,55.15,111.6,86.65,55.55,20.55,106.75,62.1,104.5,101.8,110.6,84.9,93.2,24.4,70.55,78.45,85.0,87.45,85.8,91.1,70.75,20.1,20.05,74.8,24.8,100.85,101.35,81.7,68.25,105.1,20.4,79.15,20.0,79.4,57.2,58.6,94.8,102.5,20.35,84.9,69.2,95.45,100.95,20.85,88.5,35.0,55.15,50.95,64.0,69.1,80.2,49.3,84.35,20.05,117.2,20.1,69.6,103.45,77.95,109.95,94.75,80.0,79.65,25.2,19.9,78.45,44.8,20.3,19.2,80.05,107.35,47.85,70.8,29.5,70.75,59.1,25.55,84.45,20.25,75.55,85.65,70.15,95.3,70.25,50.3,97.8,19.85,46.3,19.35,106.3,25.0,20.3,75.35,89.4,88.0,83.15,43.8,62.05,20.1,74.15,101.35,84.05,20.9,105.9,99.5,44.15,53.9,85.45,85.05,44.1,90.2,50.85,59.2,53.45,19.95,83.2,74.65,54.9,57.5,103.9,19.65,93.8,89.25,94.15,55.6,48.7,19.25,104.9,75.45,54.85,19.9,19.4,25.05,84.45,19.3,95.1,79.85,25.55,75.5,73.75,96.05,68.4,20.65,55.15,70.6,19.95,19.0,44.1,107.6,61.55,90.7,99.25,91.7,100.7,78.45,84.3,19.55,88.95,20.45,55.6,86.8,20.95,20.05,50.7,113.65,53.4,101.9,59.5,87.8,41.9,83.0,69.85,56.3,109.55,92.15,69.5,97.0,58.35,50.6,89.5,70.4,69.8,94.3,93.8,19.55,95.95,101.05,94.8,107.75,54.6,71.3,19.5,56.3,94.7,104.15,90.55,60.8,98.8,98.15,35.35,103.15,107.75,81.4,61.45,95.7,104.8,70.95,44.95,97.65,35.65,90.55,85.25,19.5,88.8,25.1,100.05,55.7,85.2,91.15,83.85,45.9,25.1,91.4,19.7,91.5,51.3,21.1,104.75,106.15,85.75,20.3,100.75,74.15,78.55,45.3,19.85,50.7,45.0,77.8,83.45,73.25,94.8,20.1,59.9,90.1,51.05,70.95,29.2,46.6,85.35,75.35,74.3,69.3,75.2,20.9,94.3,76.45,54.0,104.25,19.95,24.95,84.75,19.75,113.65,44.9,75.25,24.6,25.0,20.95,110.6,55.5,43.3,109.5,19.45,84.85,19.6,53.45,19.8,112.1,84.8,95.05,50.35,74.6,19.7,74.2,69.0,19.35,59.45,19.8,105.2,109.2,79.15,53.65,100.2,45.15,108.65,40.65,55.35,105.6,93.8,95.7,83.2,90.05,97.65,68.05,96.2,79.6,102.1,23.4,71.05,85.25,19.45,59.45,92.2,19.85,43.9,80.5,89.8,90.5,90.45,50.75,84.6,89.65,99.15,19.95,20.5,62.1,79.5,19.55,20.35,51.7,23.3,65.4,65.1,81.2,72.9,74.5,80.5,60.3,75.0,90.15,40.0,99.45,69.05,59.7,19.85,86.25,45.65,70.1,40.75,70.2,55.35,95.7,46.3,81.3,84.2,20.0,66.15,45.85,19.6,49.8,101.75,55.15,75.25,103.95,100.15,99.65,73.7,50.05,60.25,105.75,87.3,48.35,54.25,85.3,50.0,24.4,90.95,72.25,96.1,19.85,55.3,20.1,69.5,25.15,20.95,49.55,79.65,71.25,113.8,24.55,19.7,20.25,50.15,100.5,95.9,74.45,104.1,19.05,25.0,19.05,81.9,69.7,90.15,25.35,24.65,19.55,25.25,60.0,89.9,19.4,49.8,24.1,54.25,109.9,35.5,87.55,45.15,88.4,50.8,99.0,84.4,96.55,59.75,111.5,24.25,75.1,70.15,101.75,45.8,20.5,70.4,30.55,84.9,20.1,40.65,101.0,69.1,54.5,75.35,44.45,75.0,100.0,98.05,71.15,54.15,63.9,69.15,64.65,108.75,98.85,49.15,89.6,83.25,70.25,19.4,24.5,79.15,20.1,73.0,61.4,84.3,19.9,20.4,50.75,20.45,75.75,65.4,80.4,59.75,78.5,102.0,48.95,99.65,18.25,54.55,20.65,40.65,20.45,24.8,70.8,89.05,96.6,88.8,20.05,104.5,69.8,77.15,35.05,108.1,84.05,20.2,50.6,49.2,24.6,71.65,104.9,106.5,49.35,75.5,94.25,68.95,58.5,78.9,93.85,79.2,109.45,59.2,29.15,20.05,76.05,24.45,66.5,49.55,89.35,73.6,82.65,49.0,80.35,25.2,25.45,55.8,110.9,77.75,26.2,19.9,79.05,95.0,25.2,80.85,98.4,56.35,19.3,50.4,79.4,55.25,19.1,84.05,105.2,101.4,89.8,75.75,95.3,109.75,19.85,19.3,69.1,91.25,20.25,54.75,81.45,49.1,80.2,100.3,65.25,90.95,85.45,20.0,94.1,79.85,71.65,73.55,104.65,19.3,20.15,44.55,54.45,19.65,105.0,88.7,74.25,75.15,20.25,109.1,30.75,112.9,74.2,94.05,78.85,55.3,19.35,25.35,20.45,19.35,101.0,100.2,89.05,78.65,74.75,70.1,19.9,58.35,105.65,100.5,20.05,25.65,96.5,95.0,70.85,85.95,73.9,45.45,20.0,49.2,109.45,83.25,19.25,19.65,72.8,109.65,65.0,114.1,20.65,86.95,94.75,25.35,105.45,25.4,102.55,100.2,24.0,25.6,73.5,74.05,98.25,54.4,101.55,103.1,34.2,43.75,111.95,100.65,55.95,116.05,45.75,82.0,65.15,44.8,79.8,88.85,74.95,106.85,74.95,80.15,19.3,109.25,56.1,19.7,51.3,118.6,24.15,20.3,115.5,25.05,109.1,19.65,111.3,29.9,80.6,20.8,35.2,78.8,89.95,116.05,19.55,106.4,49.4,115.25,24.8,19.9,81.25,69.95,69.1,90.2,93.55,86.4,66.3,94.65,80.85,82.05,72.1,34.7,20.55,95.95,44.8,109.4,71.05,78.55,19.7,40.25,19.85,68.25,20.15,50.95,78.65,25.15,20.25,42.9,44.0,20.25,34.25,58.5,55.8,88.9,57.65,96.2,79.15,108.05,74.4,94.8,45.9,105.3,102.6,73.85,61.35,57.55,29.25,84.55,19.6,111.75,106.5,107.7,19.3,20.05,69.95,63.7,24.75,50.9,60.4,79.25,85.8,24.45,110.1,90.7,25.3,105.7,85.2,24.35,24.25,25.1,54.55,96.6,76.5,81.15,38.5,92.9,93.5,84.7,66.0,101.5,74.9,20.75,61.45,54.5,69.6,99.75,109.75,80.85,20.3,67.8,24.05,19.8,25.7,56.15,86.7,20.4,19.65,50.55,54.35,108.1,54.45,45.35,59.0,69.45,100.55,64.95,20.5,18.85,19.8,25.05,74.8,114.3,24.45,109.2,45.05,51.0,110.45,84.65,60.05,44.65,93.25,20.25,25.45,20.6,94.1,34.8,60.75,51.35,64.05,84.8,71.0,50.15,94.6,59.75,100.25,98.9,97.7,40.3,60.25,56.25,46.2,50.6,24.9,84.85,65.7,63.35,50.1,70.5,94.85,50.15,19.75,64.65,79.6,19.5,99.55,74.0,38.9,79.55,65.45,98.7,46.3,99.35,95.8,67.5,78.15,26.1,69.6,84.35,100.2,78.05,40.35,79.2,20.9,73.6,74.75,49.9,68.9,20.25,76.0,74.0,82.3,89.4,99.15,20.2,29.45,19.8,59.15,44.75,90.8,49.55,106.7,93.55,94.45,19.45,25.05,67.95,65.25,99.45,20.35,19.95,77.4,19.7,99.7,74.8,19.15,78.95,95.55,62.85,71.55,94.95,86.1,19.55,24.8,39.3,84.05,36.25,20.25,23.9,98.6,103.65,92.9,19.9,20.1,85.45,80.5,99.9,39.85,60.5,84.8,103.85,67.8,75.2,24.85,19.35,49.35,89.0,55.0,76.15,20.3,74.9,117.35,19.75,45.2,25.2,89.75,75.0,49.95,65.7,67.05,110.9,87.95,19.8,75.7,62.15,101.25,115.15,18.95,19.5,86.55,28.6,20.4,19.8,45.65,56.4,73.3,24.35,101.35,98.65,33.6,79.9,20.7,104.05,20.25,103.3,73.7,96.2,108.75,20.15,19.75,25.95,70.05,24.05,84.75,23.05,104.15,59.95,19.55,19.6,20.05,85.55,78.6,116.8,43.55,60.8,54.9,65.2,102.95,90.6,50.8,90.05,108.2,92.0,75.1,25.05,75.15,19.5,19.3,112.2,70.3,19.6,20.25,75.85,80.65,68.5,115.75,73.5,80.6,69.95,59.55,19.05,95.65,19.95,70.05,19.4,36.1,94.0,61.15,19.75,64.1,19.75,19.7,110.2,106.35,90.55,65.9,104.5,52.5,56.1,88.75,84.45,75.3,26.0,99.4,109.55,19.6,73.15,54.65,66.4,115.55,104.45,100.05,102.0,91.15,89.7,90.2,92.4,19.9,25.15,79.85,18.85,25.75,49.6,20.95,97.05,25.4,19.7,35.0,101.25,70.2,90.95,73.85,88.05,105.95,91.85,20.1,40.1,110.3,73.9,89.8,85.15,60.95,72.25,73.55,46.0,58.55,24.6,19.75,86.35,25.5,19.0,19.55,110.1,96.55,69.75,50.6,65.6,40.1,82.1,79.1,101.25,79.55,90.65,20.55,75.75,110.0,20.85,80.35,70.15,84.05,67.45,20.75,89.1,69.9,51.1,94.4,78.25,25.55,60.0,90.55,76.4,84.95,110.1,99.65,45.4,69.0,48.65,44.15,59.85,75.75,80.65,20.55,66.4,100.2,19.1,80.3,44.55,20.35,91.8,74.9,20.2,50.35,18.8,20.45,64.75,98.7,89.45,58.75,20.7,85.6,80.3,79.8,79.85,54.1,80.85,24.75,80.9,24.5,20.15,20.05,19.6,114.3,100.3,80.0,20.85,89.95,20.0,90.85,48.75,80.0,79.7,20.35,57.55,20.25,19.4,100.4,57.95,59.5,19.2,86.5,59.55,103.95,25.1,103.95,68.95,103.1,24.7,110.2,48.95,62.45,89.55,83.55,78.9,20.35,71.45,46.35,94.65,49.9,25.45,89.15,20.75,66.1,75.4,70.45,60.3,21.05,69.35,88.85,97.0,66.4,24.75,69.2,79.5,100.65,103.3,79.7,61.4,69.8,40.55,75.65,90.7,80.5,60.6,101.15,24.95,20.3,60.0,20.25,78.5,44.75,19.85,98.0,79.9,107.7,99.7,104.7,58.6,93.9,86.45,98.5,19.4,50.45,24.95,75.0,94.65,100.25,78.2,94.2,88.45,69.85,81.7,50.05,79.9,69.55,25.4,90.1,44.65,83.75,80.35,98.1,53.35,19.55,20.9,48.95,54.2,24.45,69.4,40.15,74.9,25.6,70.35,91.7,89.2,24.1,74.15,53.85,115.6,19.75,24.05,25.3,84.3,70.1,89.75,97.95,20.0,78.3,103.9,20.7,96.8,94.4,20.15,26.0,77.35,66.05,19.9,84.3,68.15,80.85,75.5,92.45,80.6,83.2,87.55,99.55,81.25,109.4,19.95,45.55,20.7,75.3,99.25,93.4,73.75,80.45,88.15,49.2,19.65,79.35,79.75,105.15,49.0,100.05,69.35,49.8,85.8,79.7,20.95,50.55,79.3,19.5,80.55,44.15,84.5,105.5,84.3,92.7,26.25,96.95,20.45,115.8,108.2,20.2,67.75,54.9,85.25,20.15,90.35,55.75,114.6,80.05,20.0,66.8,100.3,105.35,85.2,48.8,18.95,69.8,106.15,20.55,105.75,25.25,19.75,104.85,60.95,81.15,19.1,20.8,90.15,90.1,74.1,85.05,118.75,85.9,95.0,20.15,101.3,21.2,24.2,20.3,102.8,85.3,89.6,99.95,56.25,50.95,115.85,103.65,26.1,35.1,99.1,67.25,25.0,59.55,77.8,55.1,117.8,24.15,45.25,79.5,20.25,64.75,54.6,20.7,94.75,79.65,115.8,49.45,83.8,95.35,94.7,74.05,89.6,116.6,54.2,19.3,65.05,92.5,19.45,24.05,18.75,20.15,20.0,71.0,75.55,93.6,70.0,24.4,74.8,65.25,50.55,104.4,70.7,45.25,70.3,108.95,26.45,86.2,19.65,51.2,19.05,74.75,75.8,25.1,44.45,104.3,19.5,89.0,20.15,74.9,74.9,36.15,19.2,19.25,61.2,20.45,35.05,100.25,44.0,102.8,50.35,100.0,20.0,99.85,94.2,86.4,58.4,83.85,88.3,94.1,104.05,108.9,107.4,94.7,90.85,19.9,66.4,100.65,100.7,25.6,19.85,20.75,95.8,94.65,80.55,106.65,45.85,104.35,55.45,78.85,61.15,78.95,44.45,109.2,61.3,96.85,40.55,19.8,108.25,105.05,90.45,86.4,66.9,110.7,20.0,84.9,102.1,20.25,70.15,74.35,80.05,62.05,49.2,20.5,38.25,54.95,96.6,19.9,19.9,84.6,80.0,85.25,81.25,115.5,104.1,79.0,39.1,94.65,20.8,59.5,20.05,100.45,76.5,20.6,20.3,49.2,39.55,23.15,20.45,80.85,25.25,91.25,72.45,60.1,19.7,78.95,75.1,25.0,69.15,91.55,45.15,35.8,113.15,19.85,19.8,19.9,19.7,79.4,59.1,53.95,91.15,99.3,68.95,51.55,24.4,96.8,70.05,19.5,78.75,69.2,19.55,80.65,103.65,54.7,54.15,71.1,84.85,20.0,106.25,99.25,19.35,20.8,94.75,114.05,74.9,19.8,94.0,80.85,54.65,91.7,118.6,24.55,19.45,116.15,80.6,20.3,89.85,46.0,66.25,99.8,90.0,70.45,75.0,19.9,80.3,19.75,84.3,54.05,104.9,53.95,97.25,83.05,105.5,81.0,41.1,45.0,74.55,40.2,70.5,19.75,24.65,104.25,78.35,69.8,109.7,73.75,33.45,94.6,54.55,20.2,20.3,39.4,69.15,76.25,93.9,51.35,100.05,70.4,20.3,94.45,46.4,104.05,91.15,24.9,59.6,108.5,40.55,58.95,70.95,20.75,113.15,48.8,63.05,100.85,99.5,80.55,64.4,75.2,84.9,19.3,83.9,117.45,104.4,74.65,59.05,69.1,20.55,76.55,62.5,29.4,94.9,111.65,19.9,20.45,106.05,113.45,92.55,65.6,84.35,44.65,71.1,85.15,49.7,30.2,25.25,84.05,85.7,74.7,56.35,90.8,107.55,19.85,95.9,23.85,106.15,83.85,85.35,84.8,90.85,76.1,74.55,39.2,79.55,19.6,19.55,39.15,20.1,99.95,59.8,49.75,35.75,108.5,60.15,19.05,46.0,84.0,44.55,103.45,80.65,57.2,110.75,24.7,97.05,76.35,89.4,18.9,74.45,19.8,50.9,84.4,24.4,20.05,81.0,98.35,55.5,51.0,91.65,84.3,100.2,19.4,90.85,69.4,94.45,20.4,94.75,20.15,95.7,44.35,74.55,73.6,74.95,47.95,50.1,63.6,53.0,19.85,24.35,19.55,25.05,93.8,36.85,103.75,56.75,20.8,44.1,24.45,25.6,50.75,104.4,39.3,59.65,83.3,79.55,24.45,19.2,29.8,45.5,106.45,30.05,65.65,96.05,75.1,74.05,44.7,110.75,19.7,49.5,55.0,43.95,74.35,111.15,104.7,55.7,20.6,19.65,115.8,88.65,94.5,20.1,34.65,52.3,65.0,19.85,35.45,19.7,95.6,19.85,81.85,109.3,70.3,25.4,69.8,20.0,85.55,109.9,50.3,94.5,101.5,89.15,19.4,29.9,78.8,85.35,79.65,19.3,79.6,96.8,20.65,19.8,90.6,104.6,80.05,45.15,73.15,99.1,20.2,106.05,105.35,45.65,79.95,54.45,25.1,84.7,75.85,48.8,99.15,35.2,76.25,55.9,82.35,40.4,24.9,54.3,66.3,20.9,75.35,85.15,75.35,104.45,49.45,19.45,92.15,93.8,19.85,100.25,95.7,93.15,69.7,19.8,71.35,20.75,40.6,20.4,20.35,19.75,54.4,94.7,30.5,20.45,66.15,89.85,45.05,86.85,96.75,77.0,20.1,75.3,106.65,110.15,82.85,20.1,99.2,59.45,58.6,49.7,65.85,73.5,85.5,20.05,113.65,83.4,65.65,70.4,61.35,85.9,75.65,49.75,70.9,49.85,75.3,20.1,94.0,103.05,118.35,99.7,81.9,30.45,96.1,66.2,104.25,80.2,19.75,72.6,116.5,106.8,24.95,89.25,19.25,104.55,87.2,30.75,25.7,86.2,30.1,99.35,19.2,20.1,20.35,25.65,94.55,104.2,94.4,56.1,68.25,24.75,76.25,74.35,54.15,19.45,34.95,53.65,69.65,104.0,70.35,80.8,64.85,19.65,45.9,20.0,44.8,80.3,20.35,45.8,84.1,108.95,69.35,64.35,90.8,24.95,79.6,84.7,70.8,36.45,104.4,101.5,54.3,103.95,91.1,19.95,26.45,89.4,75.1,108.1,110.15,80.35,111.5,106.5,19.9,111.1,70.7,24.85,91.2,65.6,40.65,59.45,109.95,60.45,84.9,38.5,92.55,73.55,20.15,34.7,24.5,19.7,20.6,58.0,107.45,65.5,25.45,100.15,104.45,21.15,96.2,44.4,107.55,94.35,98.75,20.3,101.15,105.75,81.15,89.55,54.75,53.75,105.75,105.85,64.2,88.7,87.7,89.3,20.15,79.75,94.55,20.05,67.2,94.55,69.05,107.5,73.0,114.75,76.05,96.25,101.1,104.7,77.9,90.65,110.45,68.7,44.85,29.8,88.9,58.75,19.85,86.9,59.65,55.25,66.4,90.1,20.15,108.1,53.75,56.9,89.3,109.6,25.15,79.15,66.75,95.2,48.8,45.7,80.7,74.5,20.55,79.65,115.1,59.7,86.45,33.7,80.1,104.05,108.75,41.1,20.35,105.9,101.3,80.05,89.2,65.5,40.45,70.45,78.8,83.65,90.1,82.45,20.25,66.25,19.5,51.25,89.7,64.55,45.6,93.65,49.65,73.6,109.75,61.45,106.4,81.9,105.2,54.6,20.55,20.0,19.7,66.05,34.0,92.5,54.05,58.9,88.35,107.95,96.9,19.1,50.0,45.4,85.45,84.1,74.45,64.75,66.25,76.9,89.8,74.6,116.95,40.65,114.35,69.7,95.5,98.65,61.65,89.35,95.4,35.4,19.95,19.25,29.65,84.5,20.4,24.75,25.35,90.7,20.0,59.75,82.5,70.3,20.35,90.8,103.95,104.95,105.25,74.75,50.8,23.75,61.3,75.8,98.0,80.25,78.9,52.0,84.75,64.4,85.45,45.8,30.5,19.9,69.15,99.45,49.25,39.35,70.6,105.1,81.0,20.1,84.85,19.75,19.75,70.4,20.45,20.35,86.2,95.65,103.8,97.2,63.55,24.95,89.15,99.0,24.8,85.55,94.0,105.65,50.3,95.0,61.4,80.55,78.5,114.3,20.05,62.65,80.85,92.7,100.45,75.2,84.75,89.45,79.5,72.15,19.8,76.4,100.9,95.3,90.95,54.5,61.6,79.9,96.15,49.6,65.3,25.0,45.45,107.75,89.1,19.65,44.75,101.6,103.15,84.65,95.65,75.1,61.35,69.55,19.7,31.05,51.0,51.0,88.85,20.05,65.1,70.15,44.35,20.75,56.05,19.95,98.6,79.7,79.0,89.45,74.2,81.0,49.6,84.6,55.0,84.85,84.2,106.3,69.05,45.4,73.65,73.9,77.75,99.35,50.75,87.1,20.15,98.7,25.2,55.7,65.35,25.3,84.35,84.95,73.85,24.25,51.8,46.0,79.4,60.5,25.1,71.8,20.05,88.4,30.25,20.2,59.9,25.15,46.0,101.3,76.95,55.3,92.45,48.45,19.35,51.75,86.7,94.4,55.7,84.25,64.65,70.15,69.2,54.65,24.75,23.95,105.0,59.85,20.05,92.15,44.8,20.9,95.4,80.35,85.1,34.7,115.05,81.1,19.95,20.55,106.6,86.15,78.85,106.75,86.55,42.4,89.45,24.25,97.9,20.5,19.6,20.25,55.7,20.6,19.8,79.8,80.2,116.4,31.65,94.15,20.65,76.85,20.15,55.25,39.05,82.15,103.0,95.1,83.9,95.15,79.8,74.8,69.85,20.45,78.35,53.55,19.1,20.0,93.9,19.95,113.15,24.0,84.95,80.5,19.3,19.15,91.3,49.65,54.35,19.15,88.45,19.75,75.5,83.75,19.4,26.5,90.5,19.15,94.85,69.95,40.9,80.25,48.6,70.8,60.2,55.2,55.8,54.15,80.15,75.5,100.4,62.55,70.45,85.5,20.2,54.5,20.75,20.35,91.0,104.8,74.75,104.05,51.1,89.8,20.55,64.05,74.85,96.65,20.05,103.45,25.0,20.3,26.35,19.9,54.7,46.35,90.25,19.95,20.65,79.6,25.45,19.5,75.9,76.2,66.15,19.25,69.1,39.1,20.05,59.8,84.3,48.6,79.0,105.35,25.1,49.75,94.75,93.0,71.9,77.55,19.85,70.25,95.25,84.6,25.05,53.15,20.15,101.25,100.55,24.1,25.3,71.8,19.7,49.85,69.6,19.75,80.8,60.0,86.55,20.85,64.2,35.0,50.75,105.5,19.2,85.15,90.65,20.0,74.65,61.2,19.95,54.8,73.45,51.45,80.45,54.2,109.5,104.4,85.3,79.3,76.5,105.1,25.4,86.85,19.65,75.7,45.55,78.1,19.3,110.5,90.8,20.3,81.35,97.95,108.15,55.3,56.55,80.5,19.7,104.05,52.85,104.3,80.65,71.35,24.65,21.3,110.2,89.4,51.05,19.8,19.9,87.3,19.85,89.4,20.0,20.05,83.25,20.6,102.9,39.1,99.95,114.5,20.2,55.8,24.2,81.0,72.8,99.85,99.5,70.15,20.25,26.0,19.9,19.05,96.5,19.85,25.7,20.3,70.15,91.55,39.4,105.7,70.25,93.75,96.55,60.0,59.8,90.65,109.0,68.1,20.4,81.95,60.55,65.6,82.5,82.3,68.15,20.3,95.55,20.2,89.2,69.65,89.3,74.8,20.2,84.4,87.55,25.15,19.8,50.85,102.4,96.3,55.5,109.75,106.4,60.0,88.8,85.2,35.1,80.05,75.55,49.55,81.3,23.9,66.4,19.6,18.8,108.4,85.95,85.45,80.9,71.0,111.8,20.6,85.05,44.6,44.4,105.1,115.15,59.8,26.3,70.55,20.05,79.85,70.3,79.35,90.05,24.45,59.95,25.35,90.8,70.45,34.3,105.05,19.3,19.15,51.4,71.85,75.4,49.7,45.25,78.75,81.6,70.4,75.8,76.1,94.0,103.95,19.95,71.3,110.8,69.1,96.1,48.8,50.55,44.65,88.25,19.45,89.3,70.0,19.25,70.5,97.35,19.65,20.85,19.65,19.35,44.0,94.4,25.9,55.65,69.65,75.4,100.6,71.0,86.0,106.95,21.2,61.05,29.6,79.95,19.7,20.3,59.9,24.35,19.75,50.3,95.6,50.25,85.35,41.6,51.65,24.0,100.85,59.85,25.45,23.9,24.15,75.7,40.2,84.5,50.85,91.6,98.9,85.0,78.95,44.3,20.2,80.2,60.9,34.2,85.2,87.15,54.3,19.1,112.75,19.95,19.5,65.55,78.8,78.2,105.25,89.25,20.65,68.7,78.65,24.75,19.75,89.1,84.7,98.0,94.45,105.0,93.85,59.9,19.95,84.0,108.9,33.6,85.85,34.85,48.75,84.85,56.65,95.3,73.9,24.5,84.6,44.95,24.7,100.3,25.45,50.7,55.0,68.4,89.9,78.55,55.05,19.8,84.45,35.9,80.75,78.65,61.75,63.7,99.45,25.2,74.05,87.6,89.15,20.0,55.0,104.4,20.05,89.75,34.3,20.65,84.25,19.65,79.85,20.2,19.8,50.35,85.15,74.6,79.15,20.35,21.05,94.6,94.7,94.25,72.45,74.95,105.2,111.95,19.85,89.75,20.05,108.95,19.65,24.9,82.85,93.2,84.8,71.75,30.35,54.85,19.5,103.85,24.2,19.35,83.6,100.65,94.1,74.55,108.45,56.15,20.35,80.55,61.25,20.45,18.9,19.6,91.5,45.2,19.45,25.45,80.85,94.9,49.05,29.3,105.3,88.95,20.25,110.85,110.5,109.4,114.2,36.5,70.75,19.95,19.6,40.15,76.6,19.6,85.3,65.85,94.45,20.05,99.4,20.0,78.45,25.1,97.35,55.0,71.1,61.55,45.9,40.3,87.1,49.5,73.8,19.2,45.3,25.0,94.95,35.3,44.55,76.75,81.0,105.55,18.8,24.9,23.45,64.9,61.35,113.95,90.15,54.1,29.7,49.8,101.1,24.4,95.0,50.65,69.9,39.95,55.4,90.6,103.25,86.85,94.25,47.05,20.55,19.65,70.2,81.0,75.9,24.7,99.05,110.25,85.0,19.75,23.9,111.25,55.1,19.95,25.15,54.15,59.8,83.85,104.9,75.3,66.65,109.5,73.85,19.3,118.2,51.45,59.45,19.5,19.55,93.55,59.3,102.25,95.9,109.8,78.1,39.9,64.9,95.05,53.4,24.9,44.7,114.0,20.25,53.85,83.85,20.2,19.95,104.2,50.25,20.35,90.0,54.2,99.5,99.1,66.9,25.85,91.05,71.0,93.2,20.95,109.2,19.35,85.8,19.85,19.65,20.5,89.65,74.35,49.45,89.1,75.15,70.65,104.2,90.05,79.25,44.9,19.4,88.75,70.1,91.0,29.65,90.8,77.85,54.3,18.95,95.15,102.4,99.9,88.7,54.3,55.7,103.95,110.85,20.15,20.05,91.95,80.5,55.65,74.7,104.15,83.65,72.2,110.05,51.5,25.5,89.55,19.5,80.7,77.5,105.1,25.15,95.25,95.65,85.0,80.8,24.85,54.75,85.75,50.75,20.15,20.05,98.25,71.6,81.45,58.4,25.7,53.7,19.6,89.4,69.0,84.2,106.1,25.75,46.05,64.95,85.45,20.05,76.4,100.5,20.7,25.3,40.05,100.6,69.95,74.0,99.4,93.3,49.15,107.45,83.6,99.05,80.1,65.3,89.55,60.8,74.5,99.15,19.25,39.45,44.85,97.2,110.55,35.05,73.0,19.9,76.95,35.4,20.45,96.75,54.2,100.1,45.25,83.85,70.1,20.85,33.45,20.2,85.9,61.0,70.65,86.9,69.4,20.35,20.35,104.3,44.95,49.45,20.6,19.55,99.0,93.5,54.55,20.05,83.95,79.45,116.2,93.7,79.85,100.0,19.6,19.7,20.2,50.4,113.35,80.0,80.95,24.9,54.9,75.55,109.25,77.65,95.0,116.3,19.9,70.35,25.6,44.45,100.15,105.4,95.85,73.85,70.1,25.25,79.15,21.05,24.95,64.5,19.65,79.0,105.95,75.85,91.85,43.6,91.25,89.75,104.4,90.15,40.3,105.25,106.0,104.0,69.65,74.3,100.9,20.25,49.9,96.9,100.35,104.1,20.1,74.95,56.55,49.25,68.6,69.05,19.7,20.05,103.7,94.4,54.95,93.7,110.25,98.9,89.75,80.45,79.4,20.3,62.8,74.9,74.85,25.85,101.95,68.3,48.4,94.0,105.05,89.3,25.15,19.5,92.95,20.7,74.3,19.35,44.65,84.05,80.7,104.35,19.55,74.05,40.1,20.1,101.7,83.55,56.85,20.4,19.55,106.15,78.95,49.75,92.4,58.2,102.6,91.95,65.25,106.0,73.1,59.75,55.1,59.8,116.6,109.3,101.4,50.65,56.15,106.5,19.2,83.0,70.1,108.3,91.05,25.25,45.35,43.9,77.5,79.3,84.9,79.25,71.05,53.75,24.25,54.2,44.25,50.05,20.15,69.25,69.35,19.35,19.15,61.0,20.5,50.5,50.2,79.6,24.9,74.4,106.9,101.35,55.35,50.55,19.5,79.45,90.65,89.85,79.0,104.65,19.55,19.9,116.25,87.75,100.05,81.3,44.3,70.35,44.45,49.15,29.45,100.55,85.3,95.65,69.1,70.35,20.6,74.15,75.05,44.6,21.45,43.45,20.05,94.15,94.4,19.55,75.9,64.15,109.55,110.8,55.0,53.45,69.95,101.45,97.0,90.6,73.55,67.95,94.35,69.5,18.85,19.4,69.2,19.75,54.6,29.8,69.65,101.85,103.05,82.3,20.3,35.1,105.7,56.25,60.35,79.25,59.8,84.6,93.4,94.2,25.05,99.65,50.65,60.9,59.65,64.7,25.1,48.95,54.85,45.3,91.35,85.85,25.1,34.0,45.9,95.2,20.5,100.6,55.3,20.35,74.85,36.1,65.8,20.35,105.8,96.75,102.35,24.4,115.65,79.85,73.05,64.35,20.5,76.0,54.75,104.75,74.65,51.15,41.95,54.35,56.25,106.1,96.0,79.75,61.45,68.65,19.65,19.0,100.0,20.25,98.7,19.8,73.8,100.2,74.9,20.05,106.2,116.55,99.7,19.7,19.5,29.15,55.0,90.8,51.0,90.1,59.05,20.3,72.95,73.55,84.3,78.0,72.1,106.75,19.25,20.55,20.0,24.65,103.5,23.85,25.8,70.85,69.8,59.45,54.55,20.05,82.55,81.25,70.75,74.3,94.1,29.7,109.7,96.35,66.6,44.5,110.9,105.0,25.3,55.15,20.1,80.1,69.05,69.9,20.4,19.7,50.1,101.4,83.45,86.65,20.15,80.8,19.4,62.05,76.45,60.05,91.3,95.75,20.35,94.05,84.1,78.75,55.55,62.65,74.5,102.1,20.1,70.3,53.65,20.75,103.4,50.8,50.15,79.0,74.6,96.5,20.1,19.4,77.55,20.05,19.85,20.2,67.45,18.55,29.75,86.5,24.2,23.55,20.45,81.45,92.3,69.15,53.65,39.65,54.65,104.8,29.3,83.85,79.55,103.65,99.05,73.35,100.05,20.35,43.95,23.5,70.7,94.3,29.15,20.85,37.7,95.5,91.05,92.45,44.15,36.05,50.25,109.75,79.2,20.3,112.35,94.3,41.15,74.65,48.25,76.15,71.1,96.55,79.3,89.6,20.5,106.3,100.35,85.6,45.25,106.15,51.1,19.9,25.7,74.3,99.4,69.7,98.35,85.45,95.9,100.75,89.2,74.1,100.6,75.0,25.75,84.1,79.3,107.05,20.05,70.2,19.5,70.75,45.3,115.15,72.95,19.65,19.55,89.55,50.35,50.25,87.25,20.8,109.25,20.35,55.9,79.2,96.0,79.2,24.0,101.35,100.1,56.5,35.45,85.0,79.4,35.2,19.65,49.85,68.75,61.9,79.9,89.75,59.3,19.4,93.65,49.4,19.9,55.0,72.9,69.2,25.6,19.75,55.7,117.5,19.85,78.9,20.65,62.3,92.5,19.65,79.75,79.95,29.9,19.75,45.0,44.8,69.65,51.1,53.15,24.7,111.6,48.55,109.95,20.8,20.2,25.6,39.65,24.9,108.4,19.55,85.1,56.7,69.05,70.15,111.15,105.95,89.35,89.1,91.25,90.35,105.55,19.1,20.4,100.45,74.95,29.7,50.35,85.7,47.85,94.0,69.85,70.3,25.85,71.1,98.8,93.35,99.85,80.3,50.55,80.45,81.3,20.7,79.05,19.05,19.6,20.2,86.8,20.9,103.6,38.8,88.4,84.2,79.7,99.0,100.75,19.3,55.75,19.95,91.75,89.65,45.85,79.55,55.95,69.0,83.55,65.7,94.9,61.9,111.1,20.0,67.7,25.15,92.85,89.1,111.3,101.9,91.65,88.85,60.6,25.3,65.5,95.45,19.95,91.1,54.15,74.6,94.6,81.15,89.05,49.2,19.45,104.3,69.7,89.5,86.05,25.2,35.15,99.65,105.35,35.15,73.75,101.35,24.3,80.7,89.85,61.1,29.05,99.7,55.9,105.9,46.0,43.95,80.4,100.05,45.1,94.0,68.95,68.45,69.0,43.85,44.5,18.7,70.25,55.35,53.55,114.6,20.1,85.5,108.75,103.0,97.85,19.55,84.05,103.75,89.4,19.7,79.85,74.45,74.1,69.35,18.8,73.85,64.4,55.8,20.05,75.15,99.15,56.75,69.6,104.15,110.8,80.15,35.75,69.9,89.2,55.65,50.7,20.0,30.5,19.1,98.3,45.55,101.05,103.7,36.25,49.4,19.9,107.4,82.0,19.8,45.05,64.55,86.25,19.75,89.1,95.55,75.4,101.25,102.6,56.3,94.2,43.05,89.5,74.4,20.5,74.35,99.75,111.95,94.0,98.85,64.35,72.0,49.7,80.7,24.2,39.0,65.45,74.35,83.2,25.0,40.2,94.1,108.35,69.5,76.0,93.6,95.65,100.55,88.05,24.45,89.55,66.5,76.1,80.5,35.45,20.55,49.9,105.4,35.75,95.1,19.3,104.5,63.1,75.05,81.0,74.45,60.4,84.95,93.4,89.2,85.2,49.95,20.65,70.65,20.15,19.2,59.8,104.95,103.5,84.8,95.05,44.2,73.35,64.1,44.4,20.05,60.0,75.75,69.5,102.95,78.7,60.65,21.15,84.8,103.2,29.6,74.4,105.65],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Total"},"showgrid":false,"showline":true,"showticklabels":true,"zeroline":false},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":""},"showgrid":false,"showline":false,"showticklabels":false,"zeroline":false},"legend":{"tracegroupgap":0},"margin":{"t":60},"boxmode":"group","title":{"text":"\u003cb\u003eBox Plot untuk monthlycharges\u003c\u002fb\u003e"}},                        {"responsive": true}                    ).then(function(){
                            
var gd = document.getElementById('76cdf4f7-3de5-4fee-ab0c-07dbeb5d95bf');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>
```
:::

::: {.output .display_data}
```{=html}
<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script charset="utf-8" src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>                <div id="08229dfe-9cbe-44df-a09d-c786eaed176a" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("08229dfe-9cbe-44df-a09d-c786eaed176a")) {                    Plotly.newPlot(                        "08229dfe-9cbe-44df-a09d-c786eaed176a",                        [{"alignmentgroup":"True","hovertemplate":"totalcharges=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#ff9e00"},"name":"","notched":false,"offsetgroup":"","orientation":"h","showlegend":false,"x":[29.85,1889.5,108.15,1840.75,151.65,820.5,1949.4,301.9,3046.05,3487.95,587.45,326.8,5681.1,5036.3,2686.05,7895.15,1022.95,7382.25,528.35,1862.9,39.65,202.25,20.15,3505.1,2970.3,1530.6,4749.15,30.2,6369.45,1093.1,6766.95,181.65,1874.45,20.2,45.25,7251.7,316.9,3548.3,3549.25,1105.4,475.7,4872.35,418.25,4861.45,981.45,3906.7,97.0,144.15,4217.8,4254.1,3838.75,1426.4,1752.65,633.3,4456.35,1752.55,6311.2,7076.35,894.3,7853.7,4707.1,5450.7,2962.0,957.1,857.25,244.1,3650.35,2497.2,930.9,887.35,49.05,1090.65,7099.0,1424.6,177.4,6139.5,2688.85,482.25,2111.3,1216.6,79.35,565.35,496.9,4327.5,973.35,918.75,2215.45,1057.0,927.1,1009.25,2570.2,74.7,5714.25,7107.0,7459.05,927.35,4748.7,113.85,1107.2,2514.5,20.2,19.45,3605.6,3027.25,7611.85,100.2,7303.05,927.65,3921.3,1363.25,5238.9,3042.25,3954.1,2868.15,3423.5,248.4,1126.35,1064.65,835.15,2151.6,5515.45,112.75,229.55,350.35,62.9,3027.65,2135.5,1723.95,19.75,3985.35,1215.65,1502.65,3260.1,35.45,81.25,1188.2,1778.5,1277.75,1170.55,70.45,6425.65,563.65,5971.25,5289.05,1756.2,6416.7,61.35,45.65,1929.95,1071.4,564.35,655.5,7930.55,5215.25,113.5,1152.8,1821.95,419.9,1024.0,251.6,764.55,1592.35,135.2,3958.25,233.9,1363.45,273.0,6254.45,2651.2,321.4,3539.25,242.8,1181.75,5000.2,654.55,780.2,1145.7,503.6,1559.25,1252.0,29.95,45.3,662.65,2453.3,1111.65,24.8,1023.85,82.15,244.8,2379.1,3173.35,531.0,1375.4,8129.3,1192.7,1901.65,587.4,6519.75,8041.65,20.75,2681.15,1112.3,7405.5,1033.95,2958.95,2684.85,4179.2,79.9,1934.45,6654.1,84.5,25.25,1124.2,540.05,1975.85,3437.45,3139.8,3789.2,5324.5,624.6,268.35,1836.9,20.2,179.35,219.35,1288.75,2545.75,55.2,2723.15,4107.25,5760.65,4747.5,84.6,1566.9,702.0,114.1,299.05,1305.95,1120.3,284.35,6350.5,7878.3,3187.65,6126.15,731.3,273.4,2531.8,1074.3,48.55,4298.45,4619.55,147.15,40.2,2633.3,193.05,4103.9,7008.15,5791.1,80.55,1228.65,132.2,1364.3,4925.35,1520.1,5032.25,5526.75,1195.25,2007.25,1099.6,1732.95,1511.2,3450.15,24.8,2172.05,70.6,401.1,5624.85,1339.8,771.95,244.75,322.9,498.25,25.4,3687.75,1779.95,1783.6,927.15,70.0,606.55,435.4,1712.7,2021.2,1940.8,567.8,220.35,20.25,5436.45,3437.5,3015.75,73.6,1509.8,396.1,356.65,4109.0,3141.7,1229.1,2303.35,2054.4,3741.85,3682.45,19.25,1886.25,4895.1,341.6,415.55,5686.4,1355.1,3058.65,2231.05,3236.35,4350.1,4264.0,44.8,422.3,4176.7,5138.1,880.05,139.05,973.65,1470.05,739.35,161.95,4422.95,511.25,155.8,5293.95,4759.85,6148.45,3565.65,6603.0,1830.1,6223.8,4508.65,1328.15,865.0,72.1,168.2,1303.5,996.85,6430.9,2278.75,681.4,574.35,371.9,840.1,846.0,889.0,6823.4,805.1,4016.75,83.75,3959.15,2878.55,945.7,1373.05,492.55,1406.0,19.15,6962.85,8126.65,690.25,181.5,830.8,5608.4,3646.8,3662.25,25.35,2566.5,5308.7,1410.25,3339.05,50.65,4732.35,90.85,5067.45,214.75,4874.7,2348.45,4063.0,44.0,2595.25,2309.55,89.3,367.55,3944.5,5965.95,3694.7,2524.45,1803.7,415.1,624.15,237.7,7007.6,3848.8,419.4,1468.75,5812.0,2861.45,19.9,19.6,233.7,1066.15,2149.05,4473.0,3545.05,1195.75,6858.9,1024.7,1845.9,75.3,132.25,515.45,2830.45,1110.5,449.3,2838.55,5376.4,858.6,1395.05,3975.7,1182.55,4784.45,119.5,518.9,899.45,1183.8,720.05,8468.2,3161.2,55.05,1882.55,5070.4,6049.5,1166.7,2937.65,6396.45,69.55,1270.25,759.55,7611.55,1642.75,1545.4,3582.4,2227.1,1417.9,2494.65,2768.35,2369.3,38.0,75.1,100.9,1614.05,385.9,673.25,8404.9,2799.75,6538.45,6588.95,868.1,734.35,330.6,55.0,564.4,1315.35,74.7,1861.5,2747.2,554.05,453.4,994.8,225.75,2145.0,1671.6,8003.8,680.05,6130.85,1415.0,6201.95,1397.475,74.35,6597.25,114.15,139.4,3902.6,20.4,903.6,1785.65,1397.65,131.05,1238.45,3899.05,5676.0,4543.15,4326.8,5502.55,1782.4,851.8,167.5,502.85,19.85,1818.3,6300.15,334.8,5916.95,2852.4,4131.95,1546.3,302.6,1929.35,265.45,6989.45,1442.0,4713.4,1758.6,3480.0,4738.3,8399.15,5430.35,686.95,5706.3,490.65,1360.25,174.45,7379.8,85.0,1021.75,5029.05,1955.4,6744.2,4946.7,8248.5,601.6,19.55,834.1,597.0,2647.2,3266.0,6744.25,5265.5,311.6,7966.9,8220.4,1153.25,514.75,2596.15,3808.0,19.9,2708.2,760.05,49.3,6033.3,89.05,516.15,5861.75,445.95,4973.4,1667.25,5357.75,3527.6,422.6,1103.25,2754.0,697.25,5614.45,3204.4,4747.65,3082.1,597.9,3365.4,38.8,233.55,75.3,346.2,19.0,61.7,85.7,3342.45,85.1,7422.1,6668.05,8071.05,1174.8,5435.0,2438.6,45.6,713.75,916.15,237.2,4614.55,1414.45,1170.5,47.7,4859.25,4903.2,3608.0,6094.25,3084.9,2356.75,8306.05,6786.4,248.95,663.05,1357.1,4860.35,3418.2,631.4,186.3,6976.75,4884.85,522.95,362.6,5755.8,3355.65,406.95,137.6,2395.7,1968.1,6819.45,7943.45,4547.25,4687.9,2473.95,6562.9,176.3,2236.2,6985.65,3109.9,4911.35,5794.65,855.3,1620.2,2499.3,89.55,4287.2,394.85,1899.65,45.7,3442.15,161.5,1732.6,222.3,74.6,655.3,475.25,164.3,865.1,6132.7,3597.5,35.9,697.65,96.05,428.7,20.05,4459.15,1167.6,238.1,145.15,1453.1,191.05,4039.3,1039.45,1336.1,75.05,493.4,2550.9,7246.15,1203.95,62.25,313.6,3775.85,80.0,4616.05,195.65,4188.4,71.1,49.9,1266.4,91.1,1623.4,4149.45,20.45,2344.5,1013.05,270.7,417.0,20.65,6316.2,168.15,4018.05,4811.6,4189.7,2848.45,2516.2,33.6,208.45,2015.35,3739.8,2964.0,2263.4,2211.8,19.55,1683.7,1519.0,1164.05,1710.9,4245.55,4145.9,2664.3,1277.5,5589.3,34.75,1305.95,381.3,141.5,3105.55,204.55,605.9,356.1,2758.15,4805.65,3941.7,92.75,1901.05,5730.7,2423.4,1653.45,3327.05,7085.5,3344.1,6697.35,2879.9,137.1,1008.55,1551.6,202.15,7882.25,8196.4,202.9,855.1,5817.0,1652.95,5600.15,515.75,1397.475,190.5,1842.8,1253.8,57.2,1269.55,6563.4,20.8,1907.85,208.85,4758.8,1292.6,363.15,1600.25,275.7,3089.1,1175.6,237.3,1444.65,19.9,454.15,3036.75,8065.65,92.5,184.65,6152.3,89.5,5154.5,220.45,1510.3,52.2,2588.95,4874.8,3983.6,2003.6,1832.4,4908.25,3590.2,5023.0,146.6,2339.3,298.7,143.65,2548.65,507.4,20.0,6125.4,5411.4,1058.25,903.8,3009.5,489.45,5468.45,1058.1,7616.0,4113.1,69.55,6017.65,7250.15,1108.2,938.65,94.15,2088.05,178.7,5656.75,2317.1,5986.45,6751.35,3566.6,4889.3,318.1,1563.95,1430.25,644.35,372.45,5453.4,1442.6,5610.7,963.95,5222.3,3340.55,292.8,5774.55,4487.3,44.4,2854.95,905.55,5509.3,7589.8,229.6,394.1,89.9,295.95,5459.2,444.75,6782.15,6510.45,8476.5,4461.85,62.0,352.65,1424.9,6413.65,6309.65,50.8,5898.6,4719.75,457.3,5822.3,1463.5,307.4,2104.55,319.15,2337.45,104.3,5084.65,121.25,7015.9,5598.0,1269.1,3027.4,4634.35,113.95,1582.75,3077.0,4039.5,1665.2,1043.3,504.2,497.55,7511.65,1782.0,20.05,609.65,2857.6,3247.55,6215.35,2823.0,5017.9,2619.25,24.6,4965.1,2679.7,8310.55,5682.25,1120.95,4914.9,27.55,923.5,1625.65,6068.65,5398.6,918.6,234.85,1231.85,170.9,7984.15,688.65,1288.3,7848.5,267.0,1798.9,73.55,1643.55,4807.45,2193.0,2239.4,1505.9,255.35,1189.4,4786.15,1820.9,2404.15,205.05,952.3,7039.45,2538.05,1212.85,2651.1,1304.8,360.1,435.45,308.05,1397.475,434.5,7118.9,320.45,531.55,382.2,2001.0,4919.7,5645.8,215.8,77.6,2896.55,3395.8,759.35,85.15,535.55,1253.15,955.15,2162.6,44.7,1813.35,245.15,2028.8,2723.75,220.45,365.8,551.95,4054.2,45.7,832.35,112.3,60.65,5550.1,174.8,90.55,4733.1,4048.95,1359.7,4542.35,7532.15,69.9,888.65,6383.35,1916.0,413.0,808.95,1886.4,86.6,1679.25,2656.5,540.95,19.75,537.35,678.8,4513.65,1423.85,555.4,225.55,268.45,2917.5,2416.1,424.45,1492.1,132.4,660.9,1893.95,284.9,784.25,417.7,5016.25,1612.75,119.75,3204.65,485.2,160.85,4145.25,827.45,49.5,990.85,696.35,5585.4,1601.2,162.45,470.2,2066.0,1426.45,392.5,3861.45,2552.9,6668.35,819.4,4615.25,2070.6,273.75,3557.7,1957.1,729.95,1416.75,5924.4,1697.7,7849.85,649.4,4557.5,3013.05,1266.1,360.35,1046.5,2347.9,447.75,4209.95,3877.65,152.3,572.2,19.65,526.95,552.7,3208.65,210.75,3706.95,620.75,412.5,832.05,185.55,1943.2,505.9,8046.85,1815.3,612.95,795.15,2169.8,973.1,2210.2,4853.75,1055.9,144.55,91.1,1304.85,713.0,21.1,5580.8,1497.9,4527.45,4590.35,200.2,614.45,4564.9,1397.475,171.15,1012.4,940.35,1047.7,2766.4,1622.45,1129.35,5680.9,2243.9,4523.25,7159.7,2839.95,80.55,580.1,2178.6,6038.55,259.4,324.15,417.65,168.15,5637.85,839.4,769.1,6253.0,1641.8,1678.05,2058.5,2424.5,387.2,6293.45,2839.65,3145.9,2200.7,914.4,4577.75,4997.5,4144.8,1493.55,4179.1,893.55,1611.0,593.05,4284.8,544.55,1533.8,529.8,3865.6,515.75,941.0,1133.65,48.35,2070.05,8333.95,1209.25,1396.25,723.35,228.65,1161.75,98.35,775.3,1074.65,35.55,2324.7,1072.6,170.5,196.9,1162.85,1677.85,18.85,370.4,3410.6,1138.8,5264.25,1005.7,5936.55,5475.9,224.05,2326.05,31.35,4991.5,1052.4,5831.2,510.8,283.95,1295.4,19.65,3011.65,8093.15,5610.25,3217.65,19.85,773.2,1029.35,669.45,3593.8,1553.95,3953.15,1971.15,1236.15,3196.0,4872.2,1500.5,60.15,3478.75,7413.55,3532.0,256.75,3887.25,2490.15,593.45,3510.3,765.45,1259.35,5538.35,340.85,844.45,1348.95,3778.0,611.65,4867.95,1505.05,467.85,74.9,194.2,571.45,80.25,5253.95,1149.65,740.8,521.35,1412.65,1532.45,250.05,1090.6,1446.8,2282.55,4300.45,1111.2,190.1,4447.75,143.35,45.85,810.2,1830.05,2820.65,4847.35,4729.3,4017.45,1398.6,2564.3,1685.9,5826.65,6066.55,228.4,270.2,1529.2,168.6,536.35,1888.45,629.35,45.3,289.3,2117.25,4730.9,2033.05,987.95,45.05,5744.35,75.8,19.45,523.15,4805.3,59.05,6110.75,1596.6,1046.2,4424.2,92.75,194.55,612.1,6127.6,6671.7,5264.3,303.7,4818.8,6448.05,7159.05,1574.5,2879.2,86.05,4159.45,6638.35,3112.05,7168.25,1326.25,2234.55,495.15,8317.95,679.8,62.8,7544.0,1188.25,676.7,74.1,3242.5,1240.15,4111.35,5899.85,632.95,5193.2,7530.8,270.95,5460.2,1614.2,402.5,1367.75,483.7,7962.2,3810.8,3533.6,1396.9,1345.55,1646.45,8127.6,2680.15,1281.0,1553.9,3207.55,2049.05,3629.2,5124.55,3474.45,202.3,147.5,86.35,579.0,19.45,3131.55,3928.3,187.75,1710.15,76.35,69.25,2151.6,5961.1,1221.55,1496.45,1292.2,25.15,1713.1,1748.9,25.2,96.45,1114.55,609.1,20.15,7133.25,1397.475,169.45,141.6,4688.65,563.05,5985.0,1258.6,373.5,857.2,2553.35,4322.85,250.8,4549.05,1359.5,1639.3,178.85,633.85,5315.1,735.5,889.9,1290.0,330.05,237.95,6474.4,4421.95,2452.7,813.85,4904.85,4484.05,2033.85,538.2,79.2,2192.9,19.85,3858.05,854.8,347.4,5815.15,3970.4,3058.15,6145.2,129.2,1165.9,49.95,1554.0,4904.25,5632.55,1643.25,740.55,3217.55,4888.2,2440.25,620.55,20.35,6840.95,3001.2,6254.2,319.6,1938.05,294.5,58.3,442.85,553.0,546.45,797.1,76.0,1673.8,343.45,7412.25,4039.0,170.85,2948.6,1308.4,6078.75,3418.2,6603.8,3166.9,865.75,6373.1,1177.05,5985.75,732.5,7869.05,1504.05,936.7,741.4,6585.2,3122.4,50.45,1088.25,615.35,2908.2,739.9,593.85,1132.75,7227.45,193.6,1291.35,2023.55,349.65,153.95,5458.8,5244.45,154.55,4507.15,2031.95,50.05,332.45,473.9,106.8,152.6,1199.4,2555.05,2979.2,654.85,3834.4,1534.75,4586.15,20.55,5941.05,424.15,2234.95,666.75,281.0,547.8,74.6,571.15,1756.6,5731.45,3475.55,156.85,2683.2,896.75,2407.3,4456.65,6998.95,36.8,6547.7,4346.4,2265.0,1309.15,4754.3,1235.55,3562.5,7213.75,2082.95,102.75,5914.4,51.25,1282.85,4738.85,19.55,1775.8,795.65,145.0,4993.4,61.45,4018.55,1146.65,6081.4,1478.85,243.65,2633.95,4735.35,1247.75,74.7,3794.5,1008.7,2130.55,1313.55,5727.15,1372.9,1203.9,25.8,1215.1,2877.05,1579.7,5514.95,96.1,72.4,55.25,2443.3,1970.5,335.4,7002.95,158.95,293.15,1493.75,1093.4,1057.85,190.05,882.55,300.4,1025.95,1436.95,475.0,5388.15,5730.15,819.55,217.1,4896.35,1434.1,937.1,330.15,1474.9,6536.5,1128.1,6873.75,2199.05,45.85,457.1,566.5,2471.6,3107.3,518.3,5769.75,91.7,832.3,1555.65,45.3,1790.6,74.95,246.6,261.65,898.35,4509.5,5480.25,653.15,1183.05,639.45,478.75,20.05,127.1,4391.45,270.6,6710.5,3975.9,1704.95,415.95,950.2,1497.05,780.15,3512.15,273.25,3517.9,3375.9,7508.55,1245.05,5347.95,493.65,1263.85,385.55,3384.0,84.2,1638.7,165.4,120.25,4473.45,520.55,5717.85,88.8,4312.5,2755.35,994.55,6511.25,1218.65,7447.7,1169.35,279.5,5720.35,3121.45,6468.6,5918.8,675.6,3521.7,923.1,1110.35,1611.65,2293.6,5553.25,44.75,3029.1,3231.05,5129.45,5508.35,655.9,1191.2,866.4,2627.2,4741.45,4009.2,1463.45,5082.8,43.8,3707.6,220.45,1133.7,1983.15,5746.75,770.6,134.05,6230.1,573.05,3419.3,3587.25,3541.35,3801.3,868.5,4859.1,1439.35,85.8,79.1,592.75,1185.95,18.8,134.5,4921.2,901.25,5341.8,4859.95,1139.2,7082.45,324.6,4812.75,4399.5,663.55,174.65,321.7,5125.5,548.9,50.15,7031.3,5016.65,4594.95,611.45,2384.15,319.85,153.3,7035.6,344.2,1431.65,1849.2,6083.1,426.65,1655.35,1943.9,1616.15,314.95,804.85,465.7,6669.05,1161.75,947.75,8375.05,34.7,3770.0,264.8,4707.85,6152.4,958.1,943.0,6615.15,2200.25,425.1,99.75,5044.8,6743.55,196.4,299.7,2093.9,417.75,1725.4,1620.2,3169.55,5233.25,967.85,438.05,1421.9,323.15,318.6,163.7,639.65,2928.5,100.35,273.2,1608.15,1441.95,2168.15,3618.7,5692.65,59.85,69.6,5969.3,19.05,418.8,8100.55,19.5,313.45,6130.95,69.9,745.3,1212.1,183.75,1583.5,4162.05,1119.9,8013.55,264.85,1102.4,5832.0,4304.5,1178.4,1421.75,6827.5,4698.05,654.5,3268.05,4362.05,1386.9,415.4,2614.1,1513.6,3161.6,80.95,4676.7,6526.65,583.3,8078.1,3503.5,6669.45,4689.5,1201.15,143.65,1292.65,48.75,7108.2,1802.55,1505.15,1859.1,168.5,390.85,6339.45,1652.4,71.65,77.5,6392.85,3264.5,4599.15,1134.25,1621.35,947.3,2722.2,3058.3,1769.6,6293.75,1761.05,1642.75,578.5,954.9,973.25,43.85,1490.4,280.0,1901.25,716.4,4720.0,930.95,76.35,1676.95,2642.05,6770.85,1835.3,1588.25,75.5,258.35,1502.25,19.2,6929.4,4453.3,3244.4,323.45,2661.1,2697.4,1424.5,1401.15,1739.6,5931.0,2333.85,949.85,572.45,696.8,1381.8,2572.95,47.95,45.1,45.0,2122.45,20.05,4931.8,116.95,6937.95,1261.7,3273.8,1415.85,3958.2,2492.25,279.2,1083.0,755.5,402.6,3252.0,68.75,46.2,45.15,43.3,936.85,2250.65,3857.1,1441.1,4338.6,1378.45,45.95,2566.3,171.0,1597.4,4744.35,6721.6,247.25,44.05,1734.65,45.55,4539.35,804.25,5011.15,3574.5,8086.4,4563.0,6362.35,67.1,70.05,165.45,1156.55,1834.15,3653.35,1477.65,1706.45,3953.7,1194.3,1327.85,419.7,21.0,45.1,207.35,1083.7,2007.85,5882.75,2657.55,1527.5,378.4,1612.2,76.65,260.7,6441.85,266.8,5124.6,962.25,1382.9,571.05,1399.35,150.0,167.2,7657.4,865.05,6153.85,174.2,1269.6,3862.55,6352.4,1348.5,50.9,471.55,5931.75,1404.65,726.1,1961.6,4194.85,4872.45,5118.95,658.95,81.05,76.95,5809.75,20.2,415.9,246.5,996.95,3145.15,265.3,20.9,21.05,4576.3,189.1,908.15,49.8,215.2,1500.95,5099.15,6385.95,159.45,6885.75,5940.85,668.85,1916.2,75.35,75.45,4613.95,7261.25,2459.8,2048.8,163.55,2888.7,2421.6,2292.75,553.4,3465.7,1210.4,1291.3,5356.45,5200.8,3237.05,576.65,433.75,1868.4,5728.55,825.7,390.4,93.55,2845.15,3894.4,886.4,1021.55,5885.4,268.4,2204.35,1259.0,309.1,6398.05,2257.75,6287.25,1662.05,1350.15,3600.65,1329.15,45.7,422.5,69.95,4627.65,6717.9,97.1,1710.45,637.4,117.95,2460.55,5464.65,2165.05,4941.8,223.15,181.1,341.45,5731.85,357.75,4616.1,4533.7,589.25,750.1,1410.25,830.85,743.5,45.3,7040.85,3865.45,6825.65,1340.1,371.65,1184.0,8477.7,7382.85,456.85,331.6,6056.15,134.6,125.5,1390.6,20.2,2511.3,2737.05,74.3,70.6,2361.8,1730.35,6404.0,165.35,1867.6,1043.3,128.6,7455.45,24.7,206.15,2030.3,5869.4,3377.8,1346.2,4946.05,4065.0,106.55,4964.7,4085.75,1742.75,6224.8,3415.25,6325.25,340.25,1683.6,3966.3,608.5,2896.6,1514.85,6792.45,4669.2,95.6,2934.3,6302.8,779.2,283.75,4600.7,5594.0,772.85,71.35,857.8,7554.05,5981.65,1702.9,467.15,20.15,69.75,2931.0,1400.85,137.85,1941.5,1932.75,1423.05,970.85,5810.9,223.9,391.7,79.95,19.3,811.65,174.75,3126.85,6841.45,3754.6,1406.65,834.7,627.4,242.0,3157.0,3092.0,2168.9,980.35,65.95,711.9,1952.8,4368.85,2647.1,8477.6,54.35,4528.0,1790.65,845.6,1210.3,20.45,854.45,2429.1,100.4,5229.45,44.45,1716.45,3023.55,75.3,4837.6,2032.3,436.9,70.55,20.15,5893.95,1430.05,313.0,3871.85,458.1,2745.7,341.35,1336.8,2181.75,147.75,818.45,7133.45,348.8,128.6,4674.4,1737.45,1498.85,50.1,1797.75,498.1,2624.25,184.1,5498.8,4845.4,369.1,6096.9,160.75,2684.35,3004.15,6994.8,273.25,5483.9,1233.65,527.9,4985.9,1258.35,111.4,43.95,308.1,383.65,2555.9,1284.2,7550.3,1110.05,99.6,6707.15,4164.4,5873.75,109.25,179.25,1338.15,862.4,8564.75,926.2,1718.2,5956.85,91.3,4824.45,1238.65,79.5,672.2,382.2,4264.6,1070.7,1345.85,1045.25,3003.55,467.55,7537.5,7482.1,3343.15,5427.05,587.1,100.8,161.15,7028.5,5232.9,225.85,274.7,1180.95,3370.2,7111.3,5958.85,5999.85,1648.45,5753.25,4492.9,3956.7,2625.55,1233.4,1309.0,813.45,1108.8,7349.35,294.2,929.2,740.0,754.5,3883.3,1414.2,3211.2,371.6,1246.4,95.85,2545.7,6448.85,1468.9,1013.6,6690.75,2088.75,7240.65,102.45,830.25,1588.7,829.3,302.45,712.25,1336.65,5360.75,6735.05,51.2,1010.0,4847.05,3019.7,161.65,217.55,2583.75,146.05,567.45,7711.25,1384.75,5481.25,8124.2,827.3,70.9,220.95,3673.6,49.85,576.65,2331.3,74.5,4495.65,6941.2,340.35,1789.9,908.55,1043.35,6822.15,71.55,157.55,5029.2,581.7,7318.2,420.45,7576.7,633.4,321.65,4965.0,6151.9,1253.9,25.15,45.2,5893.15,5420.65,2627.35,5037.55,743.75,6589.6,92.45,6733.15,3410.0,199.85,593.3,7288.4,5229.8,4464.8,5068.05,401.85,4451.85,6688.95,2661.1,73.05,1211.65,7030.65,1020.2,5597.65,6223.3,1024.65,2933.2,1258.3,82.9,7789.6,2067.0,3452.55,5468.95,1802.15,733.55,3021.45,3687.85,1391.15,274.35,1214.05,5510.65,1322.85,1973.75,2196.3,7843.55,3211.9,130.15,692.35,85.5,6849.4,203.95,2117.2,6565.85,424.75,3306.85,168.9,253.8,197.4,3838.2,2065.15,5064.45,1140.05,2447.45,1263.9,45.95,1838.15,44.75,1185.0,1743.9,70.15,85.55,8529.5,6549.45,7806.5,25.7,6287.3,3251.85,50.6,7904.25,729.95,2654.05,2416.55,3520.75,5969.95,226.8,1082.75,565.75,4370.75,90.05,2169.4,550.6,55.45,6300.85,160.05,436.6,1781.35,3467.0,5552.5,2835.5,3591.25,88.35,63.75,617.15,870.25,601.25,111.65,6046.1,3923.8,777.3,512.45,141.45,892.15,1682.05,3398.9,4984.85,1445.3,305.55,507.9,3640.45,2673.45,389.8,2401.05,651.55,156.1,2796.35,70.05,407.05,6465.0,511.25,646.05,35.9,3777.15,4903.15,1653.85,47.5,1306.3,463.6,60.65,824.85,2094.65,438.9,847.3,329.75,674.55,162.55,546.95,7887.25,3186.65,1972.35,1527.35,490.55,1531.4,683.25,8058.85,2847.4,1133.9,294.45,1719.15,461.7,1160.45,5199.8,5163.3,162.3,883.35,1341.5,70.45,659.45,77.15,35.25,1205.05,4917.9,201.0,599.3,1733.15,5149.5,4677.1,119.3,4849.1,5424.25,5878.9,244.85,220.75,4261.2,5574.75,1501.75,89.75,4541.2,255.5,1072.0,2509.25,1440.75,1715.65,5333.35,3895.35,869.9,706.85,512.25,2636.05,814.75,388.6,93.45,1389.85,2894.55,4025.6,1354.4,1856.4,926.0,189.2,682.1,1778.7,1816.2,7069.25,1841.2,74.25,2656.7,1689.45,1054.75,2187.55,7334.05,1581.2,69.5,2404.85,171.45,2839.45,3132.75,3942.45,873.4,1529.65,6991.6,19.4,803.3,679.3,2791.5,715.0,1681.6,4016.2,3281.65,7317.1,6474.45,676.35,8164.1,1325.85,1654.85,7795.95,3952.45,2495.15,1230.25,876.75,4263.45,1992.95,1982.1,562.7,33.7,1090.1,227.45,1250.85,37.2,892.7,487.75,3713.95,141.7,678.2,8425.3,4154.8,7061.65,3364.55,2655.25,1425.45,332.5,5963.95,5742.9,278.85,1871.85,2866.45,4303.65,1753.0,54.75,3759.05,617.65,5935.1,94.6,2911.3,982.95,2974.5,198.7,1275.65,4003.0,3791.6,813.3,780.25,552.9,408.25,231.8,2191.15,5611.7,246.25,1296.15,7082.85,5662.25,1215.45,7854.15,525.0,5265.2,70.1,7129.45,7266.95,8496.7,2878.75,261.3,3205.6,69.75,279.25,6281.45,1131.3,738.2,1137.05,80.2,6341.45,6697.2,260.8,19.6,505.45,3655.45,1299.8,5564.85,1381.8,188.1,1077.05,4922.4,2911.5,826.1,83.4,140.7,5377.8,665.45,3617.1,6643.5,84.8,1559.3,7987.6,1547.35,5426.85,1127.35,2142.8,287.85,4045.65,2757.85,600.0,19.8,4534.45,40.9,2094.9,1584.8,1302.65,2238.5,55.55,5437.75,90.75,365.65,2209.15,1912.15,255.55,5175.3,670.65,3177.25,90.35,6557.75,5791.85,3055.5,5196.1,8405.0,19.75,1789.25,5000.05,6713.2,562.6,2317.1,91.45,828.85,617.35,266.9,625.05,301.4,6029.9,1842.7,206.6,604.7,7386.05,7334.05,1471.75,2626.15,412.1,8277.05,583.45,369.25,1049.05,1414.8,169.75,4354.45,2719.2,6365.35,51.6,3190.25,812.5,1143.8,44.0,50.1,3297.0,1887.0,151.8,293.65,1308.1,2215.4,482.8,2598.95,216.45,20.45,5064.85,996.45,839.65,6733.0,2618.3,4084.35,765.5,793.55,613.95,402.85,1244.8,46.4,581.85,462.8,1540.2,169.8,5168.1,6780.1,94.5,55.3,208.0,3829.75,5294.6,6889.8,3254.35,6383.9,44.4,477.7,4447.55,7321.05,4135.0,86.05,697.7,168.65,174.3,2753.8,69.25,434.8,1077.5,95.65,107.25,851.2,20.95,5848.6,109.8,19.65,595.5,329.8,2513.5,5135.35,2000.2,931.75,7396.15,3958.85,260.9,297.3,1515.1,816.8,4868.4,688.0,288.35,3229.65,1178.25,185.4,966.25,758.6,1863.8,265.8,3297.0,4107.3,396.3,2809.05,1567.55,1851.45,6028.95,2072.75,5551.15,4317.35,736.8,336.15,1522.65,622.9,4959.6,329.95,1048.85,1001.5,442.6,6703.5,3351.55,779.25,259.8,3483.45,4890.5,136.75,184.15,1315.0,6767.1,757.95,6890.0,1657.4,3008.55,262.05,8165.1,875.55,2649.15,220.65,1301.9,74.4,3326.2,790.0,1237.65,378.6,592.65,50.15,20.45,560.85,3638.25,1060.2,2921.75,4017.45,854.9,4174.35,4920.55,20.5,810.3,772.4,1936.85,79.8,202.25,1070.5,347.65,999.9,113.1,2015.8,1454.25,246.7,6017.9,5817.45,5595.3,4765.0,1201.65,867.3,50.15,3007.25,252.75,6306.5,6841.05,81.95,451.1,44.6,226.2,7082.5,1017.35,527.35,2146.5,455.3,5969.85,1235.55,1014.25,2362.1,1225.65,1873.7,386.5,6010.05,1850.65,7101.5,1043.4,1910.75,716.1,1686.15,6716.45,7839.85,6236.75,45.05,71.0,2919.85,3309.25,79.7,20.45,1254.7,2896.4,717.5,253.8,1110.05,348.8,3888.65,69.25,6843.15,450.9,432.25,1767.35,1742.45,297.35,1820.45,1742.95,2444.25,949.8,73.5,2970.8,3334.95,2010.95,4684.3,2181.55,1303.25,371.4,2409.9,6155.4,829.1,2602.9,4667.0,824.75,5153.5,8182.85,69.9,6683.4,1564.05,755.6,3465.05,6292.7,1664.3,198.1,6045.9,4108.15,5980.75,5043.2,1029.75,2854.55,7114.25,907.05,973.95,605.75,661.55,4116.9,4494.65,4259.3,3282.75,55.7,1274.45,493.95,2239.65,480.75,635.6,5038.15,324.2,19.55,2793.55,2095.0,648.65,95.05,917.15,1346.9,4242.35,672.7,6561.25,268.45,7337.55,331.9,5194.05,4478.85,6283.3,2659.4,196.9,439.2,19.5,2107.15,3035.8,1866.45,1430.95,1071.6,6457.15,684.05,3914.05,3842.6,670.35,7880.25,19.2,298.45,3371.75,442.45,20.4,2345.55,25.25,1156.35,6143.15,144.8,414.95,1059.55,181.8,2212.55,2180.55,89.15,459.95,1036.75,2276.95,211.95,25.0,3162.65,210.65,3566.7,2080.1,4040.2,5186.0,196.15,1087.45,1672.15,1206.05,1113.95,107.05,38.15,6339.3,184.1,1688.9,1378.25,137.6,219.0,1067.15,79.55,3444.85,369.05,7553.6,84.5,1813.1,3321.35,707.5,7112.15,4641.1,7690.9,49.9,1380.1,78.65,45.4,3094.65,6518.35,2585.95,82.7,58.85,425.9,6342.7,2030.75,6700.05,7244.7,3678.3,3486.65,411.15,937.5,1559.15,970.4,2483.5,828.2,3810.55,1493.2,45.8,777.3,556.35,4911.05,187.35,307.6,4318.35,4820.55,3038.55,2136.9,7472.15,389.1,2296.25,187.45,261.25,38.45,299.2,3043.6,1506.4,163.7,323.25,1640.0,762.1,3846.35,5646.6,53.5,79.05,25.1,1516.6,2224.5,888.75,4310.35,42.9,2365.15,353.65,5073.1,4526.85,308.25,19.45,678.45,1237.3,1023.95,8182.75,4532.3,1444.05,19.1,7372.65,7325.1,3474.2,392.65,1058.6,3990.75,7475.85,835.5,2628.6,1718.35,1023.9,1193.55,1205.5,5776.45,78.9,1025.15,856.35,122.0,6602.9,1712.9,5682.25,74.3,3635.15,702.9,3734.25,1144.5,4454.25,45.3,75.6,1117.55,180.25,19.8,930.05,110.15,333.6,465.05,1669.4,3545.1,504.05,440.2,1151.55,2475.35,1249.25,317.75,535.05,461.3,431.0,878.35,335.75,3121.4,324.8,1394.55,3413.25,3143.65,439.75,664.4,4759.55,1033.0,3482.85,2688.45,435.25,2448.75,307.0,2689.35,1329.2,1281.25,3527.0,4348.65,561.15,63.6,5129.3,4285.8,93.7,5894.5,25.05,1160.75,3091.75,365.55,272.95,3632.0,381.2,1035.5,52.0,2342.2,653.9,71.2,1498.2,4178.65,1821.8,5278.15,4024.2,55.25,6520.8,854.9,8308.9,109.2,92.05,3420.5,93.85,4068.0,337.9,3168.75,1412.4,3974.7,3439.0,50.45,42.45,5461.45,571.75,5498.2,96.85,111.05,94.4,521.1,923.85,141.1,628.65,5576.3,1046.1,1245.6,1287.85,1939.35,118.25,452.55,2696.55,875.35,1267.05,494.9,799.0,5763.3,20.1,106.9,710.05,568.2,1900.25,159.15,8012.75,856.65,551.3,184.95,4056.75,1096.65,3684.95,1752.45,7210.85,5264.5,2157.3,24.4,433.95,2435.15,5607.75,2874.15,4433.3,964.35,1974.8,1460.85,951.55,1821.2,1600.95,399.25,1218.55,81.7,2171.15,3634.8,292.4,317.25,218.55,5071.9,1423.35,3068.6,4690.65,6157.6,1442.65,3369.05,4671.65,3474.05,1875.55,272.35,3645.05,135.75,1319.85,229.4,370.25,69.1,5714.2,1233.25,3571.6,83.3,8035.95,579.4,69.6,3066.45,305.55,7634.25,3653.0,241.3,3870.3,387.9,527.5,78.45,2104.55,20.3,19.2,3019.1,7051.95,1190.5,4448.8,255.25,146.9,1011.05,1714.95,762.5,535.35,75.55,338.9,2497.35,6273.4,70.25,908.75,4913.3,1397.475,46.3,212.3,4443.45,316.2,1079.05,564.65,1095.65,6161.9,446.05,2106.05,2511.55,318.6,811.8,7323.15,186.05,454.0,7521.95,1056.95,44.15,834.15,85.45,999.8,2369.7,6297.65,239.55,59.2,1461.45,416.4,1060.6,4869.35,54.9,3265.95,2254.2,358.15,2939.8,4652.4,4408.45,580.8,2495.2,180.3,5913.95,158.4,104.2,1389.35,19.4,1629.2,6033.1,44.4,95.1,3320.75,1867.7,438.0,325.45,1148.1,3972.25,155.9,3673.15,70.6,1126.75,73.45,2413.05,6912.7,1581.95,5586.45,5846.65,6424.7,6018.65,3373.4,1537.9,1080.55,355.2,82.85,2016.45,5327.25,683.25,1263.05,151.3,7714.65,188.7,5549.4,130.5,2621.75,1875.25,5685.8,837.5,401.5,6118.95,6480.9,1800.05,7104.2,4214.25,50.6,863.1,1992.2,69.8,1948.35,4750.95,1007.9,5036.9,2391.8,6859.05,6521.9,617.85,157.75,516.3,2364.0,1687.95,7689.95,6239.05,2042.05,2807.1,4116.8,1317.95,4594.65,6332.75,3213.75,229.55,4976.15,7308.95,4555.2,44.95,6982.5,1025.15,90.55,5714.2,19.5,2274.35,789.55,4834.0,3131.8,602.55,3369.25,2674.15,199.75,1790.8,449.75,19.7,1400.3,411.6,741.0,5841.35,4512.7,5688.45,31.9,6674.65,1345.75,1843.05,196.95,1433.8,214.55,865.85,1358.6,3147.15,131.05,4535.85,1078.75,542.4,2312.55,415.05,3250.45,98.5,87.9,754.65,75.35,1952.25,153.8,3198.6,20.9,5610.15,4519.5,2440.25,6860.6,1377.7,190.25,1651.95,78.3,7939.25,44.9,576.7,1279.0,1260.7,267.35,6586.85,934.15,123.65,7534.65,19.45,3645.6,314.45,3053.0,677.05,7965.95,906.85,4888.7,237.25,74.6,342.4,140.1,1108.0,295.55,892.65,198.25,4822.85,6741.15,79.15,1355.45,7209.0,438.4,7726.35,2070.75,1636.95,7581.5,3019.5,4729.75,6126.1,6333.4,6687.85,4158.25,3183.4,79.6,7149.35,1429.65,472.65,1734.5,113.5,1884.65,2568.15,470.0,278.4,2088.8,502.6,2595.85,5825.5,50.75,4449.75,1208.35,1956.4,310.6,290.55,2096.1,2665.0,543.8,20.35,3005.8,1623.15,2498.4,2586.0,3292.3,651.4,4674.55,232.35,2448.5,658.1,3128.8,223.45,919.4,653.95,1825.5,943.1,181.65,45.65,551.35,359.4,115.95,165.2,2338.35,46.3,3541.1,4146.05,1087.25,2522.4,81.0,717.3,1734.2,7069.3,742.9,3017.65,6423.0,1164.3,4220.35,1558.7,2743.45,4055.5,2710.25,6055.55,810.7,2538.2,6129.2,1750.85,36.55,6652.45,2575.45,6019.35,1379.6,1284.2,296.15,653.25,412.6,85.5,49.55,1928.7,71.25,7845.8,1750.7,216.2,178.5,115.1,6029.0,2745.2,3721.9,3121.1,990.45,1004.35,157.65,3219.75,572.85,4554.85,1847.55,1766.75,1462.05,25.25,2193.2,2433.5,641.15,2398.4,861.85,583.0,7332.4,249.55,4475.9,416.45,788.6,202.3,6994.6,4116.15,2263.45,1374.35,4915.15,838.5,75.1,3715.65,4273.45,45.8,20.5,2044.75,30.55,1398.25,20.1,328.95,4680.05,69.1,3778.2,3161.4,2188.45,999.45,1888.65,3990.6,71.15,1240.25,2635.0,235.0,2347.85,7156.2,3089.6,270.8,2901.8,4949.1,2198.9,374.5,761.95,1520.9,190.25,5163.0,4059.85,2281.6,1356.7,231.45,50.75,242.4,4264.25,2774.55,3605.2,4069.9,874.2,1145.35,1323.7,5497.05,534.7,2455.05,38.7,947.4,357.0,1476.25,70.8,1448.6,291.9,5903.15,1201.65,5921.35,146.65,1759.4,844.45,7774.05,134.05,140.95,249.95,1701.65,592.65,135.75,7732.65,4282.4,219.65,2018.1,669.0,68.95,224.85,3771.5,196.75,943.85,6572.85,3175.85,29.15,20.05,318.9,24.45,2762.75,49.55,631.85,232.5,5919.35,749.25,307.4,245.15,134.75,55.8,8240.85,4266.4,1077.5,1115.6,79.05,95.0,25.2,4079.55,4187.75,2391.15,890.5,137.25,5154.6,3119.9,529.5,966.55,6936.85,3496.3,914.3,1929.0,5817.7,6110.2,178.8,28.3,435.0,2351.8,186.15,445.85,912.0,679.55,3581.6,4222.95,1994.3,5930.05,1505.85,20.0,5638.3,797.25,71.65,1359.45,2542.45,54.7,989.05,44.55,87.3,351.55,7297.75,6301.7,210.3,3822.45,1048.45,6393.65,1489.3,8061.5,74.2,518.75,5763.15,238.5,1031.7,1397.475,34.8,1319.95,4388.4,420.2,2177.45,3950.85,827.05,3913.3,533.5,3756.45,443.9,2599.95,284.3,1740.8,3436.1,2462.55,70.85,3110.1,280.85,411.75,198.6,103.7,6144.55,4089.45,864.2,19.65,3249.4,5405.8,3363.8,7132.15,93.55,4138.9,5121.75,566.1,2715.3,1782.05,1742.5,2983.8,24.0,485.9,1905.7,1565.7,4858.7,3723.65,298.35,970.45,1782.0,405.7,4534.9,2415.95,1157.05,8297.5,45.75,2083.1,4681.75,176.2,1001.2,1594.75,212.4,7677.4,815.5,80.15,259.65,8109.8,2386.85,340.35,419.35,7990.05,1402.25,749.35,8425.15,1171.5,5647.95,708.8,7567.2,29.9,4348.1,635.9,108.95,78.8,1682.4,6925.9,223.15,5127.95,874.8,6758.45,1874.3,791.15,4639.45,143.9,69.1,1776.55,536.4,6172.0,1559.45,6079.0,80.85,4029.95,2658.4,383.55,51.15,1745.5,2230.85,7281.6,1837.7,149.55,180.7,411.45,1328.35,576.95,68.45,123.05,78.65,468.35,174.7,42.9,44.0,172.35,139.35,539.85,300.4,4968.0,992.7,4718.25,5536.5,7806.6,434.1,1563.9,1332.4,545.2,6296.75,1284.2,3645.5,161.45,226.95,646.85,1441.65,7511.3,5621.85,7919.8,593.2,1423.65,69.95,2763.35,692.1,2298.55,2640.55,2911.8,1727.5,86.6,6705.7,237.65,1672.35,2979.5,695.75,1654.6,24.25,1857.85,825.1,6424.25,837.95,4126.2,330.8,1337.45,362.2,5991.05,4891.5,5373.1,2068.55,487.05,4131.2,2301.15,131.65,4036.0,4900.65,5727.45,743.05,4804.65,24.05,1414.65,1443.65,2898.95,5309.5,20.4,451.55,50.55,117.05,5839.3,1893.5,45.35,1654.45,477.05,1415.55,4546.0,20.5,163.2,849.9,189.95,321.9,8058.55,482.8,7049.75,2560.1,286.8,7982.5,5683.6,3994.05,369.15,4631.7,401.95,1534.05,1093.0,701.3,1980.3,2893.4,262.3,3886.85,5917.55,914.0,2139.1,5948.7,3624.35,4753.85,5780.7,6869.7,1172.95,662.95,1765.95,2431.95,155.35,1859.2,3303.05,134.35,3409.1,709.5,70.5,953.45,50.15,19.75,3665.55,5515.8,272.0,6668.0,4052.4,664.4,718.55,937.6,5812.6,2546.85,6944.5,1346.3,1544.05,765.15,692.55,69.6,4059.35,6851.65,78.05,1187.05,5401.9,247.0,520.0,706.6,49.9,1370.35,20.25,2215.25,74.0,214.4,1871.15,6171.2,20.2,161.45,1013.2,336.7,333.65,6511.8,393.45,7009.5,2264.05,6921.7,600.25,56.35,4664.15,1441.8,5623.7,695.85,1028.75,4155.95,395.6,6330.4,2971.7,638.0,2034.25,2510.2,3419.5,2427.35,1760.25,3551.65,122.9,1424.2,2111.45,2909.95,374.0,20.25,300.8,5581.05,5676.65,3379.25,942.95,232.4,85.45,2088.45,6137.0,1434.6,3870.0,2043.45,2215.0,842.25,2576.2,1901.0,601.6,3515.25,605.45,3094.05,4929.55,595.05,469.8,8436.25,19.75,492.0,987.95,5496.9,1908.35,107.1,4575.35,4309.55,7922.75,522.35,587.7,3876.2,3778.85,1457.25,8349.45,185.6,19.5,1066.9,973.55,1226.45,342.3,985.05,3948.45,828.05,41.85,6164.7,2862.75,33.6,343.95,20.7,6590.5,717.95,2890.65,4885.85,1222.05,6871.7,405.6,208.25,1801.9,3062.45,1764.75,1816.75,1255.1,5743.05,3921.1,1463.45,189.45,96.8,408.5,1846.65,8456.75,1011.5,4263.4,2549.1,3965.05,2496.7,90.6,288.05,368.1,7840.6,6632.75,1013.35,152.95,3772.65,1026.35,19.3,5031.85,220.4,1416.5,158.35,256.6,5542.55,68.5,8443.7,791.75,5708.2,69.95,2016.3,326.65,5471.75,19.95,346.4,1061.6,1971.5,773.65,1422.05,19.75,2460.35,856.5,1275.85,7467.5,7261.75,5116.6,660.05,6590.8,1208.15,1033.9,1885.15,2467.1,989.45,2006.95,5025.0,4830.25,59.75,3088.25,3134.7,94.55,8312.4,4863.85,6871.9,6529.25,6637.9,3165.6,1454.15,6786.1,543.0,1327.15,2404.15,867.3,25.75,114.7,109.5,4692.95,546.85,1274.05,1782.4,5301.1,280.35,2897.95,3371.0,801.3,6975.25,257.05,1079.45,40.1,6997.3,2217.15,1129.1,979.05,4119.4,2568.55,3349.1,492.1,1718.95,605.25,1344.5,4267.15,1121.05,918.7,521.8,4469.1,3580.3,1729.35,1073.3,4566.5,293.3,2603.1,1783.75,2440.15,79.55,6322.1,57.4,4284.65,7138.65,1539.75,1058.1,123.8,2781.85,2731.0,20.75,89.1,497.3,711.15,1607.2,1490.95,1898.1,3273.95,2929.75,838.7,1443.65,7746.7,6951.15,214.75,2669.45,235.2,92.65,3103.25,606.25,5330.2,1403.1,2727.8,5038.45,19.1,80.3,1462.6,150.6,5960.5,74.9,1192.3,314.55,56.0,330.8,877.35,2249.1,2807.65,1696.2,1032.05,4902.8,4995.35,5034.05,1857.75,1992.85,751.65,66.95,1714.95,343.6,20.15,520.1,1387.45,7383.7,4483.95,1706.45,1327.4,5974.3,1397.475,4515.85,179.85,5040.2,165.0,422.7,3046.4,20.25,358.05,2936.25,1271.8,530.05,19.2,1808.7,1144.6,7446.9,25.1,7556.9,4858.7,6595.0,149.05,4972.1,1880.85,2045.55,2187.15,477.55,2976.95,178.7,5025.85,3353.4,1461.15,2782.4,1699.15,1496.9,452.2,4428.45,1322.55,70.45,3563.8,139.75,1927.3,3000.25,3021.3,2245.4,24.75,944.65,1264.2,4917.75,2012.7,5743.3,1864.65,1221.65,1390.85,302.35,1374.9,1336.9,1297.8,6067.4,1222.25,487.95,2548.55,835.5,242.05,44.75,63.75,6218.45,260.9,7320.9,2967.35,6333.8,939.7,4200.25,5950.2,1037.75,93.4,50.45,1614.9,1573.95,3624.3,100.25,1468.75,2607.6,1422.1,69.85,858.6,1523.4,324.3,3435.6,399.6,4549.45,322.5,3273.55,5375.15,2510.7,3090.05,61.05,20.9,955.6,140.4,1493.1,69.4,1626.05,541.15,1784.9,70.35,6075.9,5500.6,73.1,3229.4,3399.85,6431.05,19.75,1230.9,454.65,235.05,70.1,6595.9,5867.0,196.35,909.25,6449.15,762.45,5283.95,1617.5,785.75,1776.0,5396.25,574.5,400.3,84.3,2656.3,1445.95,5212.65,6440.25,2244.95,1130.0,6463.15,1131.2,585.95,6252.7,19.95,2062.15,587.1,720.45,2186.4,2979.3,956.65,3201.55,3973.2,447.9,1335.2,661.25,1111.85,7555.0,49.0,3046.15,69.35,836.35,272.2,5293.4,20.95,50.55,2570.0,798.2,80.55,44.15,916.9,6487.2,1855.65,1556.85,1988.05,5432.2,147.55,8424.9,2203.7,387.4,116.65,3045.75,2287.25,130.5,767.9,446.8,8100.25,830.7,20.0,4689.15,6754.35,3540.65,2184.6,1054.6,130.55,1540.35,6411.25,1432.55,7629.85,101.9,313.4,6312.9,629.55,2640.55,1372.45,1212.25,6237.05,6310.9,5031.0,85.05,8672.45,2196.45,3008.15,220.8,5779.6,222.65,914.6,246.3,2110.15,5560.0,1633.0,547.65,969.85,2610.65,6567.9,4747.85,1759.55,101.1,6496.15,4234.15,300.7,59.55,4323.35,1466.1,8684.8,1776.45,2933.95,4370.25,144.35,4804.75,1125.2,39.85,422.4,251.75,8332.15,314.6,4331.4,6382.0,740.3,600.15,5538.8,7049.5,690.5,279.3,1427.55,452.7,19.45,1709.15,53.15,777.35,860.85,5012.1,166.3,2404.1,70.0,24.4,4820.15,209.9,610.75,3409.6,70.7,155.35,144.0,7875.0,911.6,1270.2,478.1,1561.5,761.85,2282.95,1615.1,1097.15,369.3,6725.3,31.55,6293.2,432.5,321.75,147.15,2434.45,532.1,375.25,125.95,1042.65,1620.25,2387.75,659.35,2660.2,1285.8,4211.55,377.55,990.9,193.8,6058.95,964.9,790.15,2467.75,2322.85,7262.0,4854.3,7748.75,1914.9,6470.1,57.4,2019.8,5688.05,1522.7,1334.5,252.0,700.45,5655.45,6747.35,5265.1,5174.35,105.6,2271.85,2966.95,1772.25,61.15,494.95,44.45,5290.45,3346.8,5219.65,217.5,607.7,5431.4,6004.85,5957.9,5442.05,4370.25,4428.6,416.45,3067.2,6688.1,20.25,4224.7,74.35,4042.2,62.05,3580.95,1198.8,1755.35,3772.5,2877.95,357.7,1397.3,959.9,3182.95,3704.15,4620.4,8312.75,541.9,929.3,971.3,1285.05,1521.2,3389.25,1198.05,3414.65,162.45,754.0,467.15,216.9,373.0,245.2,481.1,302.75,1728.2,2964.05,2156.25,60.1,1051.9,78.95,5013.0,1738.9,2275.1,5511.65,98.5,1316.9,7993.3,19.85,1388.45,666.0,94.45,244.65,4134.7,2375.4,4862.5,2431.35,351.5,106.2,1413.0,1743.05,657.5,1050.5,426.35,4982.5,19.55,1451.9,7634.8,235.05,3116.15,71.1,2633.4,49.65,6979.8,4920.8,46.35,1021.8,5276.1,8289.2,2659.45,196.75,94.0,5824.75,1517.5,3479.05,7365.7,1331.05,1195.95,3946.9,4299.95,20.3,1424.95,193.6,620.55,4391.25,1993.8,2597.6,4213.9,19.9,5017.7,1052.35,4916.4,2959.8,7559.55,53.95,7133.1,1799.3,829.55,1312.15,1597.05,524.35,4191.45,711.95,2201.75,806.95,1620.45,6812.95,1837.9,69.8,7344.45,545.15,1500.25,2283.15,494.05,1376.5,755.4,825.4,488.65,2841.55,486.85,2075.1,5299.65,204.7,1356.3,5042.75,812.4,6605.55,2995.45,505.95,3509.4,6991.9,590.35,2789.7,137.95,1123.15,7953.25,349.8,1067.05,3527.3,3762.0,1248.9,3035.35,4300.8,6065.3,144.95,3233.6,5438.9,1081.45,2308.6,1882.8,3255.35,1067.65,2065.4,4136.4,221.9,3848.0,8022.85,173.15,781.25,4510.8,5317.8,4779.45,250.1,1745.2,74.9,4140.1,3670.5,1167.8,469.65,58.9,333.55,3171.15,74.7,1381.2,455.5,3645.5,1158.85,6954.15,1672.1,3152.5,4307.1,2530.4,6141.65,3186.7,1054.8,5430.65,849.9,151.75,299.4,1086.75,2692.75,1389.6,3767.4,3641.5,2535.55,35.75,6880.85,3753.2,637.55,181.6,5329.55,44.55,1539.8,2209.75,223.75,7751.7,1174.35,4385.05,2606.35,1539.45,18.9,1261.35,58.15,225.6,5969.3,253.9,400.0,340.85,2515.3,227.35,305.95,365.4,2357.75,198.5,554.25,90.85,69.4,742.95,251.65,5597.65,20.15,4816.7,768.05,1215.8,3522.65,1834.95,305.1,910.45,155.65,3656.25,52.0,150.85,389.25,1873.7,1261.0,108.7,7346.2,3708.4,469.65,44.1,1385.85,548.8,712.75,6405.0,1637.4,2536.55,6042.7,958.25,1730.65,459.6,201.95,285.2,6145.85,1529.45,4664.5,1740.7,552.95,3496.3,93.7,7053.35,301.55,312.7,1046.5,2960.1,834.2,6953.4,4134.85,899.8,541.5,116.85,7942.15,5321.25,4156.8,223.6,768.45,3765.05,2157.5,108.05,1391.65,1379.8,1273.3,810.45,1810.85,7782.85,70.3,1797.1,377.85,445.3,851.75,7624.2,355.1,575.45,906.85,1057.55,19.4,1388.75,1641.3,1375.15,152.7,185.2,195.05,1826.7,417.5,344.5,1660.0,2180.55,2835.9,45.15,2730.85,5437.1,20.2,6981.35,5794.45,747.2,1267.95,3674.95,1275.6,5893.9,724.65,1536.75,3615.6,607.3,4154.55,2184.35,1852.85,77.15,847.8,2390.45,1923.5,1493.2,338.1,3030.6,75.35,2184.85,1031.4,921.3,3875.4,3124.5,144.15,6689.0,1451.1,6368.2,3729.6,350.1,2847.2,452.35,1494.5,930.45,41.85,1272.05,475.1,673.1,208.7,150.75,3199.0,5844.65,2462.6,6263.8,3403.4,237.75,221.35,75.3,1672.1,7881.2,2320.8,370.5,4062.2,3043.7,2723.4,1081.25,4097.05,632.2,4042.3,164.85,8166.8,4113.7,3047.15,70.4,4193.4,3729.75,399.45,331.3,1964.6,1336.15,1147.45,486.05,1078.9,5925.75,7804.15,4747.2,1028.9,226.45,4364.1,4692.55,2433.9,1217.25,19.75,154.3,6382.55,7623.2,1261.45,89.25,86.05,6779.05,4345.0,82.85,1714.55,893.2,810.85,6347.55,1123.65,43.15,35.1,1388.0,3851.45,1743.5,2638.1,946.95,1114.85,1777.6,684.85,74.35,1312.45,159.2,610.2,404.35,69.65,6363.45,4124.65,713.1,950.75,19.65,505.95,1149.65,169.65,526.7,393.15,1147.0,3021.6,5718.2,191.35,4539.6,6397.6,280.4,2718.3,5711.05,3478.15,36.45,4133.95,2917.65,54.3,2964.8,2198.3,1189.9,1914.5,2001.5,5336.35,7238.6,7998.8,3825.85,5703.25,7397.0,164.6,6555.2,225.65,955.75,3382.3,2313.8,933.3,780.85,7852.4,3184.25,764.95,2763.0,1614.7,145.4,156.25,1604.5,270.15,1397.475,20.6,1734.5,7047.5,573.15,1538.6,4459.8,7459.0,306.05,639.7,348.15,4533.9,5563.65,1587.55,40.25,842.9,4228.55,784.45,3729.75,1406.9,1857.3,7322.5,6725.5,3627.3,1761.45,1725.95,4192.15,1411.2,164.5,2724.6,264.55,4671.7,1173.55,318.5,7713.55,2471.25,7842.3,2227.8,4990.25,3744.05,7220.35,2351.45,2989.6,6077.75,4070.95,2479.05,2134.3,6000.1,2203.1,183.15,6194.1,638.95,2139.2,831.75,521.3,1337.5,7181.95,608.0,2560.1,577.6,5953.0,1790.15,2531.4,4760.3,292.85,953.65,198.0,5705.05,609.9,20.55,79.65,6993.65,4122.65,5762.95,1537.85,2603.3,5566.4,5431.9,2258.25,1092.35,6401.25,2812.2,80.05,2698.35,616.9,1912.85,849.1,3460.3,1465.75,90.1,5555.3,1278.8,2907.35,146.3,51.25,4952.95,4504.9,45.6,4520.15,267.35,2316.85,8075.35,2302.35,7251.9,2078.55,6225.4,1242.25,99.45,288.05,599.25,3462.1,853.0,934.1,2375.2,2813.05,1222.8,5293.2,6314.35,19.1,1003.05,1593.1,2509.95,3187.65,74.45,2345.2,3330.1,5522.7,2335.3,4798.4,8594.4,970.55,7665.8,3686.05,1115.15,2537.0,1393.6,89.35,4445.3,978.6,1258.15,331.35,291.4,453.75,84.75,1715.1,1715.15,1597.25,1250.1,3996.8,5215.1,235.5,673.2,1442.2,5639.05,5222.35,7291.75,799.65,50.8,424.5,249.4,4415.75,5270.6,144.55,2447.95,2884.9,3050.15,253.0,4517.25,436.2,118.4,19.9,3649.6,1200.15,255.6,2395.05,70.6,5083.55,389.6,644.5,523.5,39.3,989.05,2406.1,638.55,191.1,4524.05,4664.2,3470.8,6910.3,4014.2,1288.0,2277.65,6375.8,24.8,5251.75,1505.45,6903.1,913.3,5535.8,815.55,1411.65,5602.25,8244.3,741.5,4375.8,1008.7,5968.4,3096.9,3901.25,2613.4,853.1,5661.7,794.25,695.05,160.8,5552.05,3275.15,4236.6,54.5,1174.35,741.7,3019.25,962.9,4759.75,1498.35,1233.15,4882.8,5411.65,19.65,148.05,3930.55,6895.5,84.65,6856.95,2658.8,3766.2,69.55,129.55,347.25,581.7,354.05,929.45,679.0,3846.75,4480.7,44.35,499.4,1553.2,219.5,5311.85,586.05,2576.8,6435.25,1993.25,1923.85,939.8,84.6,3092.65,415.55,5986.55,5487.0,651.5,45.4,73.65,2405.05,4458.15,6856.45,50.75,4735.2,682.15,4525.8,321.05,335.65,1424.4,1748.55,474.9,668.4,926.25,1077.95,2541.25,46.0,1156.1,3694.45,382.8,1167.8,746.75,3912.9,368.85,20.2,1654.7,1940.85,221.7,1794.65,5289.8,501.2,6140.85,48.45,309.25,201.1,6179.35,2838.7,55.7,4589.85,3735.45,70.15,477.55,2553.7,1342.15,1216.35,7578.05,2603.95,42.7,6056.9,2983.65,689.35,2025.1,1747.2,4657.95,296.1,8016.6,81.1,433.5,20.55,6428.4,5883.85,1043.8,6252.9,1857.25,146.4,240.45,1724.15,315.3,429.55,356.15,488.25,55.7,1298.7,1378.75,973.45,384.25,8543.25,389.95,5731.85,20.65,4275.75,84.5,1924.1,669.85,5784.3,5166.2,4060.55,267.4,3532.85,4914.8,5315.8,69.85,775.6,5445.95,53.55,1007.8,833.55,6579.05,1004.5,7856.0,1183.05,2169.75,896.9,19.3,501.35,4126.35,49.65,2460.15,477.6,370.65,265.75,2424.45,1849.95,61.05,1698.55,1910.6,998.1,890.6,529.5,1383.6,493.4,48.6,1207.0,563.5,864.55,2109.35,701.05,2265.25,220.6,5749.8,2796.45,5165.7,5696.6,20.2,2076.05,44.2,215.25,1859.5,7470.1,229.5,2470.1,2092.9,5629.55,469.85,733.95,485.25,1244.5,20.05,3994.45,78.25,1131.5,184.05,19.9,1178.75,667.7,5629.15,19.95,109.3,79.6,958.45,403.15,75.9,76.2,4392.5,19.25,3168.0,1096.6,669.45,2343.85,3588.4,48.6,522.95,7511.9,1725.0,49.75,1691.9,2248.05,4479.2,3471.1,63.0,70.25,3314.15,865.55,852.7,1930.9,91.4,3949.15,4304.0,409.9,1554.9,3472.05,117.8,3210.35,207.4,19.75,3132.75,60.0,649.65,20.85,2106.3,35.0,2011.4,6205.5,903.7,503.6,3882.3,879.8,383.65,4390.25,68.2,452.8,5329.0,1758.9,5737.6,1152.7,7674.55,2157.95,1219.85,2414.55,1155.6,7548.1,1809.35,1564.4,19.65,75.7,84.4,864.85,228.75,7752.05,1951.0,470.6,4060.9,384.5,3432.9,108.65,3952.65,463.05,494.05,3416.85,1498.65,2867.75,4807.35,71.35,471.35,1041.8,7689.8,1132.35,1815.0,1311.3,199.45,1637.3,1326.35,6376.55,935.9,20.05,1611.15,20.6,6989.7,2779.5,1931.75,8331.95,735.9,2283.3,1445.2,818.05,2333.05,1992.55,5890.0,916.75,1029.8,1796.55,33.7,454.05,1392.25,1049.6,734.6,475.1,70.15,1540.05,1978.65,3181.8,229.7,5625.55,6581.9,1347.15,3808.2,3974.15,7661.8,2479.25,266.6,5601.4,1982.6,339.9,4828.05,5980.55,4808.7,20.3,3692.85,1068.15,2383.6,69.65,89.3,1438.05,917.45,6096.45,3078.1,99.95,475.2,2036.55,6444.05,1426.75,767.55,7932.5,4040.65,2768.65,1672.35,474.8,446.1,1345.65,2425.4,2077.95,416.3,1663.5,1286.05,35.85,1094.35,7719.5,514.6,1451.6,4932.5,71.0,5443.65,330.25,746.5,122.7,44.4,6631.85,8250.0,3561.15,1763.55,2419.0,470.2,5234.95,70.3,79.35,3097.0,1709.1,1848.8,723.3,809.75,1470.95,577.15,6914.95,602.9,124.4,96.8,3827.9,533.05,2961.4,3264.45,995.35,2815.25,154.8,246.3,257.6,5757.2,7517.7,1234.8,1389.2,3836.3,1474.75,6001.45,720.1,2832.75,472.25,1460.65,1336.35,6388.65,153.05,677.9,1165.6,2119.5,921.55,72.0,68.35,847.25,44.0,4295.35,135.0,1400.55,69.65,1380.4,1060.2,4386.2,86.0,5785.5,52.05,2018.4,79.45,2727.3,263.65,275.4,2816.65,538.5,948.9,846.8,4783.5,2997.45,5897.4,470.6,524.5,269.65,4740.0,2341.5,1789.65,1626.4,800.3,5060.85,1448.8,4254.85,466.6,4627.8,6838.6,85.0,1101.85,44.3,20.2,4297.6,414.1,256.6,3969.35,2274.1,1296.8,1268.85,8192.6,59.25,1147.85,4361.55,2006.1,2078.95,7609.75,487.05,1218.45,1416.2,483.3,1234.6,1311.6,5618.3,6185.15,1237.85,498.1,294.45,2381.55,788.35,756.4,613.4,3625.2,550.35,4793.8,1267.2,442.2,84.85,654.85,5567.45,3160.55,740.3,5706.2,3085.35,24.7,3541.4,84.2,50.7,1165.55,4855.35,2806.9,1422.65,608.15,641.35,4959.15,35.9,1321.3,1663.75,3024.15,2188.5,4138.05,102.5,872.65,2724.25,413.25,1374.2,757.1,6692.65,218.5,608.8,1004.75,1125.6,3847.6,978.0,1387.35,746.05,304.6,1098.85,1139.2,4368.95,827.7,20.35,262.05,279.55,3512.5,1483.25,4653.85,151.75,4400.75,8033.1,1253.65,552.65,1036.0,4025.5,928.4,24.9,460.25,6506.15,5598.3,4374.55,678.75,2191.7,239.75,5485.5,609.05,683.75,404.2,5189.75,5060.9,233.65,7176.55,291.45,689.75,3263.9,1993.2,254.5,347.65,967.9,242.95,1841.9,232.1,809.25,866.45,360.55,2076.2,355.9,7299.65,2291.2,129.15,7491.75,5835.5,7031.45,7723.9,1032.0,70.75,109.6,727.8,130.75,893.0,763.1,781.4,902.25,2653.65,1016.7,5943.65,275.7,3126.45,1070.15,3457.9,340.4,4299.2,1093.2,521.9,1630.4,713.6,1265.65,4003.85,1401.4,45.3,1510.5,178.1,264.8,480.6,4541.9,4976.15,7542.25,251.25,1595.5,23.45,685.55,3874.1,6891.4,4916.95,1373.0,29.7,49.8,6039.9,1548.65,3440.25,151.3,4226.7,1023.75,55.4,90.6,7074.4,156.35,6849.75,3263.6,1252.85,67.55,70.2,5750.0,1549.75,1810.55,2952.85,7467.55,5484.4,294.9,97.5,5916.45,113.35,19.95,476.8,101.65,1130.85,3532.25,6891.45,1570.7,520.95,7854.9,3581.4,1447.9,8547.15,727.85,357.6,159.35,280.85,6069.25,3274.35,1359.0,1777.9,6109.65,1122.4,2020.9,3795.45,4504.55,3579.15,49.7,276.5,8175.9,890.35,259.8,5588.8,50.6,58.0,5568.35,2203.65,938.95,2024.1,3623.95,2369.05,3877.95,4577.9,25.85,2871.5,672.55,1573.7,1270.55,7711.45,126.05,2193.65,64.55,31.2,38.25,3348.1,533.6,2907.55,1620.8,3480.35,293.85,3243.45,4137.2,79.25,44.9,19.4,5348.65,659.65,3180.5,118.5,3023.85,3365.85,195.3,1031.1,997.65,6471.85,6241.35,6501.35,2317.1,2237.55,5231.3,5275.8,165.5,358.5,6614.9,80.5,2421.75,1294.6,1299.1,5733.4,305.55,7430.75,900.5,630.6,3856.75,1222.65,374.8,1625.0,7234.8,331.85,3959.35,5088.4,3969.4,4860.85,788.05,1266.35,470.95,688.2,387.7,845.25,560.6,4230.25,4983.05,4113.15,25.7,521.0,471.85,5976.9,506.9,4299.75,7548.6,1654.75,80.35,4551.5,6227.5,20.05,76.4,514.0,57.5,1474.35,880.2,3270.25,69.95,3919.15,7285.7,93.3,169.05,7658.3,5959.3,5295.7,4693.2,3512.9,5231.2,3603.45,217.45,6875.35,19.25,2021.35,2564.95,5611.75,7610.1,35.05,73.0,45.75,4543.95,450.4,1297.35,4442.75,1423.15,4378.35,74.2,2716.3,70.1,450.65,1175.85,558.8,2220.1,3283.05,142.35,4939.25,4237.5,335.95,33.2,7188.5,926.25,1119.35,116.6,68.8,287.4,2341.55,1362.85,163.6,2254.2,145.15,7752.3,6585.35,4786.1,3835.55,397.0,19.7,1027.25,1580.1,7222.75,3769.7,4233.95,1680.25,3725.5,413.65,7707.7,714.15,2497.2,8309.55,601.55,139.25,1888.25,2773.9,5409.75,5643.4,197.7,401.3,3238.4,1573.05,79.15,235.8,1364.75,1985.15,655.85,303.15,4335.2,647.5,1574.45,2748.7,2483.65,6367.2,4904.2,2044.95,1794.8,7173.15,6441.4,7039.05,921.4,4018.35,5448.6,20.25,49.9,2085.45,1358.85,5135.15,730.1,2869.85,118.25,49.25,1108.6,1815.65,730.4,75.45,5071.05,4014.6,568.85,5860.7,7279.35,1064.95,5769.6,5886.85,4238.45,20.3,418.3,136.05,708.2,788.55,700.85,4378.8,3442.8,181.7,7171.7,4016.85,553.0,96.85,4122.9,1482.3,74.3,1240.8,44.65,1095.3,788.8,6578.55,19.55,2802.3,857.75,184.4,364.55,6093.3,1861.1,20.4,1079.65,7475.1,2862.55,3069.45,2349.8,469.25,4213.35,3301.05,3529.95,7723.7,4144.9,4265.0,154.65,3246.45,8337.45,5731.4,6176.6,1905.4,931.9,7348.8,776.25,5243.05,141.65,7679.65,2954.5,1006.9,2540.1,3097.2,3807.35,2484.0,5785.65,2619.15,1524.85,2790.65,1784.5,3937.45,2276.1,2029.05,802.35,418.4,4653.25,275.9,343.45,2130.45,1191.4,50.5,2554.0,5589.45,467.7,74.4,3756.45,3334.9,920.5,3431.75,150.35,2587.7,367.95,5125.75,4801.1,6219.6,19.55,550.1,7862.25,1242.2,4871.05,3190.65,2666.75,3533.6,792.15,295.65,1459.35,4398.15,4297.95,167.3,4096.9,3454.6,1286.0,1387.0,786.3,641.25,705.45,345.5,345.9,5811.8,94.4,124.45,1375.6,3491.55,7920.7,6640.7,55.0,718.1,320.4,101.45,1334.45,3358.65,4764.0,350.3,5703.0,69.5,18.85,525.55,69.2,483.15,934.8,786.5,69.65,4086.3,5364.8,82.3,20.3,770.4,6816.95,2419.55,4138.7,267.6,3457.45,1115.2,5435.6,4186.3,25.05,4630.2,3221.25,688.5,867.1,4746.05,712.85,48.95,355.1,45.3,5764.7,167.3,428.45,1505.35,693.45,930.4,1177.95,5069.65,324.25,1458.1,156.4,2298.9,1679.65,369.6,2998.0,5206.55,3626.1,24.4,7968.85,152.45,1959.5,2053.05,398.55,1130.85,3425.35,4323.45,703.55,1275.7,2965.75,1647.0,56.25,2249.95,6109.75,159.4,3751.15,68.65,411.25,105.5,3320.6,327.45,5669.5,465.45,704.3,1369.8,1107.25,95.55,6375.2,8152.3,1566.75,130.25,162.15,110.05,1885.15,6302.85,2264.5,816.8,1253.5,41.2,5265.55,693.3,5997.1,3824.2,3886.05,7283.25,412.55,1070.25,817.95,1171.3,6548.65,625.65,1911.5,70.85,134.7,1507.0,2978.3,299.3,5832.65,5567.55,450.8,4166.35,1215.6,91.7,7898.45,3915.4,979.5,90.05,7432.05,4026.4,25.3,1193.05,20.1,398.55,1958.45,69.9,63.15,1301.1,484.05,4528.0,3887.85,2208.75,238.15,80.8,958.15,118.3,76.45,3845.45,1094.5,573.75,1267.0,633.45,6129.65,1218.25,1405.3,2274.9,74.5,1068.85,533.9,676.15,3804.4,1118.8,5236.4,1386.8,762.25,1902.0,239.05,5673.7,39.8,997.75,5574.35,406.05,138.85,123.65,1801.1,689.0,790.7,582.5,1618.2,1173.35,900.9,2122.05,6719.9,69.15,3784.0,1798.65,54.65,3886.45,1224.05,2310.2,723.4,3988.5,3554.6,1397.475,6034.85,531.6,85.1,173.0,2511.95,3893.6,357.15,467.5,2288.7,4627.85,289.1,6460.55,1931.3,402.6,2221.55,7758.9,172.85,224.5,7388.45,3460.95,1700.9,3090.65,1293.8,645.8,5224.95,500.1,2427.1,3488.15,1035.7,7565.35,2799.0,1601.5,85.5,6256.2,1232.9,19.9,1937.4,1096.25,5059.75,3023.65,4889.2,2289.9,6503.2,1313.25,990.3,228.0,5746.15,209.1,1864.2,5979.7,3902.45,7142.5,902.0,4481.0,805.2,154.85,528.45,8349.7,4953.25,332.65,470.2,2259.35,1411.35,593.75,6328.7,1411.9,6841.4,20.35,238.5,3233.85,1062.1,4016.3,226.55,7110.75,5440.9,235.1,1958.95,85.0,5528.9,1463.7,1025.05,552.1,3815.4,1397.475,3313.4,1938.9,3014.65,460.25,4839.15,184.4,19.9,2010.55,5139.65,69.2,1673.4,309.35,3171.6,8670.1,916.0,299.75,702.05,2354.8,3473.4,19.65,4438.2,4819.75,92.25,1567.0,1242.45,559.2,220.1,531.15,1183.2,465.85,6876.05,501.0,3782.4,460.2,20.2,1790.35,733.35,1334.0,7767.25,876.15,4600.95,113.55,1793.25,886.7,7737.55,1348.9,1686.85,1879.25,4013.8,434.5,7195.35,780.1,107.6,3801.7,308.7,438.25,50.35,3778.1,3147.5,5438.95,5102.35,70.3,1872.2,213.35,5617.75,5386.5,1776.95,2483.05,235.65,5224.35,2272.8,83.75,4663.4,201.7,125.0,684.4,620.35,1146.05,1806.35,603.0,5798.3,519.15,497.6,1301.7,1129.75,19.3,266.95,257.0,865.8,2623.65,45.85,79.55,1082.8,147.8,2570.2,4378.9,3616.25,2924.05,6014.85,32.7,2882.25,1509.9,5305.05,2368.4,7985.9,3545.35,1301.0,372.45,2985.25,77.75,564.35,95.45,1311.75,1135.7,2319.8,3720.35,5025.8,5224.5,6185.8,1498.55,1208.6,6613.65,69.7,573.3,1818.9,1787.35,1051.05,7181.25,3688.6,99.75,871.4,780.5,821.6,239.45,244.45,3357.9,129.6,4977.2,365.35,334.65,2424.05,43.95,4981.15,2090.25,45.1,4905.75,2038.7,4014.0,2441.7,2751.0,1307.8,383.65,2868.05,449.75,53.55,7882.5,1087.7,791.7,7493.05,4414.3,6841.3,819.95,6052.25,3361.05,4869.5,509.3,4308.25,221.1,3833.95,69.35,294.95,4092.85,316.9,2651.2,471.7,216.75,5720.95,503.25,69.6,7365.3,7245.9,385.0,961.4,4615.9,3251.3,3880.05,3088.75,1396.0,30.5,53.05,6859.5,2108.35,6770.5,4730.6,1151.05,232.55,1022.6,5121.3,1127.2,309.4,523.1,4250.1,770.5,246.7,3342.0,3930.6,1747.85,2754.45,897.75,2780.6,5895.45,2208.05,2196.15,1692.6,20.5,265.35,1836.25,6418.9,4871.45,4947.55,1558.65,4284.2,1218.25,5617.95,24.2,679.85,554.45,5237.4,2032.3,789.2,1525.35,2804.45,3726.15,1652.1,1588.75,3366.05,778.1,7113.75,4367.35,993.15,5012.35,2728.6,2093.4,1011.8,106.85,1343.4,130.1,6794.75,1022.5,3691.2,486.2,4036.85,4685.55,256.25,1917.1,74.45,272.15,5150.55,3756.4,3645.75,2874.45,49.95,1020.75,70.65,826.0,239.0,727.8,7544.3,6479.4,3626.35,1679.4,403.35,931.55,4326.25,263.05,39.25,3316.1,75.75,2625.25,6886.25,1495.1,743.3,1419.4,1990.5,7362.9,346.45,306.6,6844.5],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Total"},"showgrid":false,"showline":true,"showticklabels":true,"zeroline":false},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":""},"showgrid":false,"showline":false,"showticklabels":false,"zeroline":false},"legend":{"tracegroupgap":0},"margin":{"t":60},"boxmode":"group","title":{"text":"\u003cb\u003eBox Plot untuk totalcharges\u003c\u002fb\u003e"}},                        {"responsive": true}                    ).then(function(){
                            
var gd = document.getElementById('08229dfe-9cbe-44df-a09d-c786eaed176a');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>
```
:::
:::

::: {.cell .markdown id="rNPmDFK8c3uz"}
Dalam fitur numerik pada dataset ini tidak ada outlier
:::

::: {.cell .markdown id="E7jKMKEP2LNJ"}
## **Hitung Data Kategorik**
:::

::: {.cell .code execution_count="110" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="LMAV6pat2GH2" outputId="4d8438b5-d6a9-4130-9152-b5f380409a4a"}
``` python
object_col = raw_data.select_dtypes(include = 'object').columns

for col in object_col:
    print(f"\033[1;33m{col}\033[0m\n{raw_data[col].unique()}\n")
```

::: {.output .stream .stdout}
    customerid
    ['7590-VHVEG' '5575-GNVDE' '3668-QPYBK' ... '4801-JZAZL' '8361-LTMKD'
     '3186-AJIEK']

    gender
    ['Female' 'Male']

    seniorcitizen
    [0 1]

    partner
    ['Yes' 'No']

    dependents
    ['No' 'Yes']

    phoneservice
    ['No' 'Yes']

    multiplelines
    ['No phone service' 'No' 'Yes']

    internetservice
    ['DSL' 'Fiber optic' 'No']

    onlinesecurity
    ['No' 'Yes' 'No internet service']

    onlinebackup
    ['Yes' 'No' 'No internet service']

    deviceprotection
    ['No' 'Yes' 'No internet service']

    techsupport
    ['No' 'Yes' 'No internet service']

    streamingtv
    ['No' 'Yes' 'No internet service']

    streamingmovies
    ['No' 'Yes' 'No internet service']

    contract
    ['Month-to-month' 'One year' 'Two year']

    paperlessbilling
    ['Yes' 'No']

    paymentmethod
    ['Electronic check' 'Mailed check' 'Bank transfer (automatic)'
     'Credit card (automatic)']

    churn
    ['No' 'Yes']
:::
:::

::: {.cell .code execution_count="111" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="fzQfARvJ3Gm9" outputId="0b393703-8cdf-4a68-e464-2a69e6100d6b"}
``` python
raw_data = raw_data.replace('No internet service', 'No')
raw_data = raw_data.replace('No phone service', 'No')

for col in object_col:
    print(f"\033[1;33m{col}\033[0m\n{raw_data[col].unique()}\n")
```

::: {.output .stream .stdout}
    customerid
    ['7590-VHVEG' '5575-GNVDE' '3668-QPYBK' ... '4801-JZAZL' '8361-LTMKD'
     '3186-AJIEK']

    gender
    ['Female' 'Male']

    seniorcitizen
    [0 1]

    partner
    ['Yes' 'No']

    dependents
    ['No' 'Yes']

    phoneservice
    ['No' 'Yes']

    multiplelines
    ['No' 'Yes']

    internetservice
    ['DSL' 'Fiber optic' 'No']

    onlinesecurity
    ['No' 'Yes']

    onlinebackup
    ['Yes' 'No']

    deviceprotection
    ['No' 'Yes']

    techsupport
    ['No' 'Yes']

    streamingtv
    ['No' 'Yes']

    streamingmovies
    ['No' 'Yes']

    contract
    ['Month-to-month' 'One year' 'Two year']

    paperlessbilling
    ['Yes' 'No']

    paymentmethod
    ['Electronic check' 'Mailed check' 'Bank transfer (automatic)'
     'Credit card (automatic)']

    churn
    ['No' 'Yes']
:::
:::

::: {.cell .markdown id="e-Gvi7rcLP1d"}
-   Nilai No internet service dan No phone service dalam DataFrame
    raw_data diganti dengan No untuk menyederhanakan kategori dan
    menghindari kebingungan dalam analisis.
:::

::: {.cell .code execution_count="112" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="Z3rMhDkq3b3y" outputId="88483fb1-008f-4dae-c527-a9be4c6651a9"}
``` python
for col in object_col:
    print(f"\033[1;33m{col}\033[0m\n")
    print(raw_data[col].value_counts(), '\n')
```

::: {.output .stream .stdout}
    customerid

    customerid
    7590-VHVEG    1
    3791-LGQCY    1
    6008-NAIXK    1
    5956-YHHRX    1
    5365-LLFYV    1
                 ..
    9796-MVYXX    1
    2637-FKFSY    1
    1552-AAGRX    1
    4304-TSPVK    1
    3186-AJIEK    1
    Name: count, Length: 7043, dtype: int64 

    gender

    gender
    Male      3555
    Female    3488
    Name: count, dtype: int64 

    seniorcitizen

    seniorcitizen
    0    5901
    1    1142
    Name: count, dtype: int64 

    partner

    partner
    No     3641
    Yes    3402
    Name: count, dtype: int64 

    dependents

    dependents
    No     4933
    Yes    2110
    Name: count, dtype: int64 

    phoneservice

    phoneservice
    Yes    6361
    No      682
    Name: count, dtype: int64 

    multiplelines

    multiplelines
    No     4072
    Yes    2971
    Name: count, dtype: int64 

    internetservice

    internetservice
    Fiber optic    3096
    DSL            2421
    No             1526
    Name: count, dtype: int64 

    onlinesecurity

    onlinesecurity
    No     5024
    Yes    2019
    Name: count, dtype: int64 

    onlinebackup

    onlinebackup
    No     4614
    Yes    2429
    Name: count, dtype: int64 

    deviceprotection

    deviceprotection
    No     4621
    Yes    2422
    Name: count, dtype: int64 

    techsupport

    techsupport
    No     4999
    Yes    2044
    Name: count, dtype: int64 

    streamingtv

    streamingtv
    No     4336
    Yes    2707
    Name: count, dtype: int64 

    streamingmovies

    streamingmovies
    No     4311
    Yes    2732
    Name: count, dtype: int64 

    contract

    contract
    Month-to-month    3875
    Two year          1695
    One year          1473
    Name: count, dtype: int64 

    paperlessbilling

    paperlessbilling
    Yes    4171
    No     2872
    Name: count, dtype: int64 

    paymentmethod

    paymentmethod
    Electronic check             2365
    Mailed check                 1612
    Bank transfer (automatic)    1544
    Credit card (automatic)      1522
    Name: count, dtype: int64 

    churn

    churn
    No     5174
    Yes    1869
    Name: count, dtype: int64 
:::
:::

::: {.cell .markdown id="f8yh0HmoL3JU"}
Melihat jumlah distribusi per kolom / fitur
:::

::: {.cell .code execution_count="113" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="H0kV8GVd3uae" outputId="6b732671-4d4e-41c6-9bf2-057873830f9c"}
``` python
data_churn = raw_data[raw_data['churn'] == 'Yes'].reset_index(drop = True)

for col in object_col:
    print(f"\033[1;33m{col}\033[0m\n")
    print(data_churn[col].value_counts(), '\n')
```

::: {.output .stream .stdout}
    customerid

    customerid
    3668-QPYBK    1
    6633-MPWBS    1
    6532-YLWSI    1
    7067-KSAZT    1
    0511-JTEOY    1
                 ..
    0235-KGSLC    1
    1846-XWOQN    1
    5103-MHMHY    1
    4094-NSEDU    1
    8361-LTMKD    1
    Name: count, Length: 1869, dtype: int64 

    gender

    gender
    Female    939
    Male      930
    Name: count, dtype: int64 

    seniorcitizen

    seniorcitizen
    0    1393
    1     476
    Name: count, dtype: int64 

    partner

    partner
    No     1200
    Yes     669
    Name: count, dtype: int64 

    dependents

    dependents
    No     1543
    Yes     326
    Name: count, dtype: int64 

    phoneservice

    phoneservice
    Yes    1699
    No      170
    Name: count, dtype: int64 

    multiplelines

    multiplelines
    No     1019
    Yes     850
    Name: count, dtype: int64 

    internetservice

    internetservice
    Fiber optic    1297
    DSL             459
    No              113
    Name: count, dtype: int64 

    onlinesecurity

    onlinesecurity
    No     1574
    Yes     295
    Name: count, dtype: int64 

    onlinebackup

    onlinebackup
    No     1346
    Yes     523
    Name: count, dtype: int64 

    deviceprotection

    deviceprotection
    No     1324
    Yes     545
    Name: count, dtype: int64 

    techsupport

    techsupport
    No     1559
    Yes     310
    Name: count, dtype: int64 

    streamingtv

    streamingtv
    No     1055
    Yes     814
    Name: count, dtype: int64 

    streamingmovies

    streamingmovies
    No     1051
    Yes     818
    Name: count, dtype: int64 

    contract

    contract
    Month-to-month    1655
    One year           166
    Two year            48
    Name: count, dtype: int64 

    paperlessbilling

    paperlessbilling
    Yes    1400
    No      469
    Name: count, dtype: int64 

    paymentmethod

    paymentmethod
    Electronic check             1071
    Mailed check                  308
    Bank transfer (automatic)     258
    Credit card (automatic)       232
    Name: count, dtype: int64 

    churn

    churn
    Yes    1869
    Name: count, dtype: int64 
:::
:::

::: {.cell .markdown id="DAmGUumgc-2c"}
# **📌 Feature Engineering** {#-feature-engineering}
:::

::: {.cell .markdown id="oMz8a7Qx_kLS"}
## **✨Fitur Tambahan**
:::

::: {.cell .code execution_count="114" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":618}" id="np-SJ-Aoc91O" outputId="a530b896-5457-43d5-8f6e-1052e756801d"}
``` python
raw_data
```

::: {.output .execute_result execution_count="114"}
``` json
{"type":"dataframe","variable_name":"raw_data"}
```
:::
:::

::: {.cell .code execution_count="115" id="qwZLZgfydHDJ"}
``` python
# Menambahkan fitur baru tenure_range
def categorize_tenure(tenure):
    if tenure <= 12:
        return '0-12 bulan'
    elif tenure <= 24:
        return '13-24 bulan'
    elif tenure <= 36:
        return '25-36 bulan'
    elif tenure <= 48:
        return '37-48 bulan'
    else:
        return '49 bulan ke atas'

raw_data['tenure_range'] = raw_data['tenure'].apply(categorize_tenure)
```
:::

::: {.cell .markdown id="W8oi1umVdLce"}
-   Lama Berlangganan dalam Rentang (tenure_range)

fitur pada tenure ke dalam rentang waktu (misalnya, 0-12 bulan, 13-24
bulan, dsb.), dengan alasan untuk membantu model memahami risiko churn
berdasarkan periode langganan pelanggan. Pelanggan baru atau yang baru
saja berganti kontrak mungkin memiliki risiko churn yang lebih tinggi.
:::

::: {.cell .code execution_count="116" id="BUFuXuJRdN4o"}
``` python
# Menambahkan fitur baru tenure_monthly_charge_interaction
raw_data['tenure_monthly_charge_interaction'] = raw_data['tenure'] * raw_data['monthlycharges']
```
:::

::: {.cell .markdown id="v9rAZTlzdPcp"}
-   Interaksi antara monthlycharges dan tenure
    (tenure_monthly_charge_interaction)

Fitur ini dapat menunjukkan bagaimana biaya bulanan berinteraksi dengan
lama berlangganan untuk mempengaruhi churn, dengan alasan dapat
menambahkan hubungakn yang komleks melalui fitur interaksi untuk
meningkatkan kemampuan prediksi model.
:::

::: {.cell .code execution_count="117" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":618}" id="Mx2BGg59dSm3" outputId="2d881f44-509a-4dbe-83a4-f623907a1509"}
``` python
# Menampilkan hasil penambahan fitur
raw_data
```

::: {.output .execute_result execution_count="117"}
``` json
{"type":"dataframe","variable_name":"raw_data"}
```
:::
:::

::: {.cell .markdown id="ErzK3UEM5F46"}
# **📌Data Pre-processing**
:::

::: {.cell .markdown id="f7YM9q5c5UkW"}
## **Encoding**
:::

::: {.cell .code execution_count="118" id="_Peuo2IE6De_"}
``` python
data = raw_data.copy()

data = data.drop(columns = ['customerid'])
```
:::

::: {.cell .code execution_count="119" id="iPeiKIdZ5UO4"}
``` python
data['partner'] = data['partner'].map({'Yes': 0, 'No': 1})
data['internetservice'] = data['internetservice'].map({'Fiber optic': 2, 'DSL': 1, 'No' : 0})
```
:::

::: {.cell .code execution_count="120" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":226}" id="lGfr9cA_6oEr" outputId="0b4c43d2-c727-4d72-cae7-897b2e8a6bd4"}
``` python
from sklearn.preprocessing import LabelEncoder

# Membuat objek LabelEncoder
label_encoder = LabelEncoder()

object_col = data.select_dtypes(include = 'object').columns

for col in object_col:
    data[col] = label_encoder.fit_transform(data[col])

data.head()
```

::: {.output .execute_result execution_count="120"}
``` json
{"type":"dataframe","variable_name":"data"}
```
:::
:::

::: {.cell .markdown id="MsJOyJJHMyJm"}
LabelEncoder mengubah nilai-nilai kategorikal (misalnya, \"Yes\",
\"No\", nama-nama layanan internet) menjadi angka (misalnya, 0, 1, 2).
Ini memungkinkan algoritma untuk memproses dan menggunakan informasi
dari kolom-kolom kategorikal tersebut dalam proses pelatihan model.
:::

::: {.cell .markdown id="l5Z5s_fpdbE3"}
## **Korelasi**
:::

::: {.cell .code execution_count="121" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":1000}" id="wT12DpijdamW" outputId="463037b5-037e-4c2b-fec5-e57308140229"}
``` python
import matplotlib.pyplot as plt
import seaborn as sns

#Menghitung matriks korelasi
correlation_matrix = data.corr()

# Membuat heatmap untuk matriks korelasi
plt.figure(figsize=(14, 10))
sns.heatmap(correlation_matrix, annot=True, fmt=".2f", cmap='coolwarm', square=True, cbar_kws={"shrink": .8})
plt.title('Matriks Korelasi untuk Semua Fitur Setelah Encoding', fontsize=16)
plt.show()
```

::: {.output .display_data}
![](asset/ca58ba4e6b836419684a7a4502e3b5782863790b.png)
:::
:::

::: {.cell .code execution_count="122" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":740}" id="YlB9OCCbNlx4" outputId="dfe83671-7675-4718-b2e8-614d5f5f7ba8"}
``` python
corr = data.corrwith(data["churn"], numeric_only=True)
corr = corr.reset_index(name='corr value')
corr["Corr Type"] = corr["corr value"].apply(lambda x : "Positif" if x >= 0 else "Negatif")


corr["corr value"] = corr["corr value"].apply(lambda x : abs(x))
corr = corr.sort_values('corr value', ascending=False, ignore_index=True)
corr
```

::: {.output .execute_result execution_count="122"}
``` json
{"summary":"{\n  \"name\": \"corr\",\n  \"rows\": 22,\n  \"fields\": [\n    {\n      \"column\": \"index\",\n      \"properties\": {\n        \"dtype\": \"string\",\n        \"num_unique_values\": 22,\n        \"samples\": [\n          \"churn\",\n          \"partner\",\n          \"paperlessbilling\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"corr value\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.20921179937696696,\n        \"min\": 0.008612095078997883,\n        \"max\": 1.0,\n        \"num_unique_values\": 22,\n        \"samples\": [\n          1.0,\n          0.15044754495917667,\n          0.19182533166646867\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Corr Type\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2,\n        \"samples\": [\n          \"Negatif\",\n          \"Positif\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe","variable_name":"corr"}
```
:::
:::

::: {.cell .code execution_count="123" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":489}" id="x2Psl4KIN4Rz" outputId="7352654c-9ba8-44d5-aa6f-aec4e7fcc8b8"}
``` python
# Filter features with correlation above a certain threshol
threshold = 0.15
significant_features = corr[corr['corr value'] > threshold]

significant_features
```

::: {.output .execute_result execution_count="123"}
``` json
{"summary":"{\n  \"name\": \"significant_features\",\n  \"rows\": 14,\n  \"fields\": [\n    {\n      \"column\": \"index\",\n      \"properties\": {\n        \"dtype\": \"string\",\n        \"num_unique_values\": 14,\n        \"samples\": [\n          \"onlinesecurity\",\n          \"dependents\",\n          \"churn\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"corr value\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.22227016313756784,\n        \"min\": 0.15044754495917667,\n        \"max\": 1.0,\n        \"num_unique_values\": 14,\n        \"samples\": [\n          0.1712262919485529,\n          0.16422140157972476,\n          1.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Corr Type\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2,\n        \"samples\": [\n          \"Negatif\",\n          \"Positif\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe","variable_name":"significant_features"}
```
:::
:::

::: {.cell .code execution_count="124" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":300}" id="gIvn3g6nPxfD" outputId="abfa79d4-2610-4f81-f6a6-ec8b693234fa"}
``` python
# Fitur-fitur yang korelasinya tidak signifikan dengan churn (di bawah threshold 0.15)
insignificant_features = corr[corr['corr value'] <= threshold]
insignificant_features
```

::: {.output .execute_result execution_count="124"}
``` json
{"summary":"{\n  \"name\": \"insignificant_features\",\n  \"rows\": 8,\n  \"fields\": [\n    {\n      \"column\": \"index\",\n      \"properties\": {\n        \"dtype\": \"string\",\n        \"num_unique_values\": 8,\n        \"samples\": [\n          \"onlinebackup\",\n          \"multiplelines\",\n          \"paymentmethod\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"corr value\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.033623864386964956,\n        \"min\": 0.008612095078997883,\n        \"max\": 0.10706200587340363,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.08225486893814248,\n          0.04010212769982628,\n          0.10706200587340363\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Corr Type\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2,\n        \"samples\": [\n          \"Negatif\",\n          \"Positif\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe","variable_name":"insignificant_features"}
```
:::
:::

::: {.cell .markdown id="phcFn9Lqd0pf"}
\##**✨ Fitur yang digunakan**
:::

::: {.cell .markdown id="ukw3ANibd21v"}
Berdasarkan heatmap korelasi di atas, ada beberapa fitur yang korelasi
nya signifikan dengan target fitur churn (sesuai dengan nilai treshold
yang ditetapkan sebesar 0.15).

Fitur-fitur ini sebagai berikut :

-   contract
-   tenure_range
-   internetservice
-   totalcharges
-   tenure_monthly_charge_interaction
-   monthlycharges
-   paperlessbilling
-   onlinesecurity
-   techsupport
-   dependents

Fitur yang Bisa Dipertimbangkan

-   seniorcitizen & partner - meskipun korelasi positif kecil, karena
    pelanggan senior dan memiliki pasangan/partner sedikit lebih
    berisiko churn.
-   streamingtv dan streamingmovies - meskipun korelasi sangat kecil
    dipertimbangkan karna dampak layanan hiburan terhadap churn.

fitur yang tidak digunakan

-   paymentmethod
-   onlinebackup
-   deviceprotection
-   multiplelines
-   phoneservice
-   gender
-   tenure (karena sudah digantikan oleh tenure_range menghindari
    multikolinearitas)
:::

::: {.cell .markdown id="ifw35Y9Ci3yx"}
## **Scalling Data**
:::

::: {.cell .code execution_count="125" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":226}" id="K7daBgHIi70U" outputId="cc29eb9b-063c-4f36-f667-e6a984f01a87"}
``` python
from sklearn.preprocessing import RobustScaler

# Misalkan data sudah diproses sebelumnya
data = data.copy()

# Inisialisasi objek RobustScaler
scaler = RobustScaler()

# Fit dan transform data pada dataframe
scaled_values = scaler.fit_transform(data)

# Membuat dataframe baru dengan data yang telah discaling
df_scaled = pd.DataFrame(scaled_values, columns=data.columns)

# Tampilkan data yang telah discaling
df_scaled.head()
```

::: {.output .execute_result execution_count="125"}
``` json
{"type":"dataframe","variable_name":"df_scaled"}
```
:::
:::

::: {.cell .markdown id="x5z0S-qhSmmn"}
`RobustScaler` lebih tahan terhadap outlier dibandingkan
`StandardScaler` atau `MinMaxScaler`. Meskipun dalam eksplorasi data
awal tidak ditemukan outlier yang jelas pada fitur numerik, penggunaan
RobustScaler memberikan proteksi tambahan terhadap potensi
`outlier yang mungkin tersembunyi atau muncul setelah proses encoding data kategorikal`.
Dengan demikian, RobustScaler membantu menjaga stabilitas dan akurasi
model machine learning yang akan digunakan selanjutnya.
:::

::: {.cell .markdown id="ZRFvwMAS73Td"}
## **Data Split**
:::

::: {.cell .code execution_count="126" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="afNJw8q_75ZC" outputId="228c7c85-bcbc-4521-c43f-799027f8d071"}
``` python
# Import library untuk splitting data
from sklearn.model_selection import train_test_split

# Definisikan fitur dan target
X = df_scaled.drop(columns=['churn','paymentmethod', 'onlinebackup', 'deviceprotection', 'multiplelines', 'gender', 'phoneservice', 'tenure'])
y = df_scaled['churn']

# Proses splitting data
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,   # 20% data untuk pengujian
    random_state=42  # Untuk reprodusibilitas
)

# Periksa banyak data masing-masing
print(f'Banyak data latih = {X_train.shape[0]}')
print(f'Banyak data test  = {X_test.shape[0]}')
```

::: {.output .stream .stdout}
    Banyak data latih = 5634
    Banyak data test  = 1409
:::
:::

::: {.cell .code execution_count="127" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="URvr5nUwjPUw" outputId="0c613fa2-9ff3-4422-f529-94748842d734"}
``` python
X.columns
```

::: {.output .execute_result execution_count="127"}
    Index(['seniorcitizen', 'partner', 'dependents', 'internetservice',
           'onlinesecurity', 'techsupport', 'streamingtv', 'streamingmovies',
           'contract', 'paperlessbilling', 'monthlycharges', 'totalcharges',
           'tenure_range', 'tenure_monthly_charge_interaction'],
          dtype='object')
:::
:::

::: {.cell .code execution_count="128" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="-el4jSSP8Sc9" outputId="2e631ed7-d793-474d-b92b-db0bab22b0e9"}
``` python
X.shape
```

::: {.output .execute_result execution_count="128"}
    (7043, 14)
:::
:::

::: {.cell .code execution_count="129" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="YU43fETh8TyX" outputId="8878ceed-28c4-4a77-ff94-276794513a68"}
``` python
X_train.shape
```

::: {.output .execute_result execution_count="129"}
    (5634, 14)
:::
:::

::: {.cell .markdown id="APvtA0XI8Z3t"}
## **Handling Imbalanced Dataset**
:::

::: {.cell .code execution_count="130" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":632}" id="LgLFRILTTzt-" outputId="ba663fcc-4da7-44a1-ecca-11fb99e52087"}
``` python
import matplotlib.pyplot as plt
import seaborn as sns

fig = plt.figure(figsize = (10, 5))

plt.subplot(121)
plt.pie(y_train.value_counts(),
        labels = ['Stayed', 'Churn'],
        autopct = '%.1f%%',
        radius = 1,
        colors=["#e31a1c", "#a6cee3"],
        textprops={'fontsize': 10, 'fontweight': 'bold'})
plt.title('Churn Outcome Pie Chart', fontsize = 10, fontweight = 'bold')

plt.subplot(122)
resp = y_train.apply(lambda x: "Stayed" if x == 0 else "Churn")
t = sns.countplot(x=resp, palette=["#a6cee3", "#e31a1c"])
t.set_xlabel('churn', fontweight = 'bold', fontsize = 10)
t.set_ylabel('Count', fontweight = 'bold', fontsize = 10)

plt.title('Churn Outcome Distributions', fontsize = 10, fontweight = 'bold')
plt.tight_layout()
```

::: {.output .stream .stderr}
    <ipython-input-130-60cd0d317dbf>:17: FutureWarning:



    Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `x` variable to `hue` and set `legend=False` for the same effect.
:::

::: {.output .display_data}
![](asset/14807771a607518eb0346e264bda9eb326fa4859.png)
:::
:::

::: {.cell .code execution_count="131" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":178}" id="iaxyK4Fa8ZT2" outputId="b05f013f-0e5f-423d-9be6-ff21f5c13df1"}
``` python
y_train.value_counts()
```

::: {.output .execute_result execution_count="131"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
    </tr>
    <tr>
      <th>churn</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.000000</th>
      <td>4138</td>
    </tr>
    <tr>
      <th>1.000000</th>
      <td>1496</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>
```
:::
:::

::: {.cell .code execution_count="132" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="TA4ofe0Y9i_f" outputId="d7d46277-ed64-475d-bb48-3e987219aa84"}
``` python
from imblearn.over_sampling import SMOTE

# Menggunakan SMOTE untuk oversampling kelas minoritas
smote = SMOTE(random_state=42)
X_train, y_train = smote.fit_resample(X_train, y_train)
```

::: {.output .stream .stderr}
    /usr/local/lib/python3.10/dist-packages/sklearn/base.py:474: FutureWarning:

    `BaseEstimator._validate_data` is deprecated in 1.6 and will be removed in 1.7. Use `sklearn.utils.validation.validate_data` instead. This function becomes public and is part of the scikit-learn developer API.

    /usr/local/lib/python3.10/dist-packages/sklearn/utils/_tags.py:354: FutureWarning:

    The SMOTE or classes from which it inherits use `_get_tags` and `_more_tags`. Please define the `__sklearn_tags__` method, or inherit from `sklearn.base.BaseEstimator` and/or other appropriate mixins such as `sklearn.base.TransformerMixin`, `sklearn.base.ClassifierMixin`, `sklearn.base.RegressorMixin`, and `sklearn.base.OutlierMixin`. From scikit-learn 1.7, not defining `__sklearn_tags__` will raise an error.
:::
:::

::: {.cell .code execution_count="133" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":178}" id="q7KwifR49sGk" outputId="8cd35372-dfb8-46da-bec6-8c282dba7afd"}
``` python
y_train.value_counts()
```

::: {.output .execute_result execution_count="133"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
    </tr>
    <tr>
      <th>churn</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.000000</th>
      <td>4138</td>
    </tr>
    <tr>
      <th>1.000000</th>
      <td>4138</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>
```
:::
:::

::: {.cell .code execution_count="134" id="WkwST9Yvk2WG"}
``` python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, roc_auc_score
from sklearn.model_selection import cross_validate

def eval_classification(model):

    # Prediksi pada data uji
    y_pred = model.predict(X_test)
    y_pred_train = model.predict(X_train)
    y_pred_proba = model.predict_proba(X_test)
    y_pred_proba_train = model.predict_proba(X_train)


    # Menampilkan metrik evaluasi
    print("Accuracy (Train Set): %.2f" % accuracy_score(y_train, y_pred_train))
    print("Accuracy (Test Set): %.2f" % accuracy_score(y_test, y_pred))

    print("Precision (Test Set): %.2f" % precision_score(y_test, y_pred))
    print("Recall (Test Set): %.2f" % recall_score(y_test, y_pred))
    print("F1-Score (Test Set): %.2f" % f1_score(y_test, y_pred))

    print("ROC AUC (Train Set): %.2f" % roc_auc_score(y_train, y_pred_proba_train[:, 1]))
    print("ROC AUC (Test Set): %.2f" % roc_auc_score(y_test, y_pred_proba[:, 1]))


    try:
        # Menggunakan cross-validation untuk recall
        score = cross_validate(model, X, y, cv=5, scoring='recall', return_train_score=True)
        print('Recall (Crossval Train): %.2f' % score['train_score'].mean())
        print('Recall (Crossval Test): %.2f' % score['test_score'].mean())
    except Exception as e:
        print(f"Skipped cross-validation due to error: {e}")
```
:::

::: {.cell .markdown id="sKQlJsT7-U94"}
# **📌Fitting Model** (Decision Tree)
:::

::: {.cell .markdown id="dAWTtf-PZHrX"}
`Decision Tree` adalah alat pemodelan yang berguna dalam analisis churn,
memberikan interpretasi yang jelas dan membantu dalam pengambilan
keputusan. Namun, penting untuk mengatasi potensi overfitting dan
mempertimbangkan penggunaan ensemble methods seperti Random Forest untuk
meningkatkan akurasi dan stabilitas model.
:::

::: {.cell .code execution_count="135" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":81}" id="lqudf3OV-VWL" outputId="e1f9d50f-7b47-4d08-fad7-c5dea68128c3"}
``` python
# Importing libraries
from sklearn.tree import DecisionTreeClassifier

# Membuat model Decision Tree
dt_model = DecisionTreeClassifier(max_leaf_nodes = 5)

# Melatih model dengan data latih
dt_model.fit(X_train, y_train)
```

::: {.output .execute_result execution_count="135"}
```{=html}
<style>#sk-container-id-2 {
  /* Definition of color scheme common for light and dark mode */
  --sklearn-color-text: #000;
  --sklearn-color-text-muted: #666;
  --sklearn-color-line: gray;
  /* Definition of color scheme for unfitted estimators */
  --sklearn-color-unfitted-level-0: #fff5e6;
  --sklearn-color-unfitted-level-1: #f6e4d2;
  --sklearn-color-unfitted-level-2: #ffe0b3;
  --sklearn-color-unfitted-level-3: chocolate;
  /* Definition of color scheme for fitted estimators */
  --sklearn-color-fitted-level-0: #f0f8ff;
  --sklearn-color-fitted-level-1: #d4ebff;
  --sklearn-color-fitted-level-2: #b3dbfd;
  --sklearn-color-fitted-level-3: cornflowerblue;

  /* Specific color for light theme */
  --sklearn-color-text-on-default-background: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, black)));
  --sklearn-color-background: var(--sg-background-color, var(--theme-background, var(--jp-layout-color0, white)));
  --sklearn-color-border-box: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, black)));
  --sklearn-color-icon: #696969;

  @media (prefers-color-scheme: dark) {
    /* Redefinition of color scheme for dark theme */
    --sklearn-color-text-on-default-background: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, white)));
    --sklearn-color-background: var(--sg-background-color, var(--theme-background, var(--jp-layout-color0, #111)));
    --sklearn-color-border-box: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, white)));
    --sklearn-color-icon: #878787;
  }
}

#sk-container-id-2 {
  color: var(--sklearn-color-text);
}

#sk-container-id-2 pre {
  padding: 0;
}

#sk-container-id-2 input.sk-hidden--visually {
  border: 0;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
}

#sk-container-id-2 div.sk-dashed-wrapped {
  border: 1px dashed var(--sklearn-color-line);
  margin: 0 0.4em 0.5em 0.4em;
  box-sizing: border-box;
  padding-bottom: 0.4em;
  background-color: var(--sklearn-color-background);
}

#sk-container-id-2 div.sk-container {
  /* jupyter's `normalize.less` sets `[hidden] { display: none; }`
     but bootstrap.min.css set `[hidden] { display: none !important; }`
     so we also need the `!important` here to be able to override the
     default hidden behavior on the sphinx rendered scikit-learn.org.
     See: https://github.com/scikit-learn/scikit-learn/issues/21755 */
  display: inline-block !important;
  position: relative;
}

#sk-container-id-2 div.sk-text-repr-fallback {
  display: none;
}

div.sk-parallel-item,
div.sk-serial,
div.sk-item {
  /* draw centered vertical line to link estimators */
  background-image: linear-gradient(var(--sklearn-color-text-on-default-background), var(--sklearn-color-text-on-default-background));
  background-size: 2px 100%;
  background-repeat: no-repeat;
  background-position: center center;
}

/* Parallel-specific style estimator block */

#sk-container-id-2 div.sk-parallel-item::after {
  content: "";
  width: 100%;
  border-bottom: 2px solid var(--sklearn-color-text-on-default-background);
  flex-grow: 1;
}

#sk-container-id-2 div.sk-parallel {
  display: flex;
  align-items: stretch;
  justify-content: center;
  background-color: var(--sklearn-color-background);
  position: relative;
}

#sk-container-id-2 div.sk-parallel-item {
  display: flex;
  flex-direction: column;
}

#sk-container-id-2 div.sk-parallel-item:first-child::after {
  align-self: flex-end;
  width: 50%;
}

#sk-container-id-2 div.sk-parallel-item:last-child::after {
  align-self: flex-start;
  width: 50%;
}

#sk-container-id-2 div.sk-parallel-item:only-child::after {
  width: 0;
}

/* Serial-specific style estimator block */

#sk-container-id-2 div.sk-serial {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: var(--sklearn-color-background);
  padding-right: 1em;
  padding-left: 1em;
}


/* Toggleable style: style used for estimator/Pipeline/ColumnTransformer box that is
clickable and can be expanded/collapsed.
- Pipeline and ColumnTransformer use this feature and define the default style
- Estimators will overwrite some part of the style using the `sk-estimator` class
*/

/* Pipeline and ColumnTransformer style (default) */

#sk-container-id-2 div.sk-toggleable {
  /* Default theme specific background. It is overwritten whether we have a
  specific estimator or a Pipeline/ColumnTransformer */
  background-color: var(--sklearn-color-background);
}

/* Toggleable label */
#sk-container-id-2 label.sk-toggleable__label {
  cursor: pointer;
  display: flex;
  width: 100%;
  margin-bottom: 0;
  padding: 0.5em;
  box-sizing: border-box;
  text-align: center;
  align-items: start;
  justify-content: space-between;
  gap: 0.5em;
}

#sk-container-id-2 label.sk-toggleable__label .caption {
  font-size: 0.6rem;
  font-weight: lighter;
  color: var(--sklearn-color-text-muted);
}

#sk-container-id-2 label.sk-toggleable__label-arrow:before {
  /* Arrow on the left of the label */
  content: "▸";
  float: left;
  margin-right: 0.25em;
  color: var(--sklearn-color-icon);
}

#sk-container-id-2 label.sk-toggleable__label-arrow:hover:before {
  color: var(--sklearn-color-text);
}

/* Toggleable content - dropdown */

#sk-container-id-2 div.sk-toggleable__content {
  max-height: 0;
  max-width: 0;
  overflow: hidden;
  text-align: left;
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-2 div.sk-toggleable__content.fitted {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

#sk-container-id-2 div.sk-toggleable__content pre {
  margin: 0.2em;
  border-radius: 0.25em;
  color: var(--sklearn-color-text);
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-2 div.sk-toggleable__content.fitted pre {
  /* unfitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

#sk-container-id-2 input.sk-toggleable__control:checked~div.sk-toggleable__content {
  /* Expand drop-down */
  max-height: 200px;
  max-width: 100%;
  overflow: auto;
}

#sk-container-id-2 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {
  content: "▾";
}

/* Pipeline/ColumnTransformer-specific style */

#sk-container-id-2 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-2 div.sk-label.fitted input.sk-toggleable__control:checked~label.sk-toggleable__label {
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Estimator-specific style */

/* Colorize estimator box */
#sk-container-id-2 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-2 div.sk-estimator.fitted input.sk-toggleable__control:checked~label.sk-toggleable__label {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-2);
}

#sk-container-id-2 div.sk-label label.sk-toggleable__label,
#sk-container-id-2 div.sk-label label {
  /* The background is the default theme color */
  color: var(--sklearn-color-text-on-default-background);
}

/* On hover, darken the color of the background */
#sk-container-id-2 div.sk-label:hover label.sk-toggleable__label {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-unfitted-level-2);
}

/* Label box, darken color on hover, fitted */
#sk-container-id-2 div.sk-label.fitted:hover label.sk-toggleable__label.fitted {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Estimator label */

#sk-container-id-2 div.sk-label label {
  font-family: monospace;
  font-weight: bold;
  display: inline-block;
  line-height: 1.2em;
}

#sk-container-id-2 div.sk-label-container {
  text-align: center;
}

/* Estimator-specific */
#sk-container-id-2 div.sk-estimator {
  font-family: monospace;
  border: 1px dotted var(--sklearn-color-border-box);
  border-radius: 0.25em;
  box-sizing: border-box;
  margin-bottom: 0.5em;
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-2 div.sk-estimator.fitted {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

/* on hover */
#sk-container-id-2 div.sk-estimator:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-2 div.sk-estimator.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Specification for estimator info (e.g. "i" and "?") */

/* Common style for "i" and "?" */

.sk-estimator-doc-link,
a:link.sk-estimator-doc-link,
a:visited.sk-estimator-doc-link {
  float: right;
  font-size: smaller;
  line-height: 1em;
  font-family: monospace;
  background-color: var(--sklearn-color-background);
  border-radius: 1em;
  height: 1em;
  width: 1em;
  text-decoration: none !important;
  margin-left: 0.5em;
  text-align: center;
  /* unfitted */
  border: var(--sklearn-color-unfitted-level-1) 1pt solid;
  color: var(--sklearn-color-unfitted-level-1);
}

.sk-estimator-doc-link.fitted,
a:link.sk-estimator-doc-link.fitted,
a:visited.sk-estimator-doc-link.fitted {
  /* fitted */
  border: var(--sklearn-color-fitted-level-1) 1pt solid;
  color: var(--sklearn-color-fitted-level-1);
}

/* On hover */
div.sk-estimator:hover .sk-estimator-doc-link:hover,
.sk-estimator-doc-link:hover,
div.sk-label-container:hover .sk-estimator-doc-link:hover,
.sk-estimator-doc-link:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

div.sk-estimator.fitted:hover .sk-estimator-doc-link.fitted:hover,
.sk-estimator-doc-link.fitted:hover,
div.sk-label-container:hover .sk-estimator-doc-link.fitted:hover,
.sk-estimator-doc-link.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

/* Span, style for the box shown on hovering the info icon */
.sk-estimator-doc-link span {
  display: none;
  z-index: 9999;
  position: relative;
  font-weight: normal;
  right: .2ex;
  padding: .5ex;
  margin: .5ex;
  width: min-content;
  min-width: 20ex;
  max-width: 50ex;
  color: var(--sklearn-color-text);
  box-shadow: 2pt 2pt 4pt #999;
  /* unfitted */
  background: var(--sklearn-color-unfitted-level-0);
  border: .5pt solid var(--sklearn-color-unfitted-level-3);
}

.sk-estimator-doc-link.fitted span {
  /* fitted */
  background: var(--sklearn-color-fitted-level-0);
  border: var(--sklearn-color-fitted-level-3);
}

.sk-estimator-doc-link:hover span {
  display: block;
}

/* "?"-specific style due to the `<a>` HTML tag */

#sk-container-id-2 a.estimator_doc_link {
  float: right;
  font-size: 1rem;
  line-height: 1em;
  font-family: monospace;
  background-color: var(--sklearn-color-background);
  border-radius: 1rem;
  height: 1rem;
  width: 1rem;
  text-decoration: none;
  /* unfitted */
  color: var(--sklearn-color-unfitted-level-1);
  border: var(--sklearn-color-unfitted-level-1) 1pt solid;
}

#sk-container-id-2 a.estimator_doc_link.fitted {
  /* fitted */
  border: var(--sklearn-color-fitted-level-1) 1pt solid;
  color: var(--sklearn-color-fitted-level-1);
}

/* On hover */
#sk-container-id-2 a.estimator_doc_link:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

#sk-container-id-2 a.estimator_doc_link.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-3);
}
</style><div id="sk-container-id-2" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>DecisionTreeClassifier(max_leaf_nodes=5)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator fitted sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-2" type="checkbox" checked><label for="sk-estimator-id-2" class="sk-toggleable__label fitted sk-toggleable__label-arrow"><div><div>DecisionTreeClassifier</div></div><div><a class="sk-estimator-doc-link fitted" rel="noreferrer" target="_blank" href="https://scikit-learn.org/1.6/modules/generated/sklearn.tree.DecisionTreeClassifier.html">?<span>Documentation for DecisionTreeClassifier</span></a><span class="sk-estimator-doc-link fitted">i<span>Fitted</span></span></div></label><div class="sk-toggleable__content fitted"><pre>DecisionTreeClassifier(max_leaf_nodes=5)</pre></div> </div></div></div></div>
```
:::
:::

::: {.cell .code execution_count="136" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":961}" id="a1b79-4z-zoR" outputId="ad89bf9e-31c0-4d23-f348-9f3fbfa41a97"}
``` python
from sklearn import tree
import matplotlib.pyplot as plt

# Membuat plot pohon keputusan
fig, ax = plt.subplots(figsize=(12, 12))
tree.plot_tree(
    dt_model,
    feature_names = dt_model.feature_names_in_,
    class_names = ['Stayed', 'Churn'],
    filled=True
)

plt.show()
```

::: {.output .display_data}
![](asset/c27776220099f555d64777752ff96870b541b4c0.png)
:::
:::

::: {.cell .markdown id="jlW-KwINPkMq"}
### **Evaluation**
:::

::: {.cell .code execution_count="137" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="jNUn0amvU0s5" outputId="41f90b85-11a9-4838-ad4d-3900007e895b"}
``` python
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# Prediksi dengan data uji
y_pred = dt_model.predict(X_test)

# Evaluasi model
accuracy = accuracy_score(y_test, y_pred)
print(f"Accuracy: {accuracy * 100:.2f}%")

# Menampilkan classification report
print("\nClassification Report:")
print(classification_report(y_test, y_pred))
```

::: {.output .stream .stdout}
    Accuracy: 72.39%

    Classification Report:
                  precision    recall  f1-score   support

             0.0       0.93      0.68      0.78      1036
             1.0       0.49      0.86      0.62       373

        accuracy                           0.72      1409
       macro avg       0.71      0.77      0.70      1409
    weighted avg       0.81      0.72      0.74      1409
:::
:::

::: {.cell .code execution_count="138" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="6uV511nflWag" outputId="2d6b4709-d968-4833-9d60-140cdc737680"}
``` python
eval_classification(dt_model)
```

::: {.output .stream .stdout}
    Accuracy (Train Set): 0.76
    Accuracy (Test Set): 0.72
    Precision (Test Set): 0.49
    Recall (Test Set): 0.86
    F1-Score (Test Set): 0.62
    ROC AUC (Train Set): 0.80
    ROC AUC (Test Set): 0.81
    Recall (Crossval Train): 0.38
    Recall (Crossval Test): 0.37
:::
:::

::: {.cell .code execution_count="139" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":564}" id="5fU5M6O2A3sk" outputId="2062b492-4d5a-4fb4-acbc-158283407cd2"}
``` python
from sklearn.metrics import confusion_matrix
import seaborn as sns

# Menghitung confusion matrix
cm = confusion_matrix(y_test, y_pred)

# Menampilkan confusion matrix menggunakan seaborn heatmap
plt.figure(figsize=(8, 6))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', xticklabels=['Stayed', 'Churn'], yticklabels=['Stayed', 'Churn'])
plt.title("Confusion Matrix")
plt.xlabel("Predicted Label")
plt.ylabel("True Label")
plt.show()
```

::: {.output .display_data}
![](asset/30e4decf537f3e1ea810ecdf10546466fa06429d.png)
:::
:::

::: {.cell .markdown id="Z3PXvvj7Vk1S"}
-   True Positives (TP): 319 (Pelanggan yang benar-benar churn dan
    diprediksi churn)
-   True Negatives (TN): 701 (Pelanggan yang benar-benar stayed dan
    diprediksi stayed)
-   False Positives (FP): 335 (Pelanggan yang benar-benar stayed tetapi
    diprediksi churn)
-   False Negatives (FN): 54 (Pelanggan yang benar-benar churn tetapi
    diprediksi stayed)
:::

::: {.cell .markdown id="2BDbDcW0WMkS"}
-   Kinerja Model: Model menunjukkan akurasi yang baik pada data train
    (76%) tetapi mengalami penurunan pada data test (72%), yang
    mengindikasikan potensi overfitting.

-   Sensitivitas: Model memiliki recall yang tinggi (86%), yang berarti
    mampu mendeteksi sebagian besar pelanggan yang churn, tetapi
    precision yang rendah (49%) menunjukkan banyak prediksi churn yang
    salah.

-   Keseimbangan: F1-Score (62%) menunjukkan bahwa meskipun model baik
    dalam mendeteksi churn, ada ruang untuk perbaikan dalam mengurangi
    kesalahan prediksi.

-   ROC AUC: Nilai yang baik (0.80 untuk train dan 0.81 untuk test)
    menunjukkan bahwa model memiliki kemampuan yang baik dalam
    membedakan antara pelanggan yang churn dan yang tidak.
:::

::: {.cell .markdown id="xn4CyE-QPn8g"}
### **HyperParameter Tunning**
:::

::: {.cell .code execution_count="140" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="f3aAszCzCYE0" outputId="cbddbb63-7e90-48ec-beb4-9ce688028e12"}
``` python
from sklearn.model_selection import GridSearchCV

# Menentukan grid hyperparameter untuk pencarian
param_grid = {
    'max_depth': [None, 5, 10, 20, 50],  # Kedalaman pohon
    'min_samples_split': [2, 5, 10],  # Minimum sampel untuk membagi node
    'min_samples_leaf': [1, 2, 5],    # Minimum sampel pada daun
    'criterion': ['gini', 'entropy'] # Kriteria pemisahan
}

# Menggunakan GridSearchCV untuk mencari hyperparameter terbaik
grid_search = GridSearchCV(estimator=dt_model, param_grid=param_grid, cv=5, n_jobs=-1, verbose=2)

# Melatih model dengan pencarian hyperparameter
grid_search.fit(X_train, y_train)

# Menampilkan hasil pencarian terbaik
print("Best Hyperparameters:", grid_search.best_params_)

# Menggunakan model dengan hyperparameter terbaik untuk prediksi
best_model = grid_search.best_estimator_
y_pred = best_model.predict(X_test)

# Evaluasi hasil
accuracy = accuracy_score(y_test, y_pred)
print(f"Accuracy of the best model: {accuracy * 100:.2f}%")

# Menampilkan classification report
print("\nClassification Report:")
print(classification_report(y_test, y_pred))

eval_classification(best_model)
```

::: {.output .stream .stdout}
    Fitting 5 folds for each of 90 candidates, totalling 450 fits
    Best Hyperparameters: {'criterion': 'gini', 'max_depth': None, 'min_samples_leaf': 1, 'min_samples_split': 2}
    Accuracy of the best model: 72.39%

    Classification Report:
                  precision    recall  f1-score   support

             0.0       0.93      0.68      0.78      1036
             1.0       0.49      0.86      0.62       373

        accuracy                           0.72      1409
       macro avg       0.71      0.77      0.70      1409
    weighted avg       0.81      0.72      0.74      1409

    Accuracy (Train Set): 0.76
    Accuracy (Test Set): 0.72
    Precision (Test Set): 0.49
    Recall (Test Set): 0.86
    F1-Score (Test Set): 0.62
    ROC AUC (Train Set): 0.80
    ROC AUC (Test Set): 0.81
    Recall (Crossval Train): 0.38
    Recall (Crossval Test): 0.37
:::
:::

::: {.cell .markdown id="G9dVub7-YbDi"}
-   Akurasi model terbaik adalah 72.39%, yang menunjukkan performa yang
    baik tetapi masih ada ruang untuk perbaikan.
-   Stabilitas Model: Metrik utama seperti akurasi, precision, recall,
    dan F1-Score menunjukkan stabilitas setelah penyesuaian
    hyperparameter, dengan sedikit peningkatan dalam akurasi.
-   Kinerja yang Baik: Model menunjukkan kemampuan yang baik dalam
    mendeteksi churn dengan recall yang tinggi, tetapi precision yang
    rendah menunjukkan bahwa banyak prediksi churn yang salah.
:::

::: {.cell .markdown id="h4yrMRc4oVbU"}
:::

::: {.cell .markdown id="2G11Q44oPgdF"}
\###**Feature Importance**
:::

::: {.cell .code execution_count="143" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":617}" id="MaR5qoCqEKwb" outputId="e99c2c79-7b37-4c7a-e30b-07b06f802ab5"}
``` python
# Import library untuk visualisasi
import plotly.express as px

# Retrieve feature importances
feature_names = dt_model.feature_names_in_
importances = dt_model.feature_importances_

# Create a DataFrame with feature names and importances
df = pd.DataFrame({'Feature': feature_names, 'Importance': importances})

# Sort the DataFrame by importance in descending order
df = df.sort_values('Importance', ascending=True)

# Create a bar plot using Plotly Express
fig = px.bar(df, y='Feature', x='Importance', title='Feature Importances', orientation = 'h')

fig.update_layout(
    width = 1200,
    height = 600,
    showlegend = False,
    margin = dict(l=160, r=200, t=100, b=30),
    plot_bgcolor = 'rgba(0, 0, 0, 0)',
    title = dict(
        text = "<b>Feature Importance</b>",
        font = dict(
            size = 23,
            color = '#757882'
        ),
        y = 0.92
    ),
    yaxis = dict(
        title = '',
        showgrid = True,
        showline = True,
        zeroline = False,
        gridcolor='lightgray',
    ),
    xaxis = dict(
        showgrid = True,
        showline = True,
    ),
)

fig.show()
```

::: {.output .display_data}
```{=html}
<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script charset="utf-8" src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>                <div id="fdfb09e4-e3f0-43a1-a8b8-5689d62ce67c" class="plotly-graph-div" style="height:600px; width:1200px;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("fdfb09e4-e3f0-43a1-a8b8-5689d62ce67c")) {                    Plotly.newPlot(                        "fdfb09e4-e3f0-43a1-a8b8-5689d62ce67c",                        [{"alignmentgroup":"True","hovertemplate":"Importance=%{x}\u003cbr\u003eFeature=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"h","showlegend":false,"textposition":"auto","x":[0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0636521874775929,0.0655145211487812,0.10706186193224328,0.7637714294413827],"xaxis":"x","y":["seniorcitizen","partner","dependents","onlinesecurity","techsupport","streamingtv","streamingmovies","paperlessbilling","totalcharges","tenure_monthly_charge_interaction","tenure_range","monthlycharges","internetservice","contract"],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Importance"},"showgrid":true,"showline":true},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":""},"showgrid":true,"showline":true,"zeroline":false,"gridcolor":"lightgray"},"legend":{"tracegroupgap":0},"title":{"text":"\u003cb\u003eFeature Importance\u003c\u002fb\u003e","font":{"size":23,"color":"#757882"},"y":0.92},"barmode":"relative","margin":{"l":160,"r":200,"t":100,"b":30},"width":1200,"height":600,"showlegend":false,"plot_bgcolor":"rgba(0, 0, 0, 0)"},                        {"responsive": true}                    ).then(function(){
                            
var gd = document.getElementById('fdfb09e4-e3f0-43a1-a8b8-5689d62ce67c');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>
```
:::
:::

::: {.cell .markdown id="YtLLdVIBXCk3"}
## **Kesimpulan**

1.  Contract: Fitur ini memiliki tingkat kepentingan tertinggi,
    menunjukkan bahwa jenis kontrak yang dimiliki pelanggan sangat
    berpengaruh terhadap keputusan mereka untuk churn. Pelanggan dengan
    kontrak jangka panjang mungkin lebih cenderung untuk tetap
    menggunakan layanan.
2.  Internet Service: Fitur ini juga menunjukkan kepentingan yang
    signifikan. Jenis layanan internet yang digunakan pelanggan dapat
    mempengaruhi kepuasan dan keputusan mereka untuk bertahan atau
    meninggalkan layanan.
3.  Monthly Charges: Tingkat kepentingan yang cukup tinggi menunjukkan
    bahwa biaya bulanan yang dibayarkan pelanggan berperan dalam
    keputusan churn. Pelanggan mungkin lebih cenderung untuk churn jika
    mereka merasa biaya tidak sebanding dengan layanan yang diterima.
4.  Tenure Range: Fitur ini menunjukkan bahwa lama pelanggan menggunakan
    layanan juga berpengaruh. Pelanggan yang baru mungkin lebih rentan
    untuk churn dibandingkan dengan pelanggan yang telah lama
    berlangganan.
:::

::: {.cell .markdown id="a0W8TshuZhmo"}
-   Pertimbangan Model Lain: Mengingat hasil yang diperoleh, saya akan
    mencoba menggunakan model lain untuk melihat performa yang lebih
    baik terutama dalam hal nilai precision dan F1-Score. Selanjutnya
    saya akan menggunakan model XGboost
:::

::: {.cell .markdown id="kEOLGCetQ7Bc"}
# **📌 Fitting Model** (XGBoost) {#-fitting-model-xgboost}
:::

::: {.cell .markdown id="TrO3RtiBcUAh"}
`XGBoost` adalah pemodelan yang sangat efektif untuk analisis churn,
memberikan akurasi tinggi dan kemampuan untuk menangani data yang
kompleks. Dengan menggunakan XGBoost, perusahaan dapat lebih baik
memahami pola churn dan merumuskan strategi retensi yang lebih baik,
sehingga meningkatkan kepuasan pelanggan dan mengurangi kehilangan
pendapatan.
:::

::: {.cell .code execution_count="145" id="DtIKSZnlQ7Bd"}
``` python
# Importing libraries
import xgboost as xgb

# Membuat model XGBoost
xgb_clf = xgb.XGBClassifier()
# Melatih model dengan data latih
xgb_clf.fit(X_train, y_train)

# Memprediksi hasil model
y_pred = xgb_clf.predict(X_test)
```
:::

::: {.cell .markdown id="rA1IBZljQ7Be"}
### **Evaluation** {#evaluation}
:::

::: {.cell .code execution_count="150" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="vb2JAphHbN2w" outputId="371237e6-5e1a-4b96-83ab-f68d30cf94d6"}
``` python
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# Prediksi dengan data uji
y_pred = xgb_clf.predict(X_test)

# Evaluasi model
accuracy = accuracy_score(y_test, y_pred)
print(f"Accuracy: {accuracy * 100:.2f}%")

# Menampilkan classification report
print("\nClassification Report:")
print(classification_report(y_test, y_pred))
eval_classification(xgb_clf)
```

::: {.output .stream .stdout}
    Accuracy: 76.86%

    Classification Report:
                  precision    recall  f1-score   support

             0.0       0.87      0.81      0.84      1036
             1.0       0.55      0.65      0.60       373

        accuracy                           0.77      1409
       macro avg       0.71      0.73      0.72      1409
    weighted avg       0.78      0.77      0.77      1409

    Accuracy (Train Set): 0.92
    Accuracy (Test Set): 0.77
    Precision (Test Set): 0.55
    Recall (Test Set): 0.65
    F1-Score (Test Set): 0.60
    ROC AUC (Train Set): 0.98
    ROC AUC (Test Set): 0.84
    Skipped cross-validation due to error: 'super' object has no attribute '__sklearn_tags__'
:::

::: {.output .stream .stderr}
    /usr/local/lib/python3.10/dist-packages/sklearn/utils/_tags.py:354: FutureWarning:

    The XGBClassifier or classes from which it inherits use `_get_tags` and `_more_tags`. Please define the `__sklearn_tags__` method, or inherit from `sklearn.base.BaseEstimator` and/or other appropriate mixins such as `sklearn.base.TransformerMixin`, `sklearn.base.ClassifierMixin`, `sklearn.base.RegressorMixin`, and `sklearn.base.OutlierMixin`. From scikit-learn 1.7, not defining `__sklearn_tags__` will raise an error.
:::
:::

::: {.cell .code execution_count="151" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":564}" id="UQnt2yW0Q7Be" outputId="944182ab-a2e5-4bba-bea2-0aaf4615d279"}
``` python
from sklearn.metrics import confusion_matrix
import seaborn as sns

# Menghitung confusion matrix
cm = confusion_matrix(y_test, y_pred)

# Menampilkan confusion matrix menggunakan seaborn heatmap
plt.figure(figsize=(8, 6))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', xticklabels=['Stayed', 'Churn'], yticklabels=['Stayed', 'Churn'])
plt.title("Confusion Matrix")
plt.xlabel("Predicted Label")
plt.ylabel("True Label")
plt.show()
```

::: {.output .display_data}
![](asset/04fb875fd68919a50dce5ac6196d7754d86a3e87.png)
:::
:::

::: {.cell .markdown id="xaHYa3wfdc-a"}
-   True Positives (TP): 244 (Pelanggan yang benar-benar churn dan
    diprediksi churn)
-   True Negatives (TN): 839 (Pelanggan yang benar-benar stayed dan
    diprediksi stayed)
-   False Positives (FP): 197 (Pelanggan yang benar-benar stayed tetapi
    diprediksi churn)
-   False Negatives (FN): 129 (Pelanggan yang benar-benar churn tetapi
    diprediksi stayed)
:::

::: {.cell .markdown id="4jeriy6Bd0qZ"}
-   Kinerja Model: Model menunjukkan akurasi yang baik secara
    keseluruhan, tetapi ada ketidakseimbangan dalam kemampuan model
    untuk mendeteksi churn. Precision yang rendah untuk kelas churn
    menunjukkan bahwa banyak prediksi churn yang salah.
-   Overfitting: Tingginya akurasi pada data train (92%) dibandingkan
    dengan data test (77%) menunjukkan potensi overfitting, yang perlu
    diatasi untuk meningkatkan generalisasi model.
:::

::: {.cell .markdown id="J8voSZmIQ7Bf"}
### **HyperParameter Tunning** {#hyperparameter-tunning}
:::

::: {.cell .code execution_count="167" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":1000}" id="Zci3iYrcQ7Bf" outputId="bdb2dbf3-b08e-45d1-dd34-5b12f9905331"}
``` python
import xgboost as xgb
from sklearn.metrics import accuracy_score

# Pilih kombinasi hyperparameter terbaik secara langsung
params = {
    'n_estimators': 100,  # Pilihan dari param_grid['n_estimators']
    'max_depth': 5,       # Pilihan dari param_grid['max_depth']
    'learning_rate': 0.1, # Pilihan dari param_grid['learning_rate']
    'subsample': 0.8,     # Pilihan dari param_grid['subsample']
    'colsample_bytree': 1.0, # Pilihan dari param_grid['colsample_bytree']
    'objective': 'binary:logistic',
    'eval_metric': 'logloss',
}

# Melatih model dengan hyperparameter yang dipilih
xgb_clf = xgb.XGBClassifier(**params)
xgb_clf.fit(X_train, y_train, eval_set=[(X_test, y_test)], verbose=False)

# Evaluasi model
y_pred = xgb_clf.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)
print(f"Akurasi Model: {accuracy * 100:.2f}%")

# Menampilkan classification report
print("\nClassification Report:")
print(classification_report(y_test, y_pred))
eval_classification(xgb_clf)

cm = confusion_matrix(y_test, y_pred)

plt.figure(figsize=(8, 6))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=['Stayed', 'Churn'], yticklabels=['Stayed', 'Churn'])
plt.title("Confusion Matrix for XGBoost after Hyperparameter Tuning")
plt.xlabel("Predicted Label")
plt.ylabel("True Label")
plt.show()
```

::: {.output .stream .stdout}
    Akurasi Model: 78.14%

    Classification Report:
                  precision    recall  f1-score   support

             0.0       0.89      0.80      0.84      1036
             1.0       0.57      0.72      0.64       373

        accuracy                           0.78      1409
       macro avg       0.73      0.76      0.74      1409
    weighted avg       0.80      0.78      0.79      1409

    Accuracy (Train Set): 0.86
    Accuracy (Test Set): 0.78
    Precision (Test Set): 0.57
    Recall (Test Set): 0.72
    F1-Score (Test Set): 0.64
    ROC AUC (Train Set): 0.94
    ROC AUC (Test Set): 0.85
    Skipped cross-validation due to error: 'super' object has no attribute '__sklearn_tags__'
:::

::: {.output .stream .stderr}
    /usr/local/lib/python3.10/dist-packages/sklearn/utils/_tags.py:354: FutureWarning:

    The XGBClassifier or classes from which it inherits use `_get_tags` and `_more_tags`. Please define the `__sklearn_tags__` method, or inherit from `sklearn.base.BaseEstimator` and/or other appropriate mixins such as `sklearn.base.TransformerMixin`, `sklearn.base.ClassifierMixin`, `sklearn.base.RegressorMixin`, and `sklearn.base.OutlierMixin`. From scikit-learn 1.7, not defining `__sklearn_tags__` will raise an error.
:::

::: {.output .display_data}
![](asset/60f40c3054ef98a5f5e418d666f99a7076ee2129.png)
:::
:::

::: {.cell .markdown id="YtuErwq_f1IO"}
-   Peningkatan Kinerja: Secara keseluruhan, model menunjukkan
    peningkatan yang signifikan setelah penyesuaian hyperparameter,
    terutama dalam akurasi, precision, dan F1-Score.
-   Keseimbangan: Meskipun recall untuk churn sedikit menurun, model
    menunjukkan peningkatan dalam kemampuan untuk memprediksi pelanggan
    yang tidak churn dengan lebih akurat.
:::

::: {.cell .markdown id="HyeRGWgRQ7Bf"}
\###**Feature Importance**
:::

::: {.cell .code execution_count="155" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":617}" id="5PU2sARDQ7Bf" outputId="36786d42-4e4e-46df-9239-70153f4d5c6c"}
``` python
# Import library untuk visualisasi
import plotly.express as px

# Retrieve feature importances
feature_names = xgb_clf.feature_names_in_
importances = xgb_clf.feature_importances_

# Create a DataFrame with feature names and importances
df = pd.DataFrame({'Feature': feature_names, 'Importance': importances})

# Sort the DataFrame by importance in descending order
df = df.sort_values('Importance', ascending=True)

# Create a bar plot using Plotly Express
fig = px.bar(df, y='Feature', x='Importance', title='Feature Importances', orientation = 'h')

fig.update_layout(
    width = 1200,
    height = 600,
    showlegend = False,
    margin = dict(l=160, r=200, t=100, b=30),
    plot_bgcolor = 'rgba(0, 0, 0, 0)',
    title = dict(
        text = "<b>Feature Importance</b>",
        font = dict(
            size = 23,
            color = '#757882'
        ),
        y = 0.92
    ),
    yaxis = dict(
        title = '',
        showgrid = True,
        showline = True,
        zeroline = False,
        gridcolor='lightgray',
    ),
    xaxis = dict(
        showgrid = True,
        showline = True,
    ),
)

fig.show()
```

::: {.output .display_data}
```{=html}
<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script charset="utf-8" src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>                <div id="006ad93e-1f08-4607-b2fa-809258e8de1e" class="plotly-graph-div" style="height:600px; width:1200px;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("006ad93e-1f08-4607-b2fa-809258e8de1e")) {                    Plotly.newPlot(                        "006ad93e-1f08-4607-b2fa-809258e8de1e",                        [{"alignmentgroup":"True","hovertemplate":"Importance=%{x}\u003cbr\u003eFeature=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"h","showlegend":false,"textposition":"auto","x":[0.022102354,0.02277939,0.022896763,0.028808814,0.030703556,0.04186211,0.043626133,0.046451285,0.05003693,0.051256876,0.054467853,0.08440347,0.14653513,0.3540693],"xaxis":"x","y":["partner","tenure_monthly_charge_interaction","totalcharges","monthlycharges","seniorcitizen","dependents","streamingmovies","techsupport","streamingtv","paperlessbilling","onlinesecurity","tenure_range","internetservice","contract"],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Importance"},"showgrid":true,"showline":true},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":""},"showgrid":true,"showline":true,"zeroline":false,"gridcolor":"lightgray"},"legend":{"tracegroupgap":0},"title":{"text":"\u003cb\u003eFeature Importance\u003c\u002fb\u003e","font":{"size":23,"color":"#757882"},"y":0.92},"barmode":"relative","margin":{"l":160,"r":200,"t":100,"b":30},"width":1200,"height":600,"showlegend":false,"plot_bgcolor":"rgba(0, 0, 0, 0)"},                        {"responsive": true}                    ).then(function(){
                            
var gd = document.getElementById('006ad93e-1f08-4607-b2fa-809258e8de1e');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>
```
:::
:::

::: {.cell .markdown id="SZpSPI9ThYxi"}
## **Kesimpulan** {#kesimpulan}

1.  `Contract`: Fitur ini memiliki tingkat kepentingan tertinggi,
    menunjukkan bahwa jenis kontrak yang dimiliki pelanggan sangat
    berpengaruh terhadap keputusan mereka untuk churn.
2.  `Internet Service`: Fitur ini juga menunjukkan kepentingan yang
    signifikan. Jenis layanan internet yang digunakan pelanggan dapat
    mempengaruhi kepuasan dan keputusan mereka untuk tetap atau
    meninggalkan layanan.
3.  `Tenure Range`: Lama pelanggan menggunakan layanan juga berpengaruh.
    Pelanggan yang baru mungkin lebih rentan untuk churn dibandingkan
    dengan pelanggan yang telah lama berlangganan.
4.  `Online Security`: Adanya layanan keamanan online dapat mempengaruhi
    keputusan pelanggan untuk tetap menggunakan layanan.
5.  `Paperless Billing`: Penggunaan penagihan tanpa kertas juga
    berkontribusi terhadap keputusan pelanggan.
6.  `Streaming TV` dan `Streaming Movies`: Kedua fitur ini menunjukkan
    bahwa layanan streaming dapat mempengaruhi kepuasan pelanggan dan
    keputusan mereka untuk churn.
7.  `Tech Support`: Ketersediaan dukungan teknis juga berperan dalam
    keputusan pelanggan untuk tetap menggunakan layanan.
8.  `Dependents dan Senior Citizen`: Status keluarga dan usia pelanggan
    dapat mempengaruhi keputusan churn, meskipun dengan tingkat
    kepentingan yang lebih rendah.
9.  `Monthly Charges` dan `Total Charges`: Biaya bulanan dan total biaya
    yang dibayarkan selama berlangganan juga berkontribusi terhadap
    keputusan churn, tetapi tidak sekuat fitur utama.
10. `Tenure Monthly Charge Interaction`: Interaksi antara lama
    berlangganan dan biaya bulanan menunjukkan bahwa hubungan antara
    kedua faktor ini dapat mempengaruhi keputusan churn.
11. `Partner`: Fitur ini memiliki tingkat kepentingan terendah,
    menunjukkan bahwa status kemitraan pelanggan tidak terlalu
    berpengaruh terhadap keputusan churn.
:::

::: {.cell .markdown id="5VQWuioWh1NZ"}
Kinerja Model:

-   Meskipun XGBoost menunjukkan kinerja yang baik, selalu ada
    kemungkinan untuk meningkatkan akurasi dan kemampuan generalisasi
    dengan mencoba model lain, saya akan membuat model `Random Forest`
    dan karena Random Forest ini cenderung lebih robust terhadap noise
    dalam data.
:::

::: {.cell .markdown id="cHHILL5TXDnV"}
# **📌 Fitting Model (Random Forest)** {#-fitting-model-random-forest}
:::

::: {.cell .code execution_count="210" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":81}" id="FUCSRrE4XyEe" outputId="ae4514c4-7999-4e30-a9fe-2c3c3a62da09"}
``` python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(random_state=42)
rf.fit(X_train, y_train)
```

::: {.output .execute_result execution_count="210"}
```{=html}
<style>#sk-container-id-4 {
  /* Definition of color scheme common for light and dark mode */
  --sklearn-color-text: #000;
  --sklearn-color-text-muted: #666;
  --sklearn-color-line: gray;
  /* Definition of color scheme for unfitted estimators */
  --sklearn-color-unfitted-level-0: #fff5e6;
  --sklearn-color-unfitted-level-1: #f6e4d2;
  --sklearn-color-unfitted-level-2: #ffe0b3;
  --sklearn-color-unfitted-level-3: chocolate;
  /* Definition of color scheme for fitted estimators */
  --sklearn-color-fitted-level-0: #f0f8ff;
  --sklearn-color-fitted-level-1: #d4ebff;
  --sklearn-color-fitted-level-2: #b3dbfd;
  --sklearn-color-fitted-level-3: cornflowerblue;

  /* Specific color for light theme */
  --sklearn-color-text-on-default-background: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, black)));
  --sklearn-color-background: var(--sg-background-color, var(--theme-background, var(--jp-layout-color0, white)));
  --sklearn-color-border-box: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, black)));
  --sklearn-color-icon: #696969;

  @media (prefers-color-scheme: dark) {
    /* Redefinition of color scheme for dark theme */
    --sklearn-color-text-on-default-background: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, white)));
    --sklearn-color-background: var(--sg-background-color, var(--theme-background, var(--jp-layout-color0, #111)));
    --sklearn-color-border-box: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, white)));
    --sklearn-color-icon: #878787;
  }
}

#sk-container-id-4 {
  color: var(--sklearn-color-text);
}

#sk-container-id-4 pre {
  padding: 0;
}

#sk-container-id-4 input.sk-hidden--visually {
  border: 0;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
}

#sk-container-id-4 div.sk-dashed-wrapped {
  border: 1px dashed var(--sklearn-color-line);
  margin: 0 0.4em 0.5em 0.4em;
  box-sizing: border-box;
  padding-bottom: 0.4em;
  background-color: var(--sklearn-color-background);
}

#sk-container-id-4 div.sk-container {
  /* jupyter's `normalize.less` sets `[hidden] { display: none; }`
     but bootstrap.min.css set `[hidden] { display: none !important; }`
     so we also need the `!important` here to be able to override the
     default hidden behavior on the sphinx rendered scikit-learn.org.
     See: https://github.com/scikit-learn/scikit-learn/issues/21755 */
  display: inline-block !important;
  position: relative;
}

#sk-container-id-4 div.sk-text-repr-fallback {
  display: none;
}

div.sk-parallel-item,
div.sk-serial,
div.sk-item {
  /* draw centered vertical line to link estimators */
  background-image: linear-gradient(var(--sklearn-color-text-on-default-background), var(--sklearn-color-text-on-default-background));
  background-size: 2px 100%;
  background-repeat: no-repeat;
  background-position: center center;
}

/* Parallel-specific style estimator block */

#sk-container-id-4 div.sk-parallel-item::after {
  content: "";
  width: 100%;
  border-bottom: 2px solid var(--sklearn-color-text-on-default-background);
  flex-grow: 1;
}

#sk-container-id-4 div.sk-parallel {
  display: flex;
  align-items: stretch;
  justify-content: center;
  background-color: var(--sklearn-color-background);
  position: relative;
}

#sk-container-id-4 div.sk-parallel-item {
  display: flex;
  flex-direction: column;
}

#sk-container-id-4 div.sk-parallel-item:first-child::after {
  align-self: flex-end;
  width: 50%;
}

#sk-container-id-4 div.sk-parallel-item:last-child::after {
  align-self: flex-start;
  width: 50%;
}

#sk-container-id-4 div.sk-parallel-item:only-child::after {
  width: 0;
}

/* Serial-specific style estimator block */

#sk-container-id-4 div.sk-serial {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: var(--sklearn-color-background);
  padding-right: 1em;
  padding-left: 1em;
}


/* Toggleable style: style used for estimator/Pipeline/ColumnTransformer box that is
clickable and can be expanded/collapsed.
- Pipeline and ColumnTransformer use this feature and define the default style
- Estimators will overwrite some part of the style using the `sk-estimator` class
*/

/* Pipeline and ColumnTransformer style (default) */

#sk-container-id-4 div.sk-toggleable {
  /* Default theme specific background. It is overwritten whether we have a
  specific estimator or a Pipeline/ColumnTransformer */
  background-color: var(--sklearn-color-background);
}

/* Toggleable label */
#sk-container-id-4 label.sk-toggleable__label {
  cursor: pointer;
  display: flex;
  width: 100%;
  margin-bottom: 0;
  padding: 0.5em;
  box-sizing: border-box;
  text-align: center;
  align-items: start;
  justify-content: space-between;
  gap: 0.5em;
}

#sk-container-id-4 label.sk-toggleable__label .caption {
  font-size: 0.6rem;
  font-weight: lighter;
  color: var(--sklearn-color-text-muted);
}

#sk-container-id-4 label.sk-toggleable__label-arrow:before {
  /* Arrow on the left of the label */
  content: "▸";
  float: left;
  margin-right: 0.25em;
  color: var(--sklearn-color-icon);
}

#sk-container-id-4 label.sk-toggleable__label-arrow:hover:before {
  color: var(--sklearn-color-text);
}

/* Toggleable content - dropdown */

#sk-container-id-4 div.sk-toggleable__content {
  max-height: 0;
  max-width: 0;
  overflow: hidden;
  text-align: left;
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-4 div.sk-toggleable__content.fitted {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

#sk-container-id-4 div.sk-toggleable__content pre {
  margin: 0.2em;
  border-radius: 0.25em;
  color: var(--sklearn-color-text);
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-4 div.sk-toggleable__content.fitted pre {
  /* unfitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

#sk-container-id-4 input.sk-toggleable__control:checked~div.sk-toggleable__content {
  /* Expand drop-down */
  max-height: 200px;
  max-width: 100%;
  overflow: auto;
}

#sk-container-id-4 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {
  content: "▾";
}

/* Pipeline/ColumnTransformer-specific style */

#sk-container-id-4 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-4 div.sk-label.fitted input.sk-toggleable__control:checked~label.sk-toggleable__label {
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Estimator-specific style */

/* Colorize estimator box */
#sk-container-id-4 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-4 div.sk-estimator.fitted input.sk-toggleable__control:checked~label.sk-toggleable__label {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-2);
}

#sk-container-id-4 div.sk-label label.sk-toggleable__label,
#sk-container-id-4 div.sk-label label {
  /* The background is the default theme color */
  color: var(--sklearn-color-text-on-default-background);
}

/* On hover, darken the color of the background */
#sk-container-id-4 div.sk-label:hover label.sk-toggleable__label {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-unfitted-level-2);
}

/* Label box, darken color on hover, fitted */
#sk-container-id-4 div.sk-label.fitted:hover label.sk-toggleable__label.fitted {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Estimator label */

#sk-container-id-4 div.sk-label label {
  font-family: monospace;
  font-weight: bold;
  display: inline-block;
  line-height: 1.2em;
}

#sk-container-id-4 div.sk-label-container {
  text-align: center;
}

/* Estimator-specific */
#sk-container-id-4 div.sk-estimator {
  font-family: monospace;
  border: 1px dotted var(--sklearn-color-border-box);
  border-radius: 0.25em;
  box-sizing: border-box;
  margin-bottom: 0.5em;
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-4 div.sk-estimator.fitted {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

/* on hover */
#sk-container-id-4 div.sk-estimator:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-4 div.sk-estimator.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Specification for estimator info (e.g. "i" and "?") */

/* Common style for "i" and "?" */

.sk-estimator-doc-link,
a:link.sk-estimator-doc-link,
a:visited.sk-estimator-doc-link {
  float: right;
  font-size: smaller;
  line-height: 1em;
  font-family: monospace;
  background-color: var(--sklearn-color-background);
  border-radius: 1em;
  height: 1em;
  width: 1em;
  text-decoration: none !important;
  margin-left: 0.5em;
  text-align: center;
  /* unfitted */
  border: var(--sklearn-color-unfitted-level-1) 1pt solid;
  color: var(--sklearn-color-unfitted-level-1);
}

.sk-estimator-doc-link.fitted,
a:link.sk-estimator-doc-link.fitted,
a:visited.sk-estimator-doc-link.fitted {
  /* fitted */
  border: var(--sklearn-color-fitted-level-1) 1pt solid;
  color: var(--sklearn-color-fitted-level-1);
}

/* On hover */
div.sk-estimator:hover .sk-estimator-doc-link:hover,
.sk-estimator-doc-link:hover,
div.sk-label-container:hover .sk-estimator-doc-link:hover,
.sk-estimator-doc-link:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

div.sk-estimator.fitted:hover .sk-estimator-doc-link.fitted:hover,
.sk-estimator-doc-link.fitted:hover,
div.sk-label-container:hover .sk-estimator-doc-link.fitted:hover,
.sk-estimator-doc-link.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

/* Span, style for the box shown on hovering the info icon */
.sk-estimator-doc-link span {
  display: none;
  z-index: 9999;
  position: relative;
  font-weight: normal;
  right: .2ex;
  padding: .5ex;
  margin: .5ex;
  width: min-content;
  min-width: 20ex;
  max-width: 50ex;
  color: var(--sklearn-color-text);
  box-shadow: 2pt 2pt 4pt #999;
  /* unfitted */
  background: var(--sklearn-color-unfitted-level-0);
  border: .5pt solid var(--sklearn-color-unfitted-level-3);
}

.sk-estimator-doc-link.fitted span {
  /* fitted */
  background: var(--sklearn-color-fitted-level-0);
  border: var(--sklearn-color-fitted-level-3);
}

.sk-estimator-doc-link:hover span {
  display: block;
}

/* "?"-specific style due to the `<a>` HTML tag */

#sk-container-id-4 a.estimator_doc_link {
  float: right;
  font-size: 1rem;
  line-height: 1em;
  font-family: monospace;
  background-color: var(--sklearn-color-background);
  border-radius: 1rem;
  height: 1rem;
  width: 1rem;
  text-decoration: none;
  /* unfitted */
  color: var(--sklearn-color-unfitted-level-1);
  border: var(--sklearn-color-unfitted-level-1) 1pt solid;
}

#sk-container-id-4 a.estimator_doc_link.fitted {
  /* fitted */
  border: var(--sklearn-color-fitted-level-1) 1pt solid;
  color: var(--sklearn-color-fitted-level-1);
}

/* On hover */
#sk-container-id-4 a.estimator_doc_link:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

#sk-container-id-4 a.estimator_doc_link.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-3);
}
</style><div id="sk-container-id-4" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>RandomForestClassifier(random_state=42)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator fitted sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-4" type="checkbox" checked><label for="sk-estimator-id-4" class="sk-toggleable__label fitted sk-toggleable__label-arrow"><div><div>RandomForestClassifier</div></div><div><a class="sk-estimator-doc-link fitted" rel="noreferrer" target="_blank" href="https://scikit-learn.org/1.6/modules/generated/sklearn.ensemble.RandomForestClassifier.html">?<span>Documentation for RandomForestClassifier</span></a><span class="sk-estimator-doc-link fitted">i<span>Fitted</span></span></div></label><div class="sk-toggleable__content fitted"><pre>RandomForestClassifier(random_state=42)</pre></div> </div></div></div></div>
```
:::
:::

::: {.cell .markdown id="n1r7UCzJYYE7"}
### **Evaluation** {#evaluation}
:::

::: {.cell .code execution_count="211" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="vtlzL1IMYAGM" outputId="407bb073-a0d6-4178-c026-d9ea61c8f09f"}
``` python
eval_classification(rf)
```

::: {.output .stream .stdout}
    Accuracy (Train Set): 1.00
    Accuracy (Test Set): 0.77
    Precision (Test Set): 0.56
    Recall (Test Set): 0.58
    F1-Score (Test Set): 0.57
    ROC AUC (Train Set): 1.00
    ROC AUC (Test Set): 0.82
    Recall (Crossval Train): 0.99
    Recall (Crossval Test): 0.49
:::
:::

::: {.cell .markdown id="yNCAKoqwZb3X"}
-   Overfitting: Model Random Forest menunjukkan tanda-tanda
    overfitting, dengan akurasi dan ROC AUC yang sangat tinggi pada data
    pelatihan tetapi penurunan kinerja pada data pengujian. Ini
    menunjukkan bahwa model mungkin terlalu kompleks untuk data yang
    ada.

-   Kinerja pada Data Pengujian: Meskipun akurasi pada data pengujian
    adalah 77%, precision dan recall yang rendah menunjukkan bahwa model
    tidak cukup baik dalam mendeteksi pelanggan yang churn.
:::

::: {.cell .markdown id="6SA_DEdPZrFR"}
### **HyperParameter Tunning** {#hyperparameter-tunning}
:::

::: {.cell .code execution_count="212" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="vZcuFDx8Zqbn" outputId="10d0c72b-ac47-418e-e000-f42dbeb5d58d"}
``` python
# tuning hyperparameter RF + oversampling
from sklearn.model_selection import RandomizedSearchCV

# Definisikan rentang hyperparameter
n_estimators = [int(x) for x in np.linspace(1, 200, 50)]
criterion = ['gini', 'entropy']
max_depth = [int(x) for x in np.linspace(2, 100, 50)]
min_samples_split = [int(x) for x in np.linspace(2, 20, 10)]
min_samples_leaf = [int(x) for x in np.linspace(1, 20, 10)]

hyperparameters = dict(n_estimators=n_estimators, criterion=criterion, max_depth=max_depth,
                       min_samples_split=min_samples_split, min_samples_leaf=min_samples_leaf)

rf = RandomForestClassifier(random_state=42)
rs = RandomizedSearchCV(rf, hyperparameters, scoring='roc_auc', random_state=1, cv=5)
rs.fit(X_train, y_train)
eval_classification(rs)
```

::: {.output .stream .stdout}
    Accuracy (Train Set): 0.89
    Accuracy (Test Set): 0.78
    Precision (Test Set): 0.57
    Recall (Test Set): 0.73
    F1-Score (Test Set): 0.64
    ROC AUC (Train Set): 0.96
    ROC AUC (Test Set): 0.85
    Recall (Crossval Train): 0.54
    Recall (Crossval Test): 0.48
:::
:::

::: {.cell .markdown id="unGrvjpRZ5CH"}
-   Peningkatan Kinerja: Setelah tuning hyperparameter, model
    menunjukkan peningkatan yang signifikan dalam recall dan F1-Score,
    yang menunjukkan bahwa model lebih baik dalam mendeteksi pelanggan
    yang berisiko churn.
-   Stabilitas Akurasi: Meskipun akurasi pada data pengujian tetap
    stabil, peningkatan dalam recall dan F1-Score menunjukkan bahwa
    model lebih efektif dalam mengidentifikasi churn.
:::

::: {.cell .code execution_count="213" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":833}" id="x9p8_3kpaHRs" outputId="98cdbf2e-0e9a-44ce-858c-fc4612aba8c2"}
``` python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import roc_auc_score

# Definisikan parameter yang ingin diuji
param_values = [int(x) for x in np.linspace(2, 50, 15)]  # Parameter yang akan diuji

train_scores = []
test_scores = []

for param in param_values:
    # Buat model dengan parameter yang diuji
    model = RandomForestClassifier(
        n_estimators=100,
        max_depth=None,  # Tidak dibatasi
        min_samples_split=2,  # Default
        min_samples_leaf=param,  # Parameter yang diuji
        random_state=42
    )
    model.fit(X_train, y_train)

    # Evaluasi pada train set
    y_pred_train_proba = model.predict_proba(X_train)
    train_auc = roc_auc_score(y_train, y_pred_train_proba[:, 1])
    train_scores.append(train_auc)

    # Evaluasi pada test set
    y_pred_proba = model.predict_proba(X_test)
    test_auc = roc_auc_score(y_test, y_pred_proba[:, 1])
    test_scores.append(test_auc)

    # Output hasil per iterasi
    print(f'Parameter value: {param}, Train AUC: {train_auc:.3f}, Test AUC: {test_auc:.3f}')

# Plot hasil learning curve
plt.figure(figsize=(10, 6))
plt.plot(param_values, train_scores, label='Train ROC AUC', marker='o')
plt.plot(param_values, test_scores, label='Test ROC AUC', marker='o')
plt.xlabel('Parameter Value')
plt.ylabel('ROC AUC Score')
plt.title('Learning Curve for Random Forest')
plt.legend()
plt.grid()
plt.show()
```

::: {.output .stream .stdout}
    Parameter value: 2, Train AUC: 0.993, Test AUC: 0.839
    Parameter value: 5, Train AUC: 0.958, Test AUC: 0.849
    Parameter value: 8, Train AUC: 0.937, Test AUC: 0.851
    Parameter value: 12, Train AUC: 0.920, Test AUC: 0.856
    Parameter value: 15, Train AUC: 0.909, Test AUC: 0.858
    Parameter value: 19, Train AUC: 0.901, Test AUC: 0.858
    Parameter value: 22, Train AUC: 0.897, Test AUC: 0.860
    Parameter value: 26, Train AUC: 0.891, Test AUC: 0.860
    Parameter value: 29, Train AUC: 0.889, Test AUC: 0.860
    Parameter value: 32, Train AUC: 0.886, Test AUC: 0.862
    Parameter value: 36, Train AUC: 0.883, Test AUC: 0.862
    Parameter value: 39, Train AUC: 0.881, Test AUC: 0.862
    Parameter value: 43, Train AUC: 0.878, Test AUC: 0.861
    Parameter value: 46, Train AUC: 0.878, Test AUC: 0.862
    Parameter value: 50, Train AUC: 0.875, Test AUC: 0.862
:::

::: {.output .display_data}
![](asset/14275b61752909f8d31cee5104b962535b3da73d.png)
:::
:::

::: {.cell .markdown id="d2yPtZwMaZau"}
-   Train AUC - Skor AUC pada data latih mencapai hampir 1.00 saat
    min_samples_leaf rendah, tetapi menurun seiring bertambahnya nilai
    parameter.

-   Test AUC - Skor AUC Stagnan pada data uji relatif stabil sekitar
    pada 0.86 seiring dengan perubahan parameter.

-   Pada parameter 15, AUC untuk data train adalah 0.909 dan untuk data
    test adalah 0.858. Ini menunjukkan bahwa model masih mampu
    mempelajari pola dalam data train tanpa overfitting yang signifikan.

-   Pada parameter 22, AUC untuk data train adalah 0.897 dan untuk data
    test adalah 0.860. Ini menunjukkan bahwa model tetap stabil dan
    mampu generalisasi dengan baik.

**Menunjukan nilai optimal pada min_samples_leaf ke 15**
:::

::: {.cell .code execution_count="214" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="IvZ-ndwNS8-I" outputId="efd8592d-4c4d-4e1a-9f13-8af905aaad46"}
``` python
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, roc_auc_score

# Model final dengan parameter optimal
final_model = RandomForestClassifier(
    n_estimators=50,  # Bisa menggunakan nilai terbaik dari eksplorasi lain
    max_depth=None,  # Jika diperlukan, bisa eksplorasi max_depth namun kami tidak menggunakan
    min_samples_split=2,  # Tetap default
    min_samples_leaf=15,  # Nilai optimal dari learning curve
    random_state=42
)

# Fit model dengan data
final_model.fit(X_train, y_train)

# Evaluasi pada train set
y_pred_train_proba = final_model.predict_proba(X_train)[:, 1]
train_auc = roc_auc_score(y_train, y_pred_train_proba)

# Evaluasi pada test set
y_pred_test_proba = final_model.predict_proba(X_test)[:, 1]
test_auc = roc_auc_score(y_test, y_pred_test_proba)

# Laporan hasil
print(f"Train ROC AUC: {train_auc:.3f}")
print(f"Test ROC AUC: {test_auc:.3f}")
print(classification_report(y_test, final_model.predict(X_test)))
eval_classification(final_model)
```

::: {.output .stream .stdout}
    Train ROC AUC: 0.908
    Test ROC AUC: 0.858
                  precision    recall  f1-score   support

             0.0       0.91      0.76      0.83      1036
             1.0       0.54      0.79      0.64       373

        accuracy                           0.77      1409
       macro avg       0.73      0.77      0.74      1409
    weighted avg       0.81      0.77      0.78      1409

    Accuracy (Train Set): 0.82
    Accuracy (Test Set): 0.77
    Precision (Test Set): 0.54
    Recall (Test Set): 0.79
    F1-Score (Test Set): 0.64
    ROC AUC (Train Set): 0.91
    ROC AUC (Test Set): 0.86
    Recall (Crossval Train): 0.54
    Recall (Crossval Test): 0.49
:::
:::

::: {.cell .code execution_count="215" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":564}" id="O6DZLDLoRZ0T" outputId="3ab737e7-2769-45ff-ec15-302b058cd5c6"}
``` python
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.metrics import confusion_matrix

# Menghitung prediksi pada data uji
y_pred = final_model.predict(X_test)

# Membuat confusion matrix
cm = confusion_matrix(y_test, y_pred)

# Membuat plot confusion matrix
plt.figure(figsize=(8, 6))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=['Tidak Churn (0)', 'Churn (1)'],
            yticklabels=['Tidak Churn (0)', 'Churn (1)'])

plt.ylabel('Actual')
plt.xlabel('Predicted')
plt.title('Confusion Matrix')
plt.show()
```

::: {.output .display_data}
![](asset/04403529f09d7d20874bfcb0ffea0dfa4ff26715.png)
:::
:::

::: {.cell .markdown id="eQDYv0DfUUdP"}
-   True Positives (TP): 293 Jumlah pelanggan yang benar-benar churn dan
    diprediksi churn oleh model. Ini adalah prediksi yang benar dan
    menunjukkan efektivitas model dalam menangkap pelanggan berisiko
    churn.
-   True Negatives (TN): 791 Jumlah pelanggan yang tidak churn dan
    diprediksi sebagai tidak churn. Ini menunjukkan bahwa model dengan
    baik dalam mengidentifikasi pelanggan yang tetap berlangganan.
-   False Positives (FP): 245 Jumlah pelanggan yang sebenarnya tidak
    churn tetapi diprediksi sebagai churn. Model mengalami kesalahan di
    sini, dan ini dapat mengarah pada upaya retention yang tidak perlu.
-   False Negatives (FN): 80 Jumlah pelanggan yang sebenarnya churn
    tetapi diprediksi sebagai tidak churn. Ini adalah kesalahan penting
    karena pelanggan yang sebenarnya berisiko meninggalkan layanan tidak
    terdeteksi, yang dapat mengakibatkan hilangnya pendapatan.
:::

::: {.cell .markdown id="dlgR4B2Smyha"}
-   Peningkatan Kinerja: Model final menunjukkan peningkatan dalam ROC
    AUC dan recall untuk churn, yang menunjukkan bahwa model lebih baik
    dalam mendeteksi pelanggan yang berisiko churn.
-   Stabilitas Model: Secara keseluruhan, model menunjukkan stabilitas
    yang baik dalam akurasi dan kemampuan generalisasi, meskipun ada
    ruang untuk perbaikan, terutama dalam meningkatkan precision untuk
    kelas churn.

**Tingkatkan Recall Kelas Churn melalui pengaturan threshold**
:::

::: {.cell .markdown id="34U7Wc6Ogurv"}
\###**Feature Importance**
:::

::: {.cell .code execution_count="216" id="j-FTHiIkefHL"}
``` python
import pandas as pd
import matplotlib.pyplot as plt

def show_feature_importance(model, feature_names):
    # Dapatkan importance dari fitur
    importance = model.feature_importances_

    # Buat DataFrame untuk menampilkan hasil dengan lebih rapi
    feature_importance_df = pd.DataFrame({
        'Feature': feature_names,
        'Importance': importance
    })

    # Urutkan fitur berdasarkan tingkat kepentingannya
    # Visualisasikan dalam bentuk bar plot
    plt.figure(figsize=(10, 6))
    plt.barh(feature_importance_df.sort_values(by='Importance', ascending=False)['Feature'], feature_importance_df.sort_values(by='Importance', ascending=False)['Importance'], color='skyblue')
    plt.gca().invert_yaxis()  # Membalik urutan agar fitur terpenting ada di atas
    plt.xlabel('Feature Importance')
    plt.title('Feature Importance from Random Forest')
    plt.show()

    return feature_importance_df.sort_values(by='Importance', ascending=False)
```
:::

::: {.cell .code execution_count="217" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":564}" id="qsDCBbBWesIW" outputId="fbba20dc-6ac2-4811-8c00-4c333b6d4217"}
``` python
# Asumsikan dalam estimator terbaik dari RandomizedSearchCV
best_rf_model = rs.best_estimator_

# Daftar nama fitur (jika tidak diketahui, gunakan X.columns untuk mengambil nama dari data training)
feature_names = X_train.columns

feature_importance_df = show_feature_importance(best_rf_model, feature_names)
```

::: {.output .display_data}
![](asset/4f1da5768b10a5069d81447797d29b5bd6ab3a7a.png)
:::
:::

::: {.cell .markdown id="UwVTIvA4cPg1"}
### **Pengaturan Threshold**

`Pengaturan threshold` merupakan tools penting dalam pengembangan model
klasifikasi, termasuk Random Forest. Dengan menyesuaikan threshold,
kinerja model dapat dioptimalkan sesuai dengan tujuan bisnis dan
karakteristik dataset yang digunakan. Dalam konteks analisis churn,
pengaturan threshold berperan dalam menemukan keseimbangan yang tepat
antara menangkap pelanggan yang churn dan mengurangi kesalahan prediksi.
:::

::: {.cell .code execution_count="218" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="giu54GsUdoYW" outputId="d3b6003a-e4bb-4a5b-9334-47624f5ccf83"}
``` python
from sklearn.metrics import precision_recall_curve, roc_auc_score, classification_report
import numpy as np

# Mendapatkan probabilitas prediksi
y_pred_proba = final_model.predict_proba(X_test)[:, 1]
thresholds = np.arange(0.1, 0.9, 0.1)

# Menampilkan hasil untuk setiap threshold
for threshold in thresholds:
    y_pred_adjusted = (y_pred_proba >= threshold).astype(int)

    # Evaluasi model pada threshold tersebut
    print(f"Threshold: {threshold:.2f}")
    print(classification_report(y_test, y_pred_adjusted))

    # Hitung ROC AUC untuk test set
    test_auc = roc_auc_score(y_test, y_pred_proba)
    # Hitung ROC AUC untuk train set
    y_train_proba = final_model.predict_proba(X_train)[:, 1]
    train_auc = roc_auc_score(y_train, y_train_proba)

    print(f"Train ROC AUC: {train_auc:.3f}")
    print(f"Test ROC AUC: {test_auc:.3f}\n")
```

::: {.output .stream .stdout}
    Threshold: 0.10
                  precision    recall  f1-score   support

             0.0       0.98      0.31      0.47      1036
             1.0       0.34      0.99      0.51       373

        accuracy                           0.49      1409
       macro avg       0.66      0.65      0.49      1409
    weighted avg       0.81      0.49      0.48      1409

    Train ROC AUC: 0.908
    Test ROC AUC: 0.858

    Threshold: 0.20
                  precision    recall  f1-score   support

             0.0       0.97      0.45      0.62      1036
             1.0       0.39      0.96      0.55       373

        accuracy                           0.59      1409
       macro avg       0.68      0.71      0.58      1409
    weighted avg       0.81      0.59      0.60      1409

    Train ROC AUC: 0.908
    Test ROC AUC: 0.858

    Threshold: 0.30
                  precision    recall  f1-score   support

             0.0       0.95      0.57      0.71      1036
             1.0       0.43      0.92      0.59       373

        accuracy                           0.66      1409
       macro avg       0.69      0.75      0.65      1409
    weighted avg       0.82      0.66      0.68      1409

    Train ROC AUC: 0.908
    Test ROC AUC: 0.858

    Threshold: 0.40
                  precision    recall  f1-score   support

             0.0       0.94      0.67      0.78      1036
             1.0       0.49      0.88      0.63       373

        accuracy                           0.73      1409
       macro avg       0.72      0.78      0.71      1409
    weighted avg       0.82      0.73      0.74      1409

    Train ROC AUC: 0.908
    Test ROC AUC: 0.858

    Threshold: 0.50
                  precision    recall  f1-score   support

             0.0       0.91      0.76      0.83      1036
             1.0       0.54      0.79      0.64       373

        accuracy                           0.77      1409
       macro avg       0.73      0.77      0.74      1409
    weighted avg       0.81      0.77      0.78      1409

    Train ROC AUC: 0.908
    Test ROC AUC: 0.858

    Threshold: 0.60
                  precision    recall  f1-score   support

             0.0       0.88      0.85      0.86      1036
             1.0       0.62      0.68      0.65       373

        accuracy                           0.80      1409
       macro avg       0.75      0.76      0.75      1409
    weighted avg       0.81      0.80      0.81      1409

    Train ROC AUC: 0.908
    Test ROC AUC: 0.858

    Threshold: 0.70
                  precision    recall  f1-score   support

             0.0       0.84      0.91      0.88      1036
             1.0       0.69      0.53      0.60       373

        accuracy                           0.81      1409
       macro avg       0.77      0.72      0.74      1409
    weighted avg       0.80      0.81      0.80      1409

    Train ROC AUC: 0.908
    Test ROC AUC: 0.858

    Threshold: 0.80
                  precision    recall  f1-score   support

             0.0       0.79      0.97      0.87      1036
             1.0       0.76      0.30      0.43       373

        accuracy                           0.79      1409
       macro avg       0.78      0.63      0.65      1409
    weighted avg       0.78      0.79      0.75      1409

    Train ROC AUC: 0.908
    Test ROC AUC: 0.858
:::
:::

::: {.cell .code execution_count="219" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":300}" id="CnYCwZIxv-_C" outputId="2ecb7894-7776-4f68-a761-fa4a41c87b31"}
``` python
# Data untuk tabel yang diambil dari tabel treshold
data = {
    "Threshold": [0.10, 0.20, 0.30, 0.40, 0.50, 0.60, 0.70, 0.80],
    "Precision (0)": [0.98, 0.97, 0.95, 0.94, 0.91, 0.88, 0.84, 0.79],
    "Recall (0)": [0.31, 0.45, 0.57, 0.67, 0.76, 0.85, 0.91, 0.97],
    "F1-Score (0)": [0.47, 0.62, 0.71, 0.78, 0.83, 0.86, 0.88, 0.87],
    "Precision (1)": [0.34, 0.39, 0.43, 0.49, 0.54, 0.62, 0.69, 0.76],
    "Recall (1)": [0.99, 0.96, 0.92, 0.88, 0.79, 0.68, 0.53, 0.30],
    "F1-Score (1)": [0.51, 0.55, 0.59, 0.63, 0.64, 0.65, 0.60, 0.43],
    "Accuracy": [0.49, 0.59, 0.66, 0.73, 0.77, 0.80, 0.81, 0.79],
    "Train ROC AUC": [0.908] * 8,
    "Test ROC AUC": [0.858] * 8
}

# Membuat DataFrame
results_df = pd.DataFrame(data)

# Menampilkan tabel hasil
results_df
```

::: {.output .execute_result execution_count="219"}
``` json
{"summary":"{\n  \"name\": \"results_df\",\n  \"rows\": 8,\n  \"fields\": [\n    {\n      \"column\": \"Threshold\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.2449489742783178,\n        \"min\": 0.1,\n        \"max\": 0.8,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.2,\n          0.6,\n          0.1\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Precision (0)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.06670832032063165,\n        \"min\": 0.79,\n        \"max\": 0.98,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.97,\n          0.88,\n          0.98\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Recall (0)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.23114234946085868,\n        \"min\": 0.31,\n        \"max\": 0.97,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.45,\n          0.85,\n          0.31\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"F1-Score (0)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.14518461154189666,\n        \"min\": 0.47,\n        \"max\": 0.88,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.62,\n          0.86,\n          0.47\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Precision (1)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.14829988922065027,\n        \"min\": 0.34,\n        \"max\": 0.76,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.39,\n          0.62,\n          0.34\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Recall (1)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.24041259177862187,\n        \"min\": 0.3,\n        \"max\": 0.99,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.96,\n          0.68,\n          0.99\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"F1-Score (1)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.07521398046336106,\n        \"min\": 0.43,\n        \"max\": 0.65,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.55,\n          0.65,\n          0.51\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Accuracy\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.11563489339913181,\n        \"min\": 0.49,\n        \"max\": 0.81,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          0.59,\n          0.8,\n          0.49\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Train ROC AUC\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1.18687833744435e-16,\n        \"min\": 0.908,\n        \"max\": 0.908,\n        \"num_unique_values\": 1,\n        \"samples\": [\n          0.908\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Test ROC AUC\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1.18687833744435e-16,\n        \"min\": 0.858,\n        \"max\": 0.858,\n        \"num_unique_values\": 1,\n        \"samples\": [\n          0.858\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe","variable_name":"results_df"}
```
:::
:::

::: {.cell .markdown id="f1noR2q60Byw"}
:::

::: {.cell .markdown id="sSBjb5KFcayz"}
## **Kesimpulan** {#kesimpulan}

-   `Threshold 0.60` tampaknya menjadi pilihan terbaik karena memberikan
    keseimbangan yang baik antara precision dan recall untuk kedua
    kategori, dengan akurasi yang tinggi (0.80) dan F1-Score yang baik
    (0.86 untuk pelanggan yang tidak churn dan 0.65 untuk pelanggan yang
    churn). Nilai ROC AUC juga menunjukkan bahwa model memiliki
    kemampuan yang baik dalam membedakan antara kategori.

-   `Threshold 0.50` juga merupakan pilihan yang baik, tetapi tidak
    seimbang dalam hal recall untuk kategori pelanggan yang churn.

-   `Threshold 0.40` memberikan recall yang sangat baik untuk kategori
    pelanggan yang churn, tetapi precision untuk kategori pelanggan yang
    tidak churn lebih rendah.
:::

::: {.cell .markdown id="cdN24N5Zwhix"}
## **Rekomendasi**

`Menggunakan Threshold 0.60` untuk aplikasi di mana keseimbangan antara
deteksi churn dan akurasi penting. Jika deteksi churn sangat kritis,
Anda mungkin ingin mempertimbangkan threshold yang lebih rendah (seperti
0.40) untuk meningkatkan recall, meskipun dengan trade-off pada
precision.

-   Tidak Churn (Pelanggan yang Tetap): Precision: 0.88 Recall: 0.85
    F1-Score: 0.86

-   Churn (Pelanggan yang Berhenti): Precision: 0.62 Recall: 0.68
    F1-Score: 0.65 Support: 373 Akurasi Model: 0.80

-   Rata-rata Makro: Precision: 0.75 Recall: 0.76 F1-Score: 0.75

-   Rata-rata Tertimbang: Precision: 0.81 Recall: 0.80 F1-Score: 0.81

-   ROC AUC Train ROC AUC: 0.908 Test ROC AUC: 0.858
:::

::: {.cell .code execution_count="220" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":564}" id="7QusSzJ9yzOM" outputId="5b624f0b-8c92-4efc-fd84-129f4f5cf1e2"}
``` python
# Menghitung prediksi pada data uji dengan threshold 0.6
y_pred_proba = final_model.predict_proba(X_test)[:, 1]
y_pred_adjusted = (y_pred_proba >= 0.6).astype(int)

# Membuat confusion matrix
cm = confusion_matrix(y_test, y_pred_adjusted)

# Membuat plot confusion matrix
plt.figure(figsize=(8, 6))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=['Stayed', 'Churn'],
            yticklabels=['Stayed', 'Churn'])

plt.ylabel('Actual')
plt.xlabel('Predicted')
plt.title('Confusion Matrix (Threshold = 0.6)')
plt.show()
```

::: {.output .display_data}
![](asset/5564b60957f5283a1579f443d18a060918610799.png)
:::
:::

::: {.cell .markdown id="LoS8SmR_zq8q"}
-   True Negatives (TN): 877 Jumlah pelanggan yang benar-benar tetap dan
    diprediksi tetap oleh model. Ini menunjukkan efektivitas model dalam
    mengidentifikasi pelanggan yang tidak berisiko churn.

-   False Positives (FP): 159 Jumlah pelanggan yang sebenarnya tetap
    tetapi diprediksi churn oleh model. Ini menunjukkan adanya kesalahan
    dalam prediksi, di mana pelanggan yang tidak berisiko churn
    teridentifikasi sebagai churn.

-   False Negatives (FN): 119 Jumlah pelanggan yang sebenarnya churn
    tetapi diprediksi tetap oleh model. Ini menunjukkan bahwa model
    gagal menangkap pelanggan yang berisiko churn, yang dapat berdampak
    negatif pada strategi retensi.

-   True Positives (TP): 254 Jumlah pelanggan yang benar-benar churn dan
    diprediksi churn oleh model. Ini adalah prediksi yang benar dan
    menunjukkan efektivitas model dalam menangkap pelanggan berisiko
    churn.
:::

::: {.cell .markdown id="AzBIqjbwj_wX"}
# **📌Evaluasi Model Terbaik dan Rekomendasi Bisnis**
:::

::: {.cell .code execution_count="221" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":363}" id="dKqu2-4_nsE3" outputId="18fe8ae9-65be-41b5-cbff-194d3a739ca0"}
``` python
import pandas as pd

# Data untuk tabel
data = {
    "Metric": [
        "Train ROC AUC",
        "Test ROC AUC",
        "Accuracy",
        "Precision (Churn)",
        "Recall (Churn)",
        "F1-Score (Churn)",
        "True Positives (TP)",
        "True Negatives (TN)",
        "False Positives (FP)",
        "False Negatives (FN)"
    ],
    "Decision Tree (Setelah Tuning)": [
        0.80,  # Train ROC AUC
        0.81,  # Test ROC AUC
        0.72,  # Accuracy
        0.49,  # Precision (Churn)
        0.86,  # Recall (Churn)
        0.62,  # F1-Score (Churn)
        319,   # True Positives (TP)
        701,   # True Negatives (TN)
        335,   # False Positives (FP)
        54    # False Negatives (FN)
    ],
    "XGBoost (Setelah Tuning)": [
        0.94,  # Train ROC AUC
        0.85,  # Test ROC AUC
        0.78,  # Accuracy
        0.57,  # Precision (Churn)
        0.72,  # Recall (Churn)
        0.64,  # F1-Score (Churn)
        269,   # True Positives (TP)
        832,   # True Negatives (TN)
        204,   # False Positives (FP)
        104    # False Negatives (FN)
    ],
    "Random Forest (Setelah Tuning & pengaturan threshold)": [
        0.91,  # Train ROC AUC
        0.86,  # Test ROC AUC
        0.80,  # Accuracy
        0.62,  # Precision (Churn)
        0.85,  # Recall (Churn)
        0.86,  # F1-Score (Churn)
        254,   # True Positives (TP)
        877,   # True Negatives (TN)
        159,   # False Positives (FP)
        119     # False Negatives (FN)
    ]
}

# Membuat DataFrame
comparison_table = pd.DataFrame(data)

# Menampilkan tabel
comparison_table
```

::: {.output .execute_result execution_count="221"}
``` json
{"summary":"{\n  \"name\": \"comparison_table\",\n  \"rows\": 10,\n  \"fields\": [\n    {\n      \"column\": \"Metric\",\n      \"properties\": {\n        \"dtype\": \"string\",\n        \"num_unique_values\": 10,\n        \"samples\": [\n          \"False Positives (FP)\",\n          \"Test ROC AUC\",\n          \"F1-Score (Churn)\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Decision Tree (Setelah Tuning)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 237.71235876813623,\n        \"min\": 0.49,\n        \"max\": 701.0,\n        \"num_unique_values\": 10,\n        \"samples\": [\n          335.0,\n          0.81,\n          0.62\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"XGBoost (Setelah Tuning)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 261.8783098226265,\n        \"min\": 0.57,\n        \"max\": 832.0,\n        \"num_unique_values\": 10,\n        \"samples\": [\n          204.0,\n          0.85,\n          0.64\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Random Forest (Setelah Tuning & pengaturan threshold)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 273.4917697725717,\n        \"min\": 0.62,\n        \"max\": 877.0,\n        \"num_unique_values\": 9,\n        \"samples\": [\n          159.0,\n          0.86,\n          254.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe","variable_name":"comparison_table"}
```
:::
:::

::: {.cell .code execution_count="222" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":635}" id="j3GzY-tksxaq" outputId="84a840fd-ec57-4981-b3b8-c7ba6e4def32"}
``` python
import matplotlib.pyplot as plt

# Data untuk tabel
data = {
    "Model": [
        "Decision Tree",
        "XGBoost",
        "Random Forest"
    ],
    "Recall (Churn)": [
        0.860,  # Recall Decision Tree
        0.720,  # Recall XGBoost
        0.850   # Recall Random Forest
    ],
    "ROC AUC (Test)": [
        0.810,  # ROC AUC Decision Tree
        0.850,  # ROC AUC XGBoost
        0.860   # ROC AUC Random Forest
    ]
}

# Membuat DataFrame
comparison_df = pd.DataFrame(data)

# Menampilkan tabel
print(comparison_df)

# Visualisasi
fig, ax1 = plt.subplots(figsize=(10, 6))

# Warna untuk setiap model
colors = ['blue', 'orange', 'green']

# Plot Recall
ax1.bar(comparison_df['Model'], comparison_df['Recall (Churn)'], color=colors, alpha=0.6)
ax1.set_ylabel('Recall (Churn)', color='black')
ax1.tick_params(axis='y', labelcolor='black')

# Membuat sumbu kedua untuk ROC AUC
ax2 = ax1.twinx()
ax2.plot(comparison_df['Model'], comparison_df['ROC AUC (Test)'], color='red', marker='o', label='ROC AUC (Test)')
ax2.set_ylabel('ROC AUC (Test)', color='red')
ax2.tick_params(axis='y', labelcolor='red')

# Menambahkan judul dan legenda
plt.title('Perbandingan Model Berdasarkan Recall dan ROC AUC')
ax1.legend(loc='upper left')
ax2.legend(loc='upper right')

# Menampilkan plot
plt.show()
```

::: {.output .stream .stderr}
    WARNING:matplotlib.legend:No artists with labels found to put in legend.  Note that artists whose label start with an underscore are ignored when legend() is called with no argument.
:::

::: {.output .stream .stdout}
               Model  Recall (Churn)  ROC AUC (Test)
    0  Decision Tree        0.860000        0.810000
    1        XGBoost        0.720000        0.850000
    2  Random Forest        0.850000        0.860000
:::

::: {.output .display_data}
![](asset/e158fa62c3c77ffd68ebfe5c82f674a0b4f00aab.png)
:::
:::

::: {.cell .markdown id="hVPm8znkrNg4"}
## **Analisis**

-   **Recall**:

`Decision Tree` memiliki nilai recall tertinggi (0.860), yang berarti
model ini paling efektif dalam menangkap pelanggan yang churn.
`Random Forest` juga memiliki recall yang baik (0.850), sedangkan
XGBoost memiliki recall terendah (0.720).

-   **ROC AUC**:

`XGBoost` memiliki nilai Train ROC AUC tertinggi (0.940), menunjukkan
kemampuan terbaik dalam membedakan antara kelas pada data latih.

`Random Forest` memiliki nilai Test ROC AUC tertinggi (0.860),
menunjukkan kinerja yang baik pada data uji.
:::

::: {.cell .markdown id="6mJvBuRlZFdM"}
## **Model Akhir**
:::

::: {.cell .markdown id="qU0QPHT3kT-J"}
Berdasarkan analisis di atas, berikut adalah rekomendasi model
berdasarkan kebutuhan bisnis:

-   XGBoost: Meskipun memiliki nilai recall yang lebih rendah, XGBoost
    menunjukkan kemampuan terbaik dalam membedakan antara kelas pada
    data latih (Train ROC AUC tertinggi). Namun, nilai recall yang lebih
    rendah dapat menjadi masalah jika tujuan utama adalah menangkap
    semua pelanggan yang churn.

-   Random Forest: Memiliki keseimbangan yang baik antara ROC AUC dan
    recall. Dengan Test ROC AUC yang tinggi (0.860) dan recall yang baik
    (0.850), Random Forest adalah pilihan yang solid untuk aplikasi yang
    memerlukan deteksi churn yang efektif.

-   Decision Tree: Meskipun memiliki recall tertinggi (0.860), nilai ROC
    AUC yang lebih rendah menunjukkan bahwa model ini mungkin tidak
    sekuat yang lain dalam membedakan antara kelas.
:::

::: {.cell .markdown id="TVr0_5Ma2kct"}
## **Kesimpulan** {#kesimpulan}

**Rekomendasi Model Terbaik**: `Random Forest`

Model ini menawarkan kombinasi yang baik antara kemampuan membedakan
kelas (ROC AUC) dan efektivitas dalam menangkap pelanggan yang churn
(recall).
:::

::: {.cell .markdown id="c6ar48_3ZZPg"}
# **📌Rekomendasi Bisnis**
:::

::: {.cell .markdown id="tP_gKm9V7yAo"}
\###**Feature Importance**
:::

::: {.cell .code execution_count="230" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":564}" id="r6cmHoXP21-c" outputId="85297ca1-d94a-45d9-9dec-6ca8cc5a14af"}
``` python
import pandas as pd
import matplotlib.pyplot as plt

# Assuming 'final_model' and 'X_train' are defined from the previous code

# Get feature importances for the threshold 0.6
y_pred_proba = final_model.predict_proba(X_test)[:, 1]
threshold = 0.6
y_pred_adjusted = (y_pred_proba >= threshold).astype(int)

# Assuming 'X_train' is your training data and 'feature_names' store column name
feature_names = X_train.columns
importances = final_model.feature_importances_

# Create a DataFrame with feature names and importances
df = pd.DataFrame({'Feature': feature_names, 'Importance': importances})

# Sort the DataFrame by importance in descending order
df = df.sort_values('Importance', ascending=False)


# Create a bar plot using Matplotlib (or any visualization library you prefer)
plt.figure(figsize=(10, 6))
plt.barh(df['Feature'], df['Importance'], color='skyblue')
plt.xlabel("Importance")
plt.ylabel("Feature")
plt.title("Feature Importance at Threshold 0.6")
plt.gca().invert_yaxis()
plt.show()
```

::: {.output .display_data}
![](asset/4493463382ed800cbc1194a5297ad700f5225a54.png)
:::
:::

::: {.cell .markdown id="veYNeKf072Ik"}
## **Kesimpulan** {#kesimpulan}
:::

::: {.cell .markdown id="WNqs1UqbctA0"}
Rekomendasi Bisnis Berdasarkan Evaluasi Model Akhir dan Feature
Importance Model Random Forest :

1.  **Fokus pada Kontrak**

Feature Penting: `contract`

-   Rekomendasi: Tawarkan opsi kontrak yang lebih fleksibel atau
    insentif untuk pelanggan yang memilih kontrak jangka panjang. Hal
    ini dapat membantu mengurangi churn dengan memberikan rasa komitmen
    kepada pelanggan.
-   Opsi Campaign : `**Kontrak Fleksibel**`: Tawarkan diskon untuk
    pelanggan yang memilih kontrak jangka panjang. Berikan opsi untuk
    keluar tanpa penalti setelah periode tertentu.

1.  **Optimalisasi Biaya Bulanan**

Feature Penting: `monthlycharges`

-   Rekomendasi: Tinjau struktur biaya bulanan dan pertimbangkan untuk
    menawarkan paket yang lebih kompetitif atau diskon untuk pelanggan
    yang berisiko churn. Penyesuaian harga dapat meningkatkan kepuasan
    pelanggan dan mengurangi churn.
-   Opsi Campaign : `Paket Hemat`: Luncurkan paket baru dengan harga
    lebih kompetitif dan manfaat tambahan. Tawarkan promo untuk
    pelanggan yang berisiko churn.

1.  **Manfaatkan Tenure dan Total Charges**

Feature `Penting: tenure_range dan totalcharges`

-   Rekomendasi: Identifikasi pelanggan dengan tenure yang lebih pendek
    dan total charges yang tinggi. Tawarkan program loyalitas atau
    penghargaan untuk meningkatkan retensi pelanggan yang telah
    berinvestasi lebih banyak dalam layanan.
-   Opsi Campaign : `Loyalty Rewards`: Program penghargaan untuk
    pelanggan yang telah berlangganan lebih dari satu tahun. Berikan
    diskon atau bonus layanan untuk meningkatkan loyalitas.

1.  **Tingkatkan Layanan Internet**

Feature Penting: `internetservice`

-   Rekomendasi: Pastikan kualitas layanan internet yang ditawarkan
    memenuhi harapan pelanggan. Pertimbangkan untuk meningkatkan
    kecepatan atau menawarkan paket tambahan yang menarik untuk
    pelanggan yang menggunakan layanan internet.
-   Opsi Campaign : `Upgrade Gratis`: Tawarkan upgrade kecepatan
    internet gratis selama satu bulan untuk pelanggan yang berisiko
    churn.

1.  **Interaksi antara Tenure dan Biaya Bulanan**

Feature Penting: `tenure_monthly_charge_interaction`

-   Rekomendasi: Analisis interaksi antara lama berlangganan dan biaya
    bulanan untuk mengidentifikasi pola churn. Tawarkan penyesuaian
    harga atau promosi khusus untuk pelanggan yang telah lama
    berlangganan tetapi merasa biaya bulanan terlalu tinggi.
-   Opsi Campaign `Penyesuaian Harga`: Tawarkan penyesuaian harga untuk
    pelanggan yang telah lama berlangganan tetapi merasa biaya terlalu
    tinggi.

1.  **Tingkatkan Layanan Pelanggan**

Feature Penting: `techsupport, onlinesecurity, dan paperlessbilling`

-   Rekomendasi: Tingkatkan layanan pelanggan dengan menyediakan
    dukungan teknis yang lebih baik dan opsi keamanan online. Edukasi
    pelanggan tentang manfaat dari layanan ini untuk meningkatkan
    kepuasan dan loyalitas.
-   Opsi Campaign `Support Anytime`: Luncurkan layanan dukungan 24/7
    dengan pelatihan khusus untuk tim layanan pelanggan. Tawarkan sesi
    edukasi online tentang penggunaan layanan.

1.  **Segmentasi Pelanggan**

Feature Penting:
`dependents, streamingmovies, streamingtv, partner, dan seniorcitizen`

-   Rekomendasi: Lakukan segmentasi pelanggan berdasarkan fitur-fitur
    ini untuk menyesuaikan penawaran dan komunikasi. Misalnya, tawarkan
    paket khusus untuk keluarga, pelanggan senior, atau pengguna layanan
    streaming.
-   Opsi Campaign `Paket Keluarga`: Tawarkan paket khusus untuk keluarga
    dengan layanan streaming dan dukungan tambahan. `Senior Special`:
    Program khusus untuk pelanggan senior dengan diskon dan layanan yang
    disesuaikan.
:::

::: {.cell .markdown id="gu8JL418_HyY"}
# **🎉Hasil Akhir🎉**
:::

::: {.cell .markdown id="oDYcaUjX_Cw-"}
`1. Fitur mana yang sebaiknya digunakan dari hasil EDA?`

Berdasarkan heatmap korelasi di atas, ada beberapa fitur yang korelasi
nya signifikan dengan target fitur churn (sesuai dengan nilai treshold
yang ditetapkan sebesar 0.15).

Fitur-fitur ini sebagai berikut :

-   contract
-   tenure_range
-   internetservice
-   totalcharges
-   tenure_monthly_charge_interaction
-   monthlycharges
-   paperlessbilling
-   onlinesecurity
-   techsupport
-   dependents

Fitur yang Bisa Dipertimbangkan

-   seniorcitizen & partner - meskipun korelasi positif kecil, karena
    pelanggan senior dan memiliki pasangan/partner sedikit lebih
    berisiko churn.
-   streamingtv dan streamingmovies - meskipun korelasi sangat kecil
    dipertimbangkan karna dampak layanan hiburan terhadap churn.

fitur yang tidak digunakan

-   paymentmethod
-   onlinebackup
-   deviceprotection
-   multiplelines
-   phoneservice
-   gender
-   tenure (karena sudah digantikan oleh tenure_range menghindari
    multikolinearitas)
:::

::: {.cell .markdown id="kLZ2aRnj_dyG"}
`2. Apakah ada feature tambahan lain yang mendukung?`

1.  Menambahkan fitur baru Lama Berlangganan dalam Rentang
    (tenure_range)
2.  Menambahkan Fitur Interaksi antara monthlycharges dan tenure
    (tenure_monthly_charge_interaction)

**Terbukti dengan hasil akhir kedua fitur ini sangat mendukung model**
:::

::: {.cell .markdown id="1Ud2grV9AGCi"}
1.  `Melakukan training model & prediksi churn sebagai variabel target`

berikut adalah training model untuk prediksi churn sebagai bariabel
target berdasarkan kebutuhan bisnis:

-   XGBoost: Meskipun memiliki nilai recall yang lebih rendah, XGBoost
    menunjukkan kemampuan terbaik dalam membedakan antara kelas pada
    data latih (Train ROC AUC tertinggi). Namun, nilai recall yang lebih
    rendah dapat menjadi masalah jika tujuan utama adalah menangkap
    semua pelanggan yang churn.

-   Random Forest: Memiliki keseimbangan yang baik antara ROC AUC dan
    recall. Dengan Test ROC AUC yang tinggi (0.860) dan recall yang baik
    (0.850), Random Forest adalah pilihan yang solid untuk aplikasi yang
    memerlukan deteksi churn yang efektif.

-   Decision Tree: Meskipun memiliki recall tertinggi (0.860), nilai ROC
    AUC yang lebih rendah menunjukkan bahwa model ini mungkin tidak
    sekuat yang lain dalam membedakan antara kelas.
:::

::: {.cell .markdown id="7pRSra6Y_cME"}
`4. Mengevaluasi model dengan metrics Recall dan ROC-AUC.`

Berikut ini adalah evaluasi model dengan metrics Recall dan ROC-AUC

-   **Recall**:

`Decision Tree` memiliki nilai recall tertinggi (0.860), yang berarti
model ini paling efektif dalam menangkap pelanggan yang churn.
`Random Forest` juga memiliki recall yang baik (0.850), sedangkan
XGBoost memiliki recall terendah (0.720).

-   **ROC AUC**:

`XGBoost` memiliki nilai Train ROC AUC tertinggi (0.940), menunjukkan
kemampuan terbaik dalam membedakan antara kelas pada data latih.

`Random Forest` memiliki nilai Test ROC AUC tertinggi (0.860),
menunjukkan kinerja yang baik pada data uji.
:::
