# Tugas Besar Visualisasi Data Menampilkan Visualisasi Interaktif

Kelompok 4

- Widi Sayyid Fadil Muhammad ( 1301190417 )
- Azrina Fazira A ( 1301194241 )
- Hafid Ahmad Adyatma ( 1301194235 )

# Visualization in Finance
Saham merupakan salah satu instrumen pasar keuangan yang paling popular. Menerbitkan saham merupakan salah satu pilihan perusahaan ketika memutuskan untuk pendanaan perusahaan. Pada sisi lain, saham merupakan instrumen investasi yang banyak dipilih oleh para investor karena saham mampu memberikan tingkat keuntungan yang menarik.

Saham dapat didefinisikan sebagai tanda penyertaan modal seseorang atau pihak (badan usaha) dalam suatu perusahaan atau perseroan terbatas. Dengan menyertakan modal tersebut, maka pihak tersebut memiliki klaim atas pendapatan perusahaan, klaim atas aset perusahaan, dan berhak hadir dalam Rapat Umum Pemegang Saham (RUPS).

Program ini akan memanfaatkan fitur-fitur yang tersedia pada Bokeh Library untuk memaksimalkan visualisasi data yang didapatkan.
Data yang dipakai
- Apple Inc. 
- Alphabet Inc.
- Microsoft Corporation
- Netflix, Inc.
- Tesla, Inc.

Saham-saham yang dipilih berasal dari berbagai sektor dan kapitalisasi pasar. Untuk bagian ini, data nya di kita set dari DEFALUT_TICKERS yang isi parameternya itu saham - saham tersebut.


# Getting Started
Berikut ini merupakan program yang menampilkan trend saham dari berbagai pasar saham berdasarkan data close dengan menggunakan Bokeh Library untuk memudahkan proses visualisasi data.

# Prequisites
Langkah utama yaitu menginstall library yang di butuhkan seperti :
```
pip install pandas
pip install bokeh
pip install yfinance
```
# Installing
Langkah selanjutnya import librarynya

```
import pandas as pd
import yfinance as yf

from bokeh.io import curdoc
from bokeh.models import ColumnDataSource, Select, DataTable, TableColumn, Tabs, Panel
from bokeh.layouts import column, row
from bokeh.plotting import figure, show
```

# Handle Dataset
membuat dataset atau memuat dataset yang diperlukan untuk di visualisasikan

```
DEFAULT_TICKERS = ["AAPL", "GOOG", "MSFT", "NFLX", "TSLA"]
START, END = "2018-01-01", "2022-01-01"
```
Selanjutnya kita load data dan get data ticker tersebut sekaligus membuang atau menghilangkan nilai yang Nan

```
def load_ticker(tickers):
    df = yf.download(tickers, start=START, end=END)
    return df["Close"].dropna()

def get_data(t1, t2):
    d = load_ticker(DEFAULT_TICKERS)
    df = d[[t1, t2]]
    returns = df.pct_change().add_suffix("_returns")
    df = pd.concat([df, returns], axis=1)
    df.rename(columns={t1: "t1", t2: "t2", t1 +
              "_returns": "t1_returns", t2+"_returns": "t2_returns"}, inplace=True)
    return df.dropna()
```

Untuk data yang sudah dipreprocess, selanjutnya akan masuk kedalam function yang akan mengubah data tersebut agar dapat dengan mudah divisualisasikan dengan Library Bokeh.

```
# source data
data = get_data(ticker1.value, ticker2.value)
source = ColumnDataSource(data=data)
```
membuat deskripsi status untuk di tampilkan dalam table

```
# Descriptive Stats
stats = round(data.describe().reset_index(), 2)
stats_source = ColumnDataSource(data=stats)
stat_columns = [TableColumn(field=col, title=col) for col in stats.columns]
data_table = DataTable(source=stats_source, columns=stat_columns,
                       width=350, height=350, index_position=None)
```







