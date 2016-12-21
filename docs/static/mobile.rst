
.. _h532c282b412d295b77556b1c74a30:

mobile
******

|CONTENT|


.. |CONTENT| raw:: html

    <form>
    <table>
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
        <input class="field" id="rest_interval" type="range" max="120" min="0" value="30"></td></tr>
    </table>
    </form>
    
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

