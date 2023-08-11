## AsyncRAT Analysis

Read this article on my webiste for more details, [AsyncRAT: Config Decryption Techniques and Salt Analysis](https://www.securityinbits.com/malware-analysis/asyncrat-config-decryption-techniques-and-salt-analysis/)

### AES Salt
I've reviewed approximately 10-20 files and have only encountered **two instances** of salts for AsyncRAT.
1. bfeb1e56fbcd973bb219022430a57843003d5644d21e62b9d4f180e7e6c33941 (Format: HEX)
> The same salt that is referenced in the AsyncRAT source code available on Github (Last commit: May 10, 2020) , is also utilized in Quasar RAT 1.3, as indicated in the [QuasarRAT-Analysis](https://github.com/JPCERTCC/QuasarRAT-Analysis/blob/main/quasarrat_decode.py) from December 1, 2020.
2. DcRatByqwqdanchun (Format: UTF8)
> DcRatByqwqdanchu , this salt also used by DcRat malware (First commit: Mar 20, 2021 ).

#### Salt 1 - bfeb1e56fbcd973bb219022430a57843003d5644d21e62b9d4f180e7e6c33941 (Format: HEX)


| Md5                                | VT First Seen |
| ---------------------------------- | ------------|
| a4135f2786b618342c40ec68df21f10a   | 2021-08-23|
| dea3149aae31bd4116adba54840af10f   | 2022-05-03|
| 213634fbd77e86f7a9708fd654f8d7e0   | 2023-06-11|
| a4c35dcd0013a04666a9d58095ff4060   | 2023-07-27|


#### Salt 2 - DcRatByqwqdanchun (Format: UTF8)
| Md5                                | VT First Seen |
| ---------------------------------- | ------------|
| b6dbbded2ca82116c6da9bae34cfa6e9   | 2022-04-27|
| 37e965330586a51125db2a420917db17   | 2023-07-24 |

### Decrypt config using CyberChef Recipe
- [CyberChef Recipe](CyberChef_Recipe.md) 


