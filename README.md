# Finger-Multi-EMG-Measurement-Slave

## 概要
指を動かした時の筋電位の取得プログラム(スレーブ側)

8つのMyoScanをDMAを用いてAD変換してデータをUSARTで送信するプログラム\
NUCLEO-F091RCによる12bit-ADCのため、0から4095の間の値が取得可能(0から3.3V)

【送信データ構造（length = 20）】\
length = 0 ：　ヘッダ（int型で0xFE）\
length = 1 ：　復元後のデータの数（int型で9）\
length = 2 ：　MyoScanのCH0から得られた値の上位2桁（int型、CH0の値が4095ならここの値は40）\
length = 3 ：　MyoScanのCH0から得られた値の下位2桁（int型、CH0の値が4095ならここの値は95）\
length = 4 ~ 17 ：　MyoScanのCH1〜7から得られた値を代入、length = 2, 3の格納方法と同様\
length = 18 ：　パリティ（int型、length = 2 ~ 17の値を全て足して0x00FFとANDをとったものを代入）\
length = 19 ：　フッタ（int型で0xFF）
