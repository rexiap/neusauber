
.. _h2164242e4c6048506f23311549231654:

加班費計算機
************

.. _h6b5f434b414c34d3452292d6e513056:

精簡版（實驗性功能)
===================

|CONTENT|


.. |CONTENT| raw:: html

    <form>
    <table id="calculator">
    <tr><th>月薪</th></tr>
    <tr><td><input class="field" type="number" id="salary" value="36000"></td></tr>
    <tr><th>類型</th></tr>
    <tr><td><select class="field" id="type">
        <option value="1">工作日</option>
        <option value="2">休息日</option>
        <option value="3"  selected>例假日</option>
        <option value="4">特休</option>
        <option value="5">國定假日</option>
        </select></td></tr>
    <tr><th>上班時間</th></tr>
    <tr><td><input class="field" id="in_time" type="time" value="09:00"></td></tr>
    <tr><th>下班時間</th></tr>
    <tr><td><input class="field" id="out_time" type="time" value="17:00"></td></tr>
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
    <tr><td><input class="field" id="ot_min" type="number" step="5" value="45"></td></tr>
    <tr><th>本日薪資明細</th></tr>
    <tr><td>
        <div id="result"></div>
        </td></tr>
    </table>
    </form>
    <h2>注意事項</h2>
    <ul>
     <li>本加班費試算機的內容及計算結果僅參考，使用加班費試算機時，代表您已經同意本公司不對使用加班費試算機的結果承擔任何責任，如不同意，請勿使用。</li>
     <li>一般工作日不足八小時的部分，本計算機不倒扣，依貴公司依據公司規定自行計算</li>
     <li>下班時間比上班時間早，視為隔日（僅適用於精簡版）</li>
    </ul>
    <style>
    table#calculator{
    	width:100%;
    }
    table#calculator th{
    	padding:10px;
    	background-color:#c9c9c9;
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
    		rest_interval:getInputValue('rest_interval'),
    		out_time:getInputValue('out_time')
    	}
    
    	var output = []
    	worker(parameters,output.join(''))
    }
    function getMinutes(str){
        var hm = str.split(':')
        return parseInt(hm[0]) \* 60 + parseInt(hm[1])
    }
    function worker(parameters,prefix){
        var min2hour = function(m){
            var h = Math.floor(m/60)
            var remain = m - h \* 60
            return h + ((remain >= parameters.ot_min) ? parameters.ot_unit : 0) / 60
        }
        var hour_pay = parameters.salary / 240
        var normal_day_pay = hour_pay \* 8
        var day_minutes = 24 \* 60
        var raw_worktime_min = (day_minutes + (getMinutes(parameters.out_time) - getMinutes(parameters.in_time))) % (day_minutes)
        var worktime_min = raw_worktime_min - parseInt(parameters.rest_interval)
        console.log([raw_worktime_min,worktime_min])
        //previous 8 hour
        var daytime_hour = (worktime_min  >= 480) ? 8 : worktime_min/60
        var daytime_12_hour = worktime_min > 120 ? 2 : min2hour(worktime_min)
        var daytime_3to8_hour =  min2hour(worktime_min-120)
        var overtime_min = (worktime_min  >= 480)  ? (worktime_min - 480) : 0
        var overtime_12_hour = overtime_min >= 120 ? 2 : min2hour(overtime_min)
        var overtime_34_hour = overtime_min >= 120 ? min2hour(overtime_min - 120) : 0
    
        var day_pay;
        var overtime_pay;
        var day_hour_law
        var ot_law
        switch(parseInt(parameters.type)){
            case 1:
                day_pay = 0
                overtime_pay = hour_pay \* 4/3 \* overtime_12_hour + hour_pay \* 5/3 \* overtime_34_hour
                day_hour_law =  daytime_hour <= 8 ? daytime_hour : 8
                ot_law = daytime_hour <= 8 ? 0 : daytime_hour-8
                break
            case 2:
                day_pay = daytime_hour <= 4 ? (hour_pay\*4/3\*2+hour_pay\*5/3\*2) : (hour_pay\*4/3\*2+hour_pay\*5/3\*6)
                //day_pay += daytime_hour >0 ? normal_day_pay : 0
                overtime_pay = overtime_12_hour > 0 ? hour_pay \* 5/3 \* 4 : 0
                day_hour_law = daytime_hour <= 4 ? 4 : 8
                ot_law = overtime_12_hour> 0 ? 4 : 0
                break
            case 3:
            case 4:
            case 5:
                day_pay = daytime_hour > 0 ? normal_day_pay : 0
                overtime_pay = hour_pay \* 2 \* overtime_12_hour + hour_pay \* 2 \* overtime_34_hour
                day_hour_law = daytime_hour  > 0 ? 8 : 0
                ot_law = daytime_hour <= 8 ? 0 : daytime_hour
                break
            default:
                throw 'unknown type'
        }
        var results = [
        	['時薪',hour_pay],
        	['日薪（A）',normal_day_pay],
        	['性質',parameters.type],
        	['實際工時',daytime_hour+'+'+overtime_12_hour+'+'+overtime_34_hour+'='+(daytime_hour+overtime_12_hour+overtime_34_hour)],
        	['法定工時',day_hour_law+'+'+ot_law],
        	['前八小時額外工資（B）',day_pay],
        	['後四小時加班工資（C）',overtime_pay],
        	['當日額外工資（B+C）',day_pay+overtime_pay],
        	['當日總工資（A＋B+C）',normal_day_pay+day_pay+overtime_pay],
        ]
        var html = []
        html.push('<table>')
        results.forEach(function(item){
            html.push('<tr><th>'+item[0]+'</th><td>'+item[1]+'</td></tr>')
        })
        html.push('</table>')
        document.getElementById('result').innerHTML = prefix+html.join('')
    }
    window.addEventListener('DOMContentLoaded',init)
    </script>
    
    

