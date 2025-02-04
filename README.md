# 台語文數位典藏資料庫

## 介紹
本資料是國史館收集的臺語文資料，目前在網路上有兩個網站。這兩個網站大部份的內容相同，不過可能文章會有小小的不一樣，可以比較著看
* 國史館網站：<http://xdcm.nmtl.gov.tw/dadwt/pbk.asp>
* 楊允言老師網站：<http://ip194097.ntcu.edu.tw/nmtl/dadwt/pbk.asp>

## 資料夾說明
本資料庫包含臺語漢羅及全羅對應的語料，以下依處理方式說明

### 原始文字檔
此資料夾的資料是台文館計劃ê文本, 是 plain text。
* `kp`表示劇本
* `ks`表示歌詩
* `sb`表示散文
* `ss`表示小說。
* `pbk.xls`是目錄
* `pbk_校對.xls`是校對後的`pbk.xls`

#### 勘誤
原本的資料有一些缺失，以下整理看到的部份。不過這勘誤是很久以前做的，可能不是很完整
* `資料路徑`
  * 勘誤說明
* `kp/K/1957/KP.1957.Ng5 Hoai5-un.Seng3-kek8 te7 1 chip8_04.tbk.txt`
  * 後壁減一字「完」
* `sb/K/1959/SB.1959.Bo5 chu3-beng5.Kim-ku3 E5 Kou3-su7_05.tbk.txt`
  * 5/24～5/30的內容無去矣
    * 佮<http://ip194097.ntcu.edu.tw/nmtl/DADWT/thak.asp?id=873>仝款，減幾仔段
  * 我去掠網站TTS的網址補的
    * 會當參考<http://xdcm.nmtl.gov.tw/dadwt/thak.asp?id=873>
* `sb/J/1940/SB.1941.Ko Kim-seng.Ko Tek-chiong e5 Sio2-toan7.tbk.txt`
  * 應該下佇1941年的資料夾
* `pbk.xls`
  * 有2169篇，目錄干焦2163篇
  * 改的是`pbk_校對.xls`

### 段落對齊
本資料夾的資料是`原始文字檔`經過上面的勘誤處理後，用[網站介面](https://github.com/sih4sing5hong5/soo3_ui7_tian2_tsong5_kau3_tui3_kang1_ku7)共漢羅和全羅的段落先對齊，才閣一句一句對齊。對齊後仍有誠萬句漢羅和全羅無法對齊。

裡面的檔案是`sql`檔，裡面有四個`sql table`：
* `原始段落資料`
  * `原始文字檔`匯入的結果
* `改過段落資料`
  * 人工從`原始段落資料`改成一段對齊一段
* `原始逝資料`
  * 用程式把`改過段落資料`轉成一句一句
* `改過逝資料`
  * 人工從`原始逝資料`改成一句對齊一句

當初是使用`postgres`，不過`mysql`應該也能匯入

### JSON格式資料
sql資料轉做電腦較好處理的json

#### 先得著csv
```
$ echo drop schema "台語文數位典藏" cascade\; | psql
$ bzcat 段落對齊/段落對齊.sql.bz2 | psql 
$ psql
=# Copy "台語文數位典藏"."改過逝資料" To '/tmp/nmtl.csv' With CSV DELIMITER ',' HEADER;
```
### 轉做json
```
$ python3 csv2json.py  # nmtl.json
```
