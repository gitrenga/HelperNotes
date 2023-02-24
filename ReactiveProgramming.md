# What is Reactive Programming

  - Declarative programming that work with data stream
  - Static (Arrays) and Dynamic (Event emitters) data stream
  - Reactive and Async is DIFFERENT
  - examples 
      - user clicks triggers an event using callbacks
      - io is done call back is triggered

# Why Reactive Programming

  - high data scale handling
  - high usage scale handling
  - cloud cost reduction

      - Mitigation : **converts blocking call** which holds the thread than necessary **to non blocking** hence server can **effectively use** its **limited threads** to serve more request
                     which means **to do more with given hardware/thread**

# Difference from Async Programming

  - Two threads can run in parallel but it may require to join the result hence we require both the threads to complete
  - In Async programming all the **functions are executed immediately** in a separate thread
  - In Reactive Programming the **functions are executed only when an Event happens** i.e the functions are lazy also it can be synchronous or asynchronous

 
 ```
 //Sequential Programming
 
 User getUserWithPreference(int userId){
      User user = getUserFromDB(userId);
      Preference preference = getUserPreferenceFromDB(userId);
      user.SetPreference(preference);
      return user;
  }
  
   //Async Programming
   
  User getUserWithPreference(int userId){
      CompleteableFuture<User> completeableFutureOfUser = CompleteableFuture.async(getUserFromDB(userId));
      CompleteableFuture<Preference> completeableFutureOfPreference = CompleteableFuture.async(getUserPreferenceFromDB(userId));
      
      CompleteableFuture<void> completeableFutureOfUserPreference = CompleteableFuture.allOf(completeableFutureOfUser,completeableFutureOfPreference);
      
      completeableFutureOfUserPreference.join();
      
      User user = completeableFutureOfUser.join();
      Preference preference = completeableFutureOfPreference.join();
      
      user.SetPreference(preference);
      return user;
   }   
      //Reactive Programming
      
 Mono<User> getUserWithPreference(int userId){
        userService.getUser(userId)
          .zipWith(userPreferenceService.getPreference())
          .map
          (
            tuple -> {
              user = tuple.getT1();
              preference = tuple.getT2();
              user.setPreference(preference);
              return user;
            }
          );
  }
  
 ```

# Reactive Programming in Java

  - Java only provides the Interface and not the implementation
        - Publisher
        - Subscriber
        - Subscription
  - Impl provided by third party lib such as Reactor,RxJava

# Reactor

  - Reactor API have - Data,Completion,Error channel - all these channels can be consumed using consumer lambda
  - Flux 0-N items
  - Mono 0-1 items
  - block() - blocking mono
  - toStream() - blocking flux
  - filter(),map(),collectList(),buffer(),take()
  - doOnError(),onErrorContinue(),onErrorResume()
  - distinct(),distinctUntillChanged()
  - defaultIfEmpty()
  - MySubscriber<T> extends BaseSubscriber<T>
      - hookOnSubscribe()
      - hookOnNext()
      - hookOnComplete()
  - FunctionRouter and FunctionHandler and FilterFunction
      - FunctionRouter in configuration map input URL to FunctionHandler
  - doOnNext(),then()
  
 ```
  @FunctionalInterface
public interface RouterFunction<T extends ServerResponse> {
    Mono<HandlerFunction<T>> route(ServerRequest request);
    // ...
}
 ```
  
  ![image](https://user-images.githubusercontent.com/55741060/221101956-13bf1692-789f-4d73-ad92-cc97a302426d.png)

  
      - FunctionRouter get ServiceRequest and returns ServiceResponse
  
   ```
 public static <T extends ServerResponse> RouterFunction<T> route(
  RequestPredicate predicate,
  HandlerFunction<T> handlerFunction)
 ```
  
  ![image](https://user-images.githubusercontent.com/55741060/221102056-24590bc5-0f49-4b46-a58a-2026a23a493e.png)

