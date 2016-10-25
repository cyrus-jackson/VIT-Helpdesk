package com.example.mysqlphpjson;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
 
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.DefaultHttpClient;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import android.os.AsyncTask;
import android.os.Bundle;
import android.app.Activity;
import android.content.Intent;
import android.view.Menu;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.AdapterView;
import android.widget.AdapterView.OnItemClickListener;
import android.widget.AutoCompleteTextView;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.SimpleAdapter;
import android.widget.TextView;
import android.widget.Toast;
 
public class MainActivity extends Activity {
 private String jsonResult;
 private String url = "http://vithelpdesk.coolpage.biz/590180/comments.php";
 private ListView listView;
 private String S1;
  public static int i=0;
 AutoCompleteTextView auto;
 private EditText edit;
 private Button button;
 public static String[] Array = new String[100];
 public static String[] Image = new String[100];
 @Override
 protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  listView = (ListView) findViewById(R.id.listView1);
  
  edit = (EditText)findViewById(R.id.inputSearch);
  auto=(AutoCompleteTextView) findViewById(R.id.autoCompleteTextView1);
  button = (Button)findViewById(R.id.button1);
  accessWebService();
  ButtonSearch();
  //S1 = listView.getSelectedView().toString();

  
  
 }
 public void ButtonSearch()
 {
	 button.setOnClickListener(new OnClickListener() {
		
		public void onClick(View v) {
			// TODO Auto-generated method stub
			//Toast.makeText(getBaseContext(), S1, Toast.LENGTH_LONG).show();
			//Arrays.asList(Array).contains(S1).getPosition();
			if(Arrays.asList(Array).contains(S1))
			{
				for(i=0;i<Array.length;i++)
			    {
					if(Array[i].contains(S1))
				{
					  Toast.makeText(getBaseContext(), ""+i, Toast.LENGTH_LONG).show();
					  Intent intent = new Intent(getApplicationContext(),Faculty.class);
					  startActivity(intent);
						break;
					}
			    }
			//	Arrays.binarySearch(Array, "")
				
			}
			else
				Toast.makeText(getBaseContext(), "False", Toast.LENGTH_LONG).show();
			
		}
	});
 }

/* public int Sorting(String[] array, String query)
 {
	 int a = 0;
		   for(int i = 0;i<array.length;i++)
			   {
			   if(array[i].contains(query))
			   
				   return a;
			 }
			 }*/
		

 @Override
 public boolean onCreateOptionsMenu(Menu menu) {
  // Inflate the menu; this adds items to the action bar if it is present.
  getMenuInflater().inflate(R.menu.main, menu);
  return true;
 }
 
 // Async Task to access the web
 private class JsonReadTask extends AsyncTask<String, Void, String> {
  @Override
  protected String doInBackground(String... params) {
   HttpClient httpclient = new DefaultHttpClient();
   HttpPost httppost = new HttpPost(params[0]);
   try {
    HttpResponse response = httpclient.execute(httppost);
    jsonResult = inputStreamToString(
      response.getEntity().getContent()).toString();
   }
 
   catch (ClientProtocolException e) {
    e.printStackTrace();
   } catch (IOException e) {
    e.printStackTrace();
   }
   return null;
  }
 
  private StringBuilder inputStreamToString(InputStream is) {
   String rLine = "";
   StringBuilder answer = new StringBuilder();
   BufferedReader rd = new BufferedReader(new InputStreamReader(is));
 
   try {
    while ((rLine = rd.readLine()) != null) {
     answer.append(rLine);
    }
   }
 
   catch (IOException e) {
    // e.printStackTrace();
    Toast.makeText(getApplicationContext(),
      "Error..." + e.toString(), Toast.LENGTH_LONG).show();
   }
   return answer;
  }
 
  @Override
  protected void onPostExecute(String result) {
   ListDrwaer();
  }
 }// end async task
 
 public void accessWebService() {
  JsonReadTask task = new JsonReadTask();
  // passes values for the urls string array
  task.execute(new String[] { url });
 }
 
 // build hash set for list view
 public void ListDrwaer() {
  List<Map<String, String>> CommentsList = new ArrayList<Map<String, String>>();
  
 
  try {
   JSONObject jsonResponse = new JSONObject(jsonResult);
   JSONArray jsonMainNode = jsonResponse.optJSONArray("posts");
   
   for (int i = 0; i < jsonMainNode.length(); i++) {
    JSONObject jsonChildNode = jsonMainNode.getJSONObject(i);
    String id = jsonChildNode.optString("post_id");
    String title = jsonChildNode.optString("title");
    String username = jsonChildNode.optString("username");
    Image[i] = jsonChildNode.optString("image");
   Array[i] = jsonChildNode.optString("message");
    String outPut = Array[i]+" - "+Image[i];
    CommentsList.add(createComment( outPut));


  

    

   }
  
  } catch (JSONException e) {
   Toast.makeText(getApplicationContext(), "Error" + e.toString(),
     Toast.LENGTH_SHORT).show();
  }
 
  SimpleAdapter simpleAdapter = new SimpleAdapter(this, CommentsList,
    android.R.layout.simple_list_item_1,
    new String[] { "" }, new int[] { android.R.id.text1 });
  listView.setAdapter(simpleAdapter);
  auto.setThreshold(1);
  auto.setAdapter(simpleAdapter);
  listView.setOnItemClickListener(new OnItemClickListener() {

	@Override
	public void onItemClick(AdapterView<?> arg0, View arg1, int arg2, long arg3) {
		// TODO Auto-generated method stub
		S1 = ((TextView)arg1).getText().toString();
		edit.setText(S1);
		Toast.makeText(getBaseContext(), S1, Toast.LENGTH_LONG).show();
	}
});
  
 }
 
 private HashMap<String, String> createComment(String number) {
  HashMap<String, String> employeeNameNo = new HashMap<String, String>();
  employeeNameNo.put("",number);
  return employeeNameNo;
 }
}