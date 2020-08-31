# escale
**Desafio Escale**

O código Python foi criado em ambiente AWS EMR. 

![Hardware AWS `hardware`](img/hardware.png)

Na **primeira etapa** do desafio os jsons.gz são lidos em loop para trazermos o primeiro dicionário que traz para cada arquivo a sessões únicas (anonymous_id).

{'part-00000.json.gz': 10234800,
 'part-00001.json.gz': 10230747,
 'part-00002.json.gz': 10227106,
 'part-00003.json.gz': 10233308,
 'part-00004.json.gz': 10233284,
 'part-00005.json.gz': 10235163,
 'part-00006.json.gz': 10232644,
 'part-00007.json.gz': 10236312,
 'part-00008.json.gz': 10228130,
 'part-00009.json.gz': 10229023}
 
 A **segunda etapa**, lemos todos os 10 arquivos com o Spark e trouxemos as sessões únicas por browser_family, os_family e device_family. 
 
 'browser_family': {'Edge Mobile': 3274,
  'Other': 2489787,
  'Chrome Mobile': 51526289,
  'PetalBot': 1155,
  'Puffin': 2400,
  'AdsBot-Google': 113642,
  'Firefox Mobile': 125298,
  'Googlebot': 7977489,
  'GooglePlusBot': 139,
  'Yahoo! Slurp': 72,
  'Pinterest': 62,
  'Opera': 174649,
  'Mobile Safari UIWebView': 4365293,
  'Investment Crawler': 3,
  ....
  
 A **última etapa**, mais desafiadora é trazer pelas mesmas famílias acima a duração em segundos. "Podemos assumir que sessões com um único evento tem duração de 0 segundos."
  
 Primeiro pegamos por família e sessão o timestamp final e o inicial, e fazemos uma subtração para termos a duração. Depois com esse agrupamento, calculamos apenas por família a mediana, (percentil 50).
 
 A leitura teve que ser particionada em parquet para a subquery acontecer. 
 
 Aparentemente não há dois casos de mesma sessão e por isso a subtração tem dado zero. Logo usamos a premissa "Podemos assumir que sessões com um único evento tem duração de 0 segundos".
 
 {'browser_family': {'Edge Mobile': '0.00',
  'Other': '0.00',
  'Chrome Mobile': '0.00',
  'PetalBot': '0.00',
  'Puffin': '0.00',
  'AdsBot-Google': '0.00',
  'Firefox Mobile': '0.00',
  'Googlebot': '0.00',
  'GooglePlusBot': '0.00',
  'Yahoo! Slurp': '0.00',
  'Pinterest': '0.00',
  'Opera': '0.00',
  'Mobile Safari UIWebView': '0.00',
  'Investment Crawler': '0.00',
  'Chromium': '0.00',
  'SMTBot': '0.00',
  'Mobile Safari': '0.00',
  'IE': '0.00',
  'Maxthon': '0.00',
  'UC Browser': '0.00',
  'Opera Mobile': '0.00',
  'Firefox': '0.00',
  'Opera Mini': '0.00',
  'Android': '0.00',
  'Facebook': '0.00',
  'Vivaldi': '0.00',
  'SeaMonkey': '0.00',
  'Amazon Silk': '0.00',
  'Firefox iOS': '0.00',
  'Applebot': '0.00',
  'Chrome Mobile iOS': '0.00',
  'Chrome': '0.00',
  'Edge': '0.00',
  'AppleMail': '0.00',
  'Safari': '0.00',
  'Yandex Browser': '0.00',
  'Pale Moon (Firefox Variant)': '0.00',
  'BingPreview': '0.00',
  'IE Mobile': '0.00'},
 'os_family': {'Other': '0.00',
  'Windows Phone': '0.00',
  'Windows XP': '0.00',
  'Windows 10': '0.00',
  'Windows 8': '0.00',
  'Ubuntu': '0.00',
  'FreeBSD': '0.00',
  'Android': '0.00',
  'Windows 7': '0.00',
  'Linux': '0.00',
  'Fedora': '0.00',
  'Chrome OS': '0.00',
  'iOS': '0.00',
  'Windows 8.1': '0.00',
  'Windows Mobile': '0.00',
  'Windows Vista': '0.00',
  'Mac OS X': '0.00'},
 'device_family': {'Samsung SM-G970N': '0.00',
  'Samsung SM-G770F': '0.00',
  'Grand X': '0.00',
  'Samsung SM-T705M': '0.00',
  'Samsung SM-N960F': '0.00',
  'MS55M': '0.00',
  'Samsung SM-G920V': '0.00',
  'Lenovo K10a40': '0.00',
  'LG-H831': '0.00',
  'Samsung SM-G950F': '0.00',
  'LG-H931': '0.00',
  'Lenovo TB2-X30F': '0.00',
  'X21 Plus': '0.00',
  'Lumia 950': '0.00',
  'Samsung SM-A750G': '0.00',
  'Samsung SM-G973U1': '0.00',
  'D5833': '0.00',
  'Grand XL': '0.00',
  'Asus X00TDB': '0.00',
  'Asus I001DD': '0.00',
  'Moto E': '0.00',
  'S10': '0.00',
  'PHONEMAX_Saturn': '0.00',
  'XiaoMi Redmi Note 5': '0.00',
  'Samsung SM-G600FY': '0.00',
  'LG-H815': '0.00',
  'XiaoMi MI 8': '0.00',
  'HUAWEI LUA-U23': '0.00',
  'Samsung SM-A920F': '0.00',
  'Advance L4': '0.00',
  'Samsung SM-T555': '0.00',
  '4009E': '0.00',
  'GO3E': '0.00',
  'LG-K220': '0.00',
  'F_Pro': '0.00',
  'Samsung SM-J105M': '0.00',
  'XiaoMi Redmi 5': '0.00',
  'Samsung SM-G6100': '0.00',
  'Asus X018D': '0.00',
  
  Há uma pequena validação rápida no final do código.
  
  Teríamos que aprofundar a análise.
  
  
