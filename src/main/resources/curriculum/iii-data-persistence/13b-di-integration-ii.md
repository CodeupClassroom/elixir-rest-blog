
# Dependency Injection & Controller Integration Part 2

This lesson focuses on adding create, update, and delete functionality for your `PostsController`. You may have already added some or all of that functionality, but once you create relationships between objects, things can get a little complicated. Furthermore, `update` requires special consideration.

## createPost

On one hand, creating a post in `PostsController` is pretty simple. You just need to use your `postsRepository` to `save` the incoming `Post` parameter. Any fields not provided will take `null` values and the database will automatically generate its primary key value.

However, now that posts have an `author` instance variable of type `User`, you should automatically set the new post's `author` appropriately. 
1. First, fetch a `User` object for whichever user you wish to associate as the author. In the future, we will set the `author` as the user logged into the front-end application. For now, let's just assign the author as the user with id 1.
2. Set the new post's `author` to that `User` object you fetched in #1 above.
3. Then save the post via the `postsRepository`.

Now... you are most likely asking "how in the world can I fetch a user in the `PostsController`??? This is for posts, not users!!!" 
Well, just use Dependency Injection to inject a `UsersRepository` into your `PostsController` in the same way you injected a `PostsRepository`. In fact, if you are using Lombok's fabulous `@AllArgsConstructor`, all you need to do is declare a `UserRepository` instance variable. And voila! Your `PostsController` can now work with `User` data. /golfclap

### Exercise

Modify your `PostsController` to save a new post with a pre-determined author of your choice (i.e., an id for a user record that is already in your database).

## updatePost



Dependency injection means that we will ***inject*** a class' ***dependencies*** instead of instantiating them manually.

Behind the scenes, Spring (and many other frameworks) create what is called a **container** to store instances of our objects which can be called upon whenever we need without having to use the `new` keyword.

Using DI (dependency injection) can be done as simply as follows:

```java
public class PostsController {
    // ...
    private final PostsRepository postRepository;
    
    public PostsController(PostsRepository postRepository) {
        this.postRepository = postRepository;
    }
    // ...
}
```

We can use DI in most of the classes in our Spring
application. We can even inject services into other services! 

This is how you can use it in order to get the list of all Ads.

```java
import com.example.restblog.data.PostRepository;

public class PostsController {

    // These two next steps are often called dependency injection, 
    // where we create a Repository instance and 
    // initialize it in the controller class constructor.
    private final PostsRepository postRepository;

    public PostsController(PostsRepository postRepository) {
        this.postRepository = postRepository;
    }

    @GetMapping
    public List<Post> getPosts() {
        
        // Because of DI, 
        // we don't have to do this:
       
        // PostRepositoryImpl repo = new PostRepositoryImpl()
        
        // Instead, we get this lovely snippet 
        // and can use postRepository over and 
        // again in this class.
        return postRepository.findAll();
    }

    // ...
}
```
Now THAT was easy, huh? 

---
## Complete Initial Integration

1. Finish integrating `Post` repository/controller by 


2. Follow the same pattern to integrate `User` repository/controller. Ignore `getByUsername` and `getByEmail` for now.


3. If you need more acute querying for your endpoints, see [Data Persistence, Pt II](14-data-persistence-iii.md).

---
## The Moment of Truth.

Now, it's time to spin up your application! 

1. Start it, then check your database to see if the `posts` and `users` tables were created!

2. Manually insert a few post and user records.

3. Use Swagger or Postman to fetch all posts and fetch one post.

4. Use Swagger or Postman to fetch all users and fetch one user
        
Congratulations! You have connected your `Post` and `User` Java classes to your database using JPA. But... we're not done yet! On to part 2 of data persistence!



---
### Further Reading
- [What is Dependency Injection?](http://stackoverflow.com/questions/130794/what-is-dependency-injection)
- [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection)
- [Spring Beans and dependency injection](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-spring-beans-and-dependency-injection.html)

## Next Up: [Data Persistence, Pt II](13-data-persistence-ii.md)
