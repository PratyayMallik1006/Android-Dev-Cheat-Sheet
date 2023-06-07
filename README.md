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
**OR :**
1. In activity_main.xml
```xml
<Button  
  android:id="@+id/a_key"   
  android:layout_width="match_parent"  
  android:layout_height="wrap_content"  
  android:onClick="playA"/>
```
2. MainActivity.java
```java
public void playA(View v){  
    mSoundPool.play(mASoundId,1.0f,1.0f,0,0,1.0f);   
}
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

```
# Media Player
Know More: 
- https://developer.android.com/guide/topics/media/mediaplayer
- https://developer.android.com/reference/android/media/SoundPool

```java
SoundPool  mSoundPool= new  SoundPool(NR_OF_SIMULTANEOUS_SOUNDS, AudioManager.STREAM_MUSIC,0);

int  mASoundId=mSoundPool.load(getApplicationContext(),R.raw.note6_a,1);
//(context, resourceId,priority)

mSoundPool.play(mASoundId,1.0f,1.0f,0,0,1.0f);
//(soundId, leftVol, rightVol, priority, loop, rate)

//Loop: 0 = no loop, -1 = loop forever


```
# Toast
```java
Toast.makeText(getApplicationContext(),"Hello",Toast.LENGTH_SHORT).show();
```
# Progress Bar
1. activity_main.xml 
```xml
<ProgressBar
android:layout_width="fill_parent"
android:layout_height="wrap_content"
android:id="@+id/progress_bar"
android:indeterminate="false"  />
```
2. MainActivity.java
```java
int  PROGRESS_BAR_INCREMENT = (int)Math.ceil(100.0/mQuestionBank.length);
ProgressBar mProgressBar=(ProgressBar) findViewById(R.id.progress_bar);
mProgressBar.incrementProgressBy(PROGRESS_BAR_INCREMENT);
```
# Screen Rotation
1. Fixing to portrait mode
- Goto **Manifests>AndroidManifest.xml**
```xml
<activity android:name=".MainActivity"
        android:screenOrientation="portrait">
		...
</activity>
```
2. onSaveInstanceState
```java
@Override
protected  void  onCreate(Bundle  savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_main);
	if(savedInstanceState != null){
		score = savedInstanceState.getInt("ScoreKey");
	} else {
		score=0;
	}
}

@Override
public void onSaveInstanceState(Bundle outState){
	super.onSaveInstanceState(outState);
	
	outState.putInt("ScoreKey", score);
}
```

# Alert Dialogue
```java
AlertDialog.Builder alert = new AlertDialog.Builder(getApplicationContext());
```
Or
```java
AlertDialog.Builder alert = new AlertDialog.Builder(this);
alert.setTitle("Game Over");
alert.setCancelable(false);
alert.setMessage("Your Score is:"+ score);

builder.setPositiveButton(``"Yes"``, (DialogInterface.OnClickListener) (dialog, which) -> {
	finish();
});

builder.setNegativeButton(``"No"``, (DialogInterface.OnClickListener) (dialog, which) -> {
	dialog.cancel();
});

alert.show();

```
