---
toc: True
layout: post
title: ChatGPT in Java
description: Overview of how to call the ChatGPT API from a Java file
courses: {'csa': {'week': 5, 'categories': []}}
categories: []
author: Bailey Say, Don Tran
type: ccc
---

## Overview

ChatGPT is a pretty useful AI model to access in your code without having to create your own model. Essentially, this tutorial will give you a pretty good idea of how to directly access the API in Java. Also, all this was written by me, Bailey, but Don also did a lot with getting this implementation to work.

Steps:

1. Get access to an API Key
2. Store the API Key securely in the code
3. Create a fetch request to the API in Java

And that's really it.

## Step 1: Get access to an API Key

Basically, not just anybody can access the ChatGPT API; you need a secret key to be allowed access to the API. Luckily, it's pretty easy to get one.

1. Create an Open AI account ([link here](https://openai.com/)). 
2. Go to your personal list of API keys ([link here](https://platform.openai.com/account/api-keys)).
3. Select "create new secret key"
4. Copy the given API key and store it somewhere
   
Note: Make sure to store the API key in somewhere safe, but you CANNOT leave this key in plaintext in a file and push it to the Github repository. Open AI will automatically detect the presence of this key and disable it for security reasons. If this ever happens, just create a new secret key. There are no penalties for this. 

5. Go to "Billing" and set up a paid account

Yes, you will have to pay money to use the API. It turns out that Open AI doesn't want you to use their all-powerful AI willy-nilly without a price, but luckily it doesn't cost that much. This billing relies on a token system, which is a bit confusing, but the gist of it is that you spend a certain amount of tokens everytime you make a request to ChatGPT (I believe the token cost is based on the number of words in the prompt, but I'm not too sure to be honest; [this article](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them) explains it well). As long as you don't completely abuse the API, you'll be paying less than $0.50 per month. Plus, I think they also give you $5 for the first 3 months, which is nice. In any case, don't worry about running out of money. 

## Step 2: Store the API Key securely in the code

If you want to just have fun accessing ChatGPT from Java without using it in a final deployed project, you can just skip this step. You can just probably put the key in plaintext and it won't complain as long as it isn't pushed to the repostiory. This step is only necessary if you want to access ChatGPT safely and securely for a deployed project.

Unfortunately, this was the most painful part of getting everything to work. There are really two ways of doing this that we know of. The first involves storing the API key inside the Github repository and directly accessing it inside the code. The second involves adding a plaintext file with the API key onto the repo but then adding it into .gitignore and using a file reader to access the key.

In theory, the first method should be the correct way to do things, but we couldn't get it to work for whatever reason. The steps will be listed below, but if you want a method that's guaranteed to work (though may be slightly scuffed) you can just skip these.

1. Go to the backend Github repository.
2. Access settings (note that only contributers with admin permissions are allowed to access settings).
3. Access secrets, then access environmental variables
4. Copy and paste your API key into a newly created environmental variable
5. Access the variable through a .yml file

Step 5 is what we were struggling on. Whether it be some issue with the environmental variable itself or the way we accessed it in the .yml file, we couldn't get it to work.

Instead, as an alternative, we (more like Don) utilized .gitignore to store the file without it being pushed to the main repo and a file reader to read that file with these steps below:

1. Create a .txt file on the AWS local server with just the key inside of it 
   
(I'm going to be honest, this part was a bit confusing for me, because I didn't actually do it, Don did. But essentially it works because the deployed website will read from the .txt file on the AWS server and not from the pushed version on the repo)

2. Add the .txt file to .gitignore

This part may seem a bit complicated, but it really isn't that bad. You can just add the below to the bottom of your .gitignore and everything should be fine.


```java
### VSCODE ###
.vscode/
key.txt
```

Obviously, key.txt should be the name of your file.

And this should prevent key.txt from being pushed into the repository, so everything's good.

3. Create a file reader to read the file (written by Don)
   
Create a class file and name it something related to reading the file. In this case we named it KeyFileReader. You could reasonably put your key reader as a function inside the class calling the ChatGPT API. First you create a string with the file path to your key.txt file (NOTE: the file path should be based on the root being your repo; for instance, the path for the following code is "src/main/java/com/nighthawk/spring_portfolio/database/chat/key.txt"). Then we create a try except case using a BufferedReader object that passes in a FileReader objectto read a file. Then we use a while loop to wait for the file to be read before returning the key which is the first line in key.txt. We return the error in case anything goes wrong.


```java
public class KeyFileReader {
    public static String getKey() {
        String filePath = "src/main/java/com/nighthawk/spring_portfolio/database/chat/key.txt";
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = br.readLine()) != null) {
                // Process first line in the file (key)
                return line;
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        return "error";
    }
}
```

We can simply call the class in our ChatGPT file to then access the key.


```java
String API_KEY = KeyFileReader.getKey();
```

## Step 3: Create a fetch request to the API in Java

This step is pretty easy, all you have to do is set up an HttpURLConnection object with the right properties and make the request. The HttpURLConnection class can basically be thought of as an equivalent to the fetch() function in Javascript, only now you have to set all the parameters of the fetch call using methods instead of parameters.

Next you set up a JSONObject object with the right properties to specify the model, prompt, maximum tokens (limit), and temperature (variability), just like the HttpURLConnection object.

Finally, you have to actually call the API with the HttpURLConnection methods .setDoOutput() and .getOutputStream() while writing the JSON data to it, then you can use some strange navigating through the output of the call to actually get the text you want. I wouldn't worry too much about this last part, it's easier just to copy than try to decipher exactly what it's doing. Just know that it's just accessing the text you want from the mess of data.


```java
public class Chat {

    public static String daVinciTest(String text) throws MalformedURLException, IOException {
        String url = "https://api.openai.com/v1/completions";
        HttpURLConnection con = (HttpURLConnection) new URL(url).openConnection();

        con.setRequestMethod("POST");
        con.setRequestProperty("Content-Type", "application/json");
        con.setRequestProperty("Authorization", "Bearer " + KeyFileReader.getKey());

        JSONObject data = new JSONObject();
        data.put("model", "text-davinci-003");
        data.put("prompt", text);
        data.put("max_tokens", 4000);
        data.put("temperature", 1.0);

        con.setDoOutput(true);
        con.getOutputStream().write(data.toString().getBytes());

        String output = new BufferedReader(new InputStreamReader(con.getInputStream())).lines()
                .reduce((a, b) -> a + b).get();

        return (new JSONObject(output).getJSONArray("choices").getJSONObject(0).getString("text"));
    }

}
```

## And that's it

That's really the basic steps to use the ChatGPT API in Java. Obviously, there's a lot more configuring that can be done though. You can mess with the prompts, the models, the number of times used, and basically whatever you can imagine. 

For our own website, we heavily took advantage of the ChatGPT API for half of our features and customized the calls in many ways to make it fun, which you can find at [Rizz AI](https://hetvit27.github.io/freelancer-theme/) (I'm not sure if the website will still be up for long though).

One thing that you may have noticed is that the model used in this tutorial is actually davinci-003, and not gpt-3.5-turbo (which is closer to the actual model used in ChatGPT). There are only subtle differences between the two, so if you don't care that much then davinci-003 will serve you well, but for those of you seeking gpt-3.5-turbo, unfortunately we could not get it to work with this Java implementation. Instead, we decided to use a Python implementation of the API call and then use a Python reader in Java to actually access its output. Unfortunately though, I (Bailey) have no idea how Don did it, and I don't really feel like explaining all too much, since there are much better tutorials out there for ChatGPT API calls in Python (but maybe Don will write that part later). 

But, if everything went well, you should be able to access ChatGPT in your own deployed website and abuse it for whatever nefarious purposes you have. Congratulations!
