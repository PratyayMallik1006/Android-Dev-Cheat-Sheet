# Android-Dev-Cheat-Sheet
# Android Studio Overview
1. Code in **MainActivity.java** file
2. Design in **res>layout>activity_main.xml** file
3. Add resources(images) in **res>drawable** folder

## Adding logo:
1. right click on **res** folder
2. Goto **new>image assets**
3. Select **Icon Type:** as "Launcher Icon"

# Design Basics
## Layouts
1. **LinearLayout(vertical):** Arranges views vertically 
2. **LinearLayout(horizontal):** Arranges views horizontally 
 ```xml
<LinearLayout
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:orientation="horizontal">

<ImageView
...
  />

<ImageView
...
 />

</LinearLayout>
 ```

# Button
1. In activity_main.xml
```xml
<Button  
  android:id="@+id/rollButton"  
  android:layout_width="wrap_content"  
  android:layout_height="wrap_content"  
  android:layout_centerHorizontal="true"  
  android:layout_centerVertical="true"  
  android:text="@string/button_text" />
```
2. MainActivity.java
```java
Button rollButton;  
rollButton = (Button) findViewById(R.id.rollButton);  
 
rollButton.setOnClickListener(new View.OnClickListener() {  
    @Override  
  public void onClick(View v) {  
        Log.d("tag","msg:Button Pressed");  
  }  
});
```
# Images
1. In activity_main.xml
```xml
<ImageView  
  android:id="@+id/image_leftDice"  
  android:layout_width="wrap_content"  
  android:layout_height="wrap_content"  
  android:layout_weight="1"  
  app:srcCompat="@drawable/dice2" />
```
2. MainActivity.java
```java
ImageView leftDice = (ImageView) findViewById(R.id.image_leftDice);  
  
final int[] diceArray ={  
	R.drawable.dice1, 
	R.drawable.dice2,
	R.drawable.dice3
};
//int array of resId

leftDice.setImageResource(diceArray[number]); // int resId
