# LiquidNetwork
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.Authenticator;
import java.net.PasswordAuthentication;

public class liquidrpcjava {

        public static void main (String []args) throws IOException{
                URL url = new URL ("http://127.0.0.1:18884");
                String rpcuser ="user1";
                String rpcpassword ="password1";

                Authenticator.setDefault(new Authenticator() {
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication (rpcuser, rpcpassword.toCharArray());
            }
        });

                HttpURLConnection conn = (HttpURLConnection)url.openConnection();
                conn.setRequestMethod("POST");
                conn.setRequestProperty("Content-Type", "application/json; utf-8");
                conn.setRequestProperty("Accept", "application/json");
                conn.setDoOutput(true);

                String json = "{\"method\": \"getwalletinfo\", \"jsonrpc\": \"2.0\"}";

                try(OutputStream os = conn.getOutputStream()){
                        byte[] input = json.getBytes("utf-8");
                        os.write(input, 0, input.length);
                }

                try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), "utf-8"))){
                        StringBuilder sb = new StringBuilder();
                        String line = null;

                        while ((line = br.readLine()) != null) {
                                sb.append(line.trim());
                        }

                        System.out.println(sb.toString());
                }
        }
}
# Compile and run code with command line
javac liquidrpcjava.java
java liquidrpcjava
