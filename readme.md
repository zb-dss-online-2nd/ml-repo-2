||항목명|ITEM|Data Type|Comment|
|:---:|:------:|:-------:|:--------------------:|:-------------------------------------------------------:|
||구단|team|int(1~10)||
||년도|year|int(2015~2022)||
||월|month|int(3~10)||
||승률|pov|float(0~1)|- 승수/(승수＋패수)|
|타자|출루율(on-base percentage)|obp|float(0~1)|- 타석에 나왔을 때 아웃을 당하지 않고 주자로 살아남는 확률 </br>- (안타+볼넷+몸에 맞은 공) / (타수+볼넷+몸에 맞은 공+희생플라이)- 베이스에서 많이 살아남는다는 증거가 되기 때문에 중요한 요인으로 선정|
|타자|장타율 Slugging Percentage|slg|float(0~1)|- 총 누타수/타수 - 좀 더 정확하게는 "(단타 수[1] + 2루타 수*2 + 3루타 수*3 + 홈런 수*4)/타수"|
|타자|타율|Batting Average|ba|float|- 안타/타수- 득점과 직결되는 변수이므로 가장 중요한 요인이기에 선정|
|투수|평균자책점 ERA|era|float|1. 야구에서 투수가 한 게임(9이닝) 당 내준 평균 자책점 2. 타자의 타율과 마찬가지로 투수의 역량을 재는 가장 유명하고 고전적이어서 상징성이 있는 비율 스탯으로, 숫자가 낮을수록 좋은 것 3. 투수로서의 능력을 직접적으로 보여주는 지표(실점과도 연관)로 이해할 수 있음|
|투수|WHIP|whip|float|1. (피안타 + 볼넷) / 이닝 ( 고의사구는 포함시키며 몸에 맞는 볼은 포함하지 않음) 2. 투수의 안정감을 측정하는 지표 (낮을 수록 좋음) 3. 볼넷 : 타자가 타석에서 4개의 볼 카운트를 얻어내 1루로 나가는 것 4.   이닝 : 야구 또는 소프트볼에서 양팀이 한 번씩의 공격을 주고 받는 단위.|
