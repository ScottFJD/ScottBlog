RecyclerView

item 的位置如何显示  通过LayoutManager 设置 linearlayout／gridviewlayout 来进行设置

item 间的分隔 ItemDecoration

item 增加与删除的动画效果 ItemAnimator

 相关重要类
 Adapter
 ViewHolder
 LayoutManager
 ItemDecoration
 ItemAnimator
 
 
 
 
 material Design

<style name = "AppTheme" parent="android:Theme.Material">

android:elevation="5dp" 让视图抬高的一个仰角

阴影效果 z-depth （1～5）

定制动画 例如 
startActivity(intent,ActivityOptions.makeSceneTranstitonAnimation(this).toBundle());



材料主题
@android:style/Theme.Material
@android:style/Theme.Material.light
@android:style/Theme.Material.light.DarkActionBar


<item name = "android:colorAccent" 设置控件颜色

CardView
layout_marginLeft = "@dimen...."
layout_marginRight = ......

android:elevation = "100dp" 阴影

app:cardElevation
app:cardCornerRadius
app:cardPreventCornerOverlap 防止拐角重叠



SVG 指令说明 www.w3.org/TR/SVG11/paths.html#PathData

SVG 编辑器
在线  svg-edit.googlecode.com/svn/branches/stable/editor/svg-editor.html
离线 inkscape.org/en/download/mac-os/



if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP)
{
	//可以使用材料设计
}else{
	//低于Android5.0
}



自定义控件
特定的风格、有些嵌套的控件可以用自定义来提高效率


android frameworks 文件在哪儿
frameworks/base/core/res/res/values/atts.xml;


**targetSdkVersion相对复杂一些，如果设置了此属性，那么在程序执行时，如果目标设备的API版本正好等于此数值，他会告诉Android平台：此程序在此版本已经经过充分测，没有问题。不必为此程序开启兼容性检查判断的工作了。也就是说，如果targetSdkVersion与目标设备的API版本相同时，运行效率可能会高一些。
但是，这个设置仅仅是一个声明、一个通知，不会有太实质的作用，比如说，使用了targetSdkVersion这个SDK版本中的一个特性，但是这个特性在低版本中是不支持的，那么在低版本的API设备上运行程序时，可能会报错：java.lang.VerifyError。也就是说，此属性不会帮你解决兼容性的测试问题。你至少需要在minSdkVersion这个版本上将程序完整的跑一遍来确定兼容性是没有问题的。**

* 在values 中新建 atts.xml 
申明控件的相关属性
<declare-styleeable name="Topbar">
	<attr name="title" format="string"/>
	.
	.
	.
	<attr name="leftBackground" format="reference|color" reference 是代表参考已经定义的资源@color color就是直接设置颜色

* 在java 中类的构造方法中将属性值给控件 
	TypeArray ta = context.obtainStyledAttributes(attrs,R.styleable.Topbar);
	leftTextColor = ta.getColor(R.stylelable.Topbar_leftTextColor,0); //0 是默认值 
	leftBackground = ta.getDrable(R.stylelable.Topbar_leftBackground);
	.
	.
	.
	ta.recycle(); 对TypeArray进行一个回收，避免浪费资源，避免因为缓存所引起的一些错误。
	
	例如这个类是继承自ReliativeLayout 然后在构造方法中将相应的控件的LayoutParams 的参数添加进去
	LayoutParams leftParams;
	leftParams = new LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT,LayoutParams.WRAP_CONTENT);
	leftParams.addRule(RelativeLayout.ALIGN_PARENT_LEFT,TRUE);
	addView(leftButton, leftParams);//将这个控件和参数添加进去
	
	然后通过接口回调来监听控件的点击事件
	
	
	
	```
	String to = "回复";
			String toUserName = commentList.get(position).getToUser() + " : ";
			StringBuffer sb = new StringBuffer();
			String contents = fromUserName + to + toUserName + content;
			sb.append(contents);
			SpannableStringBuilder spannable = new SpannableStringBuilder(sb.toString());
			int start = 0;
			int end = fromUserName.length();
			int color = Color.rgb(110, 172, 224);
			spannable.setSpan(new ForegroundColorSpan(color), start, end, Spannable.SPAN_EXCLUSIVE_INCLUSIVE);
			start = end + to.length();
			end = start + toUserName.length();
			spannable.setSpan(new ForegroundColorSpan(color), start, end, Spannable.SPAN_EXCLUSIVE_INCLUSIVE);
			holder.contents.setText(spannable);
	```



