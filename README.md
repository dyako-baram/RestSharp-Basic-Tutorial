# RestSharp Basic Tutorial
# RestSharp NuGet Package Tutorial

RestSharp is the most popular REST API client library for .NET Featuring automatic serialization and deserialization.

## GET http verb

To start first initialize `RestClient` like this:

`IRestClient client = new RestClient();` 

or add base URL in the constructor for example...

`IRestClient client = new RestClient("https://jsonplaceholder.typicode.com");`

---

then initialize `RestRequest`  like this:

`IRestRequest request = new RestRequest();`

or we can specify an end point and http verb like this:

`IRestRequest request = new RestRequest("posts",Method.GET);` 

if we have resource we also can add format we will receive for example: 

`IRestRequest singleRequest = new RestRequest("posts",Method.GET,DataFormat.Json);`

---

send GET request with URL segments like `posts/1`:

if there is a URL segments we add it like this

```c#
IRestClient client = new RestClient("https://jsonplaceholder.typicode.com");
			IRestRequest singleRequest = new RestRequest("posts/{myId}",Method.GET);
			singleRequest.AddUrlSegment("myId",1);
```

---

to add parameters like `Posts?userId=1` we do as follow

```c#
IRestClient client = new RestClient("https://jsonplaceholder.typicode.com");
            IRestRequest request = new RestRequest("posts",Method.GET);
            request.AddParameter("userId","1");
```

**simple example**

note: we used `Data` to desterilize it so it can be mapped to our object.

```C#
class Program
    {
        static void Main(string[] args)
        {
            IRestClient client = new RestClient("https://jsonplaceholder.typicode.com");
            IRestRequest request = new RestRequest("posts",Method.GET);
            List<Posts> response = client.Execute<List<Posts>>(request).Data;
            foreach (var item in response)
            {
                Console.WriteLine(item.Title);
                Console.WriteLine(item.Body);
                Console.WriteLine("---------------");
            }
        }
    }
    public class Posts
    {
        
        public int UserId { get; set; }
        public int Id { get; set; }
        public string Title { get; set; }
        public string Body { get; set; }
    }
```

## POST Http Verb

if we want to POST data all we have to change is method type and add `AddJsonBody` which it takes an object or anonymous object

in this case bellow the Api sends back newly added data so we mapped the result to an object

```c#
IRestClient client = new RestClient("https://jsonplaceholder.typicode.com");
            IRestRequest request = new RestRequest("posts",Method.POST);
            request.AddJsonBody(new
            {
                UserId = 999,
                Title="my title",
                Body="my body"
            });
            Posts response = client.Execute<Posts>(request).Data;

            Console.WriteLine(response.Id);
            Console.WriteLine(response.UserId);
            Console.WriteLine(response.Title);
            Console.WriteLine(response.Body);
```

## DELETE Http Verb

Delete requests are simpler than previous requests 

the `response.IsSuccessful` returns a Boolean 

```c#
IRestClient client = new RestClient("https://jsonplaceholder.typicode.com");
            IRestRequest request = new RestRequest("posts/{id}",Method.DELETE);
            request.AddUrlSegment("id",6);
            var response = client.Execute(request);
            Console.WriteLine(response.IsSuccessful);
```

## PUT Http Verb

```c#
 IRestClient client = new RestClient("https://jsonplaceholder.typicode.com");
            IRestRequest request = new RestRequest("posts/{id}",Method.PUT);
            request.AddUrlSegment("id","1");
            request.AddJsonBody(new
            {
                Id = 1,
                UserId=1,
                Title="my title",
                Body="my body"
            });
            Posts response = client.Execute<Posts>(request).Data;
```

## PATCH Http Verb

Patch is used for partial update

```c#
IRestClient client = new RestClient("https://jsonplaceholder.typicode.com");
            IRestRequest request = new RestRequest("posts/{id}",Method.PATCH);
            request.AddUrlSegment("id","1");
            request.AddJsonBody(new 
            {
                Id = 1,
                UserId=1,
                Title="my title",
            });
            Posts response = client.Execute<Posts>(request).Data;
```

