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

# Saving Data Locally
1. Storing
```java
private void saveDisplayName(){  
	String displayName = mUsernameView.getText().toString();  
	SharedPreferences prefs = getSharedPreferences(CHAT_PREFS,0);  
	prefs.edit().putString(DISPLAY_NAME_KEY,displayName).apply();  
}
```
2. Retriving
```java
private void setupDisplayName(){  
	SharedPreferences prefs=getSharedPreferences(RegisterActivity.CHAT_PREFS,MODE_PRIVATE);
	mDisplayName=prefs.getString(RegisterActivity.DISPLAY_NAME_KEY,null);  
	  
	if(mDisplayName == null) mDisplayName = "Anonymous";  
}
```
# Firebase
## Register user
```java
private FirebaseAuth mAuth;
mAuth = FirebaseAuth.getInstance();

String email = mEmailView.getText().toString();  
String password = mPasswordView.getText().toString();  
mAuth.createUserWithEmailAndPassword(email,password).addOnCompleteListener(this, new OnCompleteListener<AuthResult>() {  
    @Override  
  public void onComplete(@NonNull Task<AuthResult> task) {  
        Log.d("WhatsApp","createUser onComplete"+task.isSuccessful());  
  
 if(!task.isSuccessful()){  
            Log.d("WhatsApp","user creation failed");  
  }  
    }  
});
```
## Login User
```java
private FirebaseAuth mAuth;
mAuth = FirebaseAuth.getInstance();

private void attemptLogin() {  
  
    String email = mEmailView.getText().toString();  
  String password = mPasswordView.getText().toString();  
  
 if(email.equals("") || password.equals("")) return;  
  Toast.makeText(this, "Login in progress...",Toast.LENGTH_SHORT).show();  
  
  // TODO: Use FirebaseAuth to sign in with email & password  
  mAuth.signInWithEmailAndPassword(email,password).addOnCompleteListener(this, new OnCompleteListener<AuthResult>() {  
        @Override  
  public void onComplete(@NonNull Task<AuthResult> task) {  
  
            Log.d("WhatsApp","signInWithEmail() onComplete: "+task.getException());  
  
 if(!task.isSuccessful()){  
                Log.d("WhatsApp","Problem signing in:"+task.getException());  
  showErrorDialog("There was a problem signing in");  
  } else {  
                Intent intet=new Intent(LoginActivity.this,MainChatActivity.class);  
  startActivity(intet);  
  }  
  
        }  
    });  
  
  
  
}
```
## Storing and retrieving from database
1. InstanceMessage.java
```java

public class InstanceMessage {  
	private String message;  
	private String author;  
	  
	public InstanceMessage(String message, String author) {  
		this.message = message;  
		this.author = author;  
	}  
	  
	public InstanceMessage() {  
	}  
	  
	public String getMessage() {  
		return message;  
	}  
	  
	public String getAuthor() {  
		return author;  
	}
}
```
2. ChatActivity.java
```java
private DatabaseReference mDatabaseReference;
mDatabaseReference = FirebaseDatabase.getInstance().getReference();

private void sendMessage() {  
	String input = mInputText.getText().toString();  
	if(!input.equals("")){  
		InstanceMessage chat = new InstanceMessage(input,mDisplayName);  
		mDatabaseReference.child("messages").push().setValue(chat);  
		mInputText.setText("");    
	}   
}
```
3. Firebase>project> Realtime database> Rules:
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
## Retrieving and retrieving from database
1. chatListAdapter.java
```java

public class ChatListAdapter extends BaseAdapter {  
  
private Activity mActivity;  
private DatabaseReference mDatabaseReference;  
private String mDisplayName;  
private ArrayList<DataSnapshot> mSnapshotList;  
  
private ChildEventListener mListener = new ChildEventListener() {  
@Override  
public void onChildAdded(@NonNull DataSnapshot snapshot, @Nullable String previousChildName) {  
mSnapshotList.add(snapshot);  
notifyDataSetChanged();  
}  
  
@Override  
public void onChildChanged(@NonNull DataSnapshot snapshot, @Nullable String previousChildName) {  
  
}  
  
@Override  
public void onChildRemoved(@NonNull DataSnapshot snapshot) {  
  
}  
  
@Override  
public void onChildMoved(@NonNull DataSnapshot snapshot, @Nullable String previousChildName) {  
  
}  
  
@Override  
public void onCancelled(@NonNull DatabaseError error) {  
  
}  
};  
  
public ChatListAdapter(Activity activity, DatabaseReference ref, String name){  
	mActivity=activity;  
	mDisplayName=name;  
	mDatabaseReference=ref.child("messages");  
	mDatabaseReference.addChildEventListener(mListener);  
	  
	mSnapshotList = new ArrayList<>();  
	  
  
}  
  
static class ViewHolder{  
	TextView authorName;  
	TextView body;  
	ViewGroup.LayoutParams params;  
}  
  
  
@Override  
public int getCount() {  
	return mSnapshotList.size();  
}  
  
@Override  
public InstanceMessage getItem(int position) {  
	DataSnapshot snapshot = mSnapshotList.get(position);  
	return snapshot.getValue(InstanceMessage.class);  
}  
  
@Override  
public long getItemId(int position) {  
	return 0;  
}  
  
@Override  
public View getView(int position, View convertView, ViewGroup parent) {  
if(convertView == null){  
	LayoutInflater inflater=(LayoutInflater) mActivity.getSystemService(Context.LAYOUT_INFLATER_SERVICE);  
	convertView = inflater.inflate(R.layout.chat_msg_row,parent,false);  
	final ViewHolder holder=new ViewHolder();  
	holder.authorName=(TextView) convertView.findViewById(R.id.author);  
	holder.body=(TextView) convertView.findViewById(R.id.message);  
	holder.params=(LinearLayout.LayoutParams) holder.authorName.getLayoutParams();  
	convertView.setTag(holder);  
  
}  
final InstanceMessage message=getItem(position);  
final ViewHolder holder=(ViewHolder) convertView.getTag();  
  
String author = message.getAuthor();  
holder.authorName.setText(author);  
  
String msg = message.getMessage();  
holder.body.setText(msg);  
  
return convertView;  
}  
  
public void cleanup(){  
mDatabaseReference.removeEventListener(mListener);  
}  
}
```
2. mainActivity.java
```java
private ChatListAdapter mAdapter;
private DatabaseReference mDatabaseReference;

mDatabaseReference= FirebaseDatabase.getInstance().getReference();
mAdapter=new ChatListAdapter(this, mDatabaseReference,mDisplayName);  
mChatListView.setAdapter(mAdapter);
```
