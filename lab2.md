# Lab 2
## String Server

`StringServer.Java`
```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String words;
    public String handleRequest(URI url) {
	if (words == null){
	    words = "";
	}
        if (url.getPath().equals("/")) {
	    String listString = String.join("\n", words);
            return listString;
        } else if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    String word = parameters[1];
		            words += word + "\n";
                    return words;
                }
                else {
                    return "Add Query ?s=<String>";
                }
            }
		else {
            return "404 Not Found!";
        }
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```
`Server.java`
```
// A simple web server using Java's built-in HttpServer

// Examples from https://dzone.com/articles/simple-http-server-in-java were useful references
interface URLHandler {
    String handleRequest(URI url);
}

class ServerHttpHandler implements HttpHandler {
    URLHandler handler;
    ServerHttpHandler(URLHandler handler) {
      this.handler = handler;
    }
    public void handle(final HttpExchange exchange) throws IOException {
        // form return body after being handled by program
        try {
            String ret = handler.handleRequest(exchange.getRequestURI());
            // form the return string and write it on the browser
            exchange.sendResponseHeaders(200, ret.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(ret.getBytes());
            os.close();
        } catch(Exception e) {
            String response = e.toString();
            exchange.sendResponseHeaders(500, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

public class Server {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        //create request entrypoint
        server.createContext("/", new ServerHttpHandler(handler));

        //start the server
        server.start();
        System.out.println("Server Started! Visit http://localhost:" + port + " to visit.");
    }
}
```

<img width="371" alt="image" src="https://user-images.githubusercontent.com/87511256/214713401-32f02057-d2b9-4799-8341-b9ea20a5d65f.png">

The main method is used to start the Server.
The `Handler` `handleRequest` method is called whenever the server is updated with a url. 
The main method has a `String[] args` that is used to take in the integer in `args[0]` to be the port number.
The `handleRequest` method takes in a `URI` named `url` which is `https:localhost:4000/add-message?s=Hello`.
The class variable words for the Handler is created as `""` and then added to with the end of the URI `Hello` to get `words=Hello\n`.

<img width="416" alt="image" src="https://user-images.githubusercontent.com/87511256/214714914-cc8fa46b-eeff-4adc-afce-28e9e6b1be8c.png">

The `Handler` `handleRequest` method is called whenever the server is updated with a url.
The `handleRequest` method takes in a `URI` named `url` which is `https:localhost:4000/add-message?s=How are you`.
The class variable words for the Handler starts as `Hello\n` and then `How are you\n` is added to get `words=Hello\nHow are you\n`. 

## Debugging

From the `ListExamples.java`
```
static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index1 += 1;
    }
    return result;
  }
```

### The JUnit Test that fails
```
@Test
    public void testMerge() {
        List<String> input1 = new ArrayList<String>();
        List<String> input2 = new ArrayList<String>();
        input1.add("m");
        input1.add("o");
        input2.add("a");
        input2.add("b");
        input2.add("z");
        assertArrayEquals(new String[] {"a", "b", "m", "o", "z"}, ListExamples.merge(input1, input2).toArray());
    }
```

This method fails because after adding all of input1 and `a` and `b` of `input2` it needs to add `z` but the code does not increment correctly so it doesn't loop through the rest of the second array, which causes an infinite loop. The test will never end so it fails.
### The JUnit Test that passes
```
@Test
    public void testMergePass() {
        List<String> input1 = new ArrayList<String>();
        List<String> input2 = new ArrayList<String>();
        input1.add("m");
        input1.add("o");
        input2.add("a");
        input2.add("b");
        assertArrayEquals(new String[] {"a", "b", "m", "o"}, ListExamples.merge(input1, input2).toArray());
    }
```
This test checks both lists and adds the smaller character, in this case, `a` and `b` from `input2` are added first. Then, it exits the first wihle loop and enter the second one to add the rest of the `input1`. This is the expected behavior of this method.

### Running the JUnit Tests
<img width="361" alt="image" src="https://user-images.githubusercontent.com/87511256/214719587-fef0ce03-d281-4966-8582-e58d5e0e6fd9.png">

This code never has an assertion error because there is an infinite loop. It won't show a test has failed.

### Buggy Code
```
static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index1 += 1;
    }
    return result;
  }
```
### Fixed Code
```
static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index2 += 1;
    }
    return result;
  }
```

This fix was updating in the last while loop for the increment of the index. It used to be `index1` and now it is `index2`.
When the second list is longer than the first list and still has strings to add after the first list is completely added, the last while loop will iterate through the second list till the end.
Before, the second list would get stuck in an infinite loop because `index2` would never be larger than the `list2.size()`.

## What I learned from Week 2 and 3
Basic Web Servers are pretty easy to setup and can be run on a local computer through `http://localhost:<Port #>`.
It can also be accessed when you ssh into a domain and then connect with `ieng6-<Computer #>.ucsd.edu` to get to a server hosted on the ucsd server.
I also learned that urls have querys to specify certain inputs for variables. 

