# KLSea
无涯
function getStudentStationExam() {
	$('#table_select_examStudentStation').bootstrapTable('destroy');
    $('#table_select_examStudentStation').bootstrapTable( {
		data:examStudentList,
		height : 500,
		striped : true,
		pagination : true,
		detailView : true, //父子表开启
		sidePagination : 'client',
		pageSize : 10,
		pageList : [ 10, 15, 20, 25 ],
		showExport : true,  
		sortable : true,
		sortOrder : 'asc',
		sortName: 'examStudentNo',
		minimunCountColumns : 2,//当列数小于2时，不显示内容
		formatLoadingMessage: function () {
			return '<img src="../backend/images/loading.gif"/>';
    	},
		columns :
		[ 
			{field : 'examStudentNo',title : '考生考号',align : 'center',valign : 'middle',sortable : true}, 
			{field : 'userName',title : '考生姓名',align : 'center',valign : 'middle' }, 
			{field : 'userNo',title : '考生学号',align : 'center',valign : 'middle' }
		],
		//设置展开图标
		icons:{                       
	         detailOpen: 'fa fa-plus',
	         detailClose: 'fa fa-minus',
	    },
	    //当点击图标展开时候触发，加载子表
	    onExpandRow: function (index, row, $detail) {
			studentExamTableId = index;
	    	examinee = row;
	    	//创建子表考站table
	    	var studentMessage_table = $detail.html('<table id="child_table'+index+'"></table>').find('table');
	    	//初始化子表table
	    	$(studentMessage_table).bootstrapTable({
	    		data:examStationList,
	    		striped : true,
	    		uniqueId: "examStationSort",    
	    		formatLoadingMessage: function () {
	    			return '<img src="../backend/images/loading.gif"/>';
	        	},
	    		columns :
	    		[ 
					{field : 'examStationSort',title : '考站序号',align : 'center',valign : 'middle'}, 
					{field : 'examStationName',title : '考站名称',align : 'center',valign : 'middle'}, 
					{field: 'examStationType', title: '考站类型', align: 'center', valign: 'middle',formatter:stationTypeFormatter },
					{field : 'stationTimes',title : '考站时长(分钟)',align : 'center',valign : 'middle'}, 
					{field : 'examRoomList',title : '房间号',align : 'center',valign : 'middle',formatter:examRoomFormatter},
					{field : 'studentExamTime', title : '考试日期',align : 'center',valign : 'middle',formatter:studentExamTimeFormatter}, 
					{field : 'inStationTime', title : '进站时间',align : 'center',valign : 'middle',formatter:inStationTimeFormatter}, 
					{field : 'outStationTime', title : '出站时间',align : 'center',valign : 'middle',formatter:outStationTimeFormatter}, 
					{title : '操作',align : 'center',valign : 'middle',formatter:editStudentStationFormatter,events:studentOperateEvents}, 
	    		],
	    	});
	    	soleSon(index);
	    }
	});
  
}
/**
 * 关闭其他开启的子表
 */
function soleSon(index){
	var fatherData = $("#table_select_examStudentStation").bootstrapTable('getData');
	for(var i in fatherData){
		if( i !=index){
			$('#table_select_examStudentStation').bootstrapTable('collapseRow',i);
		}
	}
}
