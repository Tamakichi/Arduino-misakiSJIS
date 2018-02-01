# Arduino-misakiSJIS

Arduino用 美咲フォントライブラリ 教育漢字・内部フラッシュメモリ乗せ版

## 概要
Arduino用の美咲フォントドライバライブラリのシフトJISバージョンです。  
フォントを教育漢字1,006字(小学校で習う漢字）＋ひらがな・カタカナ・記号・半角等の1,710字に絞って  Arduino Uno(Atmega328)のフラッシュメモリ上に格納しました。  

※Arduino IDEでは文字コードにUTF-8を利用しています。スケッチ内で直接日本語を含む文字列を扱いたい場合は、[Arduino-misakiUTF16](https://github.com/Tamakichi/Arduino-misakiUTF16)をお使い下さい。  

収録文字  
![対応フォント](img/教育漢字.PNG)  

## 仕様
* 文字コード  シフトJIS  
* フォントデータ  8x8ドッド美咲フォント  
  美咲フォント： http://www.geocities.jp/littlimi/misaki.htm  
* 利用可能フォント数  1,710字（Arduinoのフラッシュメモリ上に格納）  
  * 漢字 教育漢字 1,006字(小学校で習う漢字）  
  * 非漢字 全角 546字(全角英数字、ひらがな、かたかな、記号)  
  * 半角フォント  158字(半角記号、半角英数、半角カタカナ）  

## 利用可能API
* **半角コード(記号英数字、カナ)をSJIS全角コードに変換する**  
  `uint16_t HantoZen(uint16_t sjis)`  
  引数  
  `sjis`: 文字コード(IN)  
  戻り値  
  変換したSJIS文字コード(指定したコードが全角の場合はそのままコードを返す)  


* **SJISに対応するフォントデータ(8バイト)取得**  
  `boolean getFontDataBySJIS(byte* fontdata, uint16_t sjis)`    
  引数  
  `fontdata`: フォントデータ格納アドレス(OUT)  
   `sjis`: SJIS文字コード(IN)  
  戻り値  
  true: 正常終了 false: 異常終了  
  対応するフォントが無かった場合、フォントデータとして豆腐（▢）を返す。  


* **UTF8文字列に対応する先頭文字のフォントデータ取得**  
  `char* getFontData(byte* fontdata,char *pSJIS,bool h2z=false)`  

  引数  
  `fontdata`: フォントデータ格納アドレス(OUT)  
   `pSJIS`: SJIS文字列(IN)  
   `h2z`: 半角全角変換指定(IN)  true：全角変換あり false 全角変換なし(省略時デフォルト)  

  戻り値  
  変換を行った文字の次位置のアドレスを返す(文列末は0x00を指す位置となる).    
   取得失敗時はNULLを返す.  


*  **フォントデータテーブル先頭アドレス取得**  
  `const uint8_t* getFontTableAddress()`
 *  引数  
    なし  
 *  戻り値  
     フォントデータを格納しているデータ領域の先頭アドレスを返す.  
       アドレスはフラッシュメモリ領域であるため領域参照はpgm_read_byte()を利用する必要がある.  


* 利用可能フォントの検索    
  `int findcode(uint16_t  sjis)`  
 * 引数  
    `sjis`: SJIS文字コード(IN)  
 * 戻り値  
     指定したコードに対するフォントコード(0～1709)を返す.  
       該当するフォントが存在ししない場合は-1 を返す.  
       本関数で取得したコードはフォントデータテーブル上の格納順番を示すコードである.  
       フォントの格納アドレスの取得は次の記述で行える.  
        getFontTableAddress()+findcode(sjis)*8  

## サンプルスケッチ
banner  
![banner](img/sample.png)  


