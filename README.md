# Mati Android SDK documentation 


### Install Mati SDK via Gradle

Ensure that your top-level build.gradle contains a reference to the following repository:

	 maven { url 'https://repo1.maven.org/maven2' }

Enable Java 1.8 source compatibility if you haven't yet.

	android {
	    compileOptions {
		sourceCompatibility = JavaVersion.VERSION_1_8
		targetCompatibility = JavaVersion.VERSION_1_8
	    }
	}
	

Add this line into gradle dependencies
  
    implementation ('com.getmati:mati-sdk:3.8.2'){
        exclude group: 'org.json', module: 'json'
    }
    
Sync project with gradle files
    
##### ! Dependencies (will be automatically installed with Mati library)
    
    io.socket:socket.io-client:0.8.3

## Usage

#### 1) You now need to place MatiButton inside your layout
```xml    
<com.getmati.mati_sdk.MatiButton
        android:id="@+id/matiKYCButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        app:color="@color/matiButtonColor"
        app:textColor="@color/matiButtonTextColor"
        app:text="YOUR CUSTOM TEXT" />
```

#### 2) In order to authorize app and start verification, call setParams with the following arguments:
`(clientId: @NonNull String, flowId: @Nullable String, buttonTitle:  @NonNull String (Optional) metadata: @Nullable Metadata?)` 

##### Java
```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    setContentView(R.layout.activity_main);
    
    ...

    this.<MatiButton>findViewById(R.id.matiKYCButton).setParams(
    	this,
        "CLIENT_ID", 
        "FLOW_ID", 
        "BUTTON_TITLE", 
        METADATA);
}
```

##### Kotlin
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    setContentView(R.layout.activity_main);

    ...
    
    findViewById<MatiButton>(R.id.matiKYCButton).setParams(
    	this,
        "CLIENT_ID", 
        "FLOW_ID", 
        "BUTTON_TITLE", 
        METADATA)
}
```

#### 3) Listen for KYCActivity result

##### Java
```Java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if(requestCode == MatiSdk.REQUEST_CODE) {
        if(resultCode == RESULT_OK) {
            Toast.makeText( this,"SUCCESS!!!", Toast.LENGTH_LONG).show();
        } else {
            Toast.makeText( this,"CANCELLED!!!", Toast.LENGTH_LONG).show();
        }
    } else {
        super.onActivityResult(requestCode, resultCode, data);
    }
}
```

##### Kotlin
```Kotlin
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    if (requestCode == MatiSdk.REQUEST_CODE) {
        if (resultCode == RESULT_OK) {
            Toast.makeText(this, "SUCCESS!!!", Toast.LENGTH_LONG).show()
        } else {
            Toast.makeText(this, "CANCELLED!!!", Toast.LENGTH_LONG).show()
        }
    } else {
        super.onActivityResult(requestCode, resultCode, data)
    }
}
```
    
#### 4) Check complete code for your activity

##### Java
```java
public class YourActivity extends AppCompatActivity implements MatiCallback {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_main);

        this.<MatiButton>findViewById(R.id.matiKYCButton).setParams(
    	    this,
            "CLIENT_ID", 
            "FLOW_ID", 
            "BUTTON_TITLE"
            new Metadata.Builder()
                .with("key_1", "value1")
                .with("key2", 2)
                .build());
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if(requestCode == MatiSdk.REQUEST_CODE) {
            if(resultCode == RESULT_OK) {
                Toast.makeText( this,"SUCCESS!!!", Toast.LENGTH_LONG).show();
            } else {
                Toast.makeText( this,"CANCELLED!!!", Toast.LENGTH_LONG).show();
            }
        } else {
            super.onActivityResult(requestCode, resultCode, data);
        }
    }
}
```

##### Kotlin
```kotlin
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_second)

        findViewById<MatiButton>(R.id.matiKYCButton).setParams(
    	    this,
            "CLIENT_ID", 
            "FLOW_ID", 
            "BUTTON_TITLE"
            Metadata.Builder()
                .with("key_1", "value1")
                .with("key2", 2)
                .build())
    }

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        if (requestCode == MatiSdk.REQUEST_CODE) {
            if (resultCode == RESULT_OK) {
                Toast.makeText(this, "SUCCESS!!!", Toast.LENGTH_LONG).show()
            } else {
                Toast.makeText(this, "CANCELLED!!!", Toast.LENGTH_LONG).show()
            }
        } else {
            super.onActivityResult(requestCode, resultCode, data)
        }
    }
}
```
## SPECIFIC PARAMETERS

You can use metadata to set specific parameters

##### Fixed selected language and hiding the language selection. (to make it permanent)

key: fixedLanguage
value: locale code of language

###### example

for Spain (it can be any country, if we doesn't support language yet it will be setted to English)

##### fixedLanguage: es

###### full example
```kotlin
Metadata.Builder()
                .with("fixedLanguage", "es")
                .build())
```
   
##### Set Mati button and Primary Buttons customization

When using MatiSdk class to start the flow you can set primary buttons color with metadata

key: buttonColor
value: parsed color-int value

key: buttonTextColor
value: parsed color-int value

###### full example
```kotlin
Metadata.Builder()
                .with("buttonColor", Color.parseColor("#0097A7"))
	    	.with("buttonTextColor", Color.parseColor("#FFFFFF"))
                .build())
```

In case you use MatiButton, this values will be ignored and MatiButton's colors will be applied.

### Requirements 
   
Our SDK requires Android v5.0 (API v21) or above.

   For Mati SDK below 3.x.x please use this documentation https://github.com/GetMati/mati-android-sdk/blob/master/README_old__2_x_x_.md


