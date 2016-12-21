
.. _h532c282b412d295b77556b1c74a30:

mobile
******


.. html:: raw

    <form>
    <div>Salary:<input class="field" id="salary" value="36000"></div>
    <div>Type:<select class="field" id="type">
        <option value="1" selected>工作日</option>
        <option value="2">休息日</option>
        <option value="3">例假日</option>
        <option value="4">特休</option>
        <option value="5">國定假日</option>
        </select></div>
    <div>In:<input class="field" id="in_time" type="time" value="0900"></div>
    <div>Rest Start:<input class="field" id="rest_start" type="time"  value="1200"></div>
    <div>Rest End:<input class="field" id="rest_end" type="time"  value="1300"></div>
    <div>Out:<input class="field" id="out_time" type="time" value="1700"></div>
    </form>
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
    		rest_start:getInputValue('rest_start'),
    		rest_end:getInputValue('rest_end'),
    		out_time:getInputValue('out_time')
    	}
    	alert(arguments)
    }
    window.addEventListener('DOMContentLoaded',init)
    </script>
    

