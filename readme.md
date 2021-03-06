##Overview
Users may wish to change the look of a chart in a QlikView app, and it’s possible to add shortcuts to allow them to easily make changes to properties. In this example, an app is created that allows the user to toggle the orientation and whether nulls are displayed. 

![alt tag](https://github.com/kristywedel/QlikView/blob/master/QlikView1.png)



##Installation Steps

###Step 1: Create a variable to store property value.
In this example, these variables were created in the script.

	LET vChartOrientation = 'true';
	LET vChartNulls = 'true';

###Step 2: Change macro to use object name.
Add the code below to the macros and modify the line set chart = ActiveDocument.GetSheetObject("CH01")  with the object id in the QlikView in the code.

	Sub ReverseOrientation
		set ChartOrientation = ActiveDocument.Variables("vChartOrientation")
		set chart = ActiveDocument.GetSheetObject("CH01") 
		set p = chart.GetProperties
		p.ChartProperties.Horizontal = ChartOrientation.GetContent.String
		chart.SetProperties p
	End Sub

	Sub SetNulls
		set ChartNull = ActiveDocument.Variables("vChartNulls")	
		set chart = ActiveDocument.GetSheetObject("CH01") 
		set cp = chart.GetProperties	
		set dims = cp.Dimensions	
		dims(0).NullSuppression = ChartNull.GetContent.String	
		chart.SetProperties cp	
	End Sub

###Step 3: Make a text object/button to set variable and run macro.
Add two actions to the text or button object. One action will toggle the variable and the other will run the macro. 

For the Show/Hide Nulls button,

	vChartNulls =if(vChartNulls = 'false', 'true', 'false') 
	Run Macro = SetNulls

For the Show/Hide Nulls button,

	vChartOrientation =if(vChartOrientation = 'false', 'true', 'false') 
	Run Macro = ReverseOrientation 

![alt tag](https://github.com/kristywedel/QlikView/blob/master/QlikView.png)

