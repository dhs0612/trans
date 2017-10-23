# trans
package com.light.administrator.translater;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import com.example.administrator.translater.R;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;

public class MainActivity extends AppCompatActivity {

    Button bttranslate;
    EditText edit_;
    EditText tvResult_2;
    EditText tvResult_1;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        setContentView(R.layout.activity_main);

        edit_ = (EditText) findViewById(R.id.edit);
        tvResult_1 = (EditText) findViewById(R.id.result_1);
        tvResult_2 = (EditText) findViewById(R.id.result_2);
        bttranslate = (Button) findViewById(R.id.button_trans);

        bttranslate.setOnClickListener(
                new View.OnClickListener() {

                    String clientId = "eFwtt5pRCpIYNTbzM0x_";// 애플리케이션 클라이언트 아이디값";
                    String clientSecret = "mUc7478IOS";// 애플리케이션 클라이언트 시크릿값";

                    @Override
                    public void onClick(View v) {


                        if (edit_.getText().toString().length() == 0) {
                            Toast.makeText(MainActivity.this, "번역할 내용을 입력하세요.", Toast.LENGTH_SHORT).show();
                            edit_.requestFocus();
                        }
                        else{


        try {

            String text = URLEncoder.encode(String.valueOf(edit_), "UTF-8");
            String apiURL = "https://openapi.naver.com/v1/language/translate";
            URL url = new URL(apiURL);
            HttpURLConnection con = (HttpURLConnection) url.openConnection();
            con.setRequestMethod("POST");
            con.setRequestProperty("X-Naver-Client-Id", clientId);
            con.setRequestProperty("X-Naver-Client-Secret", clientSecret);
            // post request
            String postParams = "source=ko&target=en&text=" + text;
            con.setDoOutput(true);
            DataOutputStream wr = new DataOutputStream(con.getOutputStream());
            wr.writeBytes(postParams);
            wr.flush();
            wr.close();
            int responseCode = con.getResponseCode();
            BufferedReader br;
            if (responseCode == 200) { // 정상 호출
                br = new BufferedReader(new InputStreamReader(con.getInputStream()));
            } else { // 에러 발생
                br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
            }
            String inputLine;
            StringBuilder response = new StringBuilder();
            while ((inputLine = br.readLine()) != null) {
                response.append(inputLine);
            }
            br.close();
            tvResult_2.setText(response.toString().split("translatedText")[1].replace("\":\"", "").replace("\",\"srcLangTypeko\"}}}", ""));
        } catch (Exception e) {
            tvResult_2.setText((CharSequence) e);
        }
        try {
            String text = URLEncoder.encode(String.valueOf(edit_), "UTF-8");
            String apiURL = "https://openapi.naver.com/v1/papago/n2mt";
            URL url = new URL(apiURL);
            HttpURLConnection con = (HttpURLConnection) url.openConnection();
            con.setRequestMethod("POST");
            con.setRequestProperty("X-Naver-Client-Id", clientId);
            con.setRequestProperty("X-Naver-Client-Secret", clientSecret);
            // post request
            String postParams = "source=ko&target=en&text=" + text;
            con.setDoOutput(true);
            DataOutputStream wr = new DataOutputStream(con.getOutputStream());
            wr.writeBytes(postParams);
            wr.flush();
            wr.close();
            int responseCode = con.getResponseCode();
            BufferedReader br;
            if (responseCode == 200) { // 정상 호출
                br = new BufferedReader(new InputStreamReader(con.getInputStream()));
            } else {  // 에러 발생
                br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
            }
            String inputLine;
            StringBuilder response = new StringBuilder();
            while ((inputLine = br.readLine()) != null) {
                response.append(inputLine);
            }
            br.close();
            tvResult_1.setText(response.toString().split("translatedText")[1].replace("\":\"", "").replace("\",\"srcLangTypeko\"}}}", ""));
        } catch (Exception e) {
            tvResult_1.setText((CharSequence) e);
        }
        }
        }
        });
    }
}
