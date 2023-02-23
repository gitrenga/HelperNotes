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
  - In Reactive Programming the **functions are executed only when an Event happens** i.e the functions are lazy

 
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

  - Jave only provides the Interface and not the implementation
  - Impl provided by third party lib such as Reactor,RxJava
