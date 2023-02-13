https://stackabuse.com/a-guide-to-jpa-with-hibernate-relationship-mapping/

one to one

one to many

  - fetch is lazy by default
  
many to one

 -  option = true by default
  
many to many

  - join table
  
      - joincolumn
    
      - inversejoincolumn

optional = true, false

cascading = fetch,merge,delete,drop,all

fetchtype = eager,lazy

owning side - @joincolumn will be present, best practice to put on the foriegn key side

referencing side - mappedby = owning side variable name

  
  ![image](https://user-images.githubusercontent.com/55741060/218555942-cc16d367-73e9-4ba8-affc-eb3a4052f98a.png)

  
  ![image](https://user-images.githubusercontent.com/55741060/218556062-4ff6022d-13d4-4c57-9a03-b33cfc84c246.png)
