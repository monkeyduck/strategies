
> 策略名称

抛物线转向SAR与价格高低点策略

> 策略作者

littleDreamXX

> 策略描述

- 策略名称：SAR和价格高低点策略
- 数据周期：1H等
- 策略支持：商品期货、数字货币现货，数字货币期货
- 官方网站：www.quant.la

![IMG](https://www.fmz.com/upload/asset/4d9459b9af47702f7c2c6666212927c7.png)

> 策略参数



|参数|默认值|描述|
|----|----|----|
|SLOSS|2|止损止盈百分比|
|N|60|收盘价高低点周期|
|IsCryptoCurrency|0|选择用于商品期货或者数字货币: 商品期货|数字货币现货|数字货币期货|
|CryptoCurrencyTradeAmount|true|用于数字货币市场时的每次交易下单量|


> 源码 (麦语言)

``` pascal
(*backtest
start: 2018-01-01 00:00:00
end: 2018-12-02 00:00:00
period: 1d
exchanges: [{"eid":"Futures_CTP","currency":"FUTURES"}]
args: [["IsCryptoCurrency",2],["ContractType","rb888",126961]]
*)

// 参数
// N:=60;
// SLOSS:=2;
// FUND:=20000;

LOTS:=IF(IsCryptoCurrency, IF(IsCryptoCurrency=1, CryptoCurrencyTradeAmount, MAX(1, INTPART(CryptoCurrencyTradeAmount))), MAX(1,INTPART(MONEYTOT/(O*UNIT*0.1))));
SARLINE:=SAR(4,2,20);

B1:=SARLINE>0;
S1:=SARLINE<0;
B2:=HIGH>=HHV(CLOSE,N);
S2:=LOW<=LLV(CLOSE,N);

BUYK:=BARPOS>N AND B1 AND B2;
SELLK:=BARPOS>N AND S1 AND S2;
SELLY:=S1 AND S2 AND BKHIGH>BKPRICE*(1+0.01*SLOSS);
BUYY:=B1 AND B2 AND SKLOW<SKPRICE*(1-0.01*SLOSS);
SELLS:=C<BKPRICE*(1-SLOSS*0.01);
BUYS:=C>SKPRICE*(1+SLOSS*0.01);
BUYK,BK(LOTS);
SELLK,SK(LOTS);
SELLY,SP(BKVOL);
BUYY,BP(SKVOL);
SELLS,SP(BKVOL);
BUYS,BP(SKVOL);
```

> 策略出处

https://www.fmz.com/strategy/128251

> 更新时间

2018-12-05 18:14:25
