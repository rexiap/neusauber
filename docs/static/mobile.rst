
.. _h2164242e4c6048506f23311549231654:

加班費計算機
************

.. _hd1b83d48586e1b393a624e28544946:

精簡版
======

|CONTENT|


.. |CONTENT| raw:: html

    <form>
    <table id="calculator">
    <tr><th>月薪</th></tr>
    <tr><td><input class="field" type="number" id="salary" value="36002"></td></tr>
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
    	var arguments = {
    		salary:getInputValue('salary'),
    		type:getSelectValue('type'),
    		in_time:getInputValue('in_time'),
    		rest_interval:getInputValue('rest_interval'),
    		out_time:getInputValue('out_time')
    	}
    	var output = []
    	for (var key in arguments){
    		output.push('<div>'+key+'='+arguments[key]+'</div>')
    	}
    	document.getElementById('result').innerHTML = output
    
    }
    window.addEventListener('DOMContentLoaded',init)
    </script>
    
    
    

