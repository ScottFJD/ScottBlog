
#Android键盘属性

##Android键盘覆盖属性	
	
		getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_STATE_HIDDEN); 初始隐藏键盘
		getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_ADJUST_PAN); 软键盘显示型式 覆盖
		android:windowSoftInputMode="adjustPan" 在Manifest中设置
		