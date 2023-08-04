### AsyncRAT Config extraction Recipe

#### Required 
- Input code
- Salt
- Variable name that stores the AES key
  
```
Regular_expression('User defined','public static string \\w+\\s+\\=[ "0-9a-zA-Z+/=]{12,}',true,true,false,false,false,false,'List matches')
Find_/_Replace({'option':'Simple string','string':'public static string '},'',true,false,true,false)
Register('([\\s\\S]*)',true,false,false)
Comment('Input: In the filter operation box below, add the variable name that stores the AES key, e.g. Key')
Filter('Line feed','',false)
Regular_expression('User defined','(?<=\\")(.+?)(?=\\")',true,true,false,false,false,false,'List matches')
Register('([\\s\\S]*)',true,false,false)
Comment('Input: If you don\'t know the salt value, then try using the following salts. Don\'t forget to select the proper salt format HEX or UTF8.\n\nI have encountered only 2 salts for AsyncRAT:\n\nbfeb1e56fbcd973bb219022430a57843003d5644d21e62b9d4f180e7e6c33941 (Format: HEX)\n\nDcRatByqwqdanchun (Format: UTF8)')
Derive_PBKDF2_key({'option':'Base64','string':'$R1'},256,50000,'SHA1',{'option':'Hex','string':''})
Register('([\\s\\S]*)',true,false,false)
Find_/_Replace({'option':'Regex','string':'.*'},'$R0',false,false,false,true)
Comment('Input: In the filter operation box below, add the variable name that stores the AES key, e.g. Key')
Filter('Line feed','',true)
Subsection('(?<=\\")([0-9a-zA-Z+/=]{32,})(?=\\")',true,true,false)
Fork('\\n','\\n',false)
From_Base64('A-Za-z0-9+/=',true,false)
To_Hex('None',0)
Drop_bytes(0,64,false)
Register('(.{32})(.*)',true,false,false)
Drop_bytes(0,32,false)
AES_Decrypt({'option':'Hex','string':'$R2'},{'option':'Hex','string':'$R3'},'CBC','Hex','Raw',{'option':'Hex','string':''},{'option':'Hex','string':''})
Merge(true)
Find_/_Replace({'option':'Regex','string':'$'},'\\nAES_KEY = "$R2"',false,false,false,true)
```

### Example
#### dea3149aae31bd4116adba54840af10f (2022-05-03)
- Use this Salt bfeb1e56fbcd973bb219022430a57843003d5644d21e62b9d4f180e7e6c33941 (Format: HEX)
- Key is the variable name for AES key, input Key in Filter operation 

#### Input
```
	// Token: 0x04000001 RID: 1
		public static string Ports = "2kO0etQ84KjueTC0FJxITKqUqYGcQKRllhPySUfrzxzPV6TOFvFQ6m8MgGruVdWAIYY6b/HZrtKtcVI1LR7tJg==";

		// Token: 0x04000002 RID: 2
		public static string Hosts = "R/Zn85hBezXbnM/Y2Gd6titq6YBHKmc/ysTNXRAVMtJGV3OiWNEo1b6WUUZKH/8soSAIRQkBKBnuTDtDN3KPi+vkA3QQPIsg0zcdz19fE7c=";

		// Token: 0x04000003 RID: 3
		public static string Version = "CPhxmvZ2hVfyOk+2ruGqdh2d+rxslfXFjfDqX+I1o3DGdiaXwQmTjHaLFh2SOFGmsuZnoz/N73LqYnsni4g3lA==";

		// Token: 0x04000004 RID: 4
		public static string Install = "EsjUtjsZicXNqFV1Rzp4n2/r0NS/YKIDH3i2xnYPsDO1xKSx33b7VFapBX+F7dOFtGkGQsuw0X72iagrs1dZQg==";

		// Token: 0x04000005 RID: 5
		public static string InstallFolder = "%AppData%";

		// Token: 0x04000006 RID: 6
		public static string InstallFile = "2.exe";

		// Token: 0x04000007 RID: 7
		public static string Key = "cWRkdm1TRlllbVJnUjl1NWRGam9NMUFnZjVuN0hSUHg=";

		// Token: 0x04000008 RID: 8
		public static string MTX = "op60PkrIq7x8mN1vU5fm8p+6xSEzgxWZcZTXeQQj+y4cgOXqiAW3ZnbRCOj2jJTBvrCvYG2v8ILcser1C+um2wEgn/Osv3iXskh8hAOuvfA=";

		// Token: 0x04000009 RID: 9
		public static string Certificate = "dECkkt32/jIs0Ut31fgZxjH/p0ctFubnQogB2niq0Y7Vg40UeydTIWZM7G4+hzwn9PAws7Q9cG21zRzIUplqv3jZ3zf9tHWTWdzCuaqd1k0U9kYcsiNgWlSRx0z1eWLUN1SmFyUNpE8fEFOvH/ZmnSmoSRsnFyrNyb7EXQQDGOYYIN09gH8KbolkoIpR6iOXSo9H2jbia4WYPeS806jZja+PlsgiVyGG9tTrnDAOAP0aAWvCAzyQETo4bBuz8+4Dq62D8B62E/tX7UYWX4qIB6dcmYE/2L6+KSKubMGAq/lJgsb48yapeS7BLuozKxKfCcOSAHSSvYaKJbvZpJEiHhCH6kThsnWXcdGEWYw9LdKhg1z+gN8YaOwOniZu5naur9MfVHBE34cHQ7t0hB6azU3UCyzgGsruckDphzMfjFlfzs0GUtFK25F2oZSAp/drZSGsFtMDXhszWJ94zXDit8onJmhnfis9QIv+XBGXI6+hV7ZsfL5NbhKz6qrG00XySDejJ+3VmNvI7tIJwPAhMuOviRVi0Nzn7RBhAt0/g/wg6BqLfUFc853D6ERu91Veclu/HmtwNgsh+yuNcu97FHDRUGQ9baCWN87xygLAg66ImmR4R5VCZO13zYNw4Z1zn8BPMMZdcuBGj51r+UPONVBxycJBcWbhTADCSQICyTEc5WD32h9FM+cP6Vss0QW2Y+PXpjqjH6PtMBw6eLb9RFRqwRWvwHXR13vv4TXQfsWexK/fZ4uy28nSF5ppBGBCQYgFG69eGKqeC7r+9+fZesVym2QLsNFHjRPYaN+an3dFQ/Ljk6npikorWZDNHvRHAhGHPuyXB2VU64sLsj2fszz404F7BM7KOQFCgzNrrhrscPhbUvV0qP6E4F15DOftAAvHljXO7rAjvfKEhii/Zm+WLWOsxo28pMDakWiCI1dG8AFLISDFDaFauv/lJXn7fiJ2MYipE3brDG8ZVbfKMhI9oDCSLc2DrkVjIA99UXUY/clTLqLdg+OkLV+ZtefSk2CExQU+Ij3DpRM90OUbhzwAFQMOVYTwIiKBmZUekHM+baLJJsT3W/UrzKN5oler0pqUav8nPo9CIdd1TbpRwn2ekzRAQFdNkIysVyd+5AXgYqjmVsjmBtWmWb/e9CnvOahbBYgaC/SEoHabSkfZ6NYEcFwtSLbJPmuBeaK7g0Ftw+CMchCZIItRkwLh21rdZ0LX/HVwy1toUzwDQNPkFXC9Ce8qK1IwxSWoRC6aMYaIUFZG/Rug9MZULWN2UgP1/EITrJjrt17pId1OHb69Tr+U+tzk9uAWVrx46pCKLzdyDEawpuJ1O3KjkMLAPwzSXRzdfQ0ZoUHghE0p8Gx/jSkosGbc3kOZtuKru6xrTEJLKK8r+MEVOizZ8RTLtMa0y0h/w7DAT2U5VOpkKqbQR/SaHEGdOQzD7b6lZ0KcnfGSTtzO+L61DsmvR4O5j5ebQoQCp2k+Wt9DlM+UkdillG9aaFzxvShbMF11Sc9Q86rRDAQwg9bLFUaxTRZmO5UdAlvHdMm6S+P4fQBqbizakijdGT8/grUlNma6VcCaD+FeGTfDaYGa1csfj4iicB6qdVmnPSO9J9x87EkbM9euCXriao2W8S393YdgzSHzz7aszasK6u+LbTziVvsZsfdOk5RuT+sNKbPt2ukBZOmzUUsGdBfqpzN6YfEZ4I9K+3NNi8q3qLIJwL8naV5vGjDP+F9GTl4xEeejAfj2VLXF/wCW9s0m4FMJKt/bfM/zR8qllqbx+v8F1aVYiKIOlVuiJay9m8d7ScVLHHUO/Sy/+mYOyOOqNow7C3Az+H670CAaGUt/7sk4E5QgVzsxO8ikuY+JFLxxqBEW77srd0WFDRWC/OQNV5iSwM6l2WkEdi/o4VedpoIyhKiuPPTpUSoK+uodFpwQYcDY+NtbNGOC18UiXsiJYA7DaLiEKd8jjaoOODbhpyr528acdqfoYPHn33KUJ9+LRClb9LdPvgvX8Si+s9BF5ob8VFWbvmeAW9Katca3Cp+uHM67gMHoJn+uAYd6ww7pcZ8pd0y3fYgKyGeB4Q2zorNN+KoekJY1CZw7AY9kg2/w10XUGqkB6PAZyiwM5ZX1tPav8qgFpY0Xgx6xofN4NsgY2m35XbCRwMwWhwHwsvy5Wa6ZgpV0orzss5YaFM32lTUu7VUerxSc5W8wSzpXQJWWMHu32bTBiIYaX9KMG57pLonIF84zJnEokFst1x78MXFo62qgm3cKEXPvfIKRVZKx2akEktIVqfK4Tk4xMHRp8aDAcj9oaKATC60qKNq8sqHnzjjGzbGwUSnn51SAm6hdyuYKUTF3bMM=";

		// Token: 0x0400000A RID: 10
		public static string Serversignature = "tAq47cmAdduFXCjCT8UaNyeXOugyXLZJ1KbP5f1wIkGIRYOH5wz6j3LhGpZTyFjOhR7opTAkMhed1IX4wHKQlmvCxICeuSMOuEiF4kIiOe6SDY9Z8ZrsyR7GxU9nkisl5i+NngDAgP9Tsh+RkA05umBz7+hdlaV6BCsj4kbyM97/XidnYHUD3/mO+rzkse5JzhKWdPsycGxh9wRWcylhHrSfL3TLj6Wwuciuwm6M5EBTrt/+cmiEySlQv/II9O1M+fZpQj+g1dARRryzkLEkpm1jpoo3h476um+RO3etLDY+hQ9x+M0aNK5TsoNvJ2zURTV8K/GGaW+KUxXzWosGp+9iYkK04VWyTixL4t6oeJ0C+Zr0/jtMkBCSgXBJNQIlvIjWEEAkObq9Gm8IQeJ/ha3rXEXCSHtS7N+A6r6MjwIx6JMMS8W8cr6v+FLBifbo1JTdpWJDEVTFrNUUMsE7IoLlQm9BKdPzAk75jNcysa7c5/2w1DvHgwfU0tTZErnV56lzHKcvfCjeFfE1JbojkrxQpKuluxqbh7HmT/3enoRltgylhwzIR/OiiyD5VYi/c6BuI3JqyTNm+VnLiE79jSUbOvtYC4v3RTmh/mYoGwpdvlnimu1yNcP1pnzYz0iBag7uDXnDzo/b1Ddk6Pun6/zT9SO8gucPQH/GWEm11j1fq2Km3BF0Tqnt5CsRz+7kQvk2H9gRFT0VqYFypz+E8/B9QUCUMtE3k5mzUV8mVM6YmE/W5BjJPIJMpK0sNcfAQgBZeDM1jFEUdD3541LfvmVoCCB7UYuZ11alg83WIZ8n05cAlxIvuG5tswrezZ5d6UbtzG4Es6vsUtyRDz5C2ma9+qxrkTSsc+0i3wN38CNcUwjEqBQugUPJw11M2V1Y1zc/oK/leyWt+SnlHj+rh+NlWxm22qV4sbEtqJdPq/zQGWsq4bPdJzIferuUhfehOKxg3iSDZkPK8S719ktiCw==";

		// Token: 0x0400000B RID: 11
		public static X509Certificate2 ServerCertificate;

		// Token: 0x0400000C RID: 12
		public static string Anti = "zzqMXx/UFJ8ckJeF/L+xtY2pgS5erGn1CXsCEnExWawD/QI2ey8DdPRiMdfnrI4olkE6o9fT5jmfg9N1B0nX9w==";

		// Token: 0x0400000D RID: 13
		public static Aes256 aes256;

		// Token: 0x0400000E RID: 14
		public static string Pastebin = "j2vlg+ayNzjtGuRO+z6coRx2h8/7jOtan5JToG8q2f6mj0dUurE9f8uY9E0cL7VEDxYbZOBZ/M20T/HztKF2Yg==";

		// Token: 0x0400000F RID: 15
		public static string BDOS = "GXFl6EJua3KSXkUTVAL0/a7DIsAvJDboXEjztO8iOHTgrFhHOHwca5KwHLwa36Lg9uPB1ngWZcgGHNjr46u4ug==";

		// Token: 0x04000010 RID: 16
		public static string Hwid = null;

		// Token: 0x04000011 RID: 17
		public static string Delay = "3";

		// Token: 0x04000012 RID: 18
		public static string Group = "5JUbUXdg3M25dPpjn4REHek7dH5zt5BuJDgEjfNao1iSiTHbV+g2kJpjZfyylOk+sRIXH/tYKwTBvBhxZH4Q/Q==";
	}
}
```


#### b6dbbded2ca82116c6da9bae34cfa6e9 (2022-04-27)
- Use this Salt DcRatByqwqdanchun (Format: UTF8)
- Key is the variable name for AES key, input Key in Filter operation
```
// Token: 0x04000001 RID: 1
		public static string Por_ts = "SCPH5d+ou7xeC99DTH6uTxbQq5wVkaq/EP2UlwCnUB8EUVvGZeZ8Dd8eLC+C1MitBV9w22rCJknmSG3qTOLhYA==";

		// Token: 0x04000002 RID: 2
		public static string Hos_ts = "3jce4cFAiw5WQci2Ofl9GXYvvxoVWeY0hCunY9BEdpaEpLYgygGQfDK8KZ9zWfk7Tm+HPrGCvutqWkwlX/1qxQ==";

		// Token: 0x04000003 RID: 3
		public static string Ver_sion = "5jCSotLkfxFdwYjYLQIFP7eNbxMiltvdwKbCAs+bFFtej6UHvMqO0MsMf2ZFCMRM0pRlsKY5lQCPlXOyBu0wVA==";

		// Token: 0x04000004 RID: 4
		public static string In_stall = "/22mN5sXdwI68CwiMBT7FyhLtf+OyGI+dcEHdmMmvFmI+HgvDdWApMTfFuFkSW8HY8WlNANdOrOpFAi9bbhWOg==";

		// Token: 0x04000005 RID: 5
		public static string Install_Folder = "%AppData%";

		// Token: 0x04000006 RID: 6
		public static string Install_File = "";

		// Token: 0x04000007 RID: 7
		public static string Key = "ajJmQktnbmhEN3FiTFhIaDc4UnFDMW80Zk05WEZnT2s=";

		// Token: 0x04000008 RID: 8
		public static string MTX = "g5Bmk2l9lNEOLaG18le9vUKncl/YYLHV6CkVOvCFeQG9HTQGVyCS8HfRNBb3TSHM58O425wDuFQd+/26pJ9lGwsJemqgaOcM+WnkD9Um9Qg=";

		// Token: 0x04000009 RID: 9
		public static string Certifi_cate = "jFlp0vdmd78Lbke/zM7jBCPNI0QLi3/v8jHraCe9GEF815nkf0M235Li6M8OYbjiMgpBwybjIgS1U4tOheNBmEUcLzZKmJBytKgm9VH/NvGk2ON9XnUeCEeNBKzDxt4qEZ4pZB8WrLLaurZMmw5CK+OBFoXO2/6azeSSPcgv+AUbbT3ViqrTrbCmXdhz7BNbOg5c41VUMN98LOgz9QimAy8zrGOD9CEh7ZICnGz3kT3lxqx3etDvJL9gOeMr0q83a8IOb/OVCdR8aREBqGvvBsojmoPvJzOV0DB/T9rOSSPo+5OOSFqrgVcn4lkkia8U0s6Gd8jkCucD9U03V4VMdpGMWHDsNqUfMTA/+sHNnZPm5UPuloUGGZy1k7QtsoYPhyJrIcoeUzCpmLAdTDlIrd76Rzk2C7hi3h1QKkNSewYTeRSbyj1GXJ13NI6diwLGHvBhPyNGiVs7VxI6c/k9W8iMuw7eJSWNsJlgDkpvEZuPfKWKgHYwhAUQkKSep9FCupxcPPBZA3XhRsFUc+ewO4Txh1X1N+rLQw6ZNQPwwCoDVDsnOqRMJOkDkyyLWUO+PHhWVy18Okk23jSU71R/D/3tRL9T4NF0M7FzczBatP5hXoklFQbCm3apcPHvvun/YvscHd3GN3YTOUY0M5QMR1J95tH7vOPDdsa5hAtcsB9uYpqjHS3g47EtC+46JdL2/8+skAfIXMKaBsxyPfBklI3t4wSg6P0v5AnCYAuo3kM+AZNaxu4jo8KCa5EAEgsQIx+DYH7wyJoeIJ81iieJROEAZwTYlg2KjeRs3bLUyC684a5tTSnoAp909k/5ReXlrT0UEK0FWkjgQmmGxg7Wg4Q0GccAj+3jgoHPHK8mMLSqBJmFP1mq5yG7k3ne4GgQOc6Co8ETYF1/dnybIMw42r3Oche9dF1+Sg1agVxlm7BuuLOlaz9Y7k/w0Xx88lRL3WHPwcCgDub4hQTEFqqf18BUdh/VZBuVCjirXZcDfUdSAHhnVmEIxmDwd1F0ByOfGMldHeuq971j1McI0/U8JAsb6nh6RLbF9EGSF4/ity0dqMNFep6w1yiOmKNbz+UK";

		// Token: 0x0400000A RID: 10
		public static string Server_signa_ture = "wvC3TdJKafsBgZJLNqn4wiEJiUBNGWyYz6YXLTTE5TRNz7jWB/N/VDVvhWb2QW8qxV4yX/uTl9vxkWM58mdD8XYAysQXaJiSLQmqNvn7ZojgVcIWYirhwCshO7IBc8Y5RIsZJ8dQhwASv5/VoSwX2enI2SeFmSM4uxcyyIoCXF7FVKkBZoCPPqNQnK6tG46DTHpLckvz+ecKbzxBrUUiainEOZc89w7Vugz439CKGAc6iOygLN00JEiWPQPq3Ja+XAVAIXranaPx27oazjh/ouEdXVsna4Ismfyvx2BNk4g=";

		// Token: 0x0400000B RID: 11
		public static X509Certificate2 Server_Certificate;

		// Token: 0x0400000C RID: 12
		public static Aes256 aes256;

		// Token: 0x0400000D RID: 13
		public static string Paste_bin = "yRfZlbpWXr3ETM7a4wzC+LbUfu3CejPsLuK94AvRae+HNEpWnkl0XOCTk4i0sFVKCSWYrC9kgslGaxop5daJdg==";

		// Token: 0x0400000E RID: 14
		public static string BS_OD = "7lGHKaQYgUiyk5gpY+ZR4ZixeqK6QvczdEv8qa0aVxOqntGFBUr8wMoBbuEx9+aFKHT+fy7AG7+d5nR6mp+b7g==";

		// Token: 0x0400000F RID: 15
		public static string Hw_id = null;

		// Token: 0x04000010 RID: 16
		public static string De_lay = "1";

		// Token: 0x04000011 RID: 17
		public static string Group = "Z6lJERnqvEemhpXupAzjq4Aipm03MGmzsbns2ltdQy7SHDXImx8BJTh8kSYehC5tCQA77I+IJQYSIj46XbT5EQ==";

		// Token: 0x04000012 RID: 18
		public static string Anti_Process = "dxb+bD4dEnMMqm/HBwsTe/xOf7ramgTpDFgN7Ju99LUgRDE7sMtXhfavQGTM3KecYQuF1Ki8B9AIZDpQUQBODA==";

		// Token: 0x04000013 RID: 19
		public static string An_ti = "laG/3IgvNsYQSwMQnvQnV92gNI0/nozRdztB3e1Zl4+jMMvze0D2mw0r7OaeRkqviyG314TYsq9rCz3yl0QmIA==";
	}
}
```



