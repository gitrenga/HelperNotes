
# Patterns
 - **Internal iterator** i.e. stream forEach - focus on what to do than how to do in conventional for loop
 - **Stratergy Pattern** - pass function as input (stratergy - predicate) and execute operation based on stratergy
 - **Decorator** - instead of passing object to another object, do function chaining using andThen
 - **Fluent Interface** - builder pattern
 - **Execute Around Method** - 
```
    public class Resource(){
        private Resource(){}
        
        public Resource op1(){System.out.println("op1") return this;}
        public Resource op2(){System.out.println("op2") return this;}
        
        public static void use(Consumer<Resource> block){
            Resource r = new Resource();
            
            block.accept(r);
            
            r.close();
        }
        
        public Resource close(){}
        
    }
    
    psvm(){
        Consumer<Resource> blc = (resource) -> { 
            resource.op1().op2();
        }
        
        Resource.use(blc);
    }
```

# Characteristics of FP

 - Lazy Evaluation
 - Immutable
 - Parallelism
 - Less garbage collection due to reduced number of object created in FP
 - Compiler can optimize it better for memory and performance
 - Pure Function - returns same value for same input if its called any number of times
 - lamda in java have to use variables which are final or effectively final (not final but not modified in code)

# Collectors & Stream
 
 - Lazy Evaluation - functional pipeline is executed only when reduce/terminal operation is started
 - Reduce (identity,accumulator,combiner) - identity is the initial value of any generic type,accumulator - lambda fn takes identity and stream element for      
     accumulating,combiner - how to combine multiple parallel stream, takes output of two accumulator and combine it to one
 - collectors 
    - reduce(initialValueoFAnyType,accumulator,combiner) - reduce(0,(total,element) -> total+element,(r1,r2)->r1+r2)
    - collect(collector)
       - toList,toSet,toMap(keyMapperFn,ValueMapperFn)
       - partitioningBy(lambdaFn),groupingBy(lambdaFn)
       - groupingBy(lambdaFn,collector),mapping(lambdaFn,collector),filtering(lambdaFn,collector)
       - countingAndThen(collector,lambdaFn),collectingAndThen(collector,lambdaFn)
    - sum(),min(),max()
    - maxBy(comparator),minBy(comparator)

# Code examples to understand FP

```
public class GenericsExample<T> {
    //Method using generics and type inference
    public void print(List<T> lst){
        for(T t : lst){
            System.out.println(t.toString());
        }
    }

    //Method using generics and type inference
    public void printWithGenerics(T t){
        System.out.println(t.toString());
    }

    //Method NOT using generics and type inference
    public void printWithoutGenerics(Customer customer){
         System.out.println(customer.toString());
    }
}
  
public interface IAddLambda {
    int add (int x, int y);
    //int add2 (int x, int y);
}
  

public interface IGreeterLamda {
    void perform();
}
  
public interface IGreeter {
    public void perform();
}
  
public class HelloWorldGreeterImpl implements IGreeter{
    @Override
    public void perform() {
        System.out.println("hello world greeter impl");
    }
}
  
public class GreetingCaller {
    public void greet(IGreeter greeter){
        greeter.perform();
    }
}
  
import com.unisys.trans.aircore.demo.LambdaUsage.Customer;
import com.unisys.trans.aircore.demo.generics.GenericsExample;
import com.unisys.trans.aircore.demo.passing_behaviour_fp.IAddLambda;
import com.unisys.trans.aircore.demo.passing_behaviour_fp.IGreeterLamda;
import com.unisys.trans.aircore.demo.passing_behaviour_oops.GreetingCaller;
import com.unisys.trans.aircore.demo.passing_behaviour_oops.HelloWorldGreeterImpl;
import com.unisys.trans.aircore.demo.passing_behaviour_oops.IGreeter;

import java.util.*;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;
import java.util.stream.Collectors;

public class Driver {
    public static void main(String[] args) throws Exception {
        System.out.println("#####section 1########");
        //section 1 : current oops way of passing a behaviour
        /*
        * passing the object "HelloWorldGreeterImpl" that contains behaviour
        *   to be called in the "GreetingCaller"
        * */
        new GreetingCaller().greet(new HelloWorldGreeterImpl());

//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 2########");

        System.out.println("------step 1------");
        //generics and type inference
        testGenericsTypeInference();
        System.out.println("------step 2------");
        //Function(behaviour) as value in inline statement

//        f = public void perform(){System.out.println("fp helloworld");}
//        scope need not be defined since its accessed by 'f' so scope of 'f' is the real scope
//        f =  void perform(){System.out.println("fp helloworld");}
//        name of the method not required since we call this using 'f' not 'perform'
//        f =  void (){System.out.println("fp helloworld");}
//        return type is not required compiler can figure that for you if not it will give compliation error
//        f =   (){System.out.println("fp helloworld");}
//        +syntax sugar = lambda expression - to differenciate input and implementation
//        '{}' required only in case of multiple lines
//        f =   () -> System.out.println("fp helloworld");
//        f =   () -> {int a;System.out.println("fp helloworld");}
//          lambda with return values and examples
//        f =   (int a) -> return a*2; (lambda with 'return')
//        f =   (int a) -> a*2; (lambda without 'return' - in single statement compiler figures it out - wait for it in below section 3 & more - how compiler uses interface definition & type inference to figure it out)
//        f =   (int a,int b) -> a*b;
//        f =   (int a,int b)  -> {
//              if(b == 0) return 0;
//              return a/b;
//          }; (multi line lambda we need '{}')

//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 3########");
        //Section 3 : Lambda as interface implementation
        //below line compiler knows to read the input parameter and return type using the interface defined
        IGreeterLamda greeterLamdba = () -> System.out.println("hello world in lamda");
//      If compiler see mismatch between lambda and interface it will not compile (below will not work)
      //IGreeterLamda greeterLamdba1 = (int a) -> System.out.println("hello world in lamda" + a);
        IAddLambda addLambda = (int a, int b) -> a+b;
        //below will not work - mismatch between lambda and functional interface
        //IAddLambda addLambda1 = (int a, int b,int c) -> a+b;
        //Note name of the method in functional interface dont matter for compiler (change the method name in 'IAddLambda' below code still compiles
        IAddLambda addLambda3 = (int a, int b) -> a+b;
        //if you already have an existing interface that can say compiler about method input parameter type and return type no need to create new interface
        IGreeter test = () -> System.out.println("test");
//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 4########");
        //Section 4 : difference between interface and functional interface
        //try adding a new method in interface 'IAddLambda' below code will not compile

        //i.e. interface should have only one abstract method for the compiler to relate it with lambda expressions
        IAddLambda addLambda4 = (int a, int b) -> a+b;

//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

        System.out.println("#####section 5########");
        //section 5 : difference between oops & fp
        //passing a object that contains the behaviour/impl
        IGreeter a = new HelloWorldGreeterImpl();
        a.perform();
        //passing the behaviour/impl directly without object
        IGreeterLamda b = () -> System.out.println("difference between oops & fp - test1");
        b.perform();
        IGreeter c = () -> System.out.println("difference between oops & fp - test2");
        c.perform();
//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 6########");
        //section 6: passing the lambda

        //oops way of passing the behaviour wrapped inside a object
        new GreetingCaller().greet(new HelloWorldGreeterImpl());
        //fp way of passing the behaviour without object , note that compiler does type inference of lambda with the interface of the input parameter of 'greet'
        IGreeter d = () -> System.out.println("passing the lambda");
        new GreetingCaller().greet(d);
//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 7########");
        //section 7: more on type inference (compiler infers from functional interface and understands the lambda) - working code of similar concept of section 2
        IStringLengthLambda t = (String s) -> s.length();
        System.out.println(t.lengthCalc("1"));
        //no need to specify the type of input parameter
        t = (s) -> s.length();
        System.out.println(t.lengthCalc("12"));
        t = s -> s.length();
        System.out.println(t.lengthCalc("123"));
        t = s -> s.length();
        System.out.println(t.lengthCalc("1234"));
        //passing the lambda, note the input parameter type of method 'execLambda' compiler inference it and match with the passed in lambda
        execLambda(t);
//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 8########");
        //section 8: why java choose interface for providing lambda/fp - for backward compatability
        //below is the older impl
        Thread myThread = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("running thread using annonymous inner class - before lambda");
            }
        });
        myThread.run();
        //note old class of 'Thread' compatible to accept lambda
        Thread myLambdaThread = new Thread(() -> System.out.println("running thread using lambda"));
        myLambdaThread.run();
//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 9########");
        //section 9: functional interface - interface with one abstract method
        //from java 8 interface can have default implementation
        //so interface with as many as method it can have but only one abstract method

        //but assume that today one interface with only one abstract method but in future some one adds
        //additional method to that interface - lambda dependent on those interface will fail since
        //compiler will have problem to unique identify the method and do type inference

        //try adding one more method in 'IAddLambda' interface

        //so java have made it mandatory to add an annotation '@FunctionalInterface'
        //which enforces that interface have only one abstract method
        //try adding one more abstract method in 'IFunctionalInterfaceExample'

        //Also note in normal interface compiler complained on the lambda not matching interface, but if the interface is functional it complains the interface not the lambda
        IFunctionalInterfaceExample iFPexample = (p1,p2) -> p1 + p2;
        System.out.println(iFPexample.concat("fp "," programming"));
        //note the functional interface can have other methods which arent abstract as 'delimit' in 'IFunctionalInterfaceExample' but only one abstract method
        System.out.println(iFPexample.delimit("fp "," programming"));
//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 10########");
        //Section 10: implementations in Java 7
        List<Customer> custLst = Arrays.asList(
                new Customer("Puneet","5","tier5","1",Arrays.asList("AVML")),
                new Customer("Ruchira","4","tier4","2",Arrays.asList("NVML,WINDOWSEAT")),
                new Customer("Anand","3","tier3","3",Arrays.asList("NVML,WIFI")),
                new Customer("Moin","2","tier2","4",Arrays.asList("NVML,LOUNGE")),
                new Customer("Darko","1","tier1","5",Arrays.asList("NVML"))
        );

        //java 7 impl
        //skip this method for now - only placed here to verify the output before and after step 1
        printCustomersInJava7(custLst);
        System.out.println("------step 1------");
        //step 1: sort list by customer value
        Collections.sort(custLst,new CustomerValueComparator());
        System.out.println("------step 2------");
        //step 2: create a method that prints all customers in the list
        printCustomersInJava7(custLst);
        System.out.println("------step 3------");
        //step 3: create a method that prints all customer having 'AVML' as preference
        printCustomerHavingAVMLInJava7(custLst);

        System.out.println("------step 4------");
        //step4 improvise the step3 method from specific print creteria
        // to generic implementation of print creteria using interface

        //below flavour resembles oops way of passing the behaviour wrapped inside the object (data + behaviour)
        printCustomerHavingInJava7WithCreteria(custLst,new CreteriaAVMLInPreferenceOfCustImpl());
        //another flavour of same implementation - note below flavour resembles with lambda (changing/passing the behaviour on the fly)
        printCustomerHavingInJava7WithCreteria(custLst,new ICreteria(){
            @Override
            public boolean testCreteria(Customer cust) {
                return cust.getPreferenceLst().contains("NVML");
            }
        });
        //one pattern difference between oops & fp is fp tries to keep the data separate from the state where oops combine it which may have difficulty in concurrent exe, reuseability, testability
        //where fp shines
//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 11########");
        //Section 11: implementations in Java 8 using Lambda

        custLst = Arrays.asList(
                new Customer("Puneet","5","tier5","1",Arrays.asList("AVML")),
                new Customer("Ruchira","4","tier4","2",Arrays.asList("NVML,WINDOWSEAT")),
                new Customer("Anand","3","tier3","3",Arrays.asList("NVML,WIFI")),
                new Customer("Moin","2","tier2","4",Arrays.asList("NVML,LOUNGE")),
                new Customer("Darko","1","tier1","5",Arrays.asList("NVML"))
        );

        //java 8 impl
        //skip this method for now - only placed here to verify the output before and after step 1
        printCustomersInJava8UsingLambda(custLst);
        System.out.println("------step 1------");
        //step 1: sort list by customer value
        //note that the comparator in java is a functional interface
        //that takes two input parameter and returns 'int'
        //reference -> java 7 -> Collections.sort(custLst,new CustomerValueComparator());
        Collections.sort(custLst,(Customer o1,Customer o2) -> o2.getCustomerValue().compareTo(o1.getCustomerValue()));
        //the below will also work as the compiler figures out the type from the type of
        //first parameter
        // (type inference -
        // 1.compiler checks sort method  sort(@NotNull  java.util.List<T> list, @Nullable  java.util.Comparator<? super T> c )
        // 2.since we are passing 'custLst' i.e List<Customer> and it knows 'T' is Customer )
        // 3.substitute the 'T' with 'Customer' and expects second parameter as 'java.util.Comparator<? super Customer>'
        // 4. it then attempts to type infer the functional interface 'java.util.Comparator<? super Customer>' i.e look for abstract method in it which is 'int	compare(T o1, T o2)'
        // 5. match the lambda expression with 'int	compare(T o1, T o2)' and do the type inference

        //Note it already knows 'T' is customer thats why it allows auto completion of 'o2.getCustomerValue()' i.e call method belonging to 'Customer' object
        Collections.sort(custLst,(o1,o2) -> o2.getCustomerValue().compareTo(o1.getCustomerValue()));
        printCustomersInJava8UsingLambda(custLst);
        System.out.println("------step 2------");
        //step 2: create a method that prints all customers in the list
        //note the difference inside the method
        printCustomersInJava8UsingLambda(custLst);
        System.out.println("------step 3------");
        //below method re-use the java7 style for iteration and printing
        //note the second parameter is an interface 'Icreteria'
        printCustomerHavingInJava8WithCreteriaUsingLambdaV1(custLst,(cust) -> cust.getPreferenceLst().contains("NVML"));
        System.out.println("------step 4------");
        //below method re-use the java8 style for iteration and printing
        //note the second paramter is an java functional interface 'Predicate'
        printCustomerHavingInJava8WithCreteriaUsingLambdaV2(custLst,(cust) -> cust.getPreferenceLst().contains("AVML"));
        //reference -> java 7 ->
        //        printCustomerHavingInJava7WithCreteria(custLst,new CreteriaAVMLInPreferenceOfCustImpl());
        //        printCustomerHavingInJava7WithCreteria(custLst,new ICreteria(){
        //            @Override
        //            public boolean testCreteria(Customer cust) {
        //                return cust.getPreferenceLst().contains("NVML");
        //            }
        //        });
        System.out.println("------step 5------");
        //note if predicate is always true - it will print all the elements
        printCustomerHavingInJava8WithCreteriaUsingLambdaV2(custLst,cust -> true);
//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        System.out.println("#####section 12########");
        //Section 12: built in functional interface
        //package : java.util.function (https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)
        //no need to create a new functional interface every time we need to write a lambda instead java have built in for most of the required ones.
        //Predicate - we have already used this above
        System.out.println("------step 1------");
        final List<Customer> custList = Arrays.asList(
                new Customer("Puneet","5","tier5","1",Arrays.asList("AVML")),
                new Customer("Ruchira","4","tier4","2",Arrays.asList("NVML,WINDOWSEAT")),
                new Customer("Anand","3","tier3","3",Arrays.asList("NVML,WIFI")),
                new Customer("Moin","2","tier2","4",Arrays.asList("NVML,LOUNGE")),
                new Customer("Darko","1","tier1","5",Arrays.asList("NVML"))
        );
        //Supplier
        fnUsingBuiltInFuntionalInterface_Supplier(() -> custList.get(0));
        System.out.println("------step 2------");
        //Consumer
        //Note the we can pass different behaviour without changing the implementation in method 'fnUsingBuiltInFuntionalInterface_Comsumer'
        fnUsingBuiltInFuntionalInterface_Comsumer(customer -> System.out.println(customer.toString()));
        fnUsingBuiltInFuntionalInterface_Comsumer(customer -> System.out.println(customer.getName()));
        System.out.println("------step 3------");
        //Function
        fnUsingBuiltInFuntionalInterface_Function(cus -> cus.toString());

//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

        System.out.println("#####section 13########");
        //closure in lambda
        System.out.println("------step 1------");
        int l1 = 10;
        int l2 = 20;
        //java 7 similar to closure

        //note that the variable 'l2' not in scope of the method 'doProcess' where exactly the process method is executed
        //but the java compiler keeps track of this value for us
        doProcess(l1,new Process(){
            @Override
            public void process(int i) {
                System.out.println(i+l2);
            }
        });
        System.out.println("------step 2------");

        //try changing the value of l2 before this lamda and check the compiler error.
        //l2=30;
        doProcess(l1,i -> System.out.println(i+l2));

//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

        System.out.println("#####section 14########");
        System.out.println("------step 1------");
        //method reference in lambda
        //()->m1()
      Thread t1 = new Thread(() -> demoMethodReferenceNoParam());
      t1.run();
      t1 = new Thread(Driver::demoMethodReferenceNoParam);
      t1.run();
        System.out.println("------step 2------");
      //(p) -> m1(p)
        demoMethodReferenceWithParam(p -> System.out.println(p));
        demoMethodReferenceWithParam(System.out::println);

//$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

        System.out.println("#####section 15########");
        //Streams in lambda

        //conventional loop vs stream
        //source- stream,
        // operations-filter,sort,map,anyMatch,distinct,findFirst,flatmap,skip,sorted
        // terminal-collect,count,max,min,reduce,
        System.out.println("------step 1------");
        streamAndLoopCompare(custLst);
        System.out.println("------step 2------");
        simpleGrouping(custLst);
    }





    private static void doProcess(int l1, Process process) {
        process.process(l1);
    }

    public interface Process{
        void process(int i);
    }

    private static void demoMethodReferenceNoParam(){
        System.out.println("demoMethodReferenceNoParam");
    }

    private static void demoMethodReferenceWithParam(Consumer<String> consume){
        consume.accept("demoMethodReferenceWithParam");
    }

    private static void fnUsingBuiltInFuntionalInterface_Function(Function<Customer,String> function) {
        String returnValue = function.apply(new Customer("SKY","7","tier7","7",Arrays.asList("XBAG,WIFI")));
        System.out.println(returnValue);
    }

    private static void fnUsingBuiltInFuntionalInterface_Comsumer(Consumer<Customer> consumer) {
        consumer.accept(new Customer("Pandya","6","tier6","6",Arrays.asList("XBAG")));
    }

    private static void fnUsingBuiltInFuntionalInterface_Supplier(Supplier<Customer> customerSupplier) {
        System.out.println(customerSupplier.get().toString());
    }

    private static void printCustomerHavingInJava8WithCreteriaUsingLambdaV1(List<Customer> custLst, ICreteria creteriaLambda) {
        //custLst.stream().allMatch(creteriaLambda);
        //reusing the same method written in java 7 style
        //just note that the creteria is passed as lambda instead of object containing impl
        printCustomerHavingInJava7WithCreteria(custLst,creteriaLambda);
    }

    private static void printCustomerHavingInJava8WithCreteriaUsingLambdaV2(List<Customer> custLst, Predicate<Customer> predicate) {
        //note the behaviour is embeded inside the for loop which in a way not great separation of concern i.e both looping and logic what to do in loop
//        for(Customer cust : custLst){
//            if(creteria.testCreteria(cust))
//                System.out.println(cust.toString());
//        }
        //note the behaviour is not embeded inside the  loop which in a way is good separation of concern i.e  looping and logic of what to do in loop is separated

        //This makes more sense assuming this loop is inside a third party jar know we dont have flexibility to add more behaviour to the loop
        //whereas if the third party implemented it in a functional way they have a option to think of chaining multiple behiour such as 'filter' on the fly based on caller without
        //code change on their part
        custLst.stream().filter(predicate).forEach((item) -> System.out.println(item));
    }

    private static void printCustomersInJava8UsingLambda(List<Customer> custLst) {
        //reference -> java 7 ->
        //        for(Customer cust : custLst){
        //            System.out.println(cust.toString());
        //        }

        //wait for understanding below implementation in section 11, but just compare the structural difference of java 7 & java 8 (where it works with lambda)
        custLst.stream().forEach((item) -> System.out.println(item));
    }

    public interface ICreteria{
        boolean testCreteria(Customer cust);
    }

    public static class CreteriaAVMLInPreferenceOfCustImpl implements ICreteria{
        @Override
        public boolean testCreteria(Customer cust) {
            return cust.getPreferenceLst().contains("AVML");
        }
    }

    public static class CreteriaNVMLInPreferenceOfCustImpl implements ICreteria{
        @Override
        public boolean testCreteria(Customer cust) {
            return cust.getPreferenceLst().contains("NVML");
        }
    }

    private static void printCustomerHavingInJava7WithCreteria(List<Customer> custLst,ICreteria creteria) {
        //note the behaviour is embeded inside the for loop which in a way not great separation of concern i.e both looping and logic what to do in loop
        for(Customer cust : custLst){
            if(creteria.testCreteria(cust))
                System.out.println(cust.toString());
        }
    }

    private static void printCustomerHavingAVMLInJava7(List<Customer> custLst) {
        for(Customer cust : custLst){
            if(cust.getPreferenceLst().contains("AVML"))
                System.out.println(cust.toString());
        }
    }

    private static void printCustomersInJava7(List<Customer> custLst) {
        //worst case you could have also used traditional for loop
        for(Customer cust : custLst){
            System.out.println(cust.toString());
        }
    }

    public interface IStringLengthLambda{
        int lengthCalc(String a);
    }

    public static void execLambda(IStringLengthLambda l){
        System.out.println(l.lengthCalc("12345"));
    }

    @FunctionalInterface
    public interface IFunctionalInterfaceExample{
        String concat(String a,String b);
        //String concat2(String a,String b);
        default String delimit(String a,String b){
            return a + "/" + b;
        }
    }

    public static class CustomerValueComparator implements Comparator<Customer>{
        @Override
        public int compare(Customer o1, Customer o2) {
            return o2.getCustomerValue().compareTo(o1.getCustomerValue());
        }
    }

    public static void simpleGrouping(List<Customer> custLst) {
        Map<String, List<Customer>> map = custLst
                .stream()
                .collect(Collectors.groupingBy(Customer::getFrequentFlierNumber));
        map.forEach((s, cust) -> {
            System.out.println("FF " + s);
            cust.forEach(System.out::println);
        });
    }

    public static void streamAndLoopCompare(List<Customer> custLst) {



        List<Customer> highTierCustomerLst = new ArrayList<>();
        int limit = 2;
        int counter = 0;
        for (Customer customer : custLst) {
            if (Integer.valueOf(customer.getCustomerValue()) > 2) {
                highTierCustomerLst.add(customer);
                counter++;
                if (counter == limit) {
                    break;
                }
            }
        }
            System.out.println("------imperative result------");
            highTierCustomerLst.forEach(System.out::println);

        //source- stream,
        // operations-filter,sort,map,anyMatch,distinct,findFirst,flatmap,skip,sorted
        // terminal-collect,count,max,min,reduce,

            highTierCustomerLst = custLst.parallelStream()
                    .filter(customer -> Integer.valueOf(customer.getCustomerValue()) > 2)
                    .limit(2)
                    .collect(Collectors.toList());
            System.out.println("------declarative result------");
            highTierCustomerLst.forEach(System.out::println);

        }
    public static void testGenericsTypeInference(){
        List<Customer> custLst = Arrays.asList(
                new Customer("Puneet","5","tier5","1",Arrays.asList("AVML")),
                new Customer("Ruchira","4","tier4","2",Arrays.asList("NVML,WINDOWSEAT")),
                new Customer("Anand","3","tier3","3",Arrays.asList("NVML,WIFI")),
                new Customer("Moin","2","tier2","4",Arrays.asList("NVML,LOUNGE")),
                new Customer("Darko","1","tier1","5",Arrays.asList("NVML"))
        );

        List<String> dummyStringLst = Arrays.asList("A","B","C","D");

        //note the generics definition in class 'GenericsExample' and understand whats generics and how type inference happens in below code

        //method 'printWithoutGenerics' demands 'Customer' as input and it will not accept 'String' as input
        new GenericsExample<String>().printWithoutGenerics(custLst.get(0));
        //new GenericsExample<String>().printWithoutGenerics("generics type inference understanding");

        //method 'printWithGenerics' can accept whatever type that is mentioned - both 'String' or 'Customer' or any

        //1.From this line 'new GenericsExample<Customer>()' compiler understands 'T' in 'GenericsExample' is 'Customer'
        //2.And expects 'printWithGenerics' input parameter to be of type 'Customer'
        new GenericsExample<Customer>().printWithGenerics(custLst.get(0));
        //new GenericsExample<Customer>().printWithGenerics("asdas");

        //1.From this line 'new GenericsExample<String>()' compiler understands 'T' in 'GenericsExample' is 'String'
        //2.And expects 'printWithGenerics' input parameter to be of type 'String'
        new GenericsExample<String>().printWithGenerics("generics type inference");

        //and how for same implementation we can pass different type of 'List<Customer or String or xxx>'
        new GenericsExample<Customer>().print(custLst);
        new GenericsExample<String>().print(dummyStringLst);
    }

    }
    
    ```
