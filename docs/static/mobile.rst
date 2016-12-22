
.. _h2164242e4c6048506f23311549231654:

加班費計算機
************

.. _h6b5f434b414c34d3452292d6e513056:

精簡版（實驗性功能)
===================

|CONTENT|


.. |CONTENT| raw:: html

    <html>
    <body>
            <form>
            <table id="calculator">
            <tr><th>月薪</th></tr>
            <tr><td><input class="field" type="number" id="salary" value="36000"></td></tr>
            <tr><th>類型</th></tr>
            <tr><td><select class="field" id="type">
                <option value="1" selected>工作日</option>
                <option value="2">休息日</option>
                <option value="3">例假日</option>
                <option value="4">特休</option>
                <option value="5">國定假日</option>
                </select></td></tr>
            <tr><th>上班時間</th></tr>
            <tr><td><input class="field" id="in_time" type="time" value="09:00"></td></tr>
            <tr><th>下班時間</th></tr>
            <tr><td><input class="field" id="out_time" type="time" value="18:00"></td></tr>
            <tr><th>扣除午休時間(分鐘)</th></tr>
            <tr><td>
               <div style="margin-bottom:10px">
                   <output name="ageOutputName" id="rest_interval_value">60</output>
               </div>
               <div>
                   <input class="field" id="rest_interval" step="10" type="range" max="120" min="0" value="60" oninput="rest_interval_value.value = rest_interval.value"></td></tr>
               </div>
            <tr><th>每一加班單位多少分鐘</th></tr>
            <tr><td><input class="field" id="ot_unit" type="number" step="10" value="60"></td></tr>
            <tr><th>加班超過幾分鐘可算一單位</th></tr>
            <tr><td><input class="field" id="ot_min" type="number" step="5" value="30"></td></tr>
            <tr><th class="type_color" style="outline: #c0c0c0 solid thin;">本日薪資明細 <span id="over4" style="background-color:red;padding:10px;color:#f5ff59;display:none">請注意：已經超時加班</span></th></tr>
            <tr><td>
                <div class="type_color" id="result"></div>
                </td></tr>
            </table>
            </form>
            <h2>注意事項</h2>
            <ul>
             <li>本加班費試算機的內容及計算結果僅參考，使用加班費試算機時，代表您已經同意本公司不對使用加班費試算機的結果承擔任何責任，如不同意，請勿使用。</li>
             <li>一般工作日不足八小時的部分，本計算機不倒扣，依貴公司依據公司規定自行計算</li>
             <li>下班時間比上班時間早，視為隔日（僅適用於精簡版）</li>
             <li>日薪為月薪除以30日，時薪為日薪除以8小時</li>
            </ul>
            <style>
            table#calculator{
                width:100%;
            }
            table#calculator th{
                padding:10px;
                background-color:#f0f0f0;
                text-align:left;
            }
            table#calculator td{
            }
            table#calculator input,table#calculator select{
                font-size:1.2em;
                padding:10px;
                width:100%;
            }
            #rest_interval_value{
                font-size:1.2em;
                font-size:1.2em;
                padding:10px;
            }
            #result table{
                width:100%;
            }
            #result table tr:nth-child(odd){
                background-color:#ffffff;
            }
            #result table tr:nth-child(even){
                background-color:#f0f0f0;
            }
            #result table th{
                color:black;
                background-color:transparent;
            }
            #result table td{
                padding-left:10px;
                min-width:100px;
                color:black;
                background-color:transparent;
                vertical-align:middle;
            }
    
            </style>
            <script language="javascript">
            function init(){
    
                var eles = document.querySelectorAll('.field')
                eles.forEach(function(ele){
                    ele.onchange=calculate
                })
    
                calculate()
            }
            function getInputValue(id){
                return document.getElementById(id).value
            }
            function getSelectValue(id){
                var sel = document.getElementById(id)
                return sel.options[sel.selectedIndex].value
            }
            function calculate(){
                //collect value
                var parameters = {
                    salary:parseInt(getInputValue('salary')),
                    type:getSelectValue('type'),
                    in_time:getInputValue('in_time'),
                    rest_interval:parseInt(getInputValue('rest_interval')),
                    out_time:getInputValue('out_time'),
                    ot_unit:parseInt(getInputValue('ot_unit')),
                    ot_min:parseInt(getInputValue('ot_min')),
                }
    
                var output = []
                worker(parameters,output.join(''))
            }
            function getMinutes(str){
                var hm = str.split(':')
                return parseInt(hm[0]) * 60 + parseInt(hm[1])
            }
            function round(n){
                return Math.round(n*100)/100
            }
            function comma1000(n){
                var s = ''+n
                var f = ''
                if (s.indexOf('.')>=0) {
                    f = s.split('.')[1]
                    s = s.split('.')[0]
                }
                var ret = []
                var e = Math.floor(s.length/3)
                for (var i=0;i<e;i++){
                    ret.push(s.substring(s.length-(i+1)*3,s.length-i*3))
                }
                if (s.length-e*3>0) ret.push(s.substr(0,s.length-e*3))
                ret.reverse()
                return ret.join(',')+(f ? '.'+f : '')
            }
            function worker(parameters,prefix){
                var min2hour = function(m){
                    var h = Math.floor(m/60)
                    var u = Math.floor((m - h * 60)/parameters.ot_unit)
                    var remain = m - h * 60 - u*parameters.ot_unit
                    return h + u * (parameters.ot_unit/60)+ ((remain >= parameters.ot_min) ? parameters.ot_unit: 0) / 60
                }
                var hour_pay = parameters.salary / 240
                var normal_day_pay = hour_pay * 8
                var day_minutes = 24 * 60
                var raw_worktime_min = (day_minutes + (getMinutes(parameters.out_time) - getMinutes(parameters.in_time))) % (day_minutes)
                var worktime_min = raw_worktime_min - parseInt(parameters.rest_interval)
                //previous 8 hour
                var daytime_hour = (worktime_min  >= 480) ? 8 : worktime_min/60
                var daytime_12_hour = worktime_min > 120 ? 2 : min2hour(worktime_min)
                var daytime_3to8_hour =  min2hour(worktime_min-120)
                //overtime
                var overtime_min = (worktime_min  >= 480)  ? (worktime_min - 480) : 0
                var overtime_hour = min2hour(overtime_min)
                var overtime_12_hour = overtime_hour >= 2 ? 2 : overtime_hour
                var overtime_34_hour = overtime_hour >= 2 ? overtime_hour - 2 : 0
    
    			//overtime over 4 hours
                if (overtime_34_hour > 2) {
                    overtime_34_hour=2
                    document.getElementById('over4').style.display=''
                }
                else{
                    document.getElementById('over4').style.display='none'
                }
    
                var day_pay;
                var overtime_pay;
                var day_hour_law
                var ot_law
                switch(parseInt(parameters.type)){
                    case 1:
                        day_pay = 0
                        overtime_pay = hour_pay * 4/3 * overtime_12_hour + hour_pay * 5/3 * overtime_34_hour
                        day_hour_law =  daytime_hour <= 8 ? daytime_hour : 8
                        ot_law = overtime_min ? overtime_12_hour+overtime_34_hour : 0
                        break
                    case 2:
                        day_pay = daytime_hour <= 4 ? (hour_pay * 4/3 * 2+ hour_pay * 5/3 * 2) : (hour_pay * 4/3 * 2+hour_pay * 5/3 * 6)
                        //day_pay += daytime_hour >0 ? normal_day_pay : 0
                        overtime_pay = overtime_12_hour > 0 ? hour_pay * (1+5/3) * 4 : 0
                        day_hour_law = daytime_hour <= 4 ? 4 : 8
                        ot_law = overtime_12_hour> 0 ? 4 : 0
                        break
                    case 3:
                        day_pay = daytime_hour > 0 ? normal_day_pay : 0
                        overtime_pay = hour_pay * 2 * overtime_12_hour + hour_pay * 2 * overtime_34_hour
                        day_hour_law = daytime_hour  > 0 ? 8 : 0
                        ot_law = daytime_hour <= 8 ? 0 : daytime_hour
                        break
                    case 4:
                    case 5:
                        day_pay = daytime_hour > 0 ? normal_day_pay : 0
                        overtime_pay = hour_pay * (4/3) * overtime_12_hour + hour_pay * (5/3) * overtime_34_hour
                        day_hour_law = daytime_hour  > 0 ? 8 : 0
                        ot_law = daytime_hour <= 8 ? 0 : daytime_hour
                        break
                    default:
                        throw 'unknown type'
                }
                var types = ['','工作日','休息日','例假日','休假日','休假ㄖ']
                var typesBgColor = ['','#f0f0f0','#93c47d','#c27ba0','#6d9eeb','#6d9eeb']
                var typesColor =   ['','black','white','white','white','white']
                var results = [
                    ['時薪',comma1000(round(hour_pay))],
                    ['日薪（A）',comma1000(round(normal_day_pay))],
                    ['性質',types[parameters.type]],
                    ['實際工時',round(daytime_hour)+'+'+round(overtime_12_hour)+'+'+round(overtime_34_hour)+'='+round(daytime_hour+overtime_12_hour+overtime_34_hour)],
                    ['法定工時',round(day_hour_law)+'+'+round(ot_law)],
                    ['前八小時額外工資（B）',comma1000(round(day_pay))],
                    ['後四小時加班工資（C）',comma1000(round(overtime_pay))],
                    ['當日額外工資（B+C）',comma1000(round(day_pay+overtime_pay))],
                    ['當日總工資（A＋B+C）',comma1000(round(normal_day_pay+day_pay+overtime_pay))],
                ]
                var html = []
                html.push('<table class="result">')
                results.forEach(function(item){
                    html.push('<tr><th>'+item[0]+'</th><td>'+item[1]+'</td></tr>')
                })
                html.push('</table>')
                document.getElementById('result').innerHTML = prefix+html.join('')
                var bgcolor = typesBgColor[parameters.type]
                var color = typesColor[parameters.type]
                document.querySelectorAll('.type_color').forEach(function(ele){
                    ele.style.backgroundColor = bgcolor
                    ele.style.color = color
                })
            }
            window.addEventListener('DOMContentLoaded',init)
            </script>
    </body>
    </html>
    


.. bottom of content
