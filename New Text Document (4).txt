package com.example.mysqlphpjson;
import com.example.mysqlphpjson.MainActivity;
import android.app.Activity;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ImageView;
import android.widget.TextView;

public class Faculty extends Activity{

	TextView  text;
	Button button;
	 
	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.faculty);
		text = (TextView)findViewById(R.id.takeName);
	text.setText(MainActivity.Array[MainActivity.i]);
	 int loader = R.drawable.ic_launcher;
     
     // Imageview to show
     ImageView image = (ImageView) findViewById(R.id.imageView1);
      
     // Image url
    // String image_url = MainActivity.Array[MainActivity.i];
    String image_url  = MainActivity.Image[MainActivity.i];
     ImageLoader imgLoader = new ImageLoader(getApplicationContext());
      
     // whenever you want to load an image from url
     // call DisplayImage function
     // url - image url to load
     // loader - loader image, will be displayed before getting image
     // image - ImageView 
     imgLoader.DisplayImage(image_url, loader, image);
	}
}
