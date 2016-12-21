
.. _h106d6a60386b4471802c17574203f54:

加班費計算機（精簡版）
**********************

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
    <tr><td><input class="field" id="in_time" type="time" step="10" value="09:00:00"></td></tr>
    <tr><th>下班時間</th></tr>
    <tr><td><input class="field" id="out_time" type="time" step="10" value="17:00:00"></td></tr>
    <tr><th>扣除中間休息時間分鐘</th></tr>
    <tr><td>
       <div style="margin-bottom:10px">
    	   <output name="ageOutputName" id="rest_interval_value">0</output>
       </div>
       <div>
    	   <input class="field" id="rest_interval" type="range" max="120" min="0" value="0" oninput="rest_interval_value.value = rest_interval.value"></td></tr>
       </div>
    </table>
    </form>
    <style>
    table#calculator th,table#calculator td{
    	padding:10px;
    }
    table#calculator th{
    	background-color:#c9c9c9;
    }
    </style>
    <div id="result"></div>
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
    

