<div id="top"></div>
<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->

<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/oojn4/DashboardVisdat">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>

<h3 align="center">Dashboard Visdat</h3>

  <p align="center">
    Dashboard Visdat
    <br />
    <a href="https://github.com/oojn4/DashboardVisdat"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/oojn4/DashboardVisdat">View Demo</a>
    ·
    <a href="https://github.com/oojn4/DashboardVisdat/issues">Report Bug</a>
    ·
    <a href="https://github.com/oojn4/DashboardVisdat/issues">Request Feature</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#tentang">Tentang Projek ini</a>
    </li>
    <li>
      <a href="#alsumdat">Alat dan Sumber data</a>
    </li>
    <li>
      <a href="#tahapanperancangan">Tahapan Perancangan</a>
      <ul>
        <li><a href="#pengumpulan">Pengumpulan Data</a></li>
        <li><a href="#preparasi">Preparasi data</a></li>
        <li><a href="#visualisasi">Visualisasi Data</a></li>
        <li><a href="#dashboard">Pembuatan Dashboard</a></li>
      </ul>
    </li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->

## Tentang Projek ini

<div id="tentang" align = "justify">
Jumlah investor pasar modal selama 2018 -Maret 2022 mengalami kenaikan jumlah investor sebanyak 223.39 persen. Sekitar 85 -90 persen investor akan gagal, hal tersebut disebabkan para investor cenderung spekulatif dalam berinvestasi khususnya pada instrumen saham. Tindakan spekulatif dapat menjerumuskan investor kedalam kerugian. Dashboard interaktif adalah dashboard yang dalam memvisualisasikan data membutuhkan perhatian pengguna, artinya informasi dapat ditampilkan secara dinamis tergantung apa yang diatur oleh pengguna. Dengan melakukan visualisasi data laporan keuangan perusahaan secara interaktif dapat dengan mudah memahami fundamental dari suatu perusahaan dan tentunya akan mengurangi tindakan spekulatif dalam berinvestasi. Dewan Syariah Nasional - Majelis Ulama Indonesia (DSN-MUI) mengeluarkan fatwa yang mengatur Prinsip Syariah di Pasar Modal termasuk pengkategorisasi saham syariah. Pada projek ini akan mengimplementasikan dashboard interaktif pada data laporan keuangan untuk menganalisis fundamental saham ISSI yang bersumber dari Yahoo Finance.
</div>
<p align="right">(<a href="#top">back to top</a>)</p>

## Alat dan Sumber Data

<div id="alsumdat" align = "justify">
1. Alat

- [Tableau](https://www.tableau.com/)
- [Python](https://www.python.org/)

2. Sumber data

- [Yahoo Finance](https://finance.yahoo.com/)
</div>
<p align="right">(<a href="#top">back to top</a>)</p>

## Tahapan Perancangan

<div id="tahapan perancangan" align = "justify"></div>

### Pengumpulan data

<div id="pengumpulan" align = "justify">
Pada penelitian ini akan menggunakan data dari Yahoo Finance diambil dengan cara web-scraping. Pengambilan data mengguanak bahasa python dengan package yfinance. Karena platform yahoo finance tidak hanya terdapat data saham dalam negeri perlu melakukan seleksi data dilakukan untuk hanya mendapatkan saham-saham syariah yang listing di Bursa Efek Indonesia.

1. Mengimport list nama saham syariah

```sh
  import pandas as pd
  import yfinance as yf

  data = pd.read_excel("Index Member ISSI_WINR.xlsx")
  names = data["Kode"] + ".JK"
  names
```

2. Melakukan scraping

```sh
  info_table = pd.DataFrame()
  hist_table = pd.DataFrame()
  actions_table = pd.DataFrame()
  annual_financials_table = pd.DataFrame()
  annual_balance_sheet_table = pd.DataFrame()
  annual_earnings_table = pd.DataFrame()
  annual_cashflow_table = pd.DataFrame()

  for name in names:
      #info
      stock = yf.Ticker(name)
      info = pd.DataFrame([stock.info])
      info["stock"] = name
      info_table = pd.concat([info_table,info])

      #historical
      hist = pd.DataFrame(stock.history(period="max"))
      hist["stock"] = name
      hist_table = pd.concat([hist_table,hist])

      # actions
      actions = pd.DataFrame(stock.actions)
      actions["stock"] = name
      actions_table = pd.concat([actions_table,actions])

      #financials
      annual_financials = pd.DataFrame(stock.financials)
      annual_financials = annual_financials.transpose().reset_index()
      annual_financials["stock"] = name
      annual_financials_table = pd.concat([annual_financials_table,annual_financials])

      #balance sheet
      annual_balance_sheet = pd.DataFrame(stock.balance_sheet)
      annual_balance_sheet = annual_balance_sheet.transpose().reset_index()
      annual_balance_sheet["stock"] = name
      annual_balance_sheet_table = pd.concat([annual_balance_sheet_table,annual_balance_sheet])

      #cashflow
      annual_cashflow = pd.DataFrame(stock.cashflow)
      annual_cashflow = annual_cashflow.transpose().reset_index()
      annual_cashflow["stock"] = name
      annual_cashflow_table = pd.concat([annual_cashflow_table,annual_cashflow])


      #earnings
      annual_earnings = pd.DataFrame(stock.earnings)
      annual_earnings = annual_earnings.transpose().reset_index()
      annual_earnings["stock"] = name
      annual_earnings_table = pd.concat([annual_earnings_table,annual_earnings])
```

3. Menyimpan data

```sh
  info_table.to_csv("info_table.csv")
  hist_table.to_csv("hist_table.csv")
  actions_table.to_csv("actions_table.csv")
  annual_financials_table.to_csv("annual_financials_table.csv")
  annual_balance_sheet_table.to_csv("annual_balance_sheet_table.csv")
  annual_earnings_table.to_csv("annual_earnings_table.csv")
  annual_cashflow_table.to_csv("annual_cashflow_table.csv")
```

</div>

### Preparasi data

<div id="preparasi" align = "justify">
Setelah mendapatkan saham syariah, dilakukan preparasi data untuk membentuk variabel yang dibutuhkan.  Data masih memiliki satuan rupiah dan US dollar sehingg dilakukan konversi ke rupiah dengan mengali nilai tukar rupiah setiap akhir tahun. Variabel tersebut dapat dikategorikan dalam berbagai dimensi. Dimensi, variabel, dan rumus preparasi data yang digunakan adalah sebagai berikut.
</div>

#### Tabel dimensi, variabel, dan rumus preparasi data

[![Tabel][tabel]](images/table.jpeg)

### Visualisasi Data

<div id="visualisasi" align = "justify">
Data yang telah diseleksi kemudian dilakukan visualisasi dengan mengggunakan Tableu. Setiap data akan divisualisasikan kedalam berbagai bentuk visualisasi. Berikut merupakan jenis visualisasi dan beberapa contoh variabel yang digunakan.

1. Table

   [![Table][table]](images/table.png)

2. Bar chart

   [![Barchart][barchart1]](images/barchart1.png)

3. Line chart

   [![Linechart][linechart]](images/linechart.png)

4. Candle stick

   [![Candlestick][candlestick]](images/candlestick.png)

</div>

### Pembuatan Dashboard

<div id="dashboard" align = "justify">

Berbagai bentuk visualisasi disatupadukan kedalam dashboard. Pada tahapan ini dilakukan modifikasi terhadap visualisasi yang dibuat seperti penambahan filter, harmonisasi warna, tataletak, penambahan legend dan sebagainya.

</div>

#### Tampilan dashboard dark mode

[![Tampilan Dashboard][dashboard-black]](images/dashboard-hitam.png)

#### Tampilan dashboard light mode

[![Tampilan Dashboard][dashboard-white]](images/dashboard-putih.png)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- CARA MENGAKSES -->

## Cara Mengakses

<!-- <div id="akses" align = "justify">
Untuk mengakses dashboard dapat mendownload file <strong>DASHBOARD.twb</strong>. Setelah itu, sesuaikan konfigurasi data pada file .twb dengan <strong>data laporan keuangan.xlsx</strong> dan <strong>data harga.xlsx</strong>. -->

Jika hanya ingin melihat cukup mengunjungi laman <a href="https://public.tableau.com/app/profile/fauzan.faldy.anggita/viz/DASHBOARD_16538161920600/Dashboard2?publish=yes">berikut.</a>

</div>
<p align="right">(<a href="#top">back to top</a>)</p>

<!-- CONTACT -->

## Contact

<div id="contact" align = "justify">
Fauzan Faldy Anggita - fauzanfaldy4@gmail.com

Linkedin : [https://linkedin.com/in/fauzanfaldy](https://linkedin.com/in/fauzanfaldy)

Project Link: [https://github.com/oojn4/DashboardVisdat](https://github.com/oojn4/DashboardVisdat)

</div>
<p align="right">(<a href="#top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[contributors-shield]: https://img.shields.io/github/contributors/oojn4/DashboardVisdat.svg?style=for-the-badge
[contributors-url]: https://github.com/oojn4/DashboardVisdat/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/oojn4/DashboardVisdat.svg?style=for-the-badge
[forks-url]: https://github.com/oojn4/DashboardVisdat/network/members
[stars-shield]: https://img.shields.io/github/stars/oojn4/DashboardVisdat.svg?style=for-the-badge
[stars-url]: https://github.com/oojn4/DashboardVisdat/stargazers
[issues-shield]: https://img.shields.io/github/issues/oojn4/DashboardVisdat.svg?style=for-the-badge
[issues-url]: https://github.com/oojn4/DashboardVisdat/issues
[license-shield]: https://img.shields.io/github/license/oojn4/DashboardVisdat.svg?style=for-the-badge
[license-url]: https://github.com/oojn4/DashboardVisdat/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/fauzanfaldy
[product-screenshot]: images/screenshot.png
[dashboard-black]: images/dashboard-hitam.png
[dashboard-white]: images/dashboard-putih.png
[table]: images/table.png
[linechart]: images/linechart.png
[candlestick]: images/candlestick.png
[barchart1]: images/barchart1.png
[tabel]: images/table.jpeg
