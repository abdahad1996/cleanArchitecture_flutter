# cleanArchitecture_flutter

![alt text](https://i0.wp.com/resocoder.com/wp-content/uploads/2019/08/Clean-Architecture-Flutter-Diagram.png?w=556&ssl=1)

# Explanation & Project Organization
    Every "feature" of the app, like getting some interesting trivia about a number, will be divided into 3 layers - presentation, domain and data.
    
# Presentation
    
    This is the stuff you're used to from "unclean" Flutter architecture.
    You obviously need widgets to display something on the screen.
    These widgets then dispatch  events to the Bloc and listen for states 
    (or an equivalent if you don't use Bloc for state management).
    Note that the "Presentation Logic Holder" (e.g. Bloc) doesn't do much by itself. 
    It delegates all its work to use cases. At most, the presentation layer handles basic input conversion and validation
    
   ![alt text](https://i0.wp.com/resocoder.com/wp-content/uploads/2019/08/presentation-layer-diagram.png?w=287&ssl=1)

    
 # Domain

    Domain is the inner layer which shouldn't be susceptible to the whims of changing data sources or porting our app to Angular Dart. 
    It will contain only the core business logic (use cases) and business objects (entities).
    It should be totally independent of every other layer.
    Use Cases are classes which encapsulate all the business logic of a particular use case of the app
    (e.g. GetConcreteNumberTrivia or GetRandomNumberTrivia).
    
    But... How is the domain layer completely independent when it gets data from a Repository,
    which is from the data layer?  Do you see that fancy colorful gradient for the Repository?
    That signifies that it belongs to both layers at the same time. We can accomplish this with dependency inversion.
    
    That's just a fancy way of saying that we create an abstract Repository class defining a contract of what the Repository must do - 
    this goes into the domain layer. We then depend on the Repository "contract" defined in domain, 
    knowing that the actual implementation of the Repository in the data layer will fullfill this contract.
    
    Dependency inversion principle is the last of the SOLID principles.
    It basically states that the boundaries between layers should be handled with interfaces (abstract classes in Dart).
    
   ![alt text](https://i0.wp.com/resocoder.com/wp-content/uploads/2019/08/domain-layer-diagram.png?w=141&ssl=1)

    
 # Data

    The data layer consists of a Repository implementation 
    (the contract comes from the domain layer) and data sources - one is usually for getting remote (API) data and the other for caching that data.
    Repository is where you decide if you return fresh or cached data, when to cache it and so on.
    You may notice that data sources don't return Entities but rather Models. 
    The reason behind this is that transforming raw data (e.g JSON) into Dart objects requires some JSON conversion code.
    We don't want this JSON-specific code inside the domain Entities - what if we decide to switch to XML?
    
    Therefore, we create Model classes which extend Entities and add some specific functionality (toJson, fromJson) or additional fields, 
    like database ID, for example
    
    The RemoteDataSource will perform HTTP GET requests on the Numbers API. LocalDataSource will simply cache data using the shared_preferences package.
    These two data sources will be "combined" in NumberTriviaRepository
    which will be the single source of truth for the interesting number trivia data.
    The caching policy will be very simple. 
    If there is a network connection, always get data from the API and cache it. Then,
    if there's no network, return the latest cached data.
  ![alt text](https://i0.wp.com/resocoder.com/wp-content/uploads/2019/08/data-layer-diagram.png?w=329&ssl=1)
  
  
  

 
