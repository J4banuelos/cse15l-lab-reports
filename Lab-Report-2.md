***Lab Report 2 (Week 3)***

```
import java.io.IOException;
 import java.net.URI;

 class Handler implements URLHandler {
    // The one bit of state on the server: a string that will be manipulated by
    // various requests.
    String chatHistory = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return chatHistory.isEmpty() ? "No messages yet." : chatHistory;
        } else if (url.getPath().contains("/add-message")) {
            String query = url.getQuery();
            if (query != null) {
                String[] params = query.split("&");
                String user = "", message = "";
                for (String param : params) {
                    String[] pair = param.split("=");
                    if (pair[0].equals("user")) {
                        user = pair[1];
                    } else if (pair[0].equals("s")) {
                        message = pair[1];
                    }
                }

                if (!user.isEmpty() && !message.isEmpty()) {
                    chatHistory += user + ": " + message.replace("+", " ") + "\n";
                    return chatHistory;
                }
            }
            return "Invalid request!";
        } else {
            return "404 Not Found!";
        }
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if (args.length == 0) {
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);
        Server.start(port, new Handler());
    }
}
```

![Image](LabReport2.png)

![Image](LabReport2.1.png)

![Image](LabReport2.2.png)

![Image](LabReport2.3.png)

X
