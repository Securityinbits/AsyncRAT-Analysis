### AsyncRAT Config extraction Recipe

#### Required 
- Input code
- Salt
- Variable name that stores the AES key


Note: This recipe was initially developed by Srujan Kumar. I have made few modifications to support the new AsyncRAT files.  
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

### Examples
- 37e965330586a51125db2a420917db17 (2023-07-24)
- a4c35dcd0013a04666a9d58095ff4060 (2023-07-27)
- dea3149aae31bd4116adba54840af10f (2022-05-03)
- b6dbbded2ca82116c6da9bae34cfa6e9 (2022-04-27)

#### 37e965330586a51125db2a420917db17 (2023-07-24)
- Use this Salt DcRatByqwqdanchun (Format: UTF8)
- Key is the variable name for AES key, input Key in Filter operation

#### Input
```
		// Token: 0x04000001 RID: 1
		public static string Por_ts = "LTT1S8GeyTjgJgNjfacJIH2MnnHdxcwd33rcE3PwHYDkBbkVhTFhlBhSXEcNmcSB+78/aiOCdZcNSaMr27IoOA==";

		// Token: 0x04000002 RID: 2
		public static string Hos_ts = "BNJiYrBPfDDgdZHUm4VSGV5DWUYjBYsOaucXpFN38659q4LRDwyn14rbYeMpRJDUo79mCbgVHd0cjy2mt1hYun3vx2rfBtQirBKVWeTE1w8=";

		// Token: 0x04000003 RID: 3
		public static string Ver_sion = "GlnZAyDnOneWXgD8uAuMas0thjeZIeDicM5c3uspHIaZx3eAOboPbQRwiqN/56VAR0UM1ubEdnHJidyEIkaqbQ==";

		// Token: 0x04000004 RID: 4
		public static string In_stall = "HM4W+lkN5UFJosQVJZufHDIa7M76vawe/otXnRXEhiQiEBZhW+fpUK4Bj8X1GjXsVkt61ZSPBj5o5PBe8gko5A==";

		// Token: 0x04000005 RID: 5
		public static string Install_Folder = "%AppData%";

		// Token: 0x04000006 RID: 6
		public static string Install_File = "SecurityHealthSystray.exe";

		// Token: 0x04000007 RID: 7
		public static string Key = "RmVMOVdKREkxaGF4ZjR1ek9MVmJpZkpiTm1xYWsxOWo=";

		// Token: 0x04000008 RID: 8
		public static string MTX = "31Kl8sNyigmXdB8ijKal8I2AkDlv8doKPUvPer6zqhj6Xd7iwGjiedheTu/yWncGq7pbWrGdTunOc6Qr1JbuxA==";

		// Token: 0x04000009 RID: 9
		public static string Certifi_cate = "np2sOxb7GoEJprq21AJwcoDbr9Ys8lViT19ZPB2UZYl8F2KoYpuqO54Zpy0nPuETuE9g42ePiWOEMe7NoiHp43bMa6p0u+29+ZJtp3cba+iuAKuYUETOCvZvM7yMjfmrp9g+uaas9Z+2Y1Qm7ua9S5mc8jHsvdGC+pwImZ30Sfk+diYaggdxUPdllv8c+2QaYEkhIIg5oNUsZj8gMJL9W8ALQeNNtGJJ0WmIt5xM37XRpzzFeyiHxeNv4ASz+GTUhgFSc0fdIU32DYetpewldrCAaD0K6Vkbe0dUztw6c79vwrOCUkk0fYOEqV6klJ6itqZySyVIcY853hdA2tXkOHygMALRN2Ow60MHtCS5ihyM9co8qFt2DTPDOS9wlKbQFAI5lYjPJzbdYqmNNqp54WTRgPR1pO2UiyantgYz43+7zsPyGRVLXiD5lf+Gdaq/q33I2I8ZxTIVNdPhr8RQsyD6RwPzqf62+fVhW5v1pZS+qBygnfLYZbsUfz2MF/ogzCunNCoKLsLFt0KhsqAbhbbaKQVM1cdY4iCiqdepvY9hDhMB4Scf8twZr3ad6xVJokET2i1AGq3xLI4JFkhTb5IYAhPQt5maugBMVP4NeQCmXLX3D72ZgpNOuAYebTgxvP3ker62vUAnUM7w+o4zE4/XA1auVPtBearJYtoFqehPv242EhaNo0otD1YrlBi9WyJWw83WsYkEbhtc+XNKRKEYlzYer1wYIPesKsi5lxqc7HcY23BX/yp+pF8g52O0GB8I090cqbDvcJOs6zppyNDbhOMgZgdv30QRhIalHAJ/LsAPXhDaWinGlo1hwS4fAtxQDgyEx7UJ84C6tm6LGyYsVxjsA13+hEb6axo0jq8ULiJdkAzGI6qEmWQnLICO0DqLArLgX57w3JZ2wEXoowhYP5NWH8im7eMRPh66K0zDZPvmRYhaMjamGDT5POUGclXzc4BLBQVb3VGwfUc7ykIl77LdKAR9jkrluDNlsL0Nmmv+oFguPl63T/wpJ47cmXMD7xwBcXzQpsy28qxbT/KGlI/ebjYQsIUT6QAuQkReN5Dmr21Xqfxl+n4XRpm1";

		// Token: 0x0400000A RID: 10
		public static string Server_signa_ture = "Q3ChaTA0NoS0M8pKuWJikoVgI9YkgUpccchJ8wBarLIuiGgLN+qwvxTKkh+YQ7CxUVOazTp1Isuc3wIYULO1zS/n9J0JTTbz8JfS6QCM5uiMQoLVB+bYmE9Wla7IWvLDIxhiEmdxtupzf+Nk+gJosHy21Ze51qaCSOLDFDZrmacYAXauztRz3chk3VKt/5NHTlskvoXr9uRgxnPlvt6YfqBijchS6JIvxoOzpzng4NoeEgWOXoVhxLDps5+TZx6/fNaPYneB5tsmp3HuumLCpUAAR6W8fHRXNQM4bPsX6wY=";

		// Token: 0x0400000B RID: 11
		public static X509Certificate2 Server_Certificate;

		// Token: 0x0400000C RID: 12
		public static Aes256 aes256;

		// Token: 0x0400000D RID: 13
		public static string Paste_bin = "0gGNlo/SybYe7aTDClNf/gW8/XXXUK+2Zylwdqka9/KvbwSM4z7MPH3KHuk1kv1XP4IcwkaBnJMf8B0mzY9yMQ==";

		// Token: 0x0400000E RID: 14
		public static string BS_OD = "KgRgLnCvHs1Cimzi+tzCuy9zJZVkbxKbfTTGOnbzuPh5ZdEKhmw1Q/tXo+ux/5vgYNL4/Zc36fSl64VH2fplBA==";

		// Token: 0x0400000F RID: 15
		public static string Hw_id = null;

		// Token: 0x04000010 RID: 16
		public static string De_lay = "1";

		// Token: 0x04000011 RID: 17
		public static string Group = "bcpQwvyGNRMEmIX5/wk9qOiw3WVY0hI7aGm3VwkFNzkierktzm57ZEKHImMs2mVPfU1uC8lvXtITjEjNCDYKOQ==";

		// Token: 0x04000012 RID: 18
		public static string Anti_Process = "wZpgPkDC8xv2VmF1EZNMKH5qR+6QUlRhgH13s6S4x+Krcy0eKN3//iA37dDh6ZmlNtIqLRM7460fS/TGco7Geg==";

		// Token: 0x04000013 RID: 19
		public static string An_ti = "Z/xff63xw+qSqLbHZbXHwJdspKzZkMIi6SDCHGc6K5FYpsCnzxQfDOYQDdyzzEEoh6GbnER9utqhEE9xhOiQ8w==";
	}
}
```
#### a4c35dcd0013a04666a9d58095ff4060 (2023-07-27)
- Use this Salt bfeb1e56fbcd973bb219022430a57843003d5644d21e62b9d4f180e7e6c33941 (Format: HEX)
- BtVxclpKzUIPm is the variable name for AES key, input BtVxclpKzUIPm in Filter operation

#### Input
```
		// Token: 0x04000001 RID: 1
		public static string ZCsfrdBXawOiKZjQ = "uGWsi5C5XyombAUnK/58YvWrjp23a06kT6XOWb0gUxNXV9N9UCGQS5k9cj2UmrUlpm/SwW4Uv8H07+hfcaoKIQ==";

		// Token: 0x04000002 RID: 2
		public static string khHsDnDPLhT = "HHCN59yVrresXUDrVXEm7V3SemVYfi9KhdGeR3oU/1s+Z37FCUTUpwj3qCrA3Acv2lKLILrIjThNJAOUMas92KbH+TUE78hqcxC5l8aY2OM=";

		// Token: 0x04000003 RID: 3
		public static string GUhTnJxSMIVx = "oLMmJ++KbTiC3AgMUzpeFtBNf1sBosWTDhV5dkpPRkezL4B4MQYjNBguAb31uLO2++Im64tTfpHD1ChPkDS1Xw==";

		// Token: 0x04000004 RID: 4
		public static string abaRzTmEIcRq = "/K70z0vi249e8pRlyMS3gIbtNK/kHehTlQ1DYH+73i0Hw7ZKDHugQczIRQwNZPeD0ei8ZPJ8p00WBRGGniYd5w==";

		// Token: 0x04000005 RID: 5
		public static string jVqunoxWlrfX = "%AppData%";

		// Token: 0x04000006 RID: 6
		public static string hMAdUsycUJqDP = "svchost.exe";

		// Token: 0x04000007 RID: 7
		public static string BtVxclpKzUIPm = "WXozbld0QldOeDJOSkxJNlQ1NHlLSmhVMjk0VU11TDg=";

		// Token: 0x04000008 RID: 8
		public static string FBmIjeHOhdn = "yiqJRr41qvnm+r0GJqCaQ0e+HiHqGBrn1kPtIDM8jludPPiNGsfj7SL694K7sEwHo4+F6HXwbpC7aXBUllKCarLaWKANwslR2JplQDMqJTZDR4s+TluHU2mmfacySB2Y";

		// Token: 0x04000009 RID: 9
		public static string leIjltOegYswWI = "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";

		// Token: 0x0400000A RID: 10
		public static string dfEWWMyctWxuO = "nUakg0qW+hjCKJbV4eBs0AdSTtsR2JSlA0haMPqRro+iEv/i+jjkbx7iXx4TEte202lagATuKYWDq2kyt03Evj3ABkGhprZqiXTDK5Ds2Wxb1R4dLshxSNUCkOyKFnLmZF4dfUkjc3YLIQRe8EkCWVs8fNt/Vdz1RqfwRHGrZ+SCyodmtIHl/VKO6ZYNWb3puVGTkq//O1EhszS6rIsm/dytZEu7H6RwE59eQhphJUFRDUaHJivygjizsl2brTHtl0nNSos32Cge4w3HPI7QxM6C+g2vLQIhVbI2ikGZWlooW2rRqXKXjwFqvUTiP8ZNiT+Ejs+e8UmgdLufvIhV5ixOhSdDly3Ro4nZKFAEz2uGOiD9mM/GQiOgsluPCM0kRXNlNrUW/RtNzROLbPnedn6U+o9KpW4i75S6f7TK+qeRyNZ8KXanVxg+OwoMhKKyqaeMZR1vWkA3f/X8X83v/c4FZc9oLf0MOxSRhFUyEm8xEpCECX5aGpZ2LiAZgFxcT7IgoaPlNJXcOumchqcseX9cDwFS9MjLVSf4Ev7cfVcBGb8JIfztoxom6wBEv7HsEk28vfZIgvnOEDBcc8ij4w3aNDhKWAKCEIYfioUOqDzxmeMbpXlAKkI2DMBdUlmuj1AxgKVls7DPLcbDw54WofkDr2u/w6DdFwL4CEkbdSQDisJcuoH4mfzHRuTjaKMyY6/bICQJ9oB6kT2xi0U+lVNYMDYOKMS06IIaMmYLsnFC++PAMEewOM7amTA0eA1ExGdMJTVEeA4f8niH+OvOxDwedP9dgAOTRMBcx4FXQAZHHsEuVyAG/cys/uf4PRFgGRokiAFFhtjZglJoopjb1IZC/LUj/oolOTSstsmW3qJxFAcFQ1ET7JZG9VMx6mwmItP4EMzXfV5aZ0+SpOdG8+O5sjtNQng9OZeMFvIzb1Gx5AAVFjSsGHgbQacyNJxW1D80oRPmJKZNCl6orC1j5A==";

		// Token: 0x0400000B RID: 11
		public static X509Certificate2 EcOrbeZcWWUyN;

		// Token: 0x0400000C RID: 12
		public static string NhVRdkwYZXFaf = "+E5CbU5p1/RuPK9h3rwCZti32Dsb2JS0xBScHjBUqol6WqNRnUw3MOXspuspaCtxy5UQhtkKsYqaA9FweMijHg==";

		// Token: 0x0400000D RID: 13
		public static YswClXovphLiWkj xyPKHnJWUgr;

		// Token: 0x0400000E RID: 14
		public static string dtlyuyeoFVHC = "Ix5X0OVR6DliFWz2LOLcibUbLe5ZS0XnpI23B4pI/lB8VdIcLfwJgQMaouYRwEiLZN2rUHRTXBcLYQQQj7WJ8g==";

		// Token: 0x0400000F RID: 15
		public static string dRNyhEyybjurEh = "1GIlCh3IfiYUPPztnI4uHCfhSD0ZeRQmFgYBrKARHbeVuPuLneRmUinGWPBfJ8PI+bIVGCFiNh0vKLlrn+zZhw==";

		// Token: 0x04000010 RID: 16
		public static string LmrpkpGfwSyrl = null;

		// Token: 0x04000011 RID: 17
		public static string DJkpwYlopASZZj = "3";

		// Token: 0x04000012 RID: 18
		public static string GOzvSejWYCsbX = "PRXX7VBGQDprHNh6BJ15picsLRPfZvYpX+nKeNeozUWuCiBdIF6eCBedidGY22mJutd/5CR9q3gytH3GAY+PFQ==";
	}
}

```

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
		public static string Certificate = "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";

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

#### Input
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



